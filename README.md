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
- un modele XGBoost optimise enregistre en version `v1`
- des metriques de performance (MAE, RMSE, R2)
- une table `HOUSE_PREDICTIONS` contenant les inferences
- une app Streamlit permettant une estimation de prix interactive

## Execution recommandee
1. ouvrir le notebook dans Snowflake
2. verifier les packages
3. executer les cellules dans l'ordre de 1 a 24
4. confirmer la presence du modele dans le registry
5. lancer et tester l'interface Streamlit

## Livrables
- notebook complet du pipeline
- modele enregistre dans le registry
- analyse de performance (ce README)

## Auteurs
- Thuy-Linh TO
- Yassine Kamali
