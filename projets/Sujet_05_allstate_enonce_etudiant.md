# Projet — Estimer le montant d'un sinistre

**Équipe de 3 · 6 semaines · rendu par dépôt**

---

## Le contexte

Un assureur reçoit la déclaration d'un sinistre. Il doit **estimer le montant** qu'il devra payer :
c'est ce montant qui alimente la provision du dossier et le tarif du risque.

L'estimation se fait à partir du dossier seul — les 130 variables qui le décrivent. Le montant, lui,
n'est connu qu'après règlement.

## La question à laquelle le projet répond

> Peut-on estimer le montant d'un sinistre à partir de son dossier — et de combien une méthode
> soignée bat-elle l'estimation que l'assureur applique aujourd'hui sans modèle ?

## Ce que vous recevez

| Fichier | Contenu |
|---|---|
| `train.csv` | **150 654 sinistres** dont le montant est connu — 132 colonnes, dont `loss` |
| `a_estimer.csv` | **37 664 sinistres** — les 131 mêmes colonnes, **sans `loss`** |

Les variables sont de deux sortes : **116 catégorielles** (`cat1`…`cat116`), de 2 à 326 modalités, et
**14 continues** (`cont1`…`cont14`). Toutes les colonnes sont anonymisées : il n'y a pas de sens
métier à retrouver, seulement des régularités à mesurer.

**Il n'y a aucune valeur manquante dans ce fichier.** C'est rare, et cela ne vous dispense de rien.

Aucune donnée ne permet d'identifier une personne : une ligne est un sinistre, et l'identifiant `id`
est un numéro interne.


## Ce que contient chaque colonne

| Famille | Nombre | Type | Ce que c'est |
|---|---|---|---|
| `id` | 1 | entier | identifiant du sinistre |
| `cat1` … `cat116` | **116** | **texte** (lettres `A`, `B`, `C`…) | variables catégorielles **anonymisées**, de **2 à 326 modalités** |
| `cont1` … `cont14` | **14** | décimal | variables continues **anonymisées** |
| `loss` | 1 | décimal | **la cible** : montant du sinistre, de 0,67 à 121 012 |

**Ce que l'on sait, et ce que l'on ne sait pas.** Aucune de ces variables n'a de signification publiée :
`cat57` n'est pas « le type de véhicule », c'est `cat57`. Vous ne pouvez donc pas raisonner par la
connaissance du métier — **vous devez mesurer ce que chaque variable apporte**, et c'est l'exercice.

**Ce qui est mesuré sur les données livrées :** aucune valeur manquante ; les 116 catégorielles ont de
2 à 326 modalités ; les 14 continues s'étendent de 0 à 587 602.


**Ce que Kaggle documente, et ce qu'il ne documente pas.** La page *Data* de cette compétition ne
publie **aucune description par colonne** : les variables sont anonymisées et la compétition ne donne
que leurs familles. **Les lignes ci-dessus sont donc tout ce qui est documenté** — vous ne trouverez
pas, en ligne, à quoi correspond une variable précise. C'est une contrainte du sujet, et elle est
volontaire : votre travail est de **mesurer** ce que chaque variable apporte, pas de l'interpréter.

## Ce que vous rendez

**Un dépôt de code**, pas un notebook :

1. un **pipeline exécutable** qui prend `a_estimer.csv` et produit un fichier `id,loss` ;
2. le **protocole de validation** retenu, justifié ;
3. les **essais**, y compris ceux qui ont échoué, avec leur score ;
4. une **note de 3 pages** : décision, métrique, méthode, résultat chiffré, ce qui a échoué ;
5. une **model card** ;
6. la **fiche de données** du S13.

Le pipeline doit tourner **en moins de 10 minutes sur CPU**, sans GPU, avec `scikit-learn` seul.

## Les trois temps du projet

### Bloc 1 — avant la séance 5 · *la décision et la mesure*

- **La métrique** : laquelle vous adoptez, et **ce qu'elle implique**. Une métrique ne dit pas
  seulement comment vous serez jugés : elle détermine ce qu'est une bonne réponse.
- **La constante de référence** : si l'on n'a aucun modèle, quelle valeur unique faut-il annoncer ?
  La réponse dépend de la métrique, et elle doit être **mesurée**, pas supposée.
- **Le coût** : que coûte une sous-estimation, que coûte une surestimation ? Sont-elles symétriques ?
- **Le protocole de validation** : comment vous découpez `train.csv` pour estimer honnêtement votre
  performance sur `a_estimer.csv` — et pourquoi ce découpage-là. La cible est un **montant**, pas une
  classe : cela change la façon de découper.

### Bloc 2 — avant la séance 9 · *le pipeline*

- Le pipeline complet, exécutable d'un bout à l'autre.
- **Au moins huit essais**, dont ceux qui ont échoué, chacun avec son score.
- **L'encodage des 116 variables catégorielles** : ce que vous faites, et pourquoi. Certaines ont
  plus de 300 modalités ; l'encodage le plus évident n'est pas toujours tenable.
- L'**analyse d'erreurs** : sur quels sinistres votre modèle se trompe, et de combien.

### Bloc 3 — avant la séance 13 · *le dépôt et la robustesse*

- Le dépôt propre, la note de 3 pages, la model card, la fiche de données.
- **Trois mutations nommées**, chacune applicable en **une cellule**.
- Le test de robustesse, et l'estimation finale des 37 664 sinistres.

## Comment vous êtes évalués

Voir la grille du cours. Trois points pèsent particulièrement ici :

- **la constante de référence** : votre score se rapporte à une estimation sans modèle, mesurée, pas
  à zéro ;
- **la cohérence entre la métrique et la décision** : un modèle excellent mesuré par la mauvaise
  métrique est un mauvais modèle ;
- **la vérité du journal** : le journal consigne les essais, y compris ceux qui ont échoué.

## Deux règles du cours

**Pas de fuite.** Une information qui ne serait pas connue **au moment où le dossier est instruit** ne
peut pas entrer dans le modèle.

**Le code doit rester réparable.** On doit pouvoir passer d'une version à l'autre en changeant une
cellule, pas en réécrivant le pipeline.
