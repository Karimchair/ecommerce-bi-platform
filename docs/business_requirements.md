# Business Requirements — E-commerce BI Platform

## 1. Contexte

Une entreprise e-commerce génère quotidiennement un grand volume de données provenant de plusieurs activités :

- commandes ;
- clients ;
- paiements ;
- produits ;
- vendeurs ;
- livraisons ;
- avis clients.

Ces données peuvent être difficiles à analyser directement car elles sont réparties entre plusieurs sources.

L'objectif du projet est donc de construire une plateforme décisionnelle centralisée permettant de transformer ces données brutes en informations utiles pour la prise de décision.

---

## 2. Problématique

L'entreprise souhaite répondre notamment aux questions suivantes :

- Quel est le chiffre d'affaires de l'entreprise ?
- Comment évoluent les ventes dans le temps ?
- Quelles catégories génèrent le plus de revenus ?
- Quels vendeurs sont les plus performants ?
- Quelles régions génèrent le plus de commandes ?
- Quel est le délai moyen de livraison ?
- Quel pourcentage de commandes est livré en retard ?
- Les retards ont-ils un impact sur la satisfaction client ?
- Quelles catégories ont les meilleures ou les moins bonnes évaluations ?
- Quel est le panier moyen des clients ?

---

## 3. Utilisateurs cibles

La plateforme peut être utilisée par plusieurs profils.

### Direction

Besoins :

- vision globale de la performance ;
- chiffre d'affaires ;
- évolution des ventes ;
- commandes ;
- clients ;
- principaux KPI.

---

### Équipe commerciale

Besoins :

- performances par catégorie ;
- performances par produit ;
- performances par vendeur ;
- analyse géographique des ventes ;
- évolution temporelle.

---

### Équipe logistique

Besoins :

- délai moyen de livraison ;
- commandes en retard ;
- régions avec des problèmes de livraison ;
- comparaison délai estimé / délai réel ;
- performances des vendeurs.

---

### Équipe satisfaction client

Besoins :

- note moyenne ;
- analyse des avis ;
- analyse des mauvaises évaluations ;
- relation entre délai de livraison et satisfaction.

---

## 4. Sources de données

Les données proviennent principalement du dataset Olist.

Les principales sources sont :

```text
olist_customers_dataset.csv
olist_geolocation_dataset.csv
olist_order_items_dataset.csv
olist_order_payments_dataset.csv
olist_order_reviews_dataset.csv
olist_orders_dataset.csv
olist_products_dataset.csv
olist_sellers_dataset.csv
product_category_name_translation.csv
```

---

## 5. Architecture cible

La chaîne de traitement cible est :

```text
CSV Olist
    |
    v
Azure Blob Storage / ADLS Gen2
    |
    v
Azure Data Factory
    |
    v
PostgreSQL Staging
    |
    v
Data Warehouse
    |
    v
Datamarts
    |
    v
Power BI
```

---

## 6. Exigences d'ingestion

Le système doit permettre :

- de centraliser les fichiers sources ;
- de charger automatiquement les fichiers ;
- d'éviter les traitements manuels répétitifs ;
- de surveiller les exécutions ;
- de détecter les échecs ;
- de permettre la relance des pipelines.

L'orchestration est réalisée avec :

```text
Azure Data Factory
```

---

## 7. Exigences de qualité des données

Les données doivent être contrôlées avant leur intégration dans le Data Warehouse.

### Valeurs manquantes

Identifier les champs comportant des valeurs nulles.

Selon la colonne :

- conserver ;
- remplacer ;
- exclure ;
- identifier comme inconnue.

---

### Doublons

Identifier et supprimer les doublons lorsqu'ils représentent plusieurs fois la même information.

Les identifiants uniques doivent notamment être contrôlés.

---

### Types de données

Les colonnes doivent avoir des types adaptés :

```text
dates → DATE / TIMESTAMP
prix → NUMERIC
quantités → INTEGER
identifiants → VARCHAR
```

---

### Dates

Les différentes dates doivent être vérifiées :

- date d'achat ;
- date d'approbation ;
- date d'expédition ;
- date de livraison ;
- date estimée.

Les incohérences chronologiques doivent être détectées.

---

### Valeurs numériques

Les valeurs impossibles doivent être détectées.

Exemples :

```text
prix < 0
frais_transport < 0
note < 1
note > 5
```

---

## 8. Zone Staging

La zone staging contient les données provenant directement du système source.

Exemple :

```text
staging.stg_customers
staging.stg_orders
staging.stg_order_items
staging.stg_products
staging.stg_sellers
staging.stg_payments
staging.stg_reviews
staging.stg_geolocation
```

Cette zone facilite :

- le contrôle ;
- la transformation ;
- le débogage ;
- la traçabilité.

---

## 9. Modèle dimensionnel

Le Data Warehouse est basé sur un modèle dimensionnel.

### Dimension Date

```text
dim_date
```

Attributs possibles :

- date ;
- jour ;
- numéro du jour ;
- semaine ;
- mois ;
- nom du mois ;
- trimestre ;
- année.

---

### Dimension Customer

```text
dim_customer
```

Attributs possibles :

- customer_key ;
- customer_id ;
- customer_unique_id ;
- ville ;
- état ;
- code postal.

---

### Dimension Product

```text
dim_product
```

Attributs possibles :

- product_key ;
- product_id ;
- catégorie ;
- poids ;
- longueur ;
- hauteur ;
- largeur.

---

### Dimension Seller

```text
dim_seller
```

Attributs possibles :

- seller_key ;
- seller_id ;
- ville ;
- état ;
- code postal.

---

### Fact Orders

```text
fact_orders
```

Mesures possibles :

- nombre de commandes ;
- montant total ;
- frais de transport ;
- délai de livraison ;
- retard de livraison ;
- nombre d'articles.

---

### Fact Order Items

```text
fact_order_items
```

Mesures possibles :

- prix ;
- frais de transport ;
- quantité.

---

### Fact Payments

```text
fact_payments
```

Mesures possibles :

- montant payé ;
- nombre de versements.

---

## 10. Datamart Sales

Le datamart Sales doit permettre les analyses suivantes :

- chiffre d'affaires total ;
- chiffre d'affaires mensuel ;
- nombre de commandes ;
- nombre de clients ;
- panier moyen ;
- chiffre d'affaires par produit ;
- chiffre d'affaires par catégorie ;
- chiffre d'affaires par vendeur ;
- chiffre d'affaires par région.

---

## 11. Datamart Logistics

Le datamart Logistics doit permettre :

- analyse du délai moyen ;
- analyse des commandes en retard ;
- comparaison date estimée / réelle ;
- analyse des retards par vendeur ;
- analyse des retards par région ;
- analyse des retards par catégorie.

---

## 12. Satisfaction client

L'analyse de la satisfaction doit permettre d'étudier :

- note moyenne ;
- distribution des notes ;
- nombre d'avis ;
- avis positifs ;
- avis négatifs ;
- notes par catégorie ;
- notes par vendeur ;
- impact des retards sur les notes.

Une analyse importante sera notamment :

```text
Délai de livraison
        ↓
Retard
        ↓
Satisfaction client
```

---

## 13. KPI principaux

### KPI commerciaux

```text
Chiffre d'affaires
Nombre de commandes
Nombre de clients
Panier moyen
Croissance du chiffre d'affaires
```

---

### KPI logistiques

```text
Délai moyen de livraison
Taux de retard
Nombre de commandes en retard
Écart moyen date prévue / réelle
```

---

### KPI satisfaction

```text
Note moyenne
Taux d'avis positifs
Taux d'avis négatifs
Note moyenne des commandes en retard
Note moyenne des commandes livrées à temps
```

---

## 14. Règles métier

### Chiffre d'affaires

Le chiffre d'affaires peut être calculé à partir du prix des articles vendus.

```text
CA = somme(price)
```

Selon l'analyse, les frais de transport doivent être traités séparément.

---

### Panier moyen

```text
Panier moyen =
Chiffre d'affaires / Nombre de commandes
```

---

### Délai de livraison

```text
Délai de livraison =
date_livraison_client - date_commande
```

---

### Commande en retard

Une commande est considérée comme en retard si :

```text
date_livraison_client
>
date_livraison_estimee
```

---

### Taux de retard

```text
Taux de retard =
Nombre de commandes en retard
/
Nombre de commandes livrées
```

---

## 15. Dashboards attendus

### Dashboard 1 — Executive Overview

Contenu :

- CA ;
- commandes ;
- clients ;
- panier moyen ;
- évolution mensuelle ;
- satisfaction ;
- retard.

---

### Dashboard 2 — Sales Analysis

Contenu :

- CA par catégorie ;
- CA par produit ;
- CA par vendeur ;
- évolution temporelle ;
- analyse géographique.

---

### Dashboard 3 — Logistics

Contenu :

- délai moyen ;
- retard ;
- livraison par région ;
- performance vendeur ;
- évolution mensuelle.

---

### Dashboard 4 — Customer Satisfaction

Contenu :

- note moyenne ;
- distribution des notes ;
- analyse des retards ;
- comparaison livraisons à temps / retard ;
- catégories les mieux et les moins bien évaluées.

---

## 16. Exigences techniques

Le projet doit utiliser principalement :

```text
Azure Data Factory
Azure Blob Storage / ADLS Gen2
PostgreSQL
SQL
Power BI
DAX
```

Python peut être utilisé pour :

```text
exploration
contrôle
validation
analyse complémentaire
```

mais ne doit pas remplacer Azure Data Factory pour la partie ETL principale.

---

## 17. Livrables

Le projet doit produire :

- une architecture documentée ;
- des pipelines Azure Data Factory ;
- une base PostgreSQL ;
- une zone staging ;
- un Data Warehouse ;
- des datamarts ;
- des scripts SQL ;
- un modèle Power BI ;
- plusieurs dashboards ;
- des KPI DAX ;
- une documentation GitHub ;
- des captures des pipelines et dashboards.

---

## 18. Résultat attendu

À la fin du projet, le système doit permettre de passer automatiquement de données brutes à des informations exploitables :

```text
Données brutes
      ↓
Azure Storage
      ↓
Azure Data Factory
      ↓
Staging
      ↓
Data Warehouse
      ↓
Datamarts
      ↓
Power BI
      ↓
Décision
```

Le projet doit démontrer une compréhension pratique d'une chaîne moderne de **Data Engineering et Business Intelligence**.