# Projet — Détecter la transaction d'un client

**Équipe de 3 · 6 semaines · rendu par dépôt**

---

## Le contexte

Une banque observe ses clients et veut savoir **lesquels réaliseront une transaction** dans une
fenêtre donnée : pour cibler une offre, et pour dimensionner son dispositif.

## La question à laquelle le projet répond

> Peut-on repérer, dans les 200 variables d'un client, ceux qui vont réaliser une transaction — et de
> combien une méthode soignée bat-elle le tri que la banque applique aujourd'hui sans modèle ?

## Ce que vous recevez

| Fichier | Contenu |
|---|---|
| `train.csv` | **160 000 clients** dont l'issue est connue — 202 colonnes, dont `target` |
| `a_decider.csv` | **40 000 clients** — les 201 mêmes colonnes, **sans `target`** |

**10,05 % des clients de `train.csv` ont réalisé la transaction** (20 098 sur 200 000 au total).

**Trois avertissements :**

- **les 200 variables sont continues et anonymisées** : `var_0` … `var_199`, sans aucun nom parlant.
  Il n'y a pas de sens métier à retrouver, il y a des régularités à mesurer ;
- **aucune valeur ne manque** et il n'y a aucune variable catégorielle : votre travail se joue donc
  sur **ce que vous faites de ces nombres**, et sur **lesquels vous gardez** ;
- **la métrique d'évaluation est l'aire sous la courbe ROC (AUC)**.

Aucune donnée ne permet d'identifier une personne : `ID_code` est un numéro interne.


## Ce que contient chaque colonne

| Colonne | Type | Ce que c'est |
|---|---|---|
| `ID_code` | texte | identifiant du client |
| `var_0` … `var_199` | **décimal** | **200 variables continues anonymisées** |
| `target` | 0/1 | **la cible** : 1 = transaction réalisée |

**Ce qui est mesuré sur les données livrées, et qui doit orienter votre travail :**

- les 200 variables s'étendent de **−73,8 à +65,4** : ce sont des valeurs **centrées**, pas des
  montants, et **124 d'entre elles prennent des valeurs négatives** ;
- **aucune valeur manquante** ;
- **aucun nom parlant** : il n'existe aucune description publiée de `var_57`. Vous ne pouvez pas
  raisonner par le métier — vous devez **mesurer** ce que chaque variable apporte, seule puis à
  plusieurs.


**Ce que Kaggle documente, et ce qu'il ne documente pas.** La page *Data* de cette compétition ne
publie **aucune description par colonne** : les variables sont anonymisées et la compétition ne donne
que leurs familles. **Les lignes ci-dessus sont donc tout ce qui est documenté** — vous ne trouverez
pas, en ligne, à quoi correspond une variable précise. C'est une contrainte du sujet, et elle est
volontaire : votre travail est de **mesurer** ce que chaque variable apporte, pas de l'interpréter.

## Ce que vous rendez

**Un dépôt de code**, pas un notebook :

1. un **pipeline exécutable** qui prend `a_decider.csv` et produit `ID_code,target` — une **décision** ;
2. le **protocole de validation** retenu, justifié ;
3. les **essais**, y compris ceux qui ont échoué, avec leur score ;
4. une **note de 3 pages** : décision, métrique et seuil, méthode, résultat chiffré, ce qui a échoué ;
5. une **model card** ;
6. la **fiche de données** du S13.

Le pipeline doit tourner **en moins de 10 minutes sur CPU**, sans GPU, avec `scikit-learn` seul.

## Les trois temps du projet

### Bloc 1 — avant la séance 5 · *la décision et la mesure*

- **La métrique** : ce qu'elle récompense, ce qu'elle ignore.
- **Le coût** : une vente manquée contre une sollicitation inutile. Chiffrez les deux.
- **La règle métier** : comment la banque trie-t-elle aujourd'hui, sans modèle ? Mesurez-la. C'est
  votre plancher.
- **Le protocole de validation** : comment vous découpez `train.csv`, et pourquoi.

### Bloc 2 — avant la séance 9 · *le pipeline*

- Le pipeline complet, exécutable d'un bout à l'autre.
- **Au moins huit essais**, dont ceux qui ont échoué, chacun avec son score.
- **La question de ce sujet** : que vaut **chaque variable prise seule** ? Et que vaut le modèle
  quand on n'en garde qu'une, cinq, dix, cinquante, cent ? **Tracez cette courbe.** C'est elle qui
  décidera de votre sélection — et elle vous apprendra quelque chose sur la table que les autres
  sujets ne vous apprendraient pas.
- **L'analyse d'erreurs** : sur quels clients vous vous trompez.

### Bloc 3 — avant la séance 13 · *le dépôt et la robustesse*

- Le dépôt propre, la note de 3 pages, la model card, la fiche de données.
- **Trois mutations nommées**, chacune applicable en **une cellule**.
- Le test de robustesse, et la décision finale sur les 40 000 clients.

## Comment vous êtes évalués

Voir la grille du cours. Trois points pèsent particulièrement ici :

- **la justification de la sélection de variables** : garder les 200 « au cas où » n'est pas un choix,
  c'est une absence de choix — et elle se mesure, en temps comme en qualité ;
- **le chiffre contre le plancher** : votre score se rapporte à une règle sans modèle, mesurée ;
- **la vérité du journal** : le journal consigne les essais, y compris ceux qui ont échoué.

## Deux règles du cours

**Pas de fuite.** Une information inconnue **au moment où la décision est prise** ne peut pas entrer
dans le modèle.

**Le code doit rester réparable.** On doit pouvoir passer d'une version à l'autre en changeant une
cellule, pas en réécrivant le pipeline.
