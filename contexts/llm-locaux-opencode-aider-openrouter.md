---
exported: 2026-07-12 06:18
topic: Essais de LLM locaux, OpenCode, Aider, OpenRouter
source: second cerveau Nicolas MAUVILAIN
files: 8
---

# Contexte : Essais de LLM locaux, OpenCode, Aider, OpenRouter

Export intégral de toutes les notes du périmètre (contenu brut, non résumé).

## Sommaire

- 1 PROJECTS/Perso Nicolas/LLM local — Essai infra/0 — Vue d'ensemble.md
- 1 PROJECTS/Perso Nicolas/LLM local — Essai infra/1 — Faisabilité VPS.md
- 1 PROJECTS/Perso Nicolas/LLM local — Essai infra/2 — Pilotage et coexistence.md
- 1 PROJECTS/Perso Nicolas/LLM local — Essai infra/3 — Plan maquette hybride.md
- 1 PROJECTS/Perso Nicolas/LLM local — Essai infra/4 — Note de synthèse et décisions.md
- 1 PROJECTS/Perso Nicolas/LLM local — Essai infra/5 — LLM Gateway.md
- 1 PROJECTS/Perso Nicolas/LLM local — Essai infra/6 — Déploiement maquette.md
- 1 PROJECTS/Perso Nicolas/LLM local — Essai infra/7 — Benchmark harnesses.md

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
- **Plan initial** : Qwen2.5 3B local + EUrouter pour l'escalade, piloté par OpenCode. → [[3 — Plan maquette hybride]]

## Direction validée (post-discussion) → [[4 — Note de synthèse et décisions]]

Décisions arrêtées après discussion externe :

- **Conserver le VPS actuel** (laboratoire d'expérimentation), le projet s'adapte aux ressources.
- **Cible = Qwen 2.5 3B** (pas de 7B pour l'instant).
- **Concept central : "LLM Gateway"** — une API interne unique par où passent Claude Code, n8n, Telegram, Obsidian et les futurs agents ; ils ignorent le modèle réellement utilisé.
- **Anti-lock-in** : le routeur reste maître du choix ; EUrouter optionnel ; appel direct possible à Anthropic (clé Pro), OpenAI, Google, Mistral.
- **Routage selon complexité ET charge serveur** : bascule auto vers Claude si tâche complexe ou VPS trop chargé.
- **Outils à maîtriser** : Claude Code (principal) + OpenCode + Aider (test/comparaison) pour conseiller les clients.
- **Vision** : base d'une offre de conseil IA reproductible et déployable chez les clients ([[IACMOI]]).

## Déploiement (10/07/2026) → [[6 — Déploiement maquette]]

Étapes 1 à 7 du plan exécutées en une session `/dispatch` :

- **Ollama + Qwen 2.5 3B** : service systemd limité (MemoryMax 5G, CPUQuota 400 %), 127.0.0.1:11434, ~8,7 tokens/s
- **[[5 — LLM Gateway]]** : API OpenAI-compatible sur 127.0.0.1:8095, routage auto (tokens, load, RAM), repli local si cloud non configuré
- **Aider v0.86.2 + OpenCode v1.17.18** : opérationnels sur le Qwen local, Claude Code intact
- Docs techniques reproductibles : `/home/ubuntu/llm-lab/docs/`

## Benchmark harnesses × modèles (11/07/2026) → [[7 — Benchmark harnesses]]

Comparaison Aider / OpenCode sur 5 modèles locaux, juge = pytest. **Aider 4/5, OpenCode 0/5.**
Sweet spot local = **Aider + qwen2.5-coder:3b** (réussit, rapide, tient sous le cap 5 Go).
OpenCode inadapté aux locaux ≤8B (planifie sans éditer) → à réserver au cloud.

## Prochaines étapes

- Renseigner `ANTHROPIC_API_KEY` dans `/etc/llm-gateway/env` pour activer le cloud (action manuelle)
- Passe témoin Claude Code / Sonnet sur le benchmark (plafond qualité)
- Phase 2 cloud du benchmark (OpenRouter) — où OpenCode devrait briller
- Affiner les seuils de routage à l'usage ; plus tard : streaming, `/v1/embeddings`

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

## 1 PROJECTS/Perso Nicolas/LLM local — Essai infra/4 — Note de synthèse et décisions.md

```markdown
---
type: note-projet
créé: 2026-07-10
statut: référence
---

# 4 — Note de synthèse et décisions

> Synthèse issue de la discussion externe (ChatGPT / Claude Chat) sur l'export /discussion. Fixe la direction validée du projet — supersède le plan initial de [[3 — Plan maquette hybride]].

## Objectif

Mettre en place une architecture d'assistant IA personnelle reposant sur :

- un **LLM local** pour les tâches courantes ;
- un **LLM cloud** pour les tâches nécessitant davantage de raisonnement ;
- une **architecture réutilisable** chez de futurs clients.

Réduire les coûts, garder les données sensibles localement lorsque c'est pertinent, et acquérir une expertise concrète sur les architectures hybrides.

---

## Décisions prises

### 1. Conserver le VPS actuel

Aucune montée en gamme dans un premier temps. Le VPS actuel est une excellente plateforme d'expérimentation : sauvegardes OVH quotidiennes, environnement stable, accès distant permanent, infrastructure existante déjà en place. **Le projet s'adapte aux ressources actuelles, pas l'inverse.**

### 2. Ne pas viser un modèle 7B immédiatement

- un 7B sur CPU fonctionnerait mais avec des performances modestes ;
- le rapport coût/performance d'un VPS plus puissant n'est pas assez intéressant à ce stade.

Le projet se concentre sur **Qwen 2.5 3B**, beaucoup plus cohérent avec la configuration actuelle.

### 3. Le cloud uniquement lorsqu'il apporte une réelle valeur

```
Tâche simple  ──►  Qwen 2.5 3B (local)
Tâche complexe ──►  Claude via API Anthropic
```

Le routage doit rester **entièrement configurable**. L'architecture ne doit dépendre d'aucun fournisseur spécifique.

### 4. EUrouter

Option intéressante, mais :

- le choix du modèle reste piloté par **notre** routeur ;
- EUrouter ne sert qu'à sélectionner le meilleur fournisseur européen.

L'architecture doit aussi permettre de **contourner EUrouter** et d'appeler directement Anthropic, OpenAI, Google, Mistral. Objectif : éviter tout verrouillage technologique.

### 5. Utilisation du compte Anthropic Pro

Quand une tâche nécessite Claude, le routeur peut appeler directement l'API Anthropic avec la clé API du compte professionnel — davantage de maîtrise que de dépendre exclusivement d'EUrouter.

---

## Utilisations prévues du LLM local

Pas seulement discuter : un véritable **service interne** utilisé par tout l'écosystème.

- résumer des documents ;
- reformuler un texte ;
- classifier des informations ;
- extraire des données ;
- préparer des prompts ;
- nettoyer des contenus ;
- produire des synthèses ;
- assister les workflows n8n ;
- prétraiter les informations avant envoi vers Claude.

---

## Architecture cible

```
                Telegram
                     │
              Claude Code
                     │
                   n8n
                     │
          Second cerveau (Obsidian)
                     │
                     ▼
          API interne "LLM Gateway"
             ├───────────────┐
             │               │
             ▼               ▼
      Ollama / Qwen      Claude API
      (local)            (Anthropic)
             │
             ▼
        Décision automatique
        selon la complexité
```

Tous les composants (Claude Code, n8n, Telegram, Obsidian, futurs agents) passent par la **même API interne** et n'ont jamais besoin de connaître le modèle réellement utilisé.

---

## Contraintes importantes

Le VPS héberge déjà Claude Code, les workflows n8n, le second cerveau et divers services système. **L'installation du LLM local ne doit pas dégrader ces performances.**

Avant toute mise en production, mesurer : utilisation CPU, RAM disponible, charge disque, pics de consommation. Le LLM doit fonctionner avec des **limites de ressources** pour ne jamais pénaliser les services existants.

Évolution souhaitable — règle de routage : **si le serveur est fortement chargé, les requêtes partent automatiquement vers Claude** plutôt que vers le modèle local.

---

## Outils à installer

| Outil | Rôle | Statut |
|---|---|---|
| **Claude Code** | Outil principal de développement | Aucun changement |
| **Ollama** | Serveur d'inférence local, héberge Qwen 2.5 3B | À installer |
| **OpenCode** | Mode expérimental : découvrir, comparer à Claude Code (ne le remplace pas) | À installer (test) |
| **Aider** | Tester le workflow, comparer perfs, comprendre forces/limites | À installer (test) |

Objectif transverse : **maîtriser plusieurs outils** pour pouvoir conseiller les clients avec une expérience réelle.

---

## Plan de déploiement

1. Mesure des ressources actuelles.
2. Installation d'Ollama.
3. Installation de Qwen 2.5 3B.
4. Tests de performances.
5. Installation d'OpenCode.
6. Installation d'Aider.
7. Création d'une **API interne unique ("LLM Gateway")**.
8. Connexion progressive : Claude Code, n8n, Telegram, second cerveau.
9. Ajout du **routage intelligent** vers Claude lorsque : la tâche est complexe, le serveur est trop chargé, ou le modèle local atteint ses limites.

---

## Cahier des charges pour l'IA administrant le VPS

L'IA chargée de l'administration devra :

1. analyser précisément les ressources actuelles du VPS (CPU, RAM, swap, stockage, charge moyenne) ;
2. vérifier que l'installation d'Ollama n'affectera pas les services existants ;
3. installer Ollama comme **service systemd** ;
4. déployer Qwen 2.5 3B avec une configuration adaptée aux ressources disponibles ;
5. **limiter la consommation mémoire et CPU** du LLM afin de préserver les autres services ;
6. mesurer les performances (latence, débit de génération, utilisation CPU/RAM) avant et après installation ;
7. installer OpenCode en environnement de test, sans perturber Claude Code ;
8. installer Aider dans les mêmes conditions ;
9. proposer une architecture d'API interne permettant aux différents composants (Claude Code, n8n, Telegram, Obsidian, futurs agents) d'accéder au LLM via un **point d'entrée unique** ;
10. préparer un mécanisme de routage configurable vers l'API Anthropic lorsque les tâches dépassent les capacités du modèle local ou lorsque les ressources du VPS sont fortement sollicitées ;
11. documenter toutes les installations, configurations, scripts et procédures de restauration pour garantir la **reproductibilité** de l'environnement.

---

## Vision à moyen terme

Cette infrastructure n'est pas qu'un usage personnel : c'est une **base pour une offre de conseil en IA** (rattachement [[IACMOI]]), avec une architecture modulaire, reproductible, économiquement optimisée, indépendante d'un fournisseur unique, et facilement déployable chez des clients.

Le VPS devient un **laboratoire permanent** pour expérimenter les meilleures pratiques autour des assistants IA hybrides.

```

## 1 PROJECTS/Perso Nicolas/LLM local — Essai infra/5 — LLM Gateway.md

```markdown
---
type: note-projet
créé: 2026-07-10
statut: opérationnel
---

# 5 — LLM Gateway

## Résultat

API interne OpenAI-compatible déployée sur le VPS le 10/07/2026.
Tous les clients (Claude Code, n8n, Telegram, Obsidian) passent par un endpoint unique sans savoir quel modèle tourne derrière.

## Localisation

- **Code** : `/opt/llm-gateway/` (repo git local, commits incrémentaux)
- **Service** : `llm-gateway.service` (systemd, User=ubuntu, MemoryMax=512M)
- **Port** : `127.0.0.1:8095` (localhost uniquement — jamais exposé à l'extérieur)
- **Accès n8n** : relais `llm-gateway-n8n.socket` (systemd-socket-proxyd) sur `172.19.0.1:8095`, visible des seuls conteneurs du bridge n8n — URL côté workflows : `http://172.19.0.1:8095/v1/chat/completions` (⚠️ si le stack n8n est redéployé et le réseau recréé, adapter `BindToDevice` — voir doc `05-cablage-n8n.md`)
- **CLI host** : `~/bin/llm` (`llm "prompt"`, stdin en pipe, `-m local|cloud`, `-v` métadonnées) — utilisé par les sessions Claude Code (règle CLAUDE.md) et les scripts
- **Config** : `/opt/llm-gateway/config.yaml` (hot-reload)
- **Secrets** : `/etc/llm-gateway/env` (root:root, chmod 600)
- **Docs tests** : `/home/ubuntu/llm-lab/docs/04-llm-gateway.md`

## Règles de routage (config.yaml)

| Condition | Backend |
|---|---|
| `model: "local"` | Ollama qwen2.5:3b |
| `model: "cloud"` / `"claude"` / `"smart"` | Anthropic Claude |
| `model: "auto"` (défaut) | Règles ci-dessous |
| prompt estimé > 2000 tokens | cloud |
| load avg 1m > 4.0 | cloud |
| RAM disponible < 2048 Mo | cloud |
| sinon | local |

**Repli** : si une escalade *automatique* vise le cloud alors que la clé n'est pas configurée, le gateway retombe en local dégradé (réponse quand même servie, raison tracée). Le 503 n'est renvoyé que pour une demande cloud *explicite* (`model: cloud/claude/smart`). Validé en conditions réelles le 10/07 : la charge des agents parallèles a déclenché la règle `load > 4.0`.

## Tests validés (10/07/2026)

```
GET  /health        → {"status":"ok","backends":{"local":"up","cloud":"not_configured"}}
GET  /v1/models     → 3 modèles : auto, local, cloud
POST model=auto     → réponse Qwen réelle + gateway.backend=local (6,7s)
POST model=cloud    → HTTP 503 + message explicite (pas de clé)
systemctl is-active → active
ss -tlnp | 8095     → 127.0.0.1 uniquement ✅
git grep "sk-"      → vide ✅
```

## Architecture fichiers

```
app/
├── main.py      — FastAPI routes (/health, /v1/models, /v1/chat/completions)
├── router.py    — Décision backend (load, RAM, tokens, model)
├── backends.py  — Ollama OpenAI-compat + Anthropic Messages API (converti)
└── config.py    — Chargement hot-reload config.yaml
```

## Pour activer le cloud

```bash
sudo nano /etc/llm-gateway/env
# Décommenter : ANTHROPIC_API_KEY=sk-ant-...
sudo systemctl restart llm-gateway
```

## Liens

- [[3 — Plan maquette hybride]]
- [[4 — Note de synthèse et décisions]]
- [[Architecture hybride — modèle local par défaut, modèle puissant à la demande]]

```

## 1 PROJECTS/Perso Nicolas/LLM local — Essai infra/6 — Déploiement maquette.md

```markdown
---
type: note-projet
créé: 2026-07-10
statut: fait
---

# 6 — Déploiement maquette

> Exécution des étapes 1 à 7 du plan de [[4 — Note de synthèse et décisions]], le 10/07/2026, orchestrée en `/dispatch` (Fable pilote, sous-agents Haiku/Sonnet exécutent). Documentation technique complète et reproductible dans `/home/ubuntu/llm-lab/docs/` (hors vault).

## Résultat en une ligne

La maquette hybride est **opérationnelle** : Qwen 2.5 3B tourne en local derrière le [[5 — LLM Gateway]], pilotable par Aider et OpenCode, sans aucune dégradation des services existants.

## Ce qui tourne désormais sur le VPS

| Composant | Détail | Où |
|---|---|---|
| **Ollama** | Service systemd, MemoryMax 5G / CPUQuota 400 %, bindé 127.0.0.1:11434 | `ollama.service` + override |
| **Qwen 2.5 3B** | q4_K_M, ~1,9 Go sur disque, ~3,9 Go RAM chargé | `ollama` |
| **LLM Gateway** | API OpenAI-compatible, routage auto local/cloud | 127.0.0.1:8095, `/opt/llm-gateway` |
| **Aider** | v0.86.2, venv dédié `~/.venvs/aider` | branché sur Ollama |
| **OpenCode** | v1.17.18, provider Ollama configuré | `~/.opencode/bin` |

## Mesures clés

- **Baseline avant install** : 9 Go RAM disponibles sur 11,6 — feu vert (doc `01-baseline.md`)
- **Perfs Qwen 2.5 3B sur CPU** : ~8,7 tokens/s en génération longue, ~19 t/s sur prompt court modèle chargé. Utilisable pour du service interne (résumé, classification, extraction), pas pour de l'interactif exigeant.
- **Après install** : ~6,9 Go RAM disponibles modèle chargé, 0 swap, tous les services existants (n8n, bots Telegram, sites) intacts.

## Enseignements

1. **Le routage par charge fonctionne** : pendant les tests, la charge générée par les agents parallèles a fait basculer le gateway en escalade cloud — exactement le comportement cible.
2. **Un 3B hallucine des tool calls** : OpenCode + Qwen 3B échoue quand on demande au modèle de *lire un fichier* (tool calls mal formés) ; il faut passer le contenu dans le prompt. Confirme le rôle du 3B : tâches fermées, pas d'agentique.
3. **Aider est le plus robuste des deux sur petit modèle** : édition de fichier réussie du premier coup (docstrings) en ~15 s.
4. **La confiance d'un 3B n'est pas calibrée** (câblage pipeline mail, 10/07 soir) : Qwen route un mail SCI vers « Santé » avec 0,85 de confiance et une justification hallucinée. Un seuil de confiance ne protège pas — il faut un verrou déterministe dans le code (ici : le nom du dossier choisi doit apparaître littéralement dans le mail). Pattern générique : *le LLM propose, le code vérifie*.

## Reste à faire (étapes 8-9 du plan)

- [ ] Renseigner `ANTHROPIC_API_KEY` dans `/etc/llm-gateway/env` pour activer l'escalade cloud réelle (action manuelle Nicolas — jamais par agent)
- [x] **n8n câblé (10/07 soir)** : relais socket systemd sur `172.19.0.1:8095` (bridge Docker n8n), workflow de démo `LLM Gateway - Test` (webhook `/webhook/llm-gateway-test`) validé de bout en bout — Qwen répond en ~11 s via n8n. Doc `05-cablage-n8n.md`.
- [x] **Pipeline mail câblé (10/07 soir)** : l'étape de routage vers les dossiers vault passe d'abord par le Qwen local (`model: "local"`, 5 verrous dont mention littérale du dossier dans le mail), repli Gemini sinon ; vision et synthèse restent sur Gemini (multimodal / texte long). Testé unitaire + bout en bout via le cron. Doc `06-cablage-pipeline-mail.md`.
- [x] **Second cerveau câblé (10/07 soir)** : CLI `~/bin/llm` (stdin en pipe, `-m` pour forcer un backend) + règle « LLM local — offload des tâches fermées » dans CLAUDE.md — les sessions Claude Code délestent classification/extraction/tri sur le Qwen local. Recherche sémantique et veille volontairement non câblées (embeddings incompatibles / offload Gemini délibéré). Doc `07-cablage-second-cerveau.md`.
- [ ] Affiner les seuils de routage à l'usage (`/opt/llm-gateway/config.yaml`, hot-reload)
- [ ] Benchmark comparatif OpenCode vs Aider vs Claude Code sur une vraie tâche (matière pour [[IACMOI]] / le Laboratoire)

## Liens

- [[0 — Vue d'ensemble]] · [[4 — Note de synthèse et décisions]] · [[5 — LLM Gateway]]
- [[Architecture hybride — modèle local par défaut, modèle puissant à la demande]]

```

## 1 PROJECTS/Perso Nicolas/LLM local — Essai infra/7 — Benchmark harnesses.md

```markdown
---
type: projet
statut: exploration
créé: 2026-07-11
casquette: "[[Perso Nicolas]]"
---

# 7 — Benchmark harnesses × modèles locaux

Étape 8-9 du plan : comparer les **outils de pilotage de code** (Aider / OpenCode /
Claude Code) sur les **modèles locaux**, avec un juge objectif, pour fonder l'offre
[[IACMOI]] sur des mesures et non des impressions.

## Protocole

- **Labo** : `~/llm-lab/bench/` (isolé du vault et de `~/.claude`). Voir `README.md`.
- **Tâche** : refacto multi-fichiers + correction d'un bug. `orders.py` et `invoices.py`
  dupliquent un calcul de prix ; `invoices.py` applique la taxe avant la remise (bug).
  L'outil doit factoriser dans `pricing.py`, corriger le bug, garder les signatures.
- **Juge objectif** : pytest, 4 tests. Baseline = 2 verts / 2 rouges. Succès = 4 verts.
- **Isolation** : chaque run part d'une copie fraîche du fixture. Métriques auto (succès,
  temps, création `pricing.py`, refacto effective) → tableau Markdown + JSON.
- **Modèles locaux** (Ollama, CPU-only) : qwen2.5-coder 3b & 7b, deepseek-coder 6.7b,
  mistral 7b, llama3.1 8b. Claude Code (Sonnet) = témoin cloud non lancé (fenêtre Pro).

## Résultat (2026-07-11)

| Harness | Modèle | Succès | Temps |
|---|---|:---:|---:|
| **aider** | **qwen2.5-coder:3b** | ✅ | **172 s** |
| aider | qwen2.5-coder:7b | ✅ | 290 s |
| aider | llama3.1:8b | ✅ | 288 s |
| aider | mistral:7b | ✅ | 348 s |
| aider | deepseek-coder:6.7b | ⏱ crash Ollama | — |
| opencode | *(les 5 modèles)* | ❌ | planifie sans éditer |

**Score : Aider 4/5, OpenCode 0/5.** Analyse détaillée : `~/llm-lab/bench/results/ANALYSE_2026-07-11.md`.

## Enseignements

1. **Aider >> OpenCode pour les modèles locaux ≤8B.** Aider fait recracher les fichiers
   entiers au modèle (« whole edit format ») et applique le diff → marche dès le 3B.
   OpenCode repose sur une boucle agentique de tool calls : les petits modèles émettent
   le plan (`todo`) puis n'éditent jamais. Confirme et étend le retex « le 3B n'enchaîne
   pas les tool calls » — vrai jusqu'à 8B ici.
2. **Sweet spot local = Aider + qwen2.5-coder:3b.** Seul combo qui réussit ET tient sous
   le garde-fou mémoire de prod (5 Go). Le plus rapide. Un 7B n'apporte aucun succès de plus.
3. **Sous cap 5 Go, seuls les ≤3B chargent.** Les 7-8B (~6-6,5 Go) exigent MemoryMax ~7,5 Go,
   ne laissant que ~0,8 Go de marge et aucun swap → ponctuel uniquement. Cap monté puis
   restauré à 5 Go pour ce benchmark.
4. **deepseek-coder:6.7b inutilisable ici** : crashe Ollama sous Aider, erreur immédiate
   sous OpenCode. À écarter (ou retester après MAJ Ollama).

## Reco opérationnelle

- **Édition de code assistée en local sur ce VPS = Aider + qwen2.5-coder:3b.** Fiable,
  rapide, compatible avec le garde-fou mémoire.
- **OpenCode = à réserver aux modèles cloud capables** (phase 2) : sa valeur agentique
  suppose un modèle qui enchaîne les tool calls, hors de portée des locaux ≤8B.

## Limites

Une seule tâche, un essai par cellule ; CPU-only (temps non transposables à un GPU) ;
juge binaire (ne note pas la qualité du style de code).

## Prochaines étapes

- Passe **témoin Claude Code / Sonnet** pour situer le plafond qualité.
- **Phase 2 cloud** (OpenRouter → DeepSeek-V3, GLM-4.6, Kimi, Codestral, Gemini) : là où
  OpenCode devrait enfin briller.
- Élargir le jeu de tâches et moyenner sur plusieurs runs pour robustifier les mesures.

## Liens

- [[0 — Vue d'ensemble]]
- [[6 — Déploiement maquette]]
- [[5 — LLM Gateway]]
- [[IACMOI]]

```

