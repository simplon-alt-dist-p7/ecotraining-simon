# Backlog heavy-ops

## US001

>**En tant que** utilisateur de l'outil interne, 
**je veux** que les données se rafraîchissent de manière optimisée, 
**afin de** réduire la consommation réseau inutile générée par un polling trop fréquent.

- **Objectif**:
 Réduire d'au moins 80% le nombre de requêtes automatiques (actuellement 4 endpoints toutes les 5 secondes)
- **BP éco-conception**: 
Limiter les requêtes réseau superflues — éviter le polling excessif ou non justifié
- **KPI associe**: 
Nombre de requêtes réseau par minute, volume de données transférées sur 10 minutes
- **Repo ou ecran concerné**: 
Tous les composants utilisant `setInterval(loadAll, 5000)` (dashboard, records, analytics)
- **Critere de reussite**: 
Le polling est supprimé ou son intervalle porté à ≥ 30 secondes — vérifiable via l'onglet Network des devtools
- **Niveau de priorite**: haute

## US002

> **En tant que** utilisateur du dashboard, 
**je veux** consulter les enregistrements par pages, 
**afin de** ne pas charger l'intégralité des fichiers à chaque requête.

- **Objectif**: 
Réduire la taille des réponses API d'au moins 90% pour un affichage initial
- **BP éco-conception**: 
Paginer les données côté serveur — ne transférer que ce qui est affiché
- **KPI associe**: 
Poids de la réponse sur `/api/records`, temps de chargement initial de la page
- **Repo ou ecran concerne**: 
Endpoint `api/records`, fichiers `data/records.json` et `data/analytics.json`, composant table volumineuse
- **Critere de reussite**: 
La réponse API est limitée à 20 enregistrements par page — les fichiers JSON complets ne sont plus chargés intégralement
- **Niveau de priorite**: haute

## US003

> **En tant que** développeur, 
**je veux** que les données dérivées `teamSummary` et `urgentRecords` soient calculées côté serveur, 
**afin de** alléger le travail du navigateur client à chaque render.

- **Objectif**: 
Supprimer les recalculs côté client sur chaque cycle de rendu React
- **BP éco-conception**: 
Déporter les traitements métier sur le serveur — le client affiche, il ne calcule pas
- **KPI associe**: 
Temps de rendu des composants (profiler React), charge CPU mesurée en devtools sur le dashboard
- **Repo ou ecran concerne**: 
Composants `teamSummary` et `urgentRecords`, endpoint API correspondant
- **Critere de reussite**: 
Les données dérivées arrivent pré-calculées dans la réponse API — aucun recalcul résiduel visible dans le profiler
- **Niveau de priorite**: moyenne

## US004

> **En tant que** développeur, 
**je veux** supprimer les fichiers SVG non référencés du dépôt, 
**afin de** réduire le poids des assets potentiellement servis.
- **Objectif**: 
Supprimer l'ensemble des fichiers `assets/ops-panel-*.svg` non utilisés
- **BP éco-conception**: 
Ne pas exposer de ressources statiques inutilisées — alléger le dépôt et les assets servis
- **KPI associé**: 
Poids du dossier `assets/`, nombre de fichiers SVG présents après nettoyage
- **Repo ou ecran concern**e**: Dossier `assets/`, fichiers `ops-panel-*.svg`
- **Critere de reussite**: 
Aucun fichier SVG non référencé ne subsiste dans le dépôt — vérifiable par grep sur les imports
- **Niveau de priorité**: 
basse

## US005

> **En tant que** développeur, 
**je veux** disposer d'un linter configuré sur le frontend et le backend, 
**afin de** garantir un code propre et homogène sur l'ensemble des évolutions à venir.

- **Objectif**: 
Mettre en place une configuration ESLint (+ règles TypeScript/React) avant toute évolution de code
- **BP éco-conception**: 
Valider le code avec un linter — repérer code mort, imports inutiles, mauvaises pratiques en amont
- **KPI associé**: 
Nombre d'erreurs/avertissements détectés par le linter, nombre de fichiers couverts
- **Repo ou ecran concerné**: 
`frontend/` et `backend/` (configuration racine ou par workspace)
- **Critere de reussite**: 
Une commande `lint` s'exécute sans erreur bloquante sur les deux workspaces et est lancée en CI ou avant commit
- **Niveau de priorite**: haute

## US006

> **En tant que** développeur, 
**je veux** que les déclarations CSS de `ops.css` soient regroupées et dédupliquées, 
**afin de** réduire le poids de la feuille de style et faciliter sa maintenance.

- **Objectif**: 
Réduire le poids de `ops.css` en supprimant les règles dupliquées ou redondantes identifiées après les évolutions de composants (US003)
- **BP éco-conception**: 
Grouper les déclarations CSS similaires — éviter la répétition de règles proches
- **KPI associé**: 
Poids du fichier `ops.css` (Ko), nombre de règles CSS avant/après
- **Repo ou ecran concerné**: 
`frontend/src/ops.css`
- **Critere de reussite**: 
Le poids de `ops.css` diminue d'au moins 20% sans régression visuelle constatée
- **Niveau de priorite**: basse

## US007

> **En tant que** utilisateur du dashboard, 
**je veux** que les écrans ou composants non visibles immédiatement soient chargés à la demande, 
**afin de** réduire le poids transféré au premier affichage.

- **Objectif**: 
Mettre en place le chargement paresseux (`lazy`/`Suspense`) sur les routes ou composants lourds non visibles au premier rendu
- **BP éco-conception**: 
Utiliser le chargement paresseux — ne transférer que ce qui est nécessaire à l'instant donné
- **KPI associé**: 
Poids du bundle JS initial, temps avant premier affichage (FCP)
- **Repo ou ecran concerné**: 
`frontend/src/App.tsx` et composants de routes
- **Critere de reussite**: 
Le bundle JS initial diminue d'au moins 15% sans régression fonctionnelle
- **Niveau de priorite**: moyenne

## US008

> **En tant que** utilisateur de l'outil interne, 
**je veux** que les ressources statiques déjà téléchargées soient réutilisées entre deux visites, 
**afin de** réduire la bande passante consommée à chaque rechargement.

- **Objectif**: 
Mettre en place un Service Worker pour mettre en cache les assets statiques (JS, CSS, SVG)
- **BP éco-conception**: 
Économiser la bande passante grâce aux Service Workers
- **KPI associé**: 
Volume de données transférées sur une deuxième visite (cache hit ratio)
- **Repo ou ecran concerné**: 
`frontend/` (configuration Vite + Service Worker)
- **Critere de reussite**: 
Les assets statiques sont servis depuis le cache sur une deuxième visite — vérifiable via l'onglet Network des devtools
- **Niveau de priorite**: basse

## US009

> **En tant que** administrateur de l'outil interne, 
**je veux** qu'une stratégie de purge ou d'archivage des anciens enregistrements soit définie, 
**afin de** éviter l'accumulation indéfinie de données inutiles dans `records.json` et `analytics.json`.

- **Objectif**: 
Définir et documenter une règle de rétention/archivage des données anciennes
- **BP éco-conception**: 
Avoir une stratégie de fin de vie des contenus
- **KPI associé**: 
Volume de données stocké (Ko/Mo) dans `data/records.json` et `data/analytics.json`
- **Repo ou ecran concerné**: 
`data/records.json`, `data/analytics.json`
- **Critere de reussite**: 
Une règle de rétention est documentée et appliquée (purge, archivage ou alerte de volume)
- **Niveau de priorite**: basse

