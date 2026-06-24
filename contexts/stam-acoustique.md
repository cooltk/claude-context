---
exported: 2026-06-24 12:20
topic: Projet Stam Acoustic — démonstration IA pour PMI
source: second cerveau Nicolas MAUVILAIN
---

# Contexte : Projet Stam Acoustic

## Résumé

Nicolas MAUVILAIN accompagne **Stam Acoustic** (PMI alsacienne ~50 personnes, dirigée par Pierre-Alain Mendler, Schiltigheim), dans le cadre d'un projet de démonstration IA. L'entrée du projet vient via **Delta Studio** (société du groupe, dirigée par Emmanuelle Diringer, épouse de Nicolas). Stam a perdu un appel d'offres dont le retour client était *"description technique trop imparfaite et pas assez précise"*. L'objectif est de montrer comment l'IA aurait permis d'éviter cette perte — une étincelle pédagogique, pas un déploiement. Un audit complet des 5 mémoires techniques a été réalisé, des mémoires enrichis avant/après ont été produits, et une présentation de restitution est prête. La prochaine étape est la réunion de restitution avec Pierre-Alain (~30–45 min).

---

## Notes et documents clés

### 0 — Vue d'ensemble

**Stam Acoustic** fait partie d'un collectif de 4 sociétés (STAM Acoustic, JEHU, Modul'Est, Delta Studio) — 6,5 M€ CA 2025.

**Angle d'attaque :** démonstration IA sur la prescription technique — à partir des documents réels de l'affaire perdue (Wasselonne), montrer comment l'IA aurait permis :
1. de détecter les faiblesses du mémoire avant envoi
2. de proposer des variantes de prescription plus précises
3. de gagner 15 à 25 points sur la note technique

**Livrables déjà produits :**
- Audit fusionné Claude + ChatGPT → Diagnostic mémoires
- Stratégie restitution + roadmap IA 3 niveaux
- Template note de conviction + prompts IA + test devis
- Slide lexique IA (8 slides, 6 concepts)
- 4 mémoires enrichis avant/après (Wasselonne, Wantzenau, Socomec PCO, Crédit Mutuel Equity) via pipeline `~/stam-ia/generate_dossier.py`
- Présentation diagnostic 13 slides (Reveal.js)

**Reste à faire :**
- Confirmer disponibilité Pierre-Alain (réunion 30–45 min)
- Récupérer logo Stam Acoustic
- Explorer exports Batygest (API ? CSV ?)

---

### Diagnostic des mémoires — résumé (note complète : 10 Ko)

**Score global estimé : ≈ 70–75 / 100**

| Critère | Note |
|---|---|
| Crédibilité technique | 8,5/10 |
| Respect du dossier | 9/10 |
| Moyens humains | 8/10 |
| Développement durable | 8/10 |
| Différenciation | **5/10** |
| Qualité rédactionnelle | **6/10** |
| Mise en forme | **4/10** |
| Personnalisation | **5/10** |

**Synthèse en une phrase :** vous avez la matière d'un acteur 8,5/10 sur la technique, mais vous la présentez comme un 6/10 sur la forme. Une refonte du template peut faire gagner 10 à 15 points sans rien changer au savoir-faire chantier.

**8 faiblesses identifiées :**
1. Auto-sabotage explicite — phrase PAQ Wasselonne ("nous n'avons pas de plan d'assurance qualité formalisé…") — potentiellement éliminatoire sur un critère noté 10/50
2. Copier-coller massif entre dossiers (mêmes blocs mot pour mot sur 3 docs)
3. Aucune personnalisation au contexte chantier (Golf, Crédit Mutuel, Socomec = même méthodologie)
4. Aucune alternative technique proposée (jamais de variante, toujours "on respecte les prescriptions")
5. Phrases génériques qui ne différencient pas ("Nous participerons aux réunions", "Nous respecterons les DTU")
6. Coquilles et incohérences décrédibilisantes (32 ans vs 33 ans vs 21 ans, "Lles cartons", FIPS/SFEC pour JEHU)
7. Manque chronique de chiffres (pas de KPI qualité, pas de planning détaillé, pas de bilan carbone global)
8. Mise en forme datée (gros pavés texte, pas de schéma, pas de photo, pas de timeline)

**Atouts différenciants sous-exploités :**
- Collectif STAM (4 sociétés / 6,5 M€ / multi-corps)
- 100 % équipes internes, zéro sous-traitance
- Déchetterie propre 10 flux + partenariat ReStart Tarkett (1 500 kg recyclés)
- Qualifications Qualibat (6612 plafonds + 4132 plâtrerie)
- Implantation locale (Schiltigheim)

---

### Audit ChatGPT des 5 documents (verbatim, reçu 24/06/2026)

Voici le texte exact de l'audit ChatGPT transmis par Nicolas :

**Impression globale :** vos mémoires sont rédigés comme des documents de conformité, alors que les maîtres d'œuvre attendent des documents de conviction. ✅ Vous rassurez sur la capacité à faire. ❌ Vous ne donnez pas envie de vous choisir.

**Ce qui fonctionne :** crédibilité immédiate (30+ ans, équipes internes, pas de sous-traitance), très concrets sur les moyens (effectifs, qualifications, véhicules, déchetterie 10 flux), cases du dossier réellement traitées.

**5 problèmes majeurs :**

1. **Même histoire partout** — Le MOE se dit : *"Ils ont un mémoire standard qu'ils adaptent un peu."* Golf Wantzenau (restaurant haut de gamme) / Crédit Mutuel (tertiaire occupé) / SOCOMEC (usine industrielle) : or les méthodologies restent presque identiques. Consacrer 30 % du mémoire aux spécificités DE CE chantier.

2. **Ce que tout le monde fait** — Au lieu de "Nous participerons aux réunions", écrire : *"Un point hebdomadaire interne sera organisé avant chaque réunion de chantier afin d'identifier les risques d'interface et proposer immédiatement des mesures correctives au MOE."* Là, vous montrez une méthode.

3. **Phrases dangereuses** — Dans le mémoire Wasselonne : *"Nous n'avons pas de plan d'assurance qualité formalisé à vous remettre, les procédures de travail et de contrôle sont orales."* → Un MOE peut enlever 2 à 5 points immédiatement. À remplacer par une phrase positive sur le contrôle qualité interne.

4. **Mise en page datée** — Documents Word 2010. Pas de pictogrammes, pas de schémas, pas de photos, pas de timeline. Un organigramme, un planning visuel, un schéma flux déchets = 1 page visuelle = énorme gain de lisibilité.

5. **Points forts sous-vendus** — Bandeau en page 1 : ✓ 33 ans d'expérience ✓ 100 % équipes internes ✓ Aucune sous-traitance ✓ 6,5 M€ CA collectif ✓ Déchetterie 10 flux ✓ ReStart Tarkett ✓ Qualibat ✓ Implantation locale. *"Le MOE lit en diagonale. En 30 secondes il doit comprendre : 'Eux sont solides.'"*

---

### Stratégie de restitution — Pierre-Alain Mendler

**Objectif de la séance :** amorcer une phase exploratoire IA, pas un déploiement immédiat.

**Déroulé (30–45 min) :**
| Temps | Bloc | Objectif |
|---|---|---|
| 0–5 min | Slide lexique IA | Vocabulaire commun : LLM, Token, Prompt + ChatGPT / Claude / Gemini |
| 5–25 min | Restitution du diagnostic | Critique constructive des 5 mémoires |
| 25–30 min | Démo DIY | Leur montrer qu'ils auraient pu obtenir cet audit seuls en 5 min |
| 30–40 min | Roadmap IA 3 niveaux | Où Stam peut aller, à quelle vitesse, avec quels coûts |
| 40–45 min | Décision prochain pas | Quel niveau veulent-ils tester en premier ? |

**Roadmap IA — 3 niveaux :**
- Niveau 1 : Usage individuel ChatGPT/Claude (résumés, corrections, mise en forme) — aucune infra
- Niveau 2 : Claude Projects par client/chantier — génération de mémoires avec templates définis
- Niveau 3 : Agents IA (email entrant → devis auto, second cerveau entreprise, multi-agents)

**Contexte Batygest :** colonne vertébrale du process Stam (affaires, devis, factures) mais ne génère pas de documents. Intégration Niveau 3 via exports CSV ou API REST — à investiguer.

**Repères coût IA :**
- Mémoire technique complet (Claude Pro) : 0 € (inclus dans 20 €/mois)
- Mémoire technique complet (API Sonnet) : 0,02 à 0,05 €
- 100 devis/an (Claude Pro) : 240 €/an d'abonnement

---

## Questions ouvertes / Points à approfondir

1. **Disponibilité Pierre-Alain** — créneau de restitution 30–45 min non encore fixé
2. **Logo Stam Acoustic** — manquant pour personnaliser les supports
3. **Batygest exports** — API REST disponible ? Format CSV ? Point d'entrée pour le Niveau 3
4. **Grilles MOE réelles** — les répartitions de points dans les mémoires enrichis sont illustratives ; les RC réels des AO permettraient de recaler les pondérations
5. **Niveau de maturité IA souhaité** — Pierre-Alain est-il prêt pour le Niveau 2 (Claude Projects) ou faut-il démarrer au Niveau 1 ?
6. **Chiffres internes manquants** — KPI qualité réels (% levée de réserves 1er passage, % respect planning), heures d'insertion réelles 2024
7. **FIPS/SFEC pour JEHU** — vérifier avec Pierre-Alain si JEHU est réellement adhérent FIPS ou si c'est un copier-coller depuis un mémoire STAM
8. **PAQ écrit** — Stam n'a pas de plan d'assurance qualité formalisé ; faut-il accompagner sa création ? Bloquant pour beaucoup d'AO publics

---

## Métadonnées

- Exporté depuis : second cerveau Obsidian de Nicolas MAUVILAIN (privé)
- Généré par : Claude Code (claude-sonnet-4-6)
- Date : 24 juin 2026
- Pour : discussion avec ChatGPT / Claude Chat
