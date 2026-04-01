# Atelier Data Engineering & Machine Learning avec Snowflake

## Vue d'ensemble
Ce projet implemente un pipeline complet de data engineering et machine learning dans Snowflake pour predire le prix de maisons a partir de caracteristiques structurelles et contextuelles.

Le workflow couvre:
- ingestion des donnees JSON depuis S3
- transformation et typage des donnees dans Snowflake
- exploration et visualisation
- preparation des features
- entrainement et comparaison de modeles
- optimisation de XGBoost par GridSearchCV
- enregistrement dans le Model Registry Snowflake
- inference sur nouvelles donnees
- interface Streamlit pour les utilisateurs metier

## Dataset
Source: `s3://logbrain-datalake/datasets/house_price/`

Variables principales:
- price
- area
- bedrooms
- bathrooms
- stories
- mainroad
- guestroom
- basement
- hotwaterheating
- airconditioning
- parking
- prefarea
- furnishingstatus

## Stack technique
- Snowflake
- Snowpark
- Snowflake ML Registry
- Python
- pandas, numpy
- scikit-learn
- XGBoost
- matplotlib, seaborn
- Streamlit

## Packages a installer dans Snowflake Notebook
- snowflake-snowpark-python
- snowflake-ml-python
- pandas
- numpy
- scikit-learn
- matplotlib
- seaborn
- xgboost
- streamlit

## Structure du notebook
Le notebook suit 24 cellules de travail:
1. verification des dependances et connexion Snowflake
2. ingestion JSON (file format, stage, table raw, copy)
3. creation de la table structuree
4-9. exploration des donnees et visualisations
10-12. encodage, split train/test, normalisation
13-15. entrainement et comparaison des modeles
16-18. optimisation et evaluation du meilleur modele
19-23. registry, chargement modele, inference, persistance predictions
24. application Streamlit

## Resultats attendus
- un modele XGBoost retenu et enregistre en version `v1`
- des metriques de performance (MAE, RMSE, R2)
- une table `HOUSE_PREDICTIONS` contenant les inferences
- une app Streamlit permettant une estimation de prix interactive

## Analyse des performances du modele

### 1) Decoupage train/test
- Taille X_train: (872, 12)
- Taille X_test: (218, 12)
- Repartition: 80% train / 20% test

Ce split est coherent pour evaluer la generalisation du modele sur des donnees non vues.

### 2) Comparaison des modeles

| Modele | MAE | RMSE | R2 |
|---|---:|---:|---:|
| Linear Regression | 40,253.14 | 53,984.87 | 0.6732 |
| Random Forest | 19,586.55 | 32,513.49 | 0.8815 |
| XGBoost | 12,757.49 | 27,517.19 | 0.9151 |

Lecture des resultats:
- Linear Regression est le moins performant: erreur elevee et capacite explicative limitee.
- Random Forest ameliore fortement la prediction par rapport au modele lineaire.
- XGBoost est le meilleur modele de base, avec la plus faible erreur (MAE, RMSE) et le meilleur R2.

### 3) Optimisation (GridSearchCV)
- Nombre de combinaisons testees: 8
- Validation croisee: 3 folds (24 fits)
- Meilleurs hyperparametres:
	- xgb__learning_rate: 0.1
	- xgb__max_depth: 5
	- xgb__n_estimators: 200
- Meilleur score CV: R2 = 0.8286

### 4) Evaluation du modele optimise
- MAE: 18,054
- RMSE: 29,531
- R2: 0.9022

Comparaison avec XGBoost de base:
- R2 base: 0.9151
- R2 optimise: 0.9022
- Ecart: -1.29 points de R2

Interpretation:
- L'optimisation n'a pas ameliore la performance sur le jeu de test.
- Le modele XGBoost de base generalise legerement mieux dans cette configuration.
- Le choix final pour la production peut donc rester le XGBoost de base, ou necessiter une recherche d'hyperparametres plus large (plus de combinaisons, CV differente, ou random search).

### 5) Conclusion metier
Le pipeline atteint un bon niveau de precision pour l'estimation immobiliere. Le modele XGBoost est le plus adapte dans ce contexte, avec un compromis robuste entre precision predictive et stabilite des resultats.

## Execution recommandee
1. ouvrir le notebook dans Snowflake
2. verifier les packages
3. executer les cellules dans l'ordre de 1 a 24
4. confirmer la presence du modele dans le registry
5. lancer et tester l'interface Streamlit


## Auteurs
- Thuy-Linh TO
- Yassine Kamali
