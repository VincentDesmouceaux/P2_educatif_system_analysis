# 🌍 Analyse des systèmes éducatifs – Recommandation de pays pour expansion internationale

Bienvenue dans le projet **academy** !  
Ce dépôt contient une analyse complète du dataset **EdStats** (Banque mondiale) dans le but d’**identifier les pays les plus prometteurs** pour une réflexion d’expansion à l’international.

👉 Une startup EdTech fictive cherche à évaluer des marchés potentiels à partir d’indicateurs éducatifs, numériques et socio-économiques.

---

## 🎯 Objectif

Construire une **base quantitative d’aide à la décision** et répondre à la question :

> **Quels pays semblent les plus pertinents pour une expansion internationale, à partir des données éducatives et contextuelles disponibles ?**

---

## 📁 Données utilisées

Le projet s’appuie sur les **5 fichiers du dataset EdStats** :

| Fichier                   | Description                              |
|---------------------------|------------------------------------------|
| `EdStatsCountry.csv`      | Informations sur les pays                |
| `EdStatsCountry-Series.csv`| Lien entre pays et séries d’indicateurs |
| `EdStatsData.csv`         | Valeurs des indicateurs (pays × années)  |
| `EdStatsFootNote.csv`     | Notes associées à certaines observations |
| `EdStatsSeries.csv`       | Dictionnaire des indicateurs             |

---

## 🔍 Démarche analytique (étape par étape)

### 1️⃣ Exploration et nettoyage initial  
- Détection des doublons, colonnes vides, valeurs manquantes  
- Visualisation rapide avec `missingno`  
- Suppression des colonnes inutilisables  

### 2️⃣ 🚫 Suppression des « faux pays »  
Les fichiers contiennent des agrégats régionaux (`World`, `High income`, etc.).  
On les identifie par l’absence de `Region` ou `Income Group` et on les retire avec `isin()` ou `merge()`.

### 3️⃣ 📅 Sélection des années pertinentes  
On conserve les années **2008 à 2017** (fenêtre récente, historiquement renseignée).  
Analyse du taux de valeurs manquantes par année pour justifier le choix.

### 4️⃣ 📈 Sélection des indicateurs métier  
Parmi des centaines d’indicateurs, on retient ceux qui couvrent les dimensions clés pour une expansion :  
- Connectivité numérique  
- Accès à l’éducation  
- Investissement public  
- Dynamique du marché  
- Taille potentielle du pays  

**Première sélection** : 15 indicateurs (listés dans le notebook).

### 5️⃣ 🔗 Agrégation : un pays = une ligne  
Le fichier `Data` est au format long (pays × indicateur × année).  
On utilise `pivot_table()` pour obtenir un tableau où chaque ligne est un pays et chaque colonne un indicateur (moyenne sur 2008‑2017).

### 6️⃣ 📉 Analyse des corrélations et réduction de la redondance  
- Calcul des matrices **Pearson** et **Spearman**  
- Repérage des paires d’indicateurs avec |corr| > 0.70  
- Suppression des indicateurs redondants pour garder une information non dupliquée  

### 7️⃣ 📊 Construction d’un score composite  
- Normalisation **min‑max** de chaque indicateur  
- Inversion du taux de chômage (plus il est bas, mieux c’est)  
- Pondération des dimensions (poids définis métier)  
- Calcul du score final → classement des pays

---

## ✅ Indicateurs finaux retenus

Après filtrage et décorrélation, voici les **6 indicateurs** utilisés pour le scoring :

| Code indicateur       | Signification                                  | Emoji |
|-----------------------|------------------------------------------------|:-----:|
| `IT.NET.USER.P2`      | Utilisateurs d’Internet (pour 100 pers.)       | 💻    |
| `SE.PRM.ENRR`         | Taux brut de scolarisation primaire            | 🏫    |
| `SE.SEC.ENRR`         | Taux brut de scolarisation secondaire          | 🎓    |
| `SE.XPD.TOTL.GD.ZS`   | Dépenses publiques d’éducation (% du PIB)      | 💰    |
| `SL.UEM.TOTL.ZS`      | Taux de chômage total                           | 📉    |
| `SP.POP.TOTL`         | Population totale                               | 👥    |

---

## 🏆 Résultats : Top 10 des pays recommandés

Le classement final obtenu par le score composite :

| Rang | Pays         | Score |
|------|--------------|-------|
| 1    | Denmark      | 0.671 |
| 2    | Iceland      | 0.653 |
| 3    | Norway       | 0.657 |
| 4    | Sweden       | 0.638 |
| 5    | Netherlands  | 0.654 |
| 6    | Australia    | 0.629 |
| 7    | Finland      | 0.628 |
| 8    | New Zealand  | 0.620 |
| 9    | Belgium      | 0.619 |
| 10   | Switzerland  | 0.617 |

> 📌 Ces pays se distinguent par une **forte connectivité**, un **bon niveau éducatif**, une **stabilité socio‑économique** et un **marché potentiel intéressant**.

---

## ⚠️ Limites de l’analyse

Ce travail constitue une **présélection quantitative**. Il ne remplace pas une étude business approfondie qui prendrait en compte :

- la concurrence locale  
- la réglementation du secteur éducatif / numérique  
- les coûts d’entrée sur le marché  
- la culture d’usage  
- la stratégie commerciale propre de l’entreprise  

Les résultats sont à interpréter comme un **outil d’aide à la décision** à enrichir avec des expertises métier.

---

## 📂 Structure du projet

```text
P2_educatif_system_analysis/
├── notebooks/
│   └── edstats.ipynb          # Notebook principal (analyse complète)
├── cleaned/                    # (optionnel) exports intermédiaires
├── EdStatsCountry.csv
├── EdStatsCountry-Series.csv
├── EdStatsData.csv
├── EdStatsFootNote.csv
├── EdStatsSeries.csv
├── df_final_decorrele.csv      # Table finale pays × indicateurs
├── top_pays_recommandes.csv    # Top 10 avec scores
├── liste_pays_favoris.csv      # Liste simple des pays recommandés
├── pyproject.toml              # Dépendances (Poetry)
├── poetry.lock
└── README.md                   # Ce fichier
```

---

## 🛠️ Reproduction du projet

### Prérequis
- Python 3.12  
- [Poetry](https://python-poetry.org/)  
- JupyterLab / VS Code

### Installation
```bash
cd ~/dev/P2_educatif_system_analysis
poetry config virtualenvs.in-project true
poetry install
```

### Lancer le notebook
```bash
poetry run jupyter lab
# ou ouvrir dans VS Code et sélectionner l’interpréteur ./.venv/bin/python
```

Le notebook principal est `notebooks/edstats.ipynb`.  
Il contient tout le code, les commentaires et les visualisations.

---

## 📚 Bibliothèques principales

- `pandas` – manipulation de données  
- `numpy` – calculs numériques  
- `matplotlib` / `seaborn` – visualisations  
- `missingno` – visualisation des valeurs manquantes  
- `jupyter` – environnement interactif  

---

## 👤 Auteur

**Vincent Desmouceaux**  
Projet réalisé dans le cadre d’un exercice d’analyse de données sur les systèmes éducifs (OpenClassrooms).

---

## 🎉 Conclusion

Ce projet montre comment transformer un dataset brut et hétérogène en une **analyse structurée, reproductible et orientée décision**.  
La liste des pays recommandés fournit une **base solide pour amorcer une réflexion stratégique** sur l’expansion internationale d’academy.

N’hésitez pas à explorer le notebook pour plonger dans les détails techniques ! 🚀