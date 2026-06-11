---
exported: 2026-06-11 00:00
topic: Projet B2G — Stockage & Expéditions Urmatt (SIAT)
source: second cerveau Nicolas MAUVILAIN
---

# Contexte : Projet B2G — Stockage & Expéditions Urmatt

## Résumé

Le Projet B2G est un diagnostic + plan de transformation des flux de stockage et d'expédition sur le site de la scierie SIAT Urmatt (CA ~83 M€). Missionné à B2G Consulting (avril 2026), il établit un score AS-IS de 1,9/4 et identifie 4 dysfonctionnements majeurs : camions immobilisés 1h30-3h (OTIF ~50%), 64% des emplacements bloqués par différenciation précoce, absence de KPIs, gestion empirique des stocks. Un plan en 3 phases est proposé, avec pour objectifs OTIF ≥ 85% et temps de chargement ≤ 45 min. Un risque réglementaire DREAL critique (pollution ×10 les normes) est intégré au TO-BE.

---

## Document principal — Projet B2G (B2G Consulting, Avril 2026)

**Site :** Scierie SIAT Urmatt — 22 000 m² parc extérieur + 240 emplacements couverts  
**Score AS-IS :** 1,9 / 4  

### 4 dysfonctionnements AS-IS

1. **Immobilisation des camions** — OTIF ~50%, camions bloqués 1h30-3h. Cause : préparation et chargement non séparés.
2. **Différenciation précoce** — 64% des emplacements bloqués par colis déjà affectés client dès la réception. Flexibilité quasi nulle.
3. **Absence de KPIs** — Aucun indicateur suivi. Segmentation ABC-XYZ vieille de 4 ans.
4. **Gestion empirique des stocks** — Règles basées sur l'expérience individuelle. Surstock / ruptures coexistent.

### Risque critique DREAL

- Rejets d'eaux pluviales à **10× les normes autorisées** (propiconazole, perméthrine)
- Cause : colis traités stockés en extérieur sans protection
- Risque : mise en demeure, amende, arrêt d'exploitation
- Alternative : abri couvert >1 M€ CAPEX — ou intégrée dans le TO-BE (zéro colis traités extérieur)

### Vision TO-BE — 4 principes

1. **Séparer Préparation et Chargement** — Flux J-1 : préparation à J-1, chargement à J (≤45 min). OTIF cible : 85%.
2. **Différenciation Retardée (Postponement)** — Stock neutre, affectation client uniquement au moment de la préparation J-1. Libère 64% des emplacements.
3. **Abriter les colis traités** — Zéro extérieur pour les produits traités. Résout le risque DREAL sans 1 M€ CAPEX.
4. **Rapatrier l'étiquetage sous le hall** — Potences fixes sous couvert vs postes mobiles en extérieur. Meilleures conditions de travail + productivité.

### Plan d'actions — 3 phases

| Phase | Durée | Actions | Coût |
|---|---|---|---|
| **Phase 1 — Pilote** | M1-M2 | Bon de préparation, pilote 1 commande/jour, mesure OTIF | ~0 € |
| **Phase 2 — Généralisation** | M2-M4 | Extension toutes commandes, MAJ ABC-XYZ, outil TETRIS | ~5 000 € |
| **Phase 3 — Digitalisation** | M4-M9 | TMS (15-40 k€), WMS (à chiffrer, dès M9+) | 15-40 k€+ |

### KPIs cibles

| Indicateur | AS-IS | TO-BE |
|---|---|---|
| OTIF départ | ~50% | ≥ 85% |
| Temps chargement | 1h30–3h | ≤ 45 min |
| Emplacements bloqués | 64% | < 20% |
| Conformité DREAL | Non conforme (×10) | Conforme |

### Décision Go/No-Go

- **Phase 1 : GO** — coût quasi nul, impact rapide mesurable
- **Statu quo : NO-GO** — DREAL non conforme, OTIF 50%
- **Phases 2-3 : Conditionnel** — dépend résultats Phase 1 + arbitrage CAPEX abri

### Décisions attendues (Sponsor = Nicolas, Directeur SC)

1. Valider lancement Phase 1 (bon de préparation, pilote)
2. Décider traitement DREAL (abri >1 M€ ou solution organisationnelle)
3. Commander MAJ ABC-XYZ
4. Arbitrer budget TMS/WMS Phase 3

---

## Analyse ABC-XYZ — Données chiffrées (base 2024+2025)

**Total références actives : 9 357** | CA 2025 : 81,98 M€ | Volume : 192 836 colis

| Classe | Refs | % refs | Colis 2025 | % colis | CA 2025 | % CA |
|---|---|---|---|---|---|---|
| **A-X** | 300 | 3,2% | 145 826 | 75,6% | 63,2 M€ | **77%** |
| A-Y | 33 | 0,4% | 4 574 | 2,4% | 2,0 M€ | 2,5% |
| B-X | 97 | 1,0% | 7 472 | 3,9% | 2,8 M€ | 3,4% |
| B-Y | 631 | 6,7% | 19 854 | 10,3% | 8,5 M€ | 10,4% |
| C-Z | 7 815 | 83,5% | 8 984 | 4,7% | 3,6 M€ | 4,4% |

**Lectures clés :**
- 398 refs X (toutes classes) = **79,5% du volume + 80,4% du CA** → candidats prioritaires à la différenciation retardée
- 300 refs A-X = 3,2% du catalogue = 77% du CA — cœur de la transformation
- 7 815 refs C-Z = 83,5% du catalogue = 4,4% du CA → gestion masse extérieure justifiée
- B-Y (631 refs, 10,4% CA) : segment intermédiaire — stratégie zone tampon, réappro déclenché par commande
- A-Y (33 refs, 2 M€) : vigilance rupture/surstock si traité comme A-X (fréquence irrégulière)

**Surface logistique minimale calculée : 11 943 m²** (A-X : 10 861 m² / B-X : 1 075 m²)

---

## Projet connexe — Restructuration Expédition (Christophe PORTE, Mai 2026)

Projet parallèle élaboré par Christophe PORTE (chef d'équipe expédition, en rupture conventionnelle) avant son départ. Complémentaire au B2G — focalisé sur l'organisation interne et le WMS.

**Points clés :**
- **Timeline cible :** J-30 (bois secs) → J-10 (commandes standards) → J-2 (stocks figés + lancement vagues 13h30) → J-1 18h (commandes prêtes en rack) → J (chargement)
- **Stockage dynamique :** pas d'emplacements fixes, affectation auto via SSCC, logique FIFO
- **Capacité cible :** 56 camions/j (haute) — 1 152 colis/j (vs 50 camions basse)
- **Effectifs :** 33 personnes → 33 personnes (neutre, changements structurels internes)
- **Matériel :** passage 100% chariots combinés 3 roues plateau 120 (12 unités, CAPEX à chiffrer)
- **WMS requis :** réception SSCC, gestion vagues, affectation auto picking, alertes contrat-date (3 mois)

**Points ouverts :**
- À valider avec Thibault/Nicolas (ordo) et Thomas Meyer (commerce)
- WMS : appel d'offres à lancer (candidats : Infolog, SAP...)
- Christophe part → qui reprend le pilotage ?

---

## Questions ouvertes / Points à approfondir

- Résultats Phase 1 (pilote bon de préparation) — mesure OTIF actuelle vs cible ?
- Décision DREAL : abri couvert (>1 M€) ou solution organisationnelle (TO-BE B2G) ?
- Arbitrage TMS : quels candidats, quel budget retenu pour Phase 3 ?
- WMS : appel d'offres lancé ? Budget arbitré ?
- Continuité projet Christophe post-départ — qui pilote ?
- Avancement réel Phase 1 à date (juin 2026) : bon de préparation déployé ? KPIs mesurés ?
- Interaction entre les deux projets (B2G Consulting vs Christophe) : quelle priorisation ?

---

## Métadonnées

- Exporté depuis : second cerveau Obsidian (privé)
- Généré par : Claude Code
- Pour : échange avec Claude Chat
- Fichiers source : `Projet_B2G.pdf` (75 pages, B2G Consulting, Avril 2026) + `Calcul_des_classes_ABC_et_XYZ...v2.csv`
