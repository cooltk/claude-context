---
exported: 2026-07-10 22:12
topic: Projet B2G (stockage expéditions Urmatt) et projet restructuration Christophe
source: second cerveau Nicolas MAUVILAIN
files: 6
---

# Contexte : Projet B2G et projet restructuration Christophe

Export intégral de toutes les notes du périmètre (contenu brut, non résumé).

## Sommaire

- 1 PROJECTS/SIAT - Projet B2G Urmatt/Projet B2G - Stockage Expéditions Urmatt.md
- 1 PROJECTS/SIAT - Projet B2G Urmatt/Analyse ABC-XYZ — Données chiffrées.md
- 1 PROJECTS/Professionel Nicolas/Expédition/Projet B2G.md
- 1 PROJECTS/Professionel Nicolas/Expédition/Projet restructuration Christophe.md
- 1 PROJECTS/Professionel Nicolas/Expédition/Collaborateurs/Christophe.md
- 1 PROJECTS/Professionel Nicolas/Expédition/Suivi opérationnel.md

---

## 1 PROJECTS/SIAT - Projet B2G Urmatt/Projet B2G - Stockage Expéditions Urmatt.md

```markdown
# Projet B2G — Stockage & Expéditions Urmatt

> Type : `Projet`
> Source : `Projet_B2G.pdf`
> Auteur : `B2G Consulting`
> Date : `Avril 2026`
> Site : `Urmatt`
> Sponsor : `Directeur Supply Chain`
> Tags : #siat #urmatt #projet #b2g #stockage #expéditions #lean

---

## Contexte & Périmètre

**Site :** Scierie SIAT Urmatt — 22 000 m² de parc extérieur + 240 emplacements couverts
**CA site :** ~83 M€
**Périmètre étudié :** Flux de stockage, préparation et expédition des produits finis

**Score AS-IS :** **1,9 / 4**

---

## Diagnostic AS-IS — 4 dysfonctionnements majeurs

### 1. Immobilisation des camions (OTIF ~50%)
- Camions immobilisés **1h30 à 3h** au chargement
- OTIF actuel : ~50% — départ à l'heure non garanti
- Cause : absence de séparation entre phases de préparation et de chargement

### 2. Différenciation précoce des colis
- **64% des emplacements bloqués** par des colis déjà étiquetés pour un client
- Aucune gestion neutre du stock — chaque colis est affecté dès la réception
- Résultat : flexibilité quasi nulle, réaffectation manuelle coûteuse

### 3. Absence de KPIs et pilotage obsolète
- Aucun indicateur de performance opérationnel suivi
- Segmentation ABC-XYZ datant de **4 ans** — ne reflète plus la réalité commerciale
- Décisions de stock prises sans données fiables

### 4. Gestion empirique des stocks
- Niveaux de stock basés sur l'expérience individuelle, non sur des règles formalisées
- Surstock sur certaines références, ruptures sur d'autres
- Aucun outil de pilotage des réapprovisionnements

---

## Risque critique — Conformité DREAL

> **Priorité absolue — indépendamment du projet**

- **Constat :** rejets d'eaux pluviales à des concentrations **10× supérieures aux normes** (propiconazole, perméthrine lessivage)
- **Cause :** colis traités stockés en extérieur sans protection contre les intempéries
- **Risque :** mise en demeure DREAL, amende, arrêt d'exploitation
- **Alternative couverte :** construction d'un abri → **CAPEX > 1 M€**
- **Solution intégrée au TO-BE :** zéro colis traités en extérieur (principe 3 du TO-BE)

---

## Vision TO-BE — 3 principes directeurs

### Principe 1 : Séparer Préparation et Chargement
| Phase | Timing | Action |
|---|---|---|
| J-7 | Confirmation commande | Réservation emplacement |
| J-1 matin | Préparation active | Identification physique des colis |
| J-1 journée | Consolidation | Regroupement zone départ |
| J-1 17h | Clôture préparation | Quai prêt, bon vérifié |
| J | Chargement seul | ≤ **45 min** — conducteur attend |

**Cible OTIF :** 50% → **85%**

### Principe 2 : Différenciation Retardée (Postponement)
- Stock neutre par référence — pas d'affectation client anticipée
- Affectation uniquement au moment de la préparation J-1
- Libère **64% des emplacements** bloqués → capacité retrouvée sans investissement

### Principe 3 : Abriter les colis traités
- **Zéro colis traités en extérieur**
- Zone couverte dédiée aux produits issus du traitement (autoclave, classe 2, 3)
- Suppression du risque DREAL par organisation, avant tout investissement structurel

### Principe 4 : Rapatrier l'étiquetage sous le hall de préparation
- **Situation actuelle :** équipes d'étiquetage en extérieur — exposées à la pluie, au vent et au froid
- **Cible :** postes fixes sous le hall, avec **potences** pour tenir les machines d'étiquetage (vs postes mobiles actuels)
- **Bénéfices :**
  - Amélioration significative des conditions de travail
  - Meilleure productivité (moins d'aléas météo, ergonomie améliorée)
  - Cohérence avec le flux J-1 (étiquetage intégré à la préparation couverte)

---

## Validation ABC-XYZ

**Base totale :** 9 357 références

| Segment | Refs | Colis | CA | % CA |
|---|---|---|---|---|
| **A-X** | 300 | 145 826 | 63,2 M€ | **77%** |
| C-Z | 7 815 | — | 3,6 M€ | — |

**Résultat clé :** 4,3% des références (398 refs X-class) = **79,5% du volume** et **80,4% du CA**

→ Différenciation retardée applicable immédiatement sur ce noyau dur.

---

## Analyse capacitaire

| Poste | Capacité nominale | Pic saisonnier (mars-avril) |
|---|---|---|
| Étiquetage | 45 000 étiquettes/j (3 postes) | 57 000/j → **saturation** |
| Traitement | 738 colis/j (3 équipes) | 870 colis/j → **saturation** |
| Chariots | — | Congestion quai identifiée |

**Actions capacitaires :** lissage de charge via planification avancée (J-7), pas d'investissement matériel en phase 1.

---

## Plan d'actions — 3 phases

### Phase 1 — M1 à M2 : Pilote
- Mise en place du **bon de préparation** (formalisation du flux J-1)
- Pilote sur **1 commande/jour**
- Mesure OTIF, temps chargement, incidents
- Coût : quasi nul — process et organisation uniquement

### Phase 2 — M2 à M4 : Généralisation
- Extension à toutes les commandes
- Mise à jour de la **segmentation ABC-XYZ**
- Déploiement outil **TETRIS** (optimisation placement parc) — ~5 000 €
- KPIs opérationnels formalisés et suivis hebdo

### Phase 3 — M4 à M9 : Digitalisation
- **TMS** (Transport Management System) — setup 15 000 à 40 000 €
- Planification transport intégrée, prise de RDV automatisée
- **WMS** (Warehouse Management System) — déploiement à partir de M9+
- Traçabilité colis, gestion emplacement, inventaire temps réel

---

## Comparatif outils

| Outil | Fonction | Coût estimé | Phase |
|---|---|---|---|
| Bon de préparation | Formalisation flux | 0 € | 1 |
| TETRIS | Optimisation parc | ~5 000 € | 2 |
| TMS | Planification transport | 15 000 – 40 000 € | 3 |
| WMS | Gestion entrepôt | À chiffrer | 3+ |

---

## KPIs avant / après

| Indicateur | AS-IS | TO-BE cible |
|---|---|---|
| OTIF départ | ~50% | **≥ 85%** |
| Temps chargement | 1h30 – 3h | **≤ 45 min** |
| Emplacements bloqués | 64% | **< 20%** |
| Conformité DREAL | Non conforme (×10) | **Conforme** |
| Fraîcheur ABC-XYZ | 4 ans | **Actualisé annuellement** |

---

## Décision Go / No-Go

| Option | Verdict | Raison |
|---|---|---|
| **Phase 1** | **GO** | Coût quasi nul, impact rapide mesurable, risque faible |
| **Statu quo** | **NO-GO** | DREAL non conforme, OTIF 50%, perte de valeur cachée |
| Phase 2-3 | Conditionnel | Dépend des résultats Phase 1 et arbitrage CAPEX abri |

---

## Décisions attendues (Sponsor = Directeur SC)

1. **Valider le lancement Phase 1** (bon de préparation, pilote commande/jour)
2. **Décider du traitement DREAL** : abri couvert (>1 M€ CAPEX) ou autre solution
3. **Commander la mise à jour ABC-XYZ** (Phase 2 — données actualisées)
4. **Arbitrer le budget TMS/WMS** (Phase 3 — enveloppe 15-40 k€ minimum)

---

## Fichier source

`Projet_B2G.pdf` (75 pages, B2G Consulting, Avril 2026)

---

## Liens connexes

- [[VSM générale Urmatt]]
- [[VSM Lead Time Charpente Traditionnelle]]
- [[VSM Lead Time Couverture]]
- [[Groupe SIAT]]
- [[Différenciation retardée]]
- [[ABC-XYZ]]
- [[OTIF]]

```

## 1 PROJECTS/SIAT - Projet B2G Urmatt/Analyse ABC-XYZ — Données chiffrées.md

```markdown
# Analyse ABC-XYZ Urmatt — Données chiffrées

> Type : `Ressource projet`
> Source : `Calcul_des_classes_ABC_et_XYZ_et_surfaces_Log_base_2024et02025_v2.csv`
> Auteur : `Nicolas MAUVILAIN`
> Base : `2024 + 2025`
> Date stock : `06/01/2026`
> Tags : #siat #urmatt #abc-xyz #stockage #b2g #analyse

---

## Totaux généraux

| Indicateur | 2024 | 2025 |
|---|---|---|
| Nb colis expédiés | 192 214 | 192 836 |
| Nb BL | 109 563 | 110 134 |
| Volume m3 | 311 994 | 311 478 |
| CA EXW | 79 211 671 € | **81 977 951 €** |

**Total références actives : 9 357**

---

## Distribution ABC-XYZ complète

| Classe | Nb refs | % refs | Colis 2025 | % colis | CA 2025 (€) | % CA | Surface log (m²) |
|---|---|---|---|---|---|---|---|
| **A-X** | **300** | **3,2%** | **145 826** | **75,6%** | **63 167 470** | **77,1%** | **10 861** |
| A-Y | 33 | 0,4% | 4 574 | 2,4% | 2 046 111 | 2,5% | — |
| A-Z | 0 | — | — | — | — | — | — |
| **B-X** | **97** | **1,0%** | **7 472** | **3,9%** | **2 765 699** | **3,4%** | **1 075** |
| B-Y | 631 | 6,7% | 19 854 | 10,3% | 8 524 427 | 10,4% | — |
| B-Z | 101 | 1,1% | 1 594 | 0,8% | 969 828 | 1,2% | — |
| C-X | 1 | 0,01% | 37 | 0,02% | 3 168 | 0,004% | 7 |
| C-Y | 379 | 4,1% | 4 495 | 2,3% | 934 184 | 1,1% | — |
| **C-Z** | **7 815** | **83,5%** | **8 984** | **4,7%** | **3 567 128** | **4,4%** | — |
| **TOTAL** | **9 357** | **100%** | **192 836** | **100%** | **81 977 015 €** | **100%** | **11 943** |

---

## Synthèse des classes X (fréquence élevée)

| Regroupement | Refs | % refs | Colis | % colis | CA | % CA |
|---|---|---|---|---|---|---|
| Toutes classes X (A+B+C) | **398** | **4,3%** | 153 335 | **79,5%** | 65 936 337 | **80,4%** |
| A-X seul | 300 | 3,2% | 145 826 | 75,6% | 63 167 470 | 77,1% |

→ **Cohérent avec les chiffres B2G** : 398 refs X = 79,5% du volume et 80,4% du CA

---

## Surface logistique minimale

**Surface Log mini calculée : 11 943 m²**

Répartition par classe :
- A-X : **10 861 m²** (91% de la surface totale)
- B-X : **1 075 m²** (9%)
- C-X : **7 m²** (négligeable)

> La surface log est calculée uniquement pour les articles X (flux régulier nécessitant un emplacement permanent). Les articles Y et Z n'ont pas de surface calculée — ils passent en zone tampon ou stock de masse.

---

## Lectures clés pour le projet B2G

### Concentration extrême sur A-X
300 références (3,2% du catalogue) génèrent **77% du CA** et **76% des colis expédiés**. Ce sont les candidats prioritaires à la différenciation retardée et au stock neutre.

### La queue longue C-Z
7 815 références (83,5% du catalogue) ne représentent que **4,4% du CA** et **4,7% des colis**. Leur gestion en stock de masse extérieur est justifiée — aucune surface de rack dédiée.

### B-Y : masse invisible mais significative
631 références B-Y représentent **10,4% du CA** (8,5 M€) et **10,3% des colis**. Ce segment mérite une stratégie de stock adaptée (zone tampon, réappro déclenché par commande).

### A-Y : vigilance
33 références A-Y = 2 M€ de CA mais fréquence irrégulière (classe Y). Risque de rupture ou de surstock si traitement identique aux A-X.

---

## Structure des références (colonnes du fichier)

| Colonne | Contenu |
|---|---|
| Cocatener | Code référence complet (dimension + essence + qualité + traitement + longueur + nb colis/paquet) |
| Dim Int | Dimension section (ex: 63/175) |
| ART | Type article (CHE2, BAS2, MAD2, LIT2, SOL2, POU2, VOL2...) |
| Long. com | Longueur commerciale en mètres |
| Essence | SE (sapin/épicéa), PIN, DOU (douglas), EPI |
| Qualité | C2, C3, NORD, C1 |
| Trait TT | Traitement (2J, 0, 4V, 3V, 2T, HT...) |
| Classe compil | Résultat ABC-XYZ (A-X, B-Y, C-Z...) |
| Stk colis 06/01/26 | Stock physique en colis au 06/01/2026 |
| Point de cde colis | Seuil de déclenchement réappro (colis) |
| fréquence sortie/jour | Nb de sorties par jour ouvré |
| Surface au sol stk | Surface de stockage physique (m²) |
| Surface au sol log | Surface logistique (avec allées et manœuvre) en m² |
| Nb colis par BL | Taille moyenne d'un bon de livraison |

---

## Fichier source

`Calcul_des_classes_ABC_et_XYZ_et_surfaces_Log_base_2024et02025_v2.csv`
Base de données : 9 357 lignes + 3 lignes d'en-tête

---

## Liens connexes

- [[Projet B2G - Stockage Expéditions Urmatt]]
- [[ABC-XYZ]]
- [[VSM générale Urmatt]]
- [[Groupe SIAT]]

```

## 1 PROJECTS/Professionel Nicolas/Expédition/Projet B2G.md

```markdown
---
type: projet
domaine: expédition
tags: [B2G, expédition, flux, SIAT]
---

# Projet B2G — Évolution des flux

## Contexte

Projet d'évolution des flux expédition B2G (Business to Group / client grand compte). Suivi dans le cadre de l'animation opérationnelle du service.

## État (Mai 2026)

- Projet en cours de suivi
- Évolution des flux identifiée comme chantier prioritaire
- Reprise progressive des expéditions en cours

## Liens

- [[2026-W22]]

```

## 1 PROJECTS/Professionel Nicolas/Expédition/Projet restructuration Christophe.md

```markdown
---
type: projet
domaine: expédition
tags: [expédition, restructuration, WMS, chariots-combinés, Christophe, stockage-dynamique]
date: 2026-05-26
statut: à évaluer
auteur-projet: Christophe P.
---

# Projet Restructuration Expédition — Christophe P.

## Contexte

Projet élaboré par [[Christophe]] (chef d'équipe expédition) avant son départ de l'entreprise. Objectif : améliorer productivité et satisfaction client via une nouvelle organisation d'entreposage, de préparation de commandes et de chargement.

**Sources :** 5 documents (Process_projet.pdf 16p, Besoin_personnel_et_matériel.pdf, Potentiel_chargement.pdf, Résumé_process_après_production.pdf, Schéma_process_commerce-transport-ordo.pdf)

---

## Timeline complète du process cible

| Jalon | Acteur | Action |
|---|---|---|
| **J-30** | Commerce | Carnet commandes bois séchés transmis |
| **J-30** | Ordo | Identification besoins bois sec → transfert stock + fabrication → séchoir |
| **J-10** | Commerce | Carnet commandes standards transmis |
| **J-9** | Ordo | Analyse comparative stocks vs besoins → programmation production |
| **J-5** | Transport | Préparation des affrètements |
| **J-2** | Ordo | **Stocks constitués et figés** → lancement vagues de préparation à 13h30 |
| **J-2** | Transport | Planning de chargement figé (sauf surplus stocks) |
| **J-1 18h** | Expédition | Toutes commandes prêtes en rack |
| **J** | Expédition | Chargement camion |

**Objectif déclaré :** "Stock constitués et justes, pas de manquants"

---

## Flux après production (schéma simplifié)

### Phase 1 — Production → Picking

```
Production SRM   → Cerclage → Picking SRM
Production Urmatt → RAB 1 → Cerclage → Picking Urmatt ← Navette (Nieder)
Production Nieder → Cerclage → Navette →
```

### Phase 2 — Vagues J-2 → Chargement camion

| Type colis | Chemin |
|---|---|
| Standard C1-C2 | Picking → Traitement → Navette → Rack préparation → Camion |
| GSB / KGF | Picking → Étiquetage → Rack préparation → Camion |
| 2J (≥7m) / NT | Picking → Traitement → Zone Hall mur → Rack préparation → Camion |
| NT C3 | Picking → Box commandes C3 → Camion |

---

## Stockage dynamique — Principe clé

**Pas d'emplacements fixes.** Les pickings sont définis par longueur + hauteur max.

- Affectation automatique à la sortie production via scan **SSCC**
- Classement : section × colisage × C18/C24 × semaine de fabrication
- Logique FIFO sur dates de fabrication
- Matériel : chariot combiné 3 roues plateau 120

**Exemple :** Emplacement 6m, hauteur 5m → standard : 6 colis / GSB : 8 colis / fermette : 4 colis

---

## Système d'alertes contrat date

| Alerte | Seuil | Action |
|---|---|---|
| Jaune | 1 mois en stock | Surveillance |
| Orange | 2 mois en stock | Action commerciale |
| Rouge | 3 mois en stock | Déstockage urgent |

Contrat date = 3 mois à partir de la fabrication (augmenté si passage autoclave ou séchoir).

---

## Préparation de commandes

- **48 emplacements C1-C2** par jour (racks A→X, 5 niveaux = 120 emplacements)
- **8 box C3** (non traités choix 3, préparation J-1)
- **24 racks spéciaux** niveau 5 (bois sec, non traité, incolore)
- Vagues lancées à **J-2 à 13h30** — par section et longueur
- Rotation des niveaux de racks par jour (L : niveaux 1+4 / Ma : 2+3 / Me : 1+4…)

---

## Capacité journalière

| Période | Latéraux | Frontal | Total | Colis/jour |
|---|---|---|---|---|
| Haute | 48 camions | 8 camions | **56** | **1 152** |
| Basse | 42 camions | 8 camions | **50** | **1 008** |

(Base : 24 colis/camion, 3 caristes latéraux + 1 frontal)

---

## Effectifs — 0 net (33 personnes avant et après)

| Équipe | Actuel | Futur | Delta |
|---|---|---|---|
| Équipe 1 | 10 | 10 | 0 |
| Équipe 2 | 10 | 10 | 0 |
| Nuit | 2 | 3 | **+1** |
| Journée | 10 | 9 | **-1** |
| Heiligenberg | 1 | 1 | 0 |

Changements structurels : suppression poste U→U journée, création poste "cariste combiné préparation" nuit, fusion des rôles chargement/préparation.

---

## Matériel — Passage aux chariots combinés

**Court terme :** conserver Baumann + Caces 3 existants.
**À terme :** 100 % chariots combinés 3 roues plateau 120.

### Investissement initial : 12 combinés

| Poste | Qté |
|---|---|
| Costa / Stenner | 1 |
| Sortie HW | 2 |
| Holtec | 1 |
| Ruban | 1 |
| Expéditions | 5 |
| Secours panne | 2 |
| **Total** | **12** |

Revente possible des Caces 3 remplacés.

---

## Cahier des charges WMS

Modules requis :
- Réception SSCC / QR code
- Gestion commandes + vagues de préparation
- Assignation colis → commandes via SSCC
- États des stocks + alertes contrat date
- Structure site (allées, emplacements par H/L/l)
- **Affectation automatique picking au plus près du lieu de sortie production**
- Module chargement des commandes

Candidats cités : Infolog, SAP, etc.

---

## Autres investissements

- **Picking :** panneaux d'emplacement sur socles modulables (préférence Christophe vs poteaux fixes)
- **BBS :** finir la dalle béton
- **Hall mur :** récupérer l'espace libéré par suppression des zones de retournement (inutiles avec combinés)

---

## Points d'interface Commerce / Transport / Ordo

| Contrainte | Règle cible |
|---|---|
| Commandes standards | Transmises J-10 avant chargement |
| Gel des modifications | J-5 à J-2 : changement section possible sur surplus uniquement |
| Commandes bois sec | 1 mois avant (délai séchage + bâchage) |
| Affrètement transport | J-5 (inchangé) |
| Planning de chargement figé | J-2 |

---

## Points de vigilance / Évaluation à faire

- [ ] Ce projet a été conçu par Christophe sans visibilité complète sur les contraintes Commerce et Ordo — à valider avec Thibault et Nicolas (ordo) et Thomas Meyer (commerce)
- [ ] WMS : budget à estimer, appel d'offres à lancer si décision positive
- [ ] 12 chariots combinés : CAPEX à chiffrer (démonstration plateau 120 déjà réalisée)
- [ ] Droits Scienergie (N-U et granulés) : Christophe signale qu'il n'a pas les accès pour créer la nouvelle zone — à débloquer
- [ ] BBS : finir la dalle béton (prérequis)
- [ ] Christophe part → identifier qui reprend le pilotage du projet

---

## Liens

- [[Christophe]]
- [[Suivi opérationnel]]
- [[Projet B2G]]
- [[2026-W22]]

```

## 1 PROJECTS/Professionel Nicolas/Expédition/Collaborateurs/Christophe.md

```markdown
---
type: collaborateur
role: Chef d'équipe Expédition
service: Expédition
manager: "[[Ibrahima]]"
tags: [RH, expédition, SIAT]
---

# Christophe

## Profil

Chef d'équipe Expédition. Auteur du projet de restructuration. Quitte l'entreprise prochainement (départ acté).

*(Détails RH retirés de cet export public.)*

## Projet de restructuration

Christophe a produit un dossier complet de restructuration expédition avant son départ :
→ [[Projet restructuration Christophe]]

Points clés : stockage dynamique SSCC, vagues de préparation J-2, passage aux chariots combinés (12 unités), WMS (Infolog/SAP), 0 net RH sur 33 personnes, capacité 56 camions/jour en période haute.

## Historique

| Date | Événement |
|---|---|
| 2026-05 | Remise du projet de restructuration expédition |

```

## 1 PROJECTS/Professionel Nicolas/Expédition/Suivi opérationnel.md

```markdown
---
type: operationnel
domaine: expédition
tags: [expédition, TOP, performance, SIAT]
---

# Suivi opérationnel — Expéditions

## Indicateurs suivis

- Suivi des expéditions (volumes, délais)
- Temps de chargement
- Performance opérationnelle globale

## Rituels

- **TOP quotidiens** : animés par Nicolas — points d'équipe sur l'activité du jour

## Situation (Mai 2026)

- Reprise progressive des expéditions en cours
- Rythme de sortie encore lent par rapport à la production
- Baisse progressive des stocks observée en semaine 22

## Liens

- [[Projet B2G]]
- [[2026-W22]]

```

