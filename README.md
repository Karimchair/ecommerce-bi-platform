# Plateforme Business Intelligence E-commerce

Projet de Business Intelligence visant à concevoir un système décisionnel
complet pour analyser les performances commerciales, logistiques et la
satisfaction client d’une marketplace e-commerce.

Le projet utilise le dataset public Olist Brazilian E-Commerce.

## Objectif du projet

L’objectif est de transformer des données transactionnelles brutes en
informations décisionnelles fiables permettant aux responsables de mieux
comprendre l’activité de l’entreprise et de prendre des décisions basées
sur les données.

Le système permettra notamment de :

- suivre les performances commerciales ;
- analyser l’évolution du chiffre d’affaires ;
- identifier les catégories de produits les plus performantes ;
- comparer les performances géographiques ;
- analyser les performances des vendeurs ;
- suivre les délais et les retards de livraison ;
- mesurer la satisfaction client ;
- étudier la relation entre les retards de livraison et la satisfaction.

## Architecture décisionnelle

Le projet suit la chaîne décisionnelle suivante :

Sources de données Olist
→ ETL : Extraire, Transformer, Charger
→ Data Warehouse PostgreSQL
→ Datamarts métier
→ Modèles analytiques
→ Mise à disposition des données
→ Requêtage SQL, exploration multidimensionnelle, Power BI et reporting

Le Data Warehouse pourra alimenter directement un modèle analytique
ou alimenter d’abord un datamart qui sera ensuite utilisé par le modèle
analytique.

Une zone de staging PostgreSQL pourra être utilisée comme zone technique
interne pendant le processus ETL, mais elle ne constitue pas une couche
décisionnelle principale du projet.

## Sources de données

Les principales sources utilisées seront :

- `olist_customers_dataset.csv`
- `olist_orders_dataset.csv`
- `olist_order_items_dataset.csv`
- `olist_products_dataset.csv`
- `olist_sellers_dataset.csv`
- `olist_order_reviews_dataset.csv`
- `olist_order_payments_dataset.csv`
- `product_category_name_translation.csv`

## ETL

Un pipeline ETL développé avec Python et Pandas permettra de :

### Extraction

- lire les différents fichiers CSV ;
- vérifier la présence et la structure des fichiers ;
- contrôler le nombre de lignes extraites.

### Transformation

- supprimer les doublons ;
- traiter les valeurs manquantes ;
- convertir les dates et les colonnes numériques ;
- contrôler les identifiants et les relations entre les données ;
- traduire les catégories de produits ;
- calculer les délais de livraison ;
- calculer les retards ;
- identifier les commandes livrées en retard ;
- effectuer des contrôles de qualité.

### Chargement

- charger les dimensions dans PostgreSQL ;
- charger les tables de faits ;
- garantir l’intégrité référentielle ;
- utiliser des clés substituts lorsque cela est nécessaire.

## Data Warehouse

Le Data Warehouse PostgreSQL utilisera une modélisation dimensionnelle.

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
formant ainsi une constellation.

## Datamarts

Deux datamarts métier seront construits à partir du Data Warehouse.

### Datamart Sales

Destiné principalement aux responsables commerciaux.

Il permettra d’analyser :

- le chiffre d’affaires ;
- les commandes ;
- les produits ;
- les catégories ;
- les vendeurs ;
- les clients ;
- les performances géographiques ;
- le panier moyen.

### Datamart Logistics

Destiné principalement aux responsables logistiques.

Il permettra d’analyser :

- les délais de livraison ;
- les retards de livraison ;
- le taux de commandes en retard ;
- les performances logistiques par zone géographique ;
- la satisfaction client ;
- la relation entre retard de livraison et satisfaction.

Les datamarts pourront être implémentés avec des vues matérialisées
PostgreSQL dérivées du Data Warehouse.

## Modèles analytiques

Power BI sera utilisé pour construire les modèles analytiques du projet.

Ils comprendront :

- des dimensions ;
- des relations ;
- des hiérarchies ;
- des niveaux ;
- des mesures ;
- des agrégations ;
- des mesures DAX.

### Exemples de hiérarchies

- Date : Année > Trimestre > Mois > Jour
- Produit : Catégorie > Produit
- Géographie : État > Ville

Power BI jouera le rôle de couche analytique multidimensionnelle grâce
à son modèle sémantique tabulaire.

Le projet ne prétend pas construire un cube MOLAP classique.

## Principaux KPI

- Chiffre d’affaires total
- Nombre total de commandes
- Nombre total de clients
- Panier moyen
- Croissance du chiffre d’affaires
- Délai moyen de livraison
- Retard moyen
- Taux de livraison en retard
- Note moyenne des clients

## Restitution des données

Le système permettra :

- le requêtage SQL ;
- l’exploration multidimensionnelle ;
- le drill-down et le roll-up ;
- l’utilisation de filtres ;
- la création de tableaux de bord Power BI ;
- la visualisation des données ;
- le reporting ;
- l’analyse métier et la formulation de recommandations.

## Technologies prévues

- Python
- Pandas
- PostgreSQL
- SQL
- Power BI
- DAX
- Git

## Structure du projet

```text
ecommerce-bi-platform/
├── data/
│   ├── raw/
│   └── processed/
├── docs/
├── images/
├── notebooks/
├── powerbi/
├── results/
├── sql/
├── src/
├── README.md
├── requirements.txt
└── .gitignore