# Cadrage du projet - beer/me × heavy-ops

## Contexte

**Service réel analysé (Brief #1) :** beer/me - SaaS d'accompagnement du brasseur artisanal dans sa journée. Projet en phase de planification, pas encore codé.

**Repo miroir :** heavy-ops
**Catégorie :** Dashboard / outil métier / SaaS interne.

**Posture :** beer/me n'est pas encore développé, c'est un projet en phase de concéption. Heavy-ops sert de terrain dde jeu pour pouvoir appliquer ensuite les bonnes pratiques pour beer/me.

---

## Correspondance anti-patterns heavy-ops → beer/me

| Anti-pattern heavy-ops | Équivalent beer/me |
|---|---|
| Polling toutes les 5s sur 4 endpoints | Dashboard brasserie qui refreshe les données de fermentation en continu |
| Chargement intégral des records sans pagination | Historique complet de tous les brassins chargé d'un coup |
| Calculs teamSummary / urgentRecords côté client | Calculs de rendement, densité, IBU recalculés à chaque render |
| Table sans pagination réelle | Liste des recettes/brassins sans infinite scroll ni pagination serveur |

---

## Bonnes pratiques RGESN identifiées au Brief #1

Toutes issues du RGESN, marquées pertinentes pour beer/me :

- Optimiser le parcours utilisateur (Usage)
- Mettre en cache les données calculées souvent utilisées (Usage)
- Préférer une PWA à une appli native (Manufacturing)
- Éviter d'utiliser des images matricielles pour l'interface (Usage)
- Valider le code avec un linter (Manufacturing)
- Utiliser le chargement paresseux (Manufacturing)
- Économiser la bande passante grâce aux Service Workers (Manufacturing)
- Choisir un hébergeur éco-responsable (Production)
- Avoir une stratégie de fin de vie des contenus (Fin de vie)
- Grouper les déclarations CSS similaires (Manufacturing)

---

## Ordre d'attaque des livrables

1. **Analyse EcoIndex** : lancer heavy-ops, mesurer avec l'extension (+ Lighthouse, CPU/mémoire, appels réseau), capturer les scores — baseline page d'accueil puis écran secondaire
2. **Backlog** : rédiger les 3 US minimum à partir des résultats d'audit et de ce qui est transférable à beer/me
3. **Cartographie de cadrage** : découle des US et des mesures
4. **Plan d'action 6 mois** : slides qui synthétisent les 3 précédents
5. **Lien Git** : commits au fil de l'eau
