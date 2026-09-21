# Cahier des besoins métier

## 1. Projet

Système décisionnel BI pour le pilotage d’une marketplace e-commerce

## 2. Contexte métier

L’entreprise exploite une marketplace e-commerce contenant des données
sur les clients, les commandes, les produits, les vendeurs, les paiements,
les livraisons et les avis clients.

Ces données sont réparties dans plusieurs sources transactionnelles et
ne permettent pas directement aux responsables d’obtenir une vision
globale et fiable des performances de l’entreprise.

L’objectif du projet est de transformer ces données opérationnelles en
informations intégrées, structurées et adaptées à l’aide à la décision.

## 3. Utilisateurs cibles

### Direction générale

La direction générale souhaite disposer d’une vision globale de :

- chiffre d’affaires ;
- volume de commandes ;
- activité des clients ;
- performances géographiques ;
- performances logistiques ;
- satisfaction client.

### Responsable commercial

Le responsable commercial souhaite analyser :

- l’évolution du chiffre d’affaires ;
- les performances des catégories de produits ;
- les performances des vendeurs ;
- l’activité des clients ;
- les performances commerciales par zone géographique.

### Responsable logistique

Le responsable logistique souhaite analyser :

- les délais de livraison ;
- les retards de livraison ;
- le taux de commandes en retard ;
- les zones géographiques problématiques ;
- la relation entre les retards de livraison et la satisfaction client.

## 4. Objectifs métier

Le système décisionnel devra permettre aux utilisateurs de :

- suivre les performances commerciales ;
- analyser l’évolution du chiffre d’affaires ;
- identifier les catégories de produits les plus performantes ;
- comparer les performances selon les zones géographiques ;
- analyser les performances des vendeurs ;
- suivre les performances logistiques ;
- identifier les retards de livraison ;
- analyser la satisfaction des clients ;
- étudier la relation entre les retards de livraison et la satisfaction client.

## 5. Questions décisionnelles

1. Comment évolue le chiffre d’affaires au cours du temps ?
2. Quelles catégories de produits génèrent le plus de chiffre d’affaires ?
3. Quelles régions génèrent le plus de ventes ?
4. Quels vendeurs génèrent le plus de chiffre d’affaires ?
5. Quel est le panier moyen des commandes ?
6. Quel est le délai moyen de livraison ?
7. Quel pourcentage des commandes est livré en retard ?
8. Quelles régions connaissent le plus de retards de livraison ?
9. Quelle est la note moyenne donnée par les clients ?
10. Les commandes livrées en retard reçoivent-elles de moins bonnes notes ?

## 6. Principaux indicateurs de performance (KPI)

- Chiffre d’affaires total
- Nombre total de commandes
- Nombre total de clients
- Panier moyen
- Croissance du chiffre d’affaires
- Délai moyen de livraison
- Retard moyen
- Taux de livraison en retard
- Note moyenne des clients

## 7. Architecture décisionnelle

Le projet suit l’architecture suivante :

1. Sources de données opérationnelles et externes.
2. Extraction, transformation et chargement des données (ETL).
3. Data Warehouse PostgreSQL.
4. Datamarts orientés métier.
5. Modèles analytiques.
6. Requêtage, exploration multidimensionnelle, tableaux de bord,
   reporting et analyses métier.

Le Data Warehouse et les datamarts sont distincts des modèles analytiques.

Le Data Warehouse stocke les données détaillées organisées sous forme
de faits et de dimensions.

Les modèles analytiques définissent notamment les dimensions,
les hiérarchies, les niveaux, les mesures et les règles d’agrégation.

Une zone de staging PostgreSQL pourra être utilisée comme zone technique
interne au processus ETL, mais elle ne sera pas considérée comme une
couche décisionnelle principale.

## 8. Sources de données

Les principales sources utilisées seront :

- `olist_customers_dataset.csv`
- `olist_orders_dataset.csv`
- `olist_order_items_dataset.csv`
- `olist_products_dataset.csv`
- `olist_sellers_dataset.csv`
- `olist_order_reviews_dataset.csv`
- `olist_order_payments_dataset.csv`
- `product_category_name_translation.csv`

## 9. Besoins du Data Warehouse

Le Data Warehouse devra :

- intégrer les données provenant des différentes sources ;
- stocker des données nettoyées et cohérentes ;
- conserver les données détaillées ;
- utiliser une modélisation dimensionnelle ;
- contenir des tables de faits et des tables de dimensions ;
- permettre des analyses historiques et multidimensionnelles ;
- garantir l’intégrité référentielle ;
- utiliser des clés substituts lorsque cela est nécessaire.

### Dimensions prévues

- Date
- Client
- Produit
- Vendeur
- Géographie

### Tables de faits prévues

- Ventes
- Commandes

Plusieurs tables de faits pourront partager des dimensions communes,
permettant de construire une constellation.

## 10. Besoins des datamarts

### Datamart Sales

Le Datamart Sales devra permettre l’analyse selon :

- la date ;
- le produit ;
- la catégorie ;
- le client ;
- le vendeur ;
- l’État ;
- la ville.

Principales mesures :

- chiffre d’affaires ;
- nombre de commandes ;
- quantité vendue ;
- panier moyen.

### Datamart Logistics

Le Datamart Logistics devra permettre l’analyse selon :

- la date ;
- le client ;
- l’État ;
- la ville ;
- la catégorie de produit ;
- le statut de livraison.

Principales mesures :

- délai de livraison ;
- durée du retard ;
- nombre de commandes en retard ;
- taux de livraison en retard ;
- note client.

Les datamarts seront dérivés du Data Warehouse à l’aide de vues
matérialisées PostgreSQL.

## 11. Besoins des modèles analytiques

### Modèle analytique Sales

Hiérarchies prévues :

- Date : Année > Trimestre > Mois > Jour
- Produit : Catégorie > Produit
- Géographie : État > Ville

Mesures principales :

- Chiffre d’affaires total
- Nombre total de commandes
- Nombre total d’articles vendus
- Panier moyen
- Croissance du chiffre d’affaires

### Modèle analytique Logistics

Hiérarchies prévues :

- Date : Année > Trimestre > Mois > Jour
- Géographie : État > Ville
- Produit : Catégorie > Produit

Mesures principales :

- Délai moyen de livraison
- Retard moyen
- Taux de livraison en retard
- Note moyenne des clients
- Nombre total de commandes livrées

Power BI sera utilisé pour implémenter les modèles sémantiques contenant
les relations, les hiérarchies et les mesures DAX.

Ces modèles joueront le rôle de couche analytique multidimensionnelle,
mais ne seront pas présentés comme des cubes MOLAP classiques.

## 12. Besoins de restitution

La solution devra permettre :

- des requêtes analytiques SQL ;
- la navigation drill-down et roll-up ;
- l’utilisation de filtres ;
- l’exploration multidimensionnelle ;
- la création de tableaux de bord Power BI ;
- la visualisation des données ;
- la production d’un rapport structuré ;
- la formulation d’analyses et de recommandations métier.

## 13. Besoins de qualité des données

Le processus ETL devra contrôler :

- les valeurs manquantes ;
- les doublons ;
- les dates invalides ;
- les valeurs numériques invalides ;
- les commandes sans correspondance ;
- les clients inconnus ;
- les produits inconnus ;
- les vendeurs inconnus ;
- les montants négatifs ;
- le nombre de lignes entre les sources et les données chargées.

Un rapport de qualité des données sera généré après le traitement.

## 14. Périmètre du projet

La première version du projet se concentre sur :

- l’intégration des données ;
- la qualité des données ;
- la modélisation dimensionnelle ;
- l’implémentation du Data Warehouse ;
- les datamarts ;
- les modèles analytiques ;
- les analyses SQL ;
- Power BI ;
- DAX ;
- le reporting.

Le Machine Learning n’est pas nécessaire dans la première version du projet.