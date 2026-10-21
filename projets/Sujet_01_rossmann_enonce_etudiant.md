# Énoncé du projet — Sujet 01 · Prévoir les ventes d'un magasin pour planifier les équipes

> **Document étudiant.** Ce que vous savez, ce qu'on vous donne, ce qu'on attend — et rien de plus.

## Le problème

Une chaîne de distributeurs gère **plus d'un millier de magasins**. Chaque responsable décide,
chaque lundi, des **effectifs des six semaines à venir** de son magasin. Cette décision dépend
d'une prévision : *que vendra mon magasin demain, et la semaine prochaine ?*

Trois faits du métier fondent le problème :

- un magasin **fermé** ne vend rien — et la règle qui aplatit les ventes les jours de fermeture vaut
  partout ;
- un **jour férié** ou un événement scolaire change l'affluence du jour ;
- une **promotion** est une décision du magasin : elle gonfle la vente *parce que* le responsable
  l'a choisie — elle n'arrive pas par hasard.

Se tromper coûte de deux façons, et **pas la même somme** : des heures payées à vide quand on
surestime les ventes, des ventes perdues et des files d'attente quand on les sous-estime.

## La décision à servir

Le responsable doit savoir, le **lundi**, quels effectifs affecter aux **six semaines à venir**.
Il décide avec les informations disponibles **à ce moment-là** — jamais avec celles du jour à
prévoir. Votre modèle produit une prévision de ventes **par magasin et par jour**, qui est la
matière de cette décision.

## Ce que vous recevez

Une ligne = **un couple (magasin, jour)**.

| Fichier | Contenu | Lignes |
|---|---|---|
| `train.csv` | historique **2013-01-01 → 2015-06-13**, avec la cible `Sales` | 963 689 |
| `store.csv` | attributs des magasins (type, assortiment, concurrence, promotions longues) — jointure par `Store` | 1 115 |
| `horizon.csv` | les **48 jours à prévoir** (2015-06-14 → 2015-07-31) — **sans `Sales`** | 53 520 |

Le fichier `horizon.csv` ne contient **pas** la cible : vous prévoyez, vous ne recopiez pas. Votre
rendu final est `Id,Sales` pour chaque ligne de l'horizon — et c'est l'enseignant qui mesure.

## Ce qui est votre choix, pas une donnée

**La métrique.** Elle n'est pas imposée. Vous la **choisissez**, vous la **défendez** (2 arguments,
1 contre-argument), et vous dites ce qu'elle **masque**. Référence au-dessus de laquelle votre
prévision doit se placer : la **règle métier** — ce que le manager fait déjà sans modèle — qui est
débattue en séance.

**Le protocole de validation.** Vous ne pouvez pas mesurer sur l'horizon (vous n'avez pas ses
étiquettes). Vous construisez donc votre **propre découpage de validation**, à l'intérieur de ce
que vous avez — et vous savez déjà que le futur ne doit jamais entrer dans l'apprentissage.

**Le coût de l'erreur.** Combien coûte une heure de caisse vide, combien une vente perdue ? **Les
deux montants**, vous les établissez. Dites, en une phrase, combien coûtent un **faux positif** et
un **faux négatif** — c'est le cœur de la décision.


## Ce que contient chaque colonne

> **Texte officiel de la compétition**, recopié de la page *Data* de Kaggle.

**Description du jeu :** « You are provided with historical sales data for **1,115 Rossmann stores**.
The task is to forecast the "Sales" column for the test set. Note that some stores in the dataset were
temporarily closed for refurbishment. »

**Fichiers :** `train.csv` — historical data including Sales · `test.csv` — historical data excluding
Sales · `sample_submission.csv` — a sample submission file in the correct format · `store.csv` —
supplemental information about the stores.

**Data fields** — « Most of the fields are self-explanatory. The following are descriptions for those
that aren't. »

| Champ | Description officielle |
|---|---|
| `Id` | an Id that represents a (Store, Date) duple within the test set |
| `Store` | a unique Id for each store |
| `Sales` | the turnover for any given day (this is what you are predicting) |
| `Customers` | the number of customers on a given day |
| `Open` | an indicator for whether the store was open: 0 = closed, 1 = open |
| `StateHoliday` | indicates a state holiday. Normally all stores, with few exceptions, are closed on state holidays. Note that all schools are closed on public holidays and weekends. `a` = public holiday, `b` = Easter holiday, `c` = Christmas, `0` = None |
| `SchoolHoliday` | indicates if the (Store, Date) was affected by the closure of public schools |
| `StoreType` | differentiates between 4 different store models: `a`, `b`, `c`, `d` |
| `Assortment` | describes an assortment level: `a` = basic, `b` = extra, `c` = extended |
| `CompetitionDistance` | distance in meters to the nearest competitor store |
| `CompetitionOpenSince[Month/Year]` | gives the approximate year and month of the time the nearest competitor was opened |
| `Promo` | indicates whether a store is running a promo on that day |
| `Promo2` | Promo2 is a continuing and consecutive promotion for some stores: 0 = store is not participating, 1 = store is participating |
| `Promo2Since[Year/Week]` | describes the year and calendar week when the store started participating in Promo2 |
| `PromoInterval` | describes the consecutive intervals Promo2 is started, naming the months the promotion is started anew. E.g. "Feb,May,Aug,Nov" means each round starts in February, May, August, November of any given year for that store |

**Note du cours :** `horizon.csv` est notre construction et ne figure pas dans la compétition : il
reprend ces champs sans `Sales` ni `Customers`.

## Ce qu'on attend de votre code

- **Modifiable en une cellule.** Votre dépôt permet, en **une seule cellule**, d'appliquer une
  mutation nommée — par exemple *retirer une colonne*, *changer la métrique*, *introduire une
  nouvelle modalité*, *décaler une distribution*. Ce n'est pas une formalité : un pipeline qu'on
  modifie en une cellule est un pipeline qu'on **débogue** — et c'est sur ces mutations que l'oral
  et l'acte 2 vous prendront.
- **Reproductible.** `Save & Run All`, graine fixée, tourne en **≤ 10 min sur CPU**.
- **Journal de décisions** incluant les essais **échoués** — vos chiffres, vos mots, aucun texte
  produit par une IA pour cette partie.
- **Si votre table ne tient pas en mémoire**, un **échantillon exécutable** est joint au dépôt,
  avec les instructions pour la table complète.

## Les jalons

| Jalon | Séance | Ce que vous faites |
|---|---|---|
| **B1** | S5 | la **métrique choisie et défendue** · la **baseline** (la règle métier) · le plan d'équipe et les rôles |
| **B2** | S9 | le pipeline · **≥ 8 essais documentés, dont les échoués** · la validation sous votre protocole · l'analyse des erreurs |
| **B3** | S13/S14 | le **dépôt** · la note technique de **3 pages** · la **model card** · les trois mutations · le test de robustesse · la prévision de l'horizon · la soutenance |

L'**oral individuel** (après la S14, en présentiel) porte sur votre projet : les trois blocs, et
l'épreuve de modification en direct.

## Les contraintes du contrat

- Équipes de **1 à 3** ; rôles nommés ; **un dépôt Git commun** — son historique est ce qui montre
  qui a écrit quoi.
- Donnée d'entreprise **réelle, anonymisée** : attribution obligatoire (licence ODbL-1.0) dans la
  fiche de données.
- Déclaration d'usage de l'IA sur les livrables concernés (feu 🟠/🟢 selon la section 8 du wiki).

## Rien d'autre

Vous n'avez pas les étiquettes de l'horizon — et vous n'avez pas de réponse toute faite. Ce que
vous avez est ci-dessus. **Le reste, vous le mesurez.**