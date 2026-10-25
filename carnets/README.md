# Carnets de démarrage — un par sujet

> **Ce sont des châssis, pas des solutions.** Chaque carnet trouve les données, les charge
> efficacement, les profile, écrit une soumission de référence **au bon format** — puis s'arrête
> devant la modélisation.

---

## Ce que contient chaque carnet

| Cellule | Ce qu'elle fait |
|---|---|
| 1 · Réglages | `ECHANTILLON` (lignes lues au démarrage), dossier de cache |
| 2 · Trouver les données | cherche le dossier qui contient **tous** les fichiers attendus, et **dit où il a cherché** s'il échoue |
| 3 · Chargement | cache **Parquet** construit une fois, projection de colonnes ; lecture par morceaux disponible |
| 4 · Charger et regarder | dimensions, colonnes, aperçu |
| 5 · Profil de la cible | type, manquants, distribution |
| 6 · Soumission de référence | écrit `soumission.csv` au format attendu — **valeur volontairement nulle** |
| 7 · Le travail commence ici | `NotImplementedError` : métrique, découpage, variables, modèle |

## Agnostique à la plateforme

Rien n'est codé en dur. Le carnet cherche les données, dans cet ordre :

1. la variable d'environnement **`DONNEES_PROJET`** ;
2. le dossier courant, `./donnees`, `./<Sujet_NN_…>` ;
3. les **parents** du dossier courant (jusqu'à quatre niveaux) ;
4. **`/kaggle/input`** (Kaggle), **`/content`** et **`/content/drive/MyDrive`** (Colab).

Il s'arrête au premier dossier qui contient **tous** les fichiers du sujet, sinon il lève une erreur
qui liste les fichiers manquants et les chemins essayés.

## Sur les données lourdes — et pourquoi pas « raylib »

**`raylib` est une bibliothèque graphique** (fenêtres, dessin, jeux vidéo) : elle ne lit aucune table.
Elle n'a rien à faire dans un projet de données. Les outils qui répondent au besoin sont :

| Outil | Rôle | Présent par défaut ? |
|---|---|---|
| **Parquet + pyarrow** | format colonne compressé, lecture sélective par colonne | **oui**, avec pandas récent |
| **Polars** | moteur colonne, `scan_csv` paresseux | non — `pip install polars` |
| **DuckDB** | SQL directement sur CSV et Parquet, sans chargement | non — `pip install duckdb` |

Les carnets des sujets lourds (**02 Favorita**, **03 Home Credit**, **06 Porto**, **07 Santander
transaction**, **09 GA**) utilisent **Parquet** : la conversion CSV → Parquet est payée **une fois**,
puis toutes les lectures suivantes ne prennent que les colonnes demandées. Le cache s'écrit **à côté
du carnet**, jamais dans les données — sur Kaggle, `/kaggle/input` est en lecture seule.

**Sujet 09 (`ga`)** : les données sont deux archives `train.zip` et `a_estimer.zip` dont quatre
colonnes portent du **JSON**. Le carnet lit les sessions **par lots, sans extraction sur disque**, et
rappelle que la décompression et le parsing font partie du travail de l'équipe.

## Vérification

Les neuf carnets ont été **exécutés de bout en bout** contre les vraies données
(`outils_kaggle/verifier_carnets_demarrage.py`), avec un échantillon réduit. Résultat : **9 carnets,
0 échec**, et chaque soumission de référence porte **le nombre de lignes attendu** :

| Sujet | Lignes produites | Attendu |
|---|---|---|
| 01 Rossmann | 53 520 | horizon de 48 jours × 1 115 magasins |
| 02 Favorita | 277 568 | grille dense 17 348 paires × 16 jours |
| 03 Home Credit | 61 503 | holdout stratifié |
| 04 Allstate | 37 664 | holdout stratifié |
| 05 Santander satisfaction | 15 204 | holdout stratifié |
| 06 Porto | 119 043 | holdout stratifié |
| 07 Santander transaction | 40 000 | holdout stratifié |
| 08 Covertype | 116 203 | holdout stratifié, **7 colonnes de probabilités** |
| 09 GA | 142 834 | visiteurs du jeu à estimer |

## Régénérer

```
./outils_kaggle/.venv/Scripts/python.exe outils_kaggle/construire_carnets_demarrage.py
```

```
./outils_kaggle/.venv/Scripts/python.exe outils_kaggle/verifier_carnets_demarrage.py
```
