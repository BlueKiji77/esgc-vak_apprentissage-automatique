# Projet — Prévoir les ventes par article, seize jours à l'avance

**Équipe de 3 · 6 semaines · rendu par dépôt**

---

## Le contexte

Une chaîne de 54 magasins vend des milliers de références, rangées en familles et en classes. Chaque
jour, chaque magasin enregistre **ce qu'il a vendu, article par article**.

Le responsable des réassortiments doit décider, **lundi 31 juillet 2017 au matin**, des commandes
pour les **seize jours à venir** — jusqu'au 15 août inclus — **article par article**.

## La question à laquelle le projet répond

> Peut-on prévoir les ventes d'un article dans un magasin, jour par jour, seize jours à l'avance —
> et de combien une méthode soignée bat-elle la règle que le responsable applique aujourd'hui ?

## Ce que vous recevez

| Fichier | Contenu |
|---|---|
| `train.csv` | **12 248 320 lignes** — `id, date, store_nbr, item_nbr, unit_sales, onpromotion`, du 2013-01-01 au **2017-07-30** |
| `horizon.csv` | **277 568 lignes** — `id, date, store_nbr, item_nbr, onpromotion`, du **2017-07-31 au 2017-08-15** — **la cible est absente** |
| `items.csv` | les articles — `family`, `class`, `perishable` |
| `stores.csv` | 54 magasins — ville, État, type, cluster |
| `holidays_events.csv` | les fériés |
| `oil.csv` | le prix du pétrole, jour par jour |
| `transactions.csv` | le nombre de transactions de chaque magasin, jour par jour |

**Lisez attentivement ces deux points avant de commencer.**

`unit_sales` est le nombre d'unités vendues. Il est **entier, et parfois négatif** — un retour.

Et `horizon.csv` n'est pas la suite naturelle de `train.csv` : c'est une **grille** — tous les
couples (magasin, article) de vos données, pour chacun des 16 jours. À vous de décider ce que vaut
la cible dans les cases que le fichier de ventes ne renseigne pas.


## Ce que contient chaque colonne

> **Texte officiel de la compétition**, recopié de la page *Data* de Kaggle.

**Description du jeu :** « In this competition, you will be predicting the unit sales for thousands of
items sold at different Favorita stores located in Ecuador. The training data includes dates, store
and item information, whether that item was being promoted, as well as the unit sales. »

### `train.csv`
- Training data, which includes the target `unit_sales` by `date`, `store_nbr`, and `item_nbr` and a
  unique `id` to label rows.
- The target `unit_sales` can be integer (e.g., a bag of chips) or float (e.g., 1.5 kg of cheese).
- **Negative values of `unit_sales` represent returns** of that particular item.
- The `onpromotion` column tells whether that `item_nbr` was on promotion for a specified `date` and
  `store_nbr`.
- **Approximately 16 % of the `onpromotion` values in this file are `NaN`.**
- **NOTE:** « The training data does not include rows for items that had zero `unit_sales` for a
  store/date combination. There is no information as to whether or not the item was in stock for the
  store on the date, and teams will need to decide the best way to handle that situation. »

### `stores.csv`
- Store metadata, including `city`, `state`, `type`, and `cluster`.
- `cluster` is a grouping of similar stores.

### `items.csv`
- Item metadata, including `family`, `class`, and `perishable`.
- **NOTE:** Items marked as `perishable` have a score weight of `1.25`; otherwise, the weight is `1.0`.

### `transactions.csv`
- The count of sales transactions for each `date`, `store_nbr` combination. **Only included for the
  training data timeframe.**

### `oil.csv`
- Daily oil price. Includes values during both the train and test data timeframe. (Ecuador is an
  oil-dependent country and its economical health is highly vulnerable to shocks in oil prices.)

### `holidays_events.csv`
- Holidays and Events, with metadata.
- **NOTE:** « Pay special attention to the `transferred` column. A holiday that is `transferred`
  officially falls on that calendar day, but was moved to another date by the government. A
  `transferred` day is more like a normal day than a holiday. » To find the day it was actually
  celebrated, look for the corresponding row where `type` is `Transfer` — for example, the holiday
  *Independencia de Guayaquil* was transferred from 2012-10-09 to 2012-10-12. Days of type `Bridge`
  are extra days added to a holiday; they are frequently made up by the type `Work Day`, a day not
  normally scheduled for work that pays the Bridge back. `Additional` holidays are days added to a
  regular calendar holiday, as typically happens around Christmas.

### Notes complémentaires, officielles
- « Wages in the public sector are paid every two weeks on the 15th and on the last day of the month.
  Supermarket sales could be affected by this. »
- « A magnitude 7.8 earthquake struck Ecuador on April 16, 2016. People rallied in relief efforts
  donating water and other first need products which greatly affected supermarket sales for several
  weeks after the earthquake. »

## Ce que vous rendez

**Un dépôt de code**, pas un notebook :

1. un **pipeline exécutable** qui prend `horizon.csv` et produit un fichier `id,unit_sales` ;
2. le **protocole de validation** retenu, justifié ;
3. les **essais**, y compris ceux qui ont échoué, avec leur score ;
4. une **note de 3 pages** : décision, métrique, méthode, résultat chiffré, ce qui a échoué ;
5. une **model card** ;
6. la **fiche de données** du S13.

Le pipeline doit tourner **en moins de 10 minutes sur CPU**, sans GPU, avec `scikit-learn` seul.
**12,2 millions de lignes** : c'est le premier obstacle, et il est volontaire.

## Les trois temps du projet

### Bloc 1 — avant la séance 5 · *la décision et la mesure*

- **La métrique** : laquelle vous adoptez. Dites précisément **ce qu'elle fait des retours** et
  **ce qu'elle fait des cases sans vente**.
- **Le coût** : chiffrez une unité sous-prévue contre une unité sur-prévue. Les deux ne coûtent pas
  la même chose, et certains articles sont périssables.
- **La règle métier** : quelle règle simple le responsable applique-t-il sans modèle ? Mesurez-la.
  C'est votre plancher.
- **La décision sur l'absence** : une case sans ligne de vente — qu'est-ce que c'est ? Écrivez la
  réponse et assumez-la jusqu'au bout, dans l'entraînement comme dans la prévision.

### Bloc 2 — avant la séance 9 · *le pipeline*

- Le pipeline complet, exécutable d'un bout à l'autre, sous la contrainte de temps.
- **Au moins huit essais**, dont ceux qui ont échoué, chacun avec son score.
- L'**analyse d'erreurs** : sur quelles familles, quels magasins, quels jours.
- **La question qui compte** : parmi vos colonnes, lesquelles portent encore de l'information **sur
  les seize jours à prévoir** ? Une colonne peut exister et n'être renseignée que dans le passé.

### Bloc 3 — avant la séance 13 · *le dépôt et la robustesse*

- Le dépôt propre, la note de 3 pages, la model card, la fiche de données.
- **Trois mutations nommées**, applicables en **une cellule** chacune.
- Le test de robustesse, et la prévision finale sur `horizon.csv`.

## Comment vous êtes évalués

Voir la grille du cours. Sur ce sujet, trois points pèsent particulièrement :

- **la décision sur l'absence** : c'est la décision centrale. Une équipe qui ne la prend pas
  implicitement — celle qui s'entraîne sur les seules lignes livrées — prédit une vente partout ;
- **le chiffre contre le plancher** : votre score se rapporte à la règle métier, pas à zéro ;
- **la vérité du journal** : le journal consigne les essais, y compris ceux qui ont échoué.

## Deux règles du cours

**Pas de fuite.** Une colonne qui n'existerait pas au moment de décider ne peut pas entrer dans le
modèle. Si votre score est trop beau, c'est la première chose à vérifier.

**Le code doit rester réparable.** On doit pouvoir passer d'une version à l'autre en changeant une
cellule, pas en réécrivant le pipeline.
