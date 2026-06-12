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

