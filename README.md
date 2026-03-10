# Analyse des systèmes éducatifs

## Recommandation de pays pour une expansion internationale

Projet d’analyse de données réalisé à partir du jeu de données **EdStats** dans le cadre d’un exercice orienté **data analysis** et **aide à la décision**.

---

## Contexte

Une entreprise souhaite étudier plusieurs pays afin d’éclairer une réflexion autour d’une **expansion à l’international**.

L’objectif n’est pas seulement de décrire les systèmes éducatifs, mais d’identifier des pays présentant un environnement globalement favorable selon plusieurs dimensions utiles à la prise de décision :

- niveau de développement et de connectivité ;
- accès à l’éducation ;
- effort public en matière d’éducation ;
- situation du marché ;
- taille potentielle du pays.

Le jeu de données initial étant volumineux, hétérogène et partiellement bruité, le projet a consisté à :

1. nettoyer et fiabiliser les données ;
2. réduire le périmètre des pays, années et indicateurs ;
3. construire un dataframe exploitable ;
4. analyser les corrélations entre indicateurs ;
5. produire une recommandation de pays sur une base quantitative.

---

## Objectif du projet

Construire une analyse reproductible permettant de répondre à la question suivante :

> **Quels pays semblent les plus pertinents pour une réflexion d’expansion internationale, à partir d’indicateurs éducatifs, numériques et socio-économiques ?**

---

## Données utilisées

Le projet repose sur plusieurs fichiers du dataset **EdStats** :

- `EdStatsCountry.csv`
- `EdStatsCountry-Series.csv`
- `EdStatsData.csv`
- `EdStatsFootNote.csv`
- `EdStatsSeries.csv`

### Rôle des principaux fichiers

- **Country** : informations sur les pays
- **Series** : dictionnaire des indicateurs
- **Data** : valeurs des indicateurs par pays et par année
- **FootNote** : notes associées à certaines observations
- **Country-Series** : lien entre pays et indicateurs

---

## Démarche analytique

### 1. Exploration et nettoyage

- exploration de chaque fichier ;
- détection des doublons ;
- analyse des valeurs manquantes ;
- suppression des colonnes inutilisables ;
- identification et suppression des **faux pays**.

### 2. Réduction du périmètre

- sélection d’indicateurs cohérents avec la problématique métier ;
- filtrage des années jugées les plus pertinentes ;
- réduction du nombre d’indicateurs pour conserver les plus exploitables.

### 3. Agrégation

Le fichier `Data` étant structuré à la maille **(pays, indicateur, année)**, les données ont été agrégées pour construire un dataframe final où :

- **une ligne = un pays**
- **une colonne = un indicateur**

### 4. Analyse des corrélations

Les corrélations ont été calculées avec :

- **Pearson**
- **Spearman**

Objectif :

- repérer les indicateurs redondants ;
- supprimer ceux dont la corrélation absolue dépasse un seuil de **0.70** ;
- conserver un ensemble final **plus lisible et moins redondant**.

### 5. Scoring des pays

Une méthode quantitative a ensuite été utilisée pour classer les pays :

- normalisation des indicateurs ;
- inversion des indicateurs défavorables ;
- calcul d’un **score composite** ;
- classement final des pays.

---

## Indicateurs finaux retenus

Après nettoyage, filtrage et décorrélation, les indicateurs retenus sont :

- `IT.NET.USER.P2` — Utilisateurs d’Internet (pour 100 personnes)
- `SE.PRM.ENRR` — Taux brut de scolarisation primaire
- `SE.SEC.ENRR` — Taux brut de scolarisation secondaire
- `SE.XPD.TOTL.GD.ZS` — Dépenses publiques d’éducation (% du PIB)
- `SL.UEM.TOTL.ZS` — Taux de chômage total
- `SP.POP.TOTL` — Population totale

Ces indicateurs ont été retenus car ils couvrent plusieurs dimensions utiles à une décision d’expansion :

- maturité numérique ;
- accès au système éducatif ;
- investissement public ;
- dynamique socio-économique ;
- taille du marché.

---

## Résultats principaux

### Pays les mieux classés dans l’analyse

Les pays suivants ressortent comme les plus favorables dans le scoring final :

1. Denmark
2. Iceland
3. Norway
4. Sweden
5. Netherlands
6. Australia
7. Finland
8. New Zealand
9. Belgium
10. Switzerland

### Lecture métier

Cette sélection fait ressortir des pays :

- très connectés ;
- globalement stables ;
- bien structurés sur le plan éducatif ;
- portés par de bons niveaux de développement humain et institutionnel.

---

## Limites de l’analyse

Cette étude constitue une **base de présélection** et non une décision finale à elle seule.

Elle ne prend pas directement en compte :

- la concurrence locale ;
- la réglementation du secteur ;
- les coûts d’entrée sur le marché ;
- la culture d’usage ;
- la stratégie commerciale propre de l’entreprise.

Les résultats doivent donc être interprétés comme un **outil d’aide à la décision**, à compléter par une analyse business plus large.

---

## Structure du projet

```text
P2_educatif_system_analysis/
├── notebooks/
│   └── edstats.ipynb
├── cleaned/
├── EdStatsCountry.csv
├── EdStatsCountry-Series.csv
├── EdStatsData.csv
├── EdStatsFootNote.csv
├── EdStatsSeries.csv
├── df_final_decorrele.csv
├── pyproject.toml
├── poetry.lock
└── README.md

Reproduire le projet
Pré-requis

Python 3.12

Poetry

VS Code ou JupyterLab

Installation
cd ~/dev/P2_educatif_system_analysis
poetry config virtualenvs.in-project true
poetry install
Lancer JupyterLab
poetry run jupyter lab
Ouvrir dans VS Code
code .

Puis sélectionner l’interpréteur :

./.venv/bin/python
Notebook principal

Le notebook principal du projet est :

notebooks/edstats.ipynb

Il contient :

l’exploration des jeux de données ;

le nettoyage ;

la sélection des pays et indicateurs ;

les corrélations Pearson / Spearman ;

les visualisations ;

le scoring final des pays.

Environnement reproductible

Le projet utilise Poetry pour garantir la reproductibilité de l’environnement Python via :

pyproject.toml

poetry.lock

Bibliothèques principales :

pandas

numpy

matplotlib

seaborn

jupyterlab

notebook

ipykernel

missingno

Compétences mobilisées

nettoyage de données ;

analyse exploratoire ;

filtrage et transformation de dataframes ;

gestion des valeurs manquantes ;

agrégation de données ;

analyse de corrélation ;

scoring quantitatif ;

formalisation de résultats ;

storytelling data.

Conclusion

Ce projet montre comment transformer un dataset large, imparfait et peu directement exploitable en une analyse structurée permettant de formuler une recommandation concrète.

La démarche suivie a permis de :

fiabiliser les données ;

sélectionner des indicateurs pertinents ;

réduire la redondance ;

proposer une liste argumentée de pays cibles.

Le résultat final constitue une base solide pour une discussion stratégique autour d’une expansion internationale.

Auteur

Vincent Desmouceaux

Projet réalisé dans le cadre d’un exercice d’analyse de données sur les systèmes éducatifs.