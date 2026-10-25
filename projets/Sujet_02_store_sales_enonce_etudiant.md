# Projet — Prévoir les ventes, seize jours à l'avance

**Équipe de 3 · 6 semaines · rendu par dépôt**

---

## Le contexte

Un réseau de 54 magasins vend des produits répartis en 33 familles. Chaque jour, chaque magasin
enregistre ce qu'il a vendu, par famille.

Le responsable des réassortiments doit décider, **lundi 31 juillet 2017 au matin**, des commandes et
des plannings pour les **seize jours à venir** — jusqu'au 15 août inclus. Il nous demande une
prévision par magasin et par famille.

Ce qu'il sait ce matin-là : le calendrier, les promotions annoncées, les prix du pétrole publiés.
Ce qu'il ne sait pas : combien de clients viendront.

## La question à laquelle le projet répond

> Peut-on prévoir les ventes d'une famille dans un magasin, jour par jour, seize jours à l'avance —
> et de combien une méthode soignée bat-elle la règle que le responsable applique aujourd'hui ?

## Ce que vous recevez

| Fichier | Contenu |
|---|---|
| `train.csv` | 2 972 376 lignes — `id, date, store_nbr, family, sales, onpromotion`, du 2013-01-01 au **2017-07-30** |
| `horizon.csv` | 28 512 lignes — `id, date, store_nbr, family, onpromotion`, du **2017-07-31 au 2017-08-15** — **la cible est absente** |
| `transactions.csv` | `date, store_nbr, transactions` — du 2013-01-01 au **2017-07-30** |
| `stores.csv` | 54 magasins — ville, État, type, cluster |
| `holidays_events.csv` | fériés, avec la colonne `transferred` |
| `oil.csv` | prix du pétrole, jour par jour, jusqu'au 2017-08-31 |

`sales` est le nombre d'unités vendues dans une famille, dans un magasin, un jour. Il n'y a
**aucune donnée personnelle** : une ligne est un couple (magasin, famille, jour).


## Ce que contient chaque colonne

> **Texte officiel de la compétition**, recopié de la page *Data* de Kaggle.

**Description du jeu :** « In this competition, you will predict sales for the thousands of product
families sold at Favorita stores located in Ecuador. The training data includes dates, store and
product information, whether that item was being promoted, as well as the sales numbers. »

### `train.csv`
« The training data, comprising time series of features `store_nbr`, `family`, and `onpromotion` as
well as the target `sales`. »
- `store_nbr` identifies the store at which the products are sold.
- `family` identifies the type of product sold.
- `sales` gives the total sales for a product family at a particular store at a given date.
  **Fractional values are possible** since products can be sold in fractional units (1.5 kg of cheese,
  for instance, as opposed to 1 bag of chips).
- `onpromotion` gives the **total number of items** in a product family that were being promoted at a
  store at a given date.

### `test.csv`
« The test data, having the same features as the training data. You will predict the target `sales`
for the dates in this file. The dates in the test data are for the 15 days after the last date in the
training data. »

### `stores.csv`
« Store metadata, including `city`, `state`, `type`, and `cluster`. `cluster` is a grouping of similar
stores. »

### `oil.csv`
« Daily oil price. Includes values during both the train and test data timeframes. (Ecuador is an
oil-dependent country and it's economical health is highly vulnerable to shocks in oil prices.) »

### `transactions.csv`
« The count of sales transactions for each `date`, `store_nbr` combination. Only included for the
training data timeframe. »

### `holidays_events.csv`
« Holidays and Events, with metadata. **NOTE:** Pay special attention to the `transferred` column. A
holiday that is `transferred` officially falls on that calendar day, but was moved to another date by
the government. A `transferred` day is more like a normal day than a holiday. » To find the day it was
actually celebrated, look for the row where `type` is `Transfer` — for example, *Independencia de
Guayaquil* was transferred from 2012-10-09 to 2012-10-12. Days of type `Bridge` are extra days added
to a holiday; they are frequently made up by `Work Day`, a day not normally scheduled for work that
pays the Bridge back. `Additional` holidays are days added to a regular calendar holiday, as typically
happens around Christmas.

### Notes complémentaires, officielles
- « Wages in the public sector are paid every two weeks on the 15th and on the last day of the month.
  Supermarket sales could be affected by this. »
- « A magnitude 7.8 earthquake struck Ecuador on April 16, 2016. People rallied in relief efforts
  donating water and other first need products which greatly affected supermarket sales for several
  weeks after the earthquake. »

**Note du cours :** `horizon.csv` est notre construction. Il reprend les champs de `test.csv`
(`id`, `date`, `store_nbr`, `family`, `onpromotion`) et couvre **16 jours** — 2017-07-31 au
2017-08-15.

## Ce que vous rendez

**Un dépôt de code**, pas un notebook. Il contient :

1. un **pipeline exécutable** qui prend `horizon.csv` et produit un fichier `id,sales` ;
2. le **protocole de validation** que vous avez retenu, justifié ;
3. les **essais**, y compris ceux qui ont échoué, avec leur score ;
4. une **note de 3 pages** : décision, métrique et son coût, méthode, résultat chiffré, ce qui a
   échoué ;
5. une **model card** ;
6. la **fiche de données** du S13.

Le pipeline doit tourner **en moins de 10 minutes sur CPU**, sans GPU, avec `scikit-learn` seul.

## Travailler : récupérer les données, coder, sauvegarder

Les données du cours sont sur un **Drive partagé, en lecture seule**, un dossier par sujet. Vous
téléchargez **le dossier de votre sujet** — pas les autres — et vous choisissez ensuite **où coder**.

### 1. Récupérer votre dossier

| | |
|---|---|
| Dossier du sujet | **`Sujet_02_Store_Sales/`** |
| Poids attendu | **126,3 Mo** |
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
2. Déposez les fichiers du dossier `Sujet_02_Store_Sales/` (glisser-déposer), nommez le jeu de données
   `esgc-vak-sujet-02-…`, puis **Create**.
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

- **La métrique** : laquelle vous adoptez, et pourquoi. Une partie de vos ventes est à zéro — dites
  ce que votre métrique en fait.
- **Le coût** : chiffrez ce que coûte une unité sous-prévue et une unité sur-prévue. Les deux ne
  coûtent pas la même chose.
- **La règle métier** : quelle règle simple le responsable applique-t-il aujourd'hui, sans modèle ?
  Mesurez-la. C'est votre plancher.
- **Le protocole de validation** : comment vous découpez `train.csv` pour estimer honnêtement votre
  performance sur `horizon.csv`.

### Bloc 2 — avant la séance 9 · *le pipeline*

- Le pipeline complet, exécutable d'un bout à l'autre.
- **Au moins huit essais**, dont ceux qui ont échoué, chacun avec son score.
- L'**analyse d'erreurs** : où votre modèle se trompe, sur quelles familles, quels magasins, quels
  jours.
- **La question qui compte** : parmi vos colonnes, lesquelles seront **connues** le 31 juillet au
  matin ? Celles qui ne le seront pas n'ont pas leur place dans le modèle.

### Bloc 3 — avant la séance 13 · *le dépôt et la robustesse*

- Le dépôt propre, la note de 3 pages, la model card, la fiche de données.
- **Trois mutations nommées**, chacune applicable en **une cellule** : on doit pouvoir casser votre
  pipeline d'un seul changement, et vous devez pouvoir dire ce que ça fait au score.
- Le test de robustesse, et la prévision finale sur `horizon.csv`.

## Comment vous êtes évalués

Voir la grille du cours. Trois points qui pèsent lourd :

- **la justesse du découpage** : un tirage au hasard dans `train.csv` ne mesure pas ce que vous
  croyez mesurer ;
- **le chiffre contre le plancher** : votre score se rapporte à la règle métier, pas à zéro ;
- **la vérité du journal** : le journal consigne les essais, y compris ceux qui ont échoué. Un
  journal qui ne raconte que les succès est un journal faux.

## Deux règles du cours

**Pas de fuite.** Une colonne qui n'existerait pas au moment de décider ne peut pas entrer dans le
modèle. Si votre pipeline produit un score trop beau, c'est la première chose à vérifier.

**Le code doit rester réparable.** C'est la raison des trois mutations : on doit pouvoir passer d'une
version à l'autre en changeant une cellule, pas en réécrivant le pipeline.
