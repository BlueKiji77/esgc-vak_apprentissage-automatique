# Projet — Identifier le type de couvert forestier d'une parcelle

**Équipe de 3 · 6 semaines · rendu par dépôt**

---

## Le contexte

Un service forestier doit **cartographier la couverture végétale** d'un massif. Envoyer une équipe sur
chaque parcelle coûte cher ; les relevés topographiques et pédologiques, eux, sont déjà disponibles
pour l'ensemble du territoire.

## La question à laquelle le projet répond

> Peut-on déduire, des seules variables de relief et de sol, **l'espèce qui domine** sur une parcelle
> de 30 × 30 mètres — et de combien une méthode soignée bat-elle le classement actuel, qui applique la
> même espèce partout ?

## Ce que vous recevez

| Fichier | Contenu |
|---|---|
| `train.csv` | **464 809 parcelles** dont l'espèce est connue — 55 colonnes, dont `couvert` |
| `a_classer.csv` | **116 203 parcelles** — les mêmes 54 variables, **sans `couvert`** |

**Sept espèces, très inégalement réparties** — de 48,76 % pour la plus fréquente à **0,47 %** pour la
plus rare. Les sept sont présentes dans les deux fichiers.

## Ce que contient chaque colonne

> **Documentation officielle du jeu de données** (UCI Machine Learning Repository, `covtype.info`),
> traduite et mise en tableau. « Number of Attributes: *12 measures, but 54 columns of data* (10
> quantitative variables, 4 binary wilderness areas and 40 binary soil type variables) ».

### Les 10 mesures quantitatives

| Nom | Type | Unité | Description officielle |
|---|---|---|---|
| `elevation_m` | quantitative | mètres | Elevation in meters |
| `aspect_deg` | quantitative | azimut | Aspect in degrees azimuth |
| `slope_deg` | quantitative | degrés | Slope in degrees |
| `distance_hydrology_m` | quantitative | mètres | Horz Dist to nearest surface water features |
| `vertical_hydrology_m` | quantitative | mètres | Vert Dist to nearest surface water features |
| `distance_road_m` | quantitative | mètres | Horz Dist to nearest roadway |
| `hillshade_9h` | quantitative | indice 0 à 255 | Hillshade index at 9am, summer solstice |
| `hillshade_midi` | quantitative | indice 0 à 255 | Hillshade index at noon, summer solstice |
| `hillshade_15h` | quantitative | indice 0 à 255 | Hillshade index at 3pm, summer solstice |
| `distance_fire_m` | quantitative | mètres | Horz Dist to nearest wildfire ignition points |

### Les 4 zones sauvages — binaires

« Wilderness area designation », 0 (absence) ou 1 (présence). **Une seule vaut 1 par parcelle.**

| Colonne | Zone officielle |
|---|---|
| `zone_sauvage_1` | Rawah Wilderness Area |
| `zone_sauvage_2` | Neota Wilderness Area |
| `zone_sauvage_3` | Comanche Peak Wilderness Area |
| `zone_sauvage_4` | Cache la Poudre Wilderness Area |

### Les 40 types de sol — binaires

« Soil Type designation », 0 (absence) ou 1 (présence), d'après les **USFS Ecological Landtype Units**
(ELU). **Un seul vaut 1 par parcelle.** Les 40 sont présents dans les données ; leurs libellés
officiels vont de « Cathedral family - Rock outcrop complex, extremely stony » à « Krummholz »-type
d'altitude, et sont donnés dans `covtype.info`.

### La cible

| Valeur | Espèce officielle |
|---|---|
| 1 | Spruce/Fir |
| 2 | Lodgepole Pine |
| 3 | Ponderosa Pine |
| 4 | Cottonwood/Willow |
| 5 | Aspen |
| 6 | Douglas-fir |
| 7 | Krummholz |

## Ce que vous rendez — attention, c'est une **distribution**

Votre pipeline produit, pour chaque parcelle, **sept probabilités** :

```
id,couverture_1,couverture_2,couverture_3,couverture_4,couverture_5,couverture_6,couverture_7
1,0.02,0.91,0.01,0.00,0.01,0.04,0.01
```

Chaque ligne doit sommer à 1. **Ce n'est pas une étiquette que vous rendez, c'est une distribution.**

Le dépôt contient en outre le protocole de validation justifié, les essais (dont ceux qui ont échoué),
une note de 3 pages, une model card, et la fiche de données du S13.

Le pipeline doit tourner **en moins de 10 minutes sur CPU**, sans GPU, avec `scikit-learn` seul.
**581 012 lignes et 54 variables : la contrainte de temps est réelle**, et c'est une contrainte du
projet, pas un détail.

## Travailler : récupérer les données, coder, sauvegarder

Les données du cours sont sur un **Drive partagé, en lecture seule**, un dossier par sujet. Vous
téléchargez **le dossier de votre sujet** — pas les autres — et vous choisissez ensuite **où coder**.

### 1. Récupérer votre dossier

| | |
|---|---|
| Dossier du sujet | **`Sujet_09_Covertype/`** |
| Poids attendu | **79,5 Mo** |
| Contenu | les fichiers listés dans « Ce que vous recevez » |

Après téléchargement, vérifiez que rien n'est tronqué : le poids du dossier doit correspondre, et
chaque `csv` doit s'ouvrir avec le bon nombre de lignes.

### 2. Option A — travailler en local, dans VS Code

C'est la voie la plus simple : vos fichiers sont sur votre disque, rien ne disparaît.

```
python -m venv .venv
```

```
source .venv/bin/activate
```

*(sous Windows : `.venv\Scripts\activate`)*

```
pip install pandas scikit-learn
```

Ouvrez le dossier dans VS Code, écrivez votre pipeline — `pipeline.py` par exemple — et lancez-le :

```
python pipeline.py
```

**La contrainte des 10 minutes s'applique à votre machine.** Mesurez-la : elle fait partie du
livrable, et un pipeline qui tourne en 40 minutes sur votre portable ne sera pas accepté.

### 3. Option B — travailler dans un carnet Kaggle

Kaggle ne voit pas votre disque : il faut **lui donner vos fichiers**, en créant votre propre jeu de
données. C'est vous qui le déposez, pour votre usage — le jeu est **privé par défaut**.

1. **kaggle.com/datasets** → **New Dataset**.
2. Déposez les fichiers du dossier `Sujet_09_Covertype/` (glisser-déposer), nommez le jeu de données
   `esgc-vak-sujet-09-…`, puis **Create**.
3. Dans votre carnet : **Add Input** → onglet **Datasets** → filtre **Your Datasets** → sélectionnez
   le vôtre → **Add**.
4. Les fichiers apparaissent dans **`/kaggle/input/<nom-de-votre-jeu>/`**, en **lecture seule**.
   Vérifiez avant d'écrire du code :

```
!ls /kaggle/input/<nom-de-votre-jeu>/
```

5. Réglez la session : **Session options** → **Accelerator : None** — le projet tient sur CPU, c'est
   la contrainte du cours. Laissez **Internet désactivé**.

> **La compétition d'origine n'est pas la source.** Ne cherchez pas à l'ajouter : ses fichiers
> diffèrent des vôtres — pas d'horizon sans cible, pas de jeu à estimer — et plusieurs de ces
> compétitions ne sont plus rejoignables.

### 4. Sauvegarder, et réutiliser votre travail

**En local (Option A).** Votre dossier de travail **est** la sauvegarde. Gardez les sorties dans un
sous-dossier `sorties/`, et versionnez avec `git` si vous le souhaitez.

**Sur Kaggle (Option B).** `/kaggle/input` est en **lecture seule** : écrivez dans
**`/kaggle/working/`**.

```python
import os
os.makedirs("/kaggle/working/sorties", exist_ok=True)
prediction.to_csv("/kaggle/working/sorties/prediction.csv", index=False)
```

- **Sauvegarder** : bouton **Save Version** → **Save & Run All (Commit)**. Le carnet est **ré-exécuté
  de bout en bout** dans un conteneur propre, et **tout ce qui reste dans `/kaggle/working/` est
  conservé** comme sortie de cette version.
- **Reprendre ces sorties dans un autre carnet** : **Add Input** → onglet **Notebook Output** →
  votre carnet, la version voulue → le contenu apparaît dans `/kaggle/input/<nom-du-carnet>/`.
- **⚠️ `/kaggle/working` est éphémère** : une session inactive est coupée au bout de 20 minutes. Sans
  **Save Version**, tout est perdu.

**Quel que soit votre choix**, le livrable est le même : un **dépôt de code** reproductible, dont le
pipeline tourne en moins de 10 minutes sur CPU.

## Les trois temps du projet

### Bloc 1 — avant la séance 5 · *la décision et la mesure*

- **La métrique** : ce qu'elle juge exactement, et pourquoi elle **pèse la confiance** de chaque
  réponse.
- **Le coût** : que coûte une parcelle mal classée ?
- **La règle métier** : que fait le service sans modèle ? Mesurez-la. C'est votre plancher.
- **Le protocole de validation** : comment vous découpez `train.csv`. Les sept espèces, dont une à
  0,47 %, rendent ce choix décisif.

### Bloc 2 — avant la séance 9 · *le pipeline*

- Le pipeline complet, exécutable d'un bout à l'autre, produisant **les sept probabilités**, **dans la
  contrainte de temps**.
- **Au moins huit essais**, dont ceux qui ont échoué.
- **La question de ce sujet** : que se passe-t-il si vous rendez seulement l'espèce gagnante ?
  Mesurez-le.
- **Le rappel par espèce** : la moyenne cache les classes rares. Montrez-le.

### Bloc 3 — avant la séance 13 · *le dépôt et la robustesse*

- Le dépôt propre, la note de 3 pages, la model card, la fiche de données.
- **Trois mutations nommées**, chacune applicable en **une cellule**.
- Le test de robustesse, et le classement final des 116 203 parcelles.

## Comment vous êtes évalués

Voir la grille du cours. Trois points pèsent particulièrement ici :

- **la sortie probabiliste** : répondre une seule espèce n'est pas ce qui est demandé ;
- **le chiffre contre le plancher** : votre score se rapporte à une règle sans modèle, mesurée ;
- **la vérité du journal** : le journal consigne les essais, y compris ceux qui ont échoué.

## Deux règles du cours

**Pas de fuite.** Une information inconnue **au moment de la cartographie** ne peut pas entrer dans le
modèle.

**Le code doit rester réparable.** On doit pouvoir passer d'une version à l'autre en changeant une
cellule, pas en réécrivant le pipeline.
