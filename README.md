# E-commerce BI Platform

## 1. Présentation du projet

Ce projet consiste à construire une plateforme complète de **Business Intelligence et Data Engineering** à partir du dataset public **Olist Brazilian E-Commerce**.

L’objectif est de reproduire une architecture proche de celle utilisée en entreprise, depuis l’ingestion des données brutes jusqu’à la création de tableaux de bord décisionnels.

Le projet couvre notamment :

- l’ingestion des fichiers CSV ;
- le stockage des données brutes dans Azure ;
- la création de pipelines ETL avec Azure Data Factory ;
- le chargement des données dans PostgreSQL ;
- la construction d’un Data Warehouse dimensionnel ;
- la création de datamarts ;
- l’analyse des données avec Power BI ;
- la création de KPI métiers avec DAX.

---

## 2. Objectifs

Les objectifs principaux du projet sont :

- centraliser les données e-commerce ;
- automatiser leur ingestion et leur transformation ;
- améliorer la qualité des données ;
- construire un modèle décisionnel adapté à l'analyse ;
- créer des indicateurs métier ;
- analyser les ventes, les clients, les produits et les livraisons ;
- fournir des tableaux de bord permettant d'aider à la prise de décision.

Ce projet permet également de mettre en pratique des compétences recherchées pour des stages en :

- Business Intelligence ;
- Data Engineering ;
- Data Analysis ;
- Data Science.

---

## 3. Dataset

Le projet utilise le dataset **Brazilian E-Commerce Public Dataset by Olist**.

Il contient plusieurs fichiers CSV représentant différentes informations du système e-commerce.

Exemples de données disponibles :

- commandes ;
- clients ;
- produits ;
- vendeurs ;
- paiements ;
- avis clients ;
- articles commandés ;
- géolocalisation ;
- catégories de produits.

Le dataset contient environ 100 000 commandes et plusieurs centaines de milliers de lignes réparties dans plusieurs tables.

---

## 4. Architecture du projet

L'architecture générale est la suivante :

```text
Olist CSV
    |
    v
Azure Blob Storage / ADLS Gen2
    |
    v
Azure Data Factory
    |
    |-- Ingestion
    |-- Transformation
    |-- Nettoyage
    |-- Orchestration
    |-- Gestion des erreurs
    |-- Monitoring
    |
    v
PostgreSQL - Staging
    |
    v
Data Warehouse
    |
    |-- Tables de dimensions
    |-- Tables de faits
    |
    v
Datamarts
    |
    v
Power BI
    |
    |-- Modèle sémantique
    |-- DAX
    |-- KPI
    |-- Dashboards
```

---

## 5. Technologies utilisées

### Data Engineering

- Azure Data Factory
- Azure Blob Storage / Azure Data Lake Storage Gen2
- PostgreSQL
- SQL

### Business Intelligence

- Power BI
- Power Query
- DAX

### Data Exploration

Python peut être utilisé uniquement pour certaines tâches complémentaires :

- exploration des fichiers ;
- compréhension du dataset ;
- contrôle de qualité ;
- vérifications ponctuelles.

Bibliothèques possibles :

- Pandas
- NumPy
- Matplotlib

Cependant, **Python/Pandas n'est pas utilisé comme outil ETL principal**.

L'ingestion, les transformations et l'orchestration sont réalisées principalement avec **Azure Data Factory**.

---

## 6. Pipeline ETL

Azure Data Factory est utilisé comme outil ETL principal du projet.

Le pipeline suit plusieurs étapes.

### Étape 1 — Ingestion

Les fichiers CSV du dataset Olist sont déposés dans :

```text
Azure Blob Storage
```

ou :

```text
Azure Data Lake Storage Gen2
```

Azure Data Factory récupère ensuite automatiquement les fichiers.

---

### Étape 2 — Chargement Staging

Les données sont chargées dans une zone intermédiaire PostgreSQL appelée :

```text
staging
```

Cette zone contient les données proches du format source.

Exemple :

```text
stg_customers
stg_orders
stg_order_items
stg_products
stg_sellers
stg_payments
stg_reviews
```

---

### Étape 3 — Nettoyage et transformation

Azure Data Factory permet de réaliser plusieurs transformations :

- conversion des types ;
- suppression ou gestion des doublons ;
- gestion des valeurs manquantes ;
- normalisation des données ;
- contrôle des valeurs incohérentes ;
- création de nouvelles colonnes ;
- préparation des données pour le Data Warehouse.

---

## 7. Data Warehouse

Le Data Warehouse utilise principalement un modèle dimensionnel en étoile.

### Tables de dimensions

Exemples :

```text
dim_customer
dim_product
dim_seller
dim_date
dim_location
dim_payment_type
```

### Tables de faits

Exemples :

```text
fact_orders
fact_order_items
fact_payments
```

Les dimensions fournissent les axes d'analyse.

Les tables de faits contiennent les mesures quantitatives.

---

## 8. Datamarts

Des datamarts sont créés à partir du Data Warehouse afin de simplifier les analyses Power BI.

### Sales Datamart

Analyse de :

- chiffre d'affaires ;
- commandes ;
- clients ;
- produits ;
- catégories ;
- vendeurs.

### Logistics Datamart

Analyse de :

- délais de livraison ;
- commandes en retard ;
- performances logistiques ;
- différences entre date estimée et date réelle.

D'autres datamarts pourront être ajoutés si nécessaire.

---

## 9. KPI

Les principaux KPI envisagés sont :

### Ventes

- chiffre d'affaires ;
- nombre de commandes ;
- nombre de clients ;
- panier moyen ;
- chiffre d'affaires par catégorie ;
- chiffre d'affaires par vendeur ;
- croissance du chiffre d'affaires.

### Logistique

- délai moyen de livraison ;
- taux de commandes en retard ;
- délai moyen par région ;
- performance des vendeurs.

### Satisfaction client

- note moyenne ;
- nombre d'avis ;
- taux d'avis positifs ;
- taux d'avis négatifs ;
- relation entre retard de livraison et note client.

---

## 10. Analyses Power BI

Plusieurs dashboards seront créés.

### Dashboard Executive

Vue globale des principaux KPI :

- chiffre d'affaires ;
- commandes ;
- clients ;
- panier moyen ;
- satisfaction ;
- retard de livraison.

### Dashboard Sales

Analyse par :

- période ;
- catégorie ;
- produit ;
- vendeur ;
- région.

### Dashboard Logistics

Analyse des :

- délais ;
- retards ;
- performances logistiques ;
- régions ;
- vendeurs.

### Dashboard Customer Satisfaction

Analyse de la relation entre :

- avis clients ;
- délais de livraison ;
- catégories ;
- vendeurs.

---

## 11. Structure du dépôt

```text
ecommerce-bi-platform/
│
├── data/
│   ├── raw/
│   └── processed/
│
├── docs/
│   ├── business_requirements.md
│   ├── architecture.md
│   └── data_dictionary.md
│
├── notebooks/
│
├── sql/
│   ├── staging/
│   ├── warehouse/
│   ├── datamarts/
│   └── analysis/
│
├── src/
│
├── powerbi/
│
├── images/
│
├── results/
│
└── README.md
```

---

## 12. Organisation du projet

Le développement du projet suit les grandes étapes suivantes :

```text
1. Analyse du dataset
2. Définition des besoins métier
3. Conception de l'architecture
4. Configuration Azure
5. Upload des données
6. Création des pipelines Azure Data Factory
7. Création de la zone staging PostgreSQL
8. Nettoyage et transformation
9. Création du Data Warehouse
10. Création des datamarts
11. Connexion Power BI
12. Création des KPI DAX
13. Création des dashboards
14. Documentation
```

---

## 13. Compétences mises en pratique

### Data Engineering

- ETL
- Azure Data Factory
- Azure Blob Storage
- orchestration de pipelines
- SQL
- PostgreSQL
- Data Warehouse
- modèle dimensionnel
- qualité des données

### Business Intelligence

- Power BI
- Power Query
- DAX
- KPI
- Datamarts
- Dashboarding

### Data

- analyse exploratoire ;
- préparation des données ;
- modélisation ;
- analyse métier.

---

## 14. Objectif professionnel

Ce projet est réalisé dans le cadre de ma formation d'ingénieur à l'ISIMA, filière **Systèmes d'Information et Aide à la Décision**.

Il a pour objectif de mettre en pratique une chaîne complète :

```text
Source
→ Cloud
→ ETL
→ Base de données
→ Data Warehouse
→ Datamart
→ Business Intelligence
```

Le projet servira également de projet personnel pour mes candidatures de stage dans les domaines :

- Data Engineering ;
- Business Intelligence ;
- Data Science.