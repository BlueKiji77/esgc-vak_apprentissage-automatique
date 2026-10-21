# Projet — Estimer le revenu d'un visiteur d'un site marchand

**Équipe de 3 · 6 semaines · rendu par dépôt**

---

## Le contexte

Un site marchand veut savoir, **pour chaque visiteur**, combien il va rapporter. C'est ce chiffre qui
décide où placer le budget d'acquisition, et quels visiteurs relancer.

## La question à laquelle le projet répond

> Peut-on estimer le revenu d'un visiteur à partir de son comportement de navigation — et de combien
> une méthode soignée bat-elle l'estimation actuelle, qui traite tous les visiteurs de la même façon ?

## Ce que vous recevez

| Fichier | Contenu |
|---|---|
| `train.csv` | **571 333 visiteurs** dont le revenu est connu — 17 colonnes, dont `revenu` |
| `a_estimer.csv` | **142 834 visiteurs** — les 16 mêmes colonnes, **sans `revenu`** |

**1,40 % des visiteurs seulement ont généré un revenu.** Les autres n'ont rien acheté — et ce n'est
pas une anomalie : c'est la réalité d'un site marchand.

## Ce que contient chaque colonne

> **Texte officiel de la compétition**, recopié de la page *Data* de Kaggle, puis complété par ce que
> la source contient réellement.

Trois avertissements officiels, en majuscules dans le texte d'origine :

- « Each row in the dataset is **one visit** to the store. » — la source est donc à la **session**,
  et la cible est **par visiteur** : l'agrégation est à votre charge ;
- « There are multiple columns which contain **JSON blobs of varying depth**. In one of those JSON
  columns, `totals`, the sub-column `transactionRevenue` contains the revenue information we are
  trying to predict. **This sub-column exists only for the training data.** »
- « **IMPORTANT:** Due to the formatting of `fullVisitorId` you must load the Id's as **strings** in
  order for all Id's to be properly unique! »

Et la définition de la cible : `y_user = Σ transaction_i`, puis `target_user = ln(y_user + 1)`.

**Les colonnes que vous recevez** (elles résultent de l'aplatissement des quatre colonnes JSON
d'origine : `device`, `geoNetwork`, `totals`, `trafficSource`) :

| Colonne | Type | Ce que c'est |
|---|---|---|
| `visiteur` | texte | `fullVisitorId` — **à lire comme une chaîne**, jamais comme un nombre |
| `sessions` | entier | nombre de visites du visiteur |
| `visites` | entier | total de `totals.visits` |
| `hits` | entier | total de `totals.hits` |
| `pages` | entier | total de `totals.pageviews` |
| `transactions` | entier | total de `totals.transactions` |
| `premiere_date` | entier | première date de visite, au format `AAAAMMJJ` |
| `derniere_date` | entier | dernière date de visite, au format `AAAAMMJJ` |
| `navigateur` | texte | `device.browser` |
| `systeme` | texte | `device.operatingSystem` |
| `categorie` | texte | `device.deviceCategory` — mobile, tablette, ordinateur |
| `mobile` | texte | `device.isMobile` |
| `pays` | texte | `geoNetwork.country` |
| `canal` | texte | `channelGrouping` — le canal d'acquisition |
| `medium` | texte | `trafficSource.medium` |
| `a_achete` | 0/1 | **indicateur calculé** : le visiteur a-t-il un revenu ? |

**Le revenu est exprimé en micro-unités** (comme dans Google Analytics) : un revenu de `48 360 000`
vaut `48,36`.

## Ce que vous rendez

Un **dépôt de code** :

1. un **pipeline exécutable** qui prend `a_estimer.csv` et produit `visiteur,revenu` ;
2. le **protocole de validation** retenu, justifié ;
3. les **essais**, y compris ceux qui ont échoué, avec leur score ;
4. une **note de 3 pages** : décision, métrique, méthode, résultat chiffré, ce qui a échoué ;
5. une **model card** ;
6. la **fiche de données** du S13.

Le pipeline doit tourner **en moins de 10 minutes sur CPU**, sans GPU, avec `scikit-learn` seul.

## Les trois temps du projet

### Bloc 1 — avant la séance 5 · *la décision et la mesure*

- **La métrique** : laquelle, et **sur quelle échelle**. Le revenu va de zéro à plusieurs millions —
  réfléchissez à ce que cela implique avant de mesurer quoi que ce soit.
- **Le coût** : un acheteur manqué contre un visiteur relancé pour rien.
- **La règle métier** : que fait le site sans modèle ? Mesurez-la. C'est votre plancher.
- **Le protocole de validation** : comment vous découpez `train.csv`, sachant que 1,40 % seulement
  des visiteurs ont acheté.

### Bloc 2 — avant la séance 9 · *le pipeline*

- Le pipeline complet, exécutable d'un bout à l'autre.
- **Au moins huit essais**, dont ceux qui ont échoué, chacun avec son score.
- **Les variables** : d'où viennent-elles ? Que contenaient les colonnes JSON d'origine, et que
  perdez-vous en les aplatisant ?
- **L'analyse d'erreurs** : sur quels visiteurs votre modèle se trompe, et de combien.

### Bloc 3 — avant la séance 13 · *le dépôt et la robustesse*

- Le dépôt propre, la note de 3 pages, la model card, la fiche de données.
- **Trois mutations nommées**, chacune applicable en **une cellule**.
- Le test de robustesse, et l'estimation finale des 142 834 visiteurs.

## Comment vous êtes évalués

Voir la grille du cours. Trois points pèsent particulièrement ici :

- **l'échelle de la cible** : une erreur d'échelle rend toute comparaison fausse, et rien ne plante ;
- **le chiffre contre le plancher** : votre score se rapporte à une règle sans modèle, mesurée ;
- **la vérité du journal** : le journal consigne les essais, y compris ceux qui ont échoué.

## Deux règles du cours

**Pas de fuite.** Une information inconnue **au moment de l'estimation** ne peut pas entrer dans le
modèle.

**Le code doit rester réparable.** On doit pouvoir passer d'une version à l'autre en changeant une
cellule, pas en réécrivant le pipeline.
