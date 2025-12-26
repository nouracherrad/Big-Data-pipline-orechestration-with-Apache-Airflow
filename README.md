

# Orchestration d’un pipeline Big Data complet avec Apache Airflow

---

##  Objectif de l’atelier

L’objectif de cet atelier est de mettre en œuvre un **pipeline Big Data de bout en bout**, orchestré à l’aide d’**Apache Airflow**, afin de comprendre concrètement :

* Le fonctionnement d’un pipeline Big Data end-to-end
* Le rôle du **Data Lake** et du **Data Lakehouse**
* La définition et l’orchestration d’un **DAG Airflow**
* La supervision et le suivi d’exécution via l’interface Airflow
* La gestion des erreurs et la relance partielle d’un pipeline

Cet atelier adopte une **approche pédagogique**, tout en respectant les **bonnes pratiques utilisées en environnement industriel**.

---

##  Pipeline Big Data étudié

### Pipeline logique

```
Sources → Ingestion → Data Lake (RAW) → Traitement → Data Lakehouse (CURATED) → Analytics
```

### Explication

* **Data Lake (RAW)** : stockage des données brutes sans transformation
* **Traitement Big Data** : nettoyage et structuration des données
* **Data Lakehouse (CURATED)** : données fiables et prêtes pour l’analyse
* **Analytics / BI / IA** : exploitation métier (dashboards, modèles IA)
* **Apache Airflow** orchestre et supervise l’enchaînement de ces étapes

---

##  Architecture technique

### Composants utilisés

* **Apache Airflow** (déployé avec Docker)
* **PostgreSQL** (base de métadonnées Airflow)
* **Python** (simulation des traitements Big Data)
* **Système de fichiers local** pour représenter :

  * Data Lake
  * Data Lakehouse

### Remarque

> Le volume de données est simulé, mais la logique du pipeline est identique à celle utilisée dans des architectures Big Data réelles.

---

## 📁 Structure du projet

```
airflow-bigdata-pipeline/
│
├── docker-compose.yml
├── dags/
│   └── bigdata_pipeline.py
└── data/
    ├── raw/
    ├── processed/
    └── curated/
```

### Rôle des dossiers

* `raw/` : zone **Data Lake**
* `processed/` : zone intermédiaire de traitement
* `curated/` : zone **Data Lakehouse**
* Airflow orchestre la circulation des données entre ces zones

---

##  Installation d’Apache Airflow avec Docker

### 1️⃣ Lancer Airflow

Dans le dossier du projet :

```bash
docker-compose up -d
```

### 2️⃣ Initialisation (à faire une seule fois)

```bash
docker-compose run airflow-webserver airflow db init
```

```bash
docker-compose run airflow-webserver airflow users create \
  --username airflow \
  --password airflow \
  --firstname Airflow \
  --lastname Admin \
  --role Admin \
  --email admin@airflow.local
```

### 3️⃣ Accès à l’interface Airflow

* URL : [http://localhost:8080](http://localhost:8080)
* Identifiants :

  * **Username** : airflow
  * **Password** : airflow

---
<img width="1600" height="793" alt="image" src="https://github.com/user-attachments/assets/c5e166b8-a591-45cd-a51d-ef764b609057" />

##  Définition du DAG Big Data

Le DAG est défini dans le fichier :

```
dags/bigdata_pipeline.py
```

Airflow détecte automatiquement tout fichier Python placé dans ce dossier.

---

##  Étapes du pipeline (DAG)

### 1️⃣ Ingestion (Data Lake)

* Création d’un fichier `sales.csv`
* Données stockées brutes dans la zone RAW

### 2️⃣ Validation

* Vérification de l’existence des données
* Arrêt du pipeline en cas d’erreur

### 3️⃣ Transformation Big Data

* Simulation d’un traitement Big Data (Spark / SQL)
* Données nettoyées stockées dans `processed/`

### 4️⃣ Chargement dans le Data Lakehouse

* Données finales stockées dans `curated/`
* Prêtes pour l’analyse

### 5️⃣ Analytics

* Étape finale simulant l’exploitation BI / IA

---
<img width="1600" height="781" alt="image" src="https://github.com/user-attachments/assets/dc9fd6f5-c10e-4862-b1bd-edcf0f897da7" />


##  Orchestration avec Airflow

* Le DAG **bigdata_pipeline_complete** définit l’ordre d’exécution
* Les dépendances garantissent un pipeline robuste
* Airflow assure :

  * la traçabilité
  * la gestion des erreurs
  * la supervision complète

---

##  Exécution via l’interface Airflow

### Activation du DAG

* Activer le DAG **bigdata_pipeline_complete**

<img width="1092" height="308" alt="image" src="https://github.com/user-attachments/assets/306d84bb-c98c-4b79-b0dc-8e9d6d709892" />

---



### Vue Graph

* Visualisation des tâches :

  * ingest
  * validate
  * transform
  * load_lakehouse
  * analytics

<img width="1446" height="512" alt="image" src="https://github.com/user-attachments/assets/662f33d0-58dc-4b65-8307-386f5cccd65d" />

---



##  Résultats du pipeline

Après une exécution réussie, les fichiers suivants sont générés :

```
data/raw/sales.csv
data/processed/sales_clean.csv
data/curated/sales_curated.csv
```

✔ Le Data Lake est alimenté
✔ Le Data Lakehouse contient les données finales
✔ Le pipeline Big Data fonctionne correctement

---


##  Conclusion

Cet atelier a permis de :

* Comprendre concrètement l’orchestration Big Data
* Implémenter un pipeline structuré avec Airflow
* Visualiser et superviser l’exécution via une interface graphique
* Appliquer des concepts utilisés en environnement industriel

---
