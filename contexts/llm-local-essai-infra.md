---
exported: 2026-07-10 06:11
topic: LLM local — Essai infra
source: second cerveau Nicolas MAUVILAIN
files: 4
---

# Contexte : LLM local — Essai infra

Export intégral de toutes les notes du périmètre (contenu brut, non résumé).

## Sommaire

- 1 PROJECTS/Perso Nicolas/LLM local — Essai infra/0 — Vue d'ensemble.md
- 1 PROJECTS/Perso Nicolas/LLM local — Essai infra/1 — Faisabilité VPS.md
- 1 PROJECTS/Perso Nicolas/LLM local — Essai infra/2 — Pilotage et coexistence.md
- 1 PROJECTS/Perso Nicolas/LLM local — Essai infra/3 — Plan maquette hybride.md

---

## 1 PROJECTS/Perso Nicolas/LLM local — Essai infra/0 — Vue d'ensemble.md

```markdown
---
type: projet
statut: exploration
créé: 2026-07-10
casquette: "[[Perso Nicolas]]"
---

# LLM local — Essai infra

## Intention

Monter une maquette d'**infrastructure LLM local** pour tester en conditions réelles le pattern d'[[Architecture hybride — modèle local par défaut, modèle puissant à la demande]] : un petit modèle local traite le quotidien, on escalade vers un modèle plus puissant hébergé en Europe quand la tâche l'exige. Objectif = comprendre la mécanique (serveur d'inférence, client agentique, routage, coexistence) avant tout investissement matériel.

## Origine

Note zettel du 10/07/2026 issue de Charly Hayoz (*Coder industriellement en local*, LinkedIn) : combinaison Qwen local + modèles plus gros hébergés en Europe/Suisse via un routeur souverain type [EUrouter](https://www.eurouter.ai/). Envie de reproduire ce pattern à coût quasi nul.

## Questions de départ

1. Est-ce possible sur le VPS OVH actuel ?
2. Ça se pilote avec Claude Code ou faut-il OpenCode ?
3. Est-ce que ça coexiste avec l'infra Claude Code existante ?

## Verdict (voir fiches détaillées)

- **VPS actuel = CPU-only, insuffisant** pour les modèles visés (Qwen 122B, GLM, gros Mistral). Ne convient que pour un petit modèle 3B en test. → [[1 — Faisabilité VPS]]
- **Pilotage : pas Claude Code** (câblé Anthropic). Il faut OpenCode / Aider sur endpoint OpenAI-compatible (Ollama). → [[2 — Pilotage et coexistence]]
- **Coexistence : oui**, sans conflit logiciel ; seule contrainte = la RAM. → [[2 — Pilotage et coexistence]]
- **Plan retenu = maquette hybride** : Qwen2.5 3B local + EUrouter pour l'escalade, piloté par OpenCode. → [[3 — Plan maquette hybride]]

## Prochaine étape

Valider la marge RAM service par service, puis installer Ollama + Qwen2.5 3B + OpenCode sur le VPS.

## Liens

- [[Architecture hybride — modèle local par défaut, modèle puissant à la demande]]
- [[L'arbitrage cloud vs local se joue sur le profil de charge]]
- [[La confiance prime sur l'économie dans l'adoption de l'IA sur code sensible]]
- [[Local ne veut pas dire sécurisé — scoper les droits de l'agent]]

```

## 1 PROJECTS/Perso Nicolas/LLM local — Essai infra/1 — Faisabilité VPS.md

```markdown
---
type: note-projet
créé: 2026-07-10
---

# 1 — Faisabilité VPS

## Specs relevées (VPS OVH `/opt/vault`, 10/07/2026)

| Ressource | Valeur | Commentaire |
|---|---|---|
| CPU | 6 vCPU Intel Haswell (no TSX), virtualisés, 1 thread/core | Pas d'AVX-512, perf CPU modeste |
| RAM | 11 Go total, **~8,8 Go disponibles** | Infra actuelle consomme ~2,6 Go |
| Swap | **0 B** | Aucune marge en cas de dépassement → risque OOM |
| GPU | **Aucun** | Le « Cirrus Logic GD 5446 » listé = carte d'affichage émulée, inutilisable pour l'inférence |
| Disque | 96 Go, ~71 Go libres | OK pour stocker des poids de modèles |

## Conséquence : inférence CPU-only

Sans GPU, tout tourne sur CPU. Ordre de grandeur des besoins mémoire (quantifié Q4) vs faisabilité :

| Modèle | RAM/VRAM nécessaire (Q4) | Sur ce VPS |
|---|---|---|
| Qwen 122B | ~70–140 Go | ❌ impossible |
| GLM (gros) / Mistral Large | dizaines de Go | ❌ impossible |
| Qwen2.5 **7B** Q4 | ~5–6 Go RAM | ⚠️ à la limite (0 swap) |
| Qwen2.5 **3B** Q4 | ~2–3 Go RAM | ✅ ok pour tester |

## Points durs

- **Débit** : même un 7B sur ce CPU Haswell 6 vCPU sortira quelques tokens/seconde. Suffisant pour valider une mécanique, **inutilisable pour du vrai travail agentique**.
- **OOM** : charger un 7B (~5 Go) sur 8,8 Go dispo, sans swap, met en danger n8n / ccpa-telegram / les autres services. Un 3B est plus sûr.

## Conclusion

Le concept « Qwen 122B local + escalade EU » de la note zettel **n'est pas réalisable sur ce VPS**. Il suppose une machine avec GPU (24–80 Go VRAM).

Pour du LLM local sérieux → instance GPU à l'heure (OVH, Scaleway, **RunPod** ~0,5–2 €/h) ou machine GPU maison. Le VPS ne sert qu'à valider le **workflow**, pas la qualité.

```

## 1 PROJECTS/Perso Nicolas/LLM local — Essai infra/2 — Pilotage et coexistence.md

```markdown
---
type: note-projet
créé: 2026-07-10
---

# 2 — Pilotage et coexistence

## Claude Code ne pilote pas un LLM local arbitraire

Claude Code est **câblé aux modèles Anthropic**. Il ne peut pas piloter Qwen/GLM/Mistral en local. Pour ça il faut un client agentique qui parle aux endpoints **OpenAI-compatibles**.

## Stack de pilotage

- **Serveur d'inférence : Ollama** — expose une API OpenAI-compatible sur un port local. Alternative : llama.cpp / vLLM.
- **Client agentique** (au choix) :
  - **OpenCode** — le plus proche de l'expérience Claude Code.
  - **Aider** — déjà utilisé pour le [[Second cerveau Clémence]], maîtrisé.
  - Cline (extension VS Code).

## Coexistence avec l'infra Claude Code : OUI

- Ollama tourne comme **service systemd** sur son propre port → **aucun conflit logiciel** avec Claude Code, qui reste indépendant.
- Claude Code peut même rester **orchestrateur** et déléguer des sous-tâches au modèle local via script — cohérent avec le pattern [[Commande /dispatch]].
- **Seul point de friction = la RAM**, pas le logiciel. Avec 8,8 Go dispo, 0 swap et 2,6 Go déjà pris, il faut valider la marge service par service avant de charger un modèle. Un 3B est le choix prudent.

## Rappel process (cf. mémoire infra)

- Jamais `nohup ... &` depuis Claude → **service systemd dédié** + sudoers NOPASSWD.
- Surveiller la RAM après démarrage d'Ollama (garde-fou contexte ccpa déjà en place).

```

## 1 PROJECTS/Perso Nicolas/LLM local — Essai infra/3 — Plan maquette hybride.md

```markdown
---
type: note-projet
créé: 2026-07-10
statut: à lancer
---

# 3 — Plan maquette hybride

## Objectif

Reproduire l'[[Architecture hybride — modèle local par défaut, modèle puissant à la demande]] en miniature, à coût quasi nul, pour valider le pattern avant tout investissement GPU.

## Architecture cible

```
OpenCode (client agentique)
   │
   ├── quotidien / tâches simples ──► Ollama + Qwen2.5 3B  (local, VPS)
   │
   └── escalade / tâches complexes ──► EUrouter  (modèle EU souverain, à la demande)
```

Décision de routage sur 3 critères (repris de la note zettel) : **complexité de la tâche · sensibilité des données · coût**.

## Étapes

1. **Pré-check RAM** — mesurer la conso réelle service par service (n8n, ccpa-telegram, veille, etc.) pour confirmer la marge avant de charger un modèle.
2. **Ollama** — installer en service systemd, tirer `qwen2.5:3b` (Q4).
3. **OpenCode** — installer, configurer le endpoint Ollama comme provider par défaut.
4. **Test local** — valider le workflow agentique (débit, qualité indicative) sur une tâche simple.
5. **EUrouter** — créer un compte, brancher comme second provider dans OpenCode pour l'escalade.
6. **Routage** — tester la bascule local → EU selon la tâche.
7. **Retex** — noter débit, limites, ressenti dans une fiche `4 — Retex`.

## Coût & alternatives

- Maquette VPS : **0 €** (valide le workflow, pas la qualité).
- Pour la qualité : instance GPU à l'heure (RunPod / Scaleway / OVH GPU, ~0,5–2 €/h).
- Partie souveraine sans local : EUrouter utilisable seul, directement.

## Garde-fous

- Modèle 3B (pas 7B) pour préserver les services existants (0 swap).
- Service systemd + sudoers NOPASSWD, jamais de process détaché depuis Claude.
- Surveiller OOM après démarrage.

```

