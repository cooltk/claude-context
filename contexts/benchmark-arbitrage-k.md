---
exported: 2026-07-13
topic: Arbitrage méthodo benchmark LLM — K=5 run complet vs K=3 sur le juge d'abord
source: second cerveau Nicolas MAUVILAIN (brief sanitisé, aucune donnée confidentielle)
---

# Contexte à discuter

Je (Nicolas) construis un **laboratoire de benchmarks** pour qualifier des modèles de langage avant de les autoriser dans mon infrastructure (une "gateway" qui route les requêtes vers le bon modèle selon la tâche). Un assistant IA vient de me faire une recommandation ; je veux la challenger avec un regard extérieur.

**Ce brief est volontairement abstrait** : le contenu métier détaillé du benchmark est de la propriété intellectuelle privée et n'est pas exporté. Seule la question de méthode l'est.

## L'état des lieux (juste avant la recommandation)

- J'ai un **benchmark métier privé** ("B3") que je viens de figer en version 1.0 : 33 scénarios répartis en 11 familles de compétences (structuration d'information non structurée, classification, détection d'ambiguïtés, détection de conflits d'agenda, déduplication, priorisation, mémoire/mise à jour/suppression d'objets…). Chaque scénario a un corrigé de référence.
- **Deux passes de notation** :
  - **`schema`** — vérifiable automatiquement par du code (conformité JSON, types, dates ISO, énumérations). Déterministe.
  - **`rubric`** — 4 axes subjectifs (compréhension, classification, décision, prudence) notés par un **modèle-juge** (un grand modèle, "Opus"), qui compare la sortie testée au corrigé.
- **Doctrine** : je sépare *compréhension* (le modèle a-t-il compris ?) de *comportement* (a-t-il pris la bonne décision : signaler une info manquante plutôt que l'inventer, ne pas trancher un arbitrage humain, mettre à jour au lieu de dupliquer…). Principe transverse : « le LLM propose, le code vérifie ».
- **Robustesse** : ma charte prévoit **K répétitions** d'un même scénario pour mesurer la stabilité (K=5 sur les scénarios critiques). Un modèle stable sur 5/5 est "robuste", 4/5 "sous conditions", <4/5 "instable".

## Les résultats réels obtenus jusqu'ici (modèle testé = un petit modèle local, ~3 milliards de paramètres, tournant sur CPU)

- Passe **`schema`** (K=1) : **0,879** — le JSON produit est majoritairement conforme. Échecs surtout sur les entrées longues (le modèle renvoie du vide) et quelques fautes d'énumération/type.
- Passe **`rubric`** (juge, K=1) : **compréhension 0,432 ≫ comportement 0,187**. Autrement dit : « produit du JSON propre mais range mal et décide mal ». Conclusion provisoire : ce petit modèle est **inapte à un rôle d'agent métier autonome**.

## Le hic méthodologique repéré

Le modèle-juge est appelé via un outil en ligne de commande qui **ne permet pas de fixer la température à 0**. Résultat : le juge n'est **pas déterministe** — j'observe une **variance de ±0,02** sur les scores rubric d'un run à l'autre, pour exactement les mêmes entrées. Les notes bougent un peu à chaque exécution.

## La recommandation qu'on m'a faite (à challenger)

Deux directions proposées pour passer à la "phase mesures" :

**Option A — Rejouer le benchmark complet en K=5 tout de suite** (sur les scénarios critiques), pour obtenir un premier profil *stable* du modèle plutôt qu'un instantané K=1. Argument : c'est la mesure la plus rentable, prévue par la charte, elle donne une robustesse chiffrée.

**Option B — D'abord trancher la question du juge non déterministe** : passer le juge lui-même en K=3 + moyenne, pour que les scores rubric soient reproductibles *avant* de lancer des mesures à grande échelle. Argument : mesurer avec un instrument qui bouge de ±0,02, c'est bâtir sur du sable.

## Ce sur quoi je veux ton avis

1. Dans quel ordre feriez-vous A et B, et **pourquoi** ? (Est-ce qu'attaquer un gros run K=5 avant d'avoir stabilisé le juge a du sens, ou est-ce l'inverse ?)
2. **±0,02 de variance sur le juge, est-ce réellement bloquant** vu que les écarts que je cherche à mesurer (0,432 vs 0,187) sont d'un ordre de grandeur supérieur ? Ou est-ce que je sur-optimise un détail ?
3. Y a-t-il une **troisième voie** plus maligne que je ne vois pas (ex. quantifier d'abord la variance du juge par un mini-run avant de décider, séparer les K des deux passes, etc.) ?
4. Sur le fond de la démarche : séparer *compréhension* et *comportement*, faire noter le subjectif par un modèle-juge tiers, exiger une double passe code+juge — **est-ce une méthodo saine** pour qualifier des modèles, ou y a-t-il des angles morts classiques (biais du juge, corrélation des axes, contournement du corrigé) que je devrais anticiper ?

Réponds en priorisant le **pragmatisme** : mon objectif est d'obtenir vite des résultats exploitables, pas de bâtir un protocole académique parfait. Je me méfie de la sur-conception.
