# Projet — Évaluer le risque d'un conducteur

**Équipe de 3 · 6 semaines · rendu par dépôt**

---

## Le contexte

Un assureur automobile doit fixer la prime de chaque conducteur. Mal évaluer coûte cher dans les deux
sens : un conducteur à risque tarifé comme les autres fait perdre de l'argent à chaque sinistre, et un
bon conducteur surtaxé va chez le concurrent.

## La question à laquelle le projet répond

> Peut-on estimer, à partir du dossier d'un conducteur, la probabilité qu'il provoque un sinistre — et
> de combien une méthode soignée bat-elle le tarif actuel, qui ne distingue presque personne ?

## Ce que vous recevez

| Fichier | Contenu |
|---|---|
| `train.csv` | **476 169 conducteurs** dont l'issue est connue — 59 colonnes, dont `target` |
| `a_decider.csv` | **119 043 conducteurs** — les 58 mêmes colonnes, **sans `target`** |

**3,64 % des conducteurs de `train.csv` ont déclaré un sinistre** (21 694 sur 595 212 au total).

**Trois avertissements :**

- **les variables sont anonymisées** et rangées en familles : `ps_ind_*` (indicatrices), `ps_reg_*`
  (régulières), `ps_car_*` (voiture) et **20 `ps_calc_*`** (colonnes calculées) ;
- **les modalités absentes sont codées `-1`**, pas laissées vides. C'est une convention à reconnaître :
  traiter ces `-1` comme des nombres est un choix, et il se défend ou se contredit ;
- **la métrique d'évaluation est le Gini normalisé.** À vous de dire **ce qu'elle mesure**, comment
  elle se rapporte aux métriques que vous connaissez, et ce que signifie un écart de 0,01.


## Ce que contient chaque colonne

| Famille | Nombre | Ce que c'est |
|---|---|---|
| `id` | 1 | identifiant du conducteur |
| `ps_ind_*` | **18** | **indicatrices** : caractéristiques déclaratives du conducteur et du contrat |
| `ps_reg_*` | **3** | **variables régulières** : valeurs continues décrivant le véhicule ou le contrat |
| `ps_car_*` | **16** | **variables de la voiture** : certaines catégorielles (`_cat`), certaines continues |
| `ps_calc_*` | **20** | **variables calculées** — elles n'apportent aucune information nouvelle, et c'est à vous de le vérifier |
| `target` | 1 | **la cible** : 1 = sinistre déclaré |

**La convention de valeur manquante, et elle est décisive** : les modalités absentes sont codées **`-1`**,
jamais laissées vides. **8 colonnes** en contiennent (les plus fréquentes : `ps_car_03_cat` et
`ps_car_05_cat`, avec des centaines de `-1`).

Traiter ces `-1` comme des nombres est un **choix** : il se défend, ou il se contredit. Dites lequel
vous faites, et pourquoi.


**Ce que Kaggle documente, et ce qu'il ne documente pas.** La page *Data* de cette compétition ne
publie **aucune description par colonne** : les variables sont anonymisées et la compétition ne donne
que leurs familles. **Les lignes ci-dessus sont donc tout ce qui est documenté** — vous ne trouverez
pas, en ligne, à quoi correspond une variable précise. C'est une contrainte du sujet, et elle est
volontaire : votre travail est de **mesurer** ce que chaque variable apporte, pas de l'interpréter.

## Ce que vous rendez

**Un dépôt de code**, pas un notebook :

1. un **pipeline exécutable** qui prend `a_decider.csv` et produit `id,target` — une **décision** ;
2. le **protocole de validation** retenu, justifié ;
3. les **essais**, y compris ceux qui ont échoué, avec leur score ;
4. une **note de 3 pages** : décision, métrique et seuil, méthode, résultat chiffré, ce qui a échoué ;
5. une **model card** ;
6. la **fiche de données** du S13.

Le pipeline doit tourner **en moins de 10 minutes sur CPU**, sans GPU, avec `scikit-learn` seul.

## Travailler : récupérer les données, coder, sauvegarder

Les données du cours sont sur un **Drive partagé, en lecture seule**, un dossier par sujet. Vous
téléchargez **le dossier de votre sujet** — pas les autres — et vous choisissez ensuite **où coder**.

### 1. Récupérer votre dossier

| | |
|---|---|
| Dossier du sujet | **`Sujet_06_Porto_Seguro/`** |
| Poids attendu | **113,9 Mo** |
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
2. Déposez les fichiers du dossier `Sujet_06_Porto_Seguro/` (glisser-déposer), nommez le jeu de données
   `esgc-vak-sujet-06-…`, puis **Create**.
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

- **La métrique** : ce qu'elle mesure, ce qu'elle ignore, et **comment elle se traduit** dans les
  métriques que vous connaissez.
- **Le coût** : un sinistre non anticipé contre une prime trop élevée. Chiffrez les deux.
- **La règle métier** : comment l'assureur tarife-t-il aujourd'hui, sans modèle ? Mesurez-la.
- **Le protocole de validation** : comment vous découpez `train.csv`, et pourquoi.

### Bloc 2 — avant la séance 9 · *le pipeline*

- Le pipeline complet, exécutable d'un bout à l'autre.
- **Au moins huit essais**, dont ceux qui ont échoué, chacun avec son score.
- **Le sort des 20 colonnes `ps_calc_*`** : décidez, et **mesurez** ce que votre décision coûte — en
  qualité comme en temps de calcul.
- **L'analyse d'erreurs** : sur quels conducteurs vous vous trompez.

### Bloc 3 — avant la séance 13 · *le dépôt et la robustesse*

- Le dépôt propre, la note de 3 pages, la model card, la fiche de données.
- **Trois mutations nommées**, chacune applicable en **une cellule**.
- Le test de robustesse, et la décision finale sur les 119 043 conducteurs.

## Comment vous êtes évalués

Voir la grille du cours. Trois points pèsent particulièrement ici :

- **la compréhension de la métrique** : annoncer un chiffre sans savoir ce qu'il vaut n'est pas un
  résultat ;
- **le chiffre contre le plancher** : votre score se rapporte à une règle sans modèle, mesurée ;
- **la vérité du journal** : le journal consigne les essais, y compris ceux qui ont échoué.

## Deux règles du cours

**Pas de fuite.** Une information inconnue **au moment où le contrat est souscrit** ne peut pas entrer
dans le modèle.

**Le code doit rester réparable.** On doit pouvoir passer d'une version à l'autre en changeant une
cellule, pas en réécrivant le pipeline.
