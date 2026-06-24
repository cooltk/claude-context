---
exported: 2026-06-24 12:05
topic: Projet Second cerveau Florian Astor
source: second cerveau Nicolas MAUVILAIN
---

# Contexte : Projet Second cerveau Florian Astor

## Résumé

Nicolas MAUVILAIN accompagne Florian Astor, entrepreneur en rénovation générale (SASU Aster Renov, Saverne, créée février 2023), dans la création d'un second cerveau IA. L'objectif principal est d'automatiser la chaîne post-rendez-vous client → devis envoyé. Le stack technique envisagé est similaire au second cerveau de Nicolas (Claude Code + bot Telegram + vault Obsidian), déployé en Docker isolé sur un VPS OVHcloud. Le pipeline cible : Granola génère un CR de réunion → envoyé par email → fusionné avec un message vocal de Florian depuis la voiture → LLM génère un email de devis → Florian valide → envoi au client.

---

## Notes et documents clés

### Intention du projet

Créer un second cerveau IA pour Florian Astor, entrepreneur en création dans la rénovation générale (plâtrerie, électricité, plomberie…).

**Objectif principal :** automatiser la chaîne post-rendez-vous client → devis envoyé, en minimisant le travail administratif de Florian.

- **Client :** Florian Astor
- **Statut :** Création d'entreprise (2026)
- **Secteur :** Rénovation générale (plâtrerie, électricité, plomberie, second œuvre)

---

### Vue d'ensemble — Brief technique complet

**Contexte client :**
- Société : ASTER RENOV (SASU)
- SIRET : 948 979 729 00019
- Adresse : 18 rue des Clés, 67700 Saverne
- Code APE : 43.29A — Travaux d'isolation / rénovation
- Date de création : 13 février 2023
- Brief initial : 23 juin 2026

> ⚠️ Le dirigeant enregistré au RCS est Nader BOUGOFFA — à vérifier avec Florian (associé ? ancienne immatriculation ?)

**Pipeline cible — flux de travail**

```
RDV client
  │
  ├─ [Granola] Compte rendu réunion auto-généré
  │     └─ Envoyé par email sur la boîte du second cerveau
  │
  ├─ [Voiture, chemin du retour] Message vocal Florian
  │     └─ Directives techniques pour le devis (matériaux, contraintes, surface…)
  │
  └─ [IA, à l'arrivée] Synthèse automatique
        ├─ Granola + directives vocales fusionnés
        ├─ Génération d'un email de proposition / devis
        └─ Florian relit → clique Envoyer → email part chez le client
```

**Composants à construire**

| Composant | Rôle | Technologie envisagée |
|---|---|---|
| Ingestion email Granola | Recevoir le CR de réunion | Webhook n8n / poll inbox |
| Transcription vocale | Voix dans la voiture → texte | Whisper (Telegram ou app dédiée) |
| Fusion & génération devis | Combiner CR + directives → email pro | LLM (Claude / Gemini) |
| Envoi email client | Validation Florian → envoi | n8n + compte email Florian |

**Décisions prises :**
- Email second cerveau : `florian.ai@outlook.fr` — compte dédié Outlook (même modèle que `nicolas.ai@outlook.fr`) pour recevoir les CR Granola et envoyer les devis clients (décision 23/06/2026)

---

### Infrastructure déployée (23/06/2026)

Nicolas a déjà monté la stack technique de base en Docker isolé sur le VPS :

- Déployé sur `/opt/florian-brain/` (VPS OVHcloud)
- Clone de la stack `ccpa-telegram` (Claude Code + bot Telegram texte/vocal Whisper, vault Obsidian dédié)
- Image `florian-brain:latest` (node20 + Claude Code + ccpa-telegram + ffmpeg)
- Canal de supervision : Nicolas peut injecter des instructions via `florian-supervise.sh`
- Auth : OAuth Pro de Florian (pas de partage du compte Nicolas)
- Interface : bot Telegram + vault Obsidian neuf (PARA amorcé)

**En attente côté Florian :**
- Remplir les credentials (token BotFather + user id Telegram)
- Login OAuth Claude
- Tester le bot

---

### Questions ouvertes documentées dans le vault

1. Granola exporte-t-il par email nativement ? Format PDF ou texte ?
2. Interface pour la relecture du devis : email brouillon (Outlook) ou Telegram ?
3. Template de devis souhaité par Florian ? (format, structure tarifaire)
4. Florian a-t-il déjà un compte Telegram / WhatsApp pour les vocaux en voiture ?
5. Vérifier la discordance RCS : dirigeant enregistré = Nader BOUGOFFA (pas Florian Astor)

---

### Question ouverte critique : Facturation électronique obligatoire (sept. 2026)

Florian a posé la question lors d'un échange récent : **la facturation électronique devient obligatoire pour les TPE à partir de septembre 2026**. Il se demande si le second cerveau peut intégrer une infrastructure pour envoyer les factures aux clients de manière conforme à cette réglementation.

**Contexte réglementaire :**
- Loi de finances 2024 : obligation de facturation électronique pour toutes les entreprises françaises
- Calendrier TPE/PME : septembre 2026 (obligation de réception) + obligation d'émission ultérieure
- Le système cible en France = Portail Public de Facturation (PPF) ou Plateformes de Dématérialisation Partenaires (PDP) agréées par la DGFIP
- Format imposé : Factur-X (PDF/A avec XML intégré) ou UBL / CII

**Question posée par Florian :** Peut-on monter nous-mêmes via le second cerveau une infra pour envoyer la facture au client et être conforme ?

**Ce qui n'a pas encore été analysé :**
- Faisabilité d'une intégration PPF / PDP dans le pipeline n8n
- Solutions du marché (Chorus Pro pour B2G, Pennylane, Sage, QuickBooks, etc.)
- Effort de développement vs solution SaaS existante
- Peut-on générer du Factur-X via un script Python et l'envoyer via une PDP partenaire ?

---

## Prochaines étapes identifiées

- [ ] Session de cadrage avec Florian : répondre aux 5 questions ouvertes
- [ ] Analyser la question facturation électronique : PPF / PDP vs SaaS — recommander une option
- [ ] Prototype pipeline : email Granola → transcription vocale → brouillon devis
- [ ] Formaliser un template de devis avec Florian
- [ ] Itération 2 : base de données chantiers, historique client

---

## Métadonnées

- Exporté depuis : second cerveau Obsidian de Nicolas MAUVILAIN (privé)
- Généré par : Claude Code (claude-sonnet-4-6)
- Date : 24 juin 2026
- Pour : discussion avec ChatGPT / Claude Chat
