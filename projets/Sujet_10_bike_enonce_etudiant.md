# Projet — Estimer la demande horaire de vélos en libre-service

**Équipe de 3 · 6 semaines · rendu par dépôt**

---

## Le contexte

Un opérateur de vélos en libre-service doit **prévoir combien de vélos seront loués, heure par heure**.
C'est ce chiffre qui décide de la répartition de la flotte entre les stations et du nombre de camions
de rééquilibrage.

## La question à laquelle le projet répond

> Peut-on prévoir la demande horaire sur les quatre mois et demi à venir — et de combien une méthode
> soignée bat-elle la règle que l'opérateur applique aujourd'hui sans modèle ?

## Ce que vous recevez

| Fichier | Contenu |
|---|---|
| `train.csv` | **8 708 heures** — `datetime`, `season`, `holiday`, `workingday`, `weather`, `temp`, `atemp`, `humidity`, `windspeed`, `casual`, `registered`, `count` |
| `a_estimer.csv` | **2 178 heures** — de **2012-08-05 05:00** à **2012-12-19 23:00** — les mêmes colonnes **sauf `count`** |

**Deux avertissements, et ils comptent :**

- **la cible est un nombre entier de locations**, pas un montant : c'est une régression de comptage,
  avec une double saisonnalité — l'heure de la journée et la saison ;
- **vous prévoyez 91 jours à l'avance**, sur une période dont les volumes ne ressemblent pas à ceux de
  l'entraînement. Vérifiez-le avant de vous fier à votre validation.

Aucune donnée ne permet d'identifier une personne : `datetime` est l'horodatage de l'heure.


## Ce que contient chaque colonne

> **Texte officiel de la compétition**, recopié de la page *Data* de Kaggle.

**Description du jeu :** « You are provided hourly rental data spanning two years. For this
competition, the training set is comprised of the **first 19 days of each month**, while the test set
is the 20th to the end of the month. You must predict the total count of bikes rented during each
hour covered by the test set, **using only information available prior to the rental period**. »

**Data Fields**

| Champ | Description officielle |
|---|---|
| `datetime` | hourly date + timestamp |
| `season` | 1 = spring, 2 = summer, 3 = fall, 4 = winter |
| `holiday` | whether the day is considered a holiday |
| `workingday` | whether the day is neither a weekend nor holiday |
| `weather` | **1**: Clear, Few clouds, Partly cloudy, Partly cloudy · **2**: Mist + Cloudy, Mist + Broken clouds, Mist + Few clouds, Mist · **3**: Light Snow, Light Rain + Thunderstorm + Scattered clouds, Light Rain + Scattered clouds · **4**: Heavy Rain + Ice Pallets + Thunderstorm + Mist, Snow + Fog |
| `temp` | temperature in Celsius |
| `atemp` | "feels like" temperature in Celsius |
| `humidity` | relative humidity |
| `windspeed` | wind speed |
| `casual` | number of non-registered user rentals initiated |
| `registered` | number of registered user rentals initiated |
| `count` | number of total rentals |

**Deux écarts entre la compétition et notre sujet, et ils sont assumés :**

1. **le découpage.** La compétition découpe **à l'intérieur de chaque mois** (jours 1-19 pour
   l'entraînement, 20-fin pour le test). Notre sujet découpe **dans le temps** : les 2 178 dernières
   heures, soit **91 jours d'avance**. C'est plus dur, et c'est le cas réaliste d'une prévision de
   flotte.
2. **la règle d'information.** Kaggle l'écrit : « using only information available prior to the rental
   period ». C'est mot pour mot la règle du cours, et elle est plus forte que ce que le fichier laisse
   croire — voir l'avertissement de la section « Ce que vous recevez ».

## Ce que vous rendez

**Un dépôt de code**, pas un notebook :

1. un **pipeline exécutable** qui prend `a_estimer.csv` et produit `datetime,count` ;
2. le **protocole de validation** retenu, justifié ;
3. les **essais**, y compris ceux qui ont échoué, avec leur score ;
4. une **note de 3 pages** : décision, métrique, méthode, résultat chiffré, ce qui a échoué ;
5. une **model card** ;
6. la **fiche de données** du S13.

Le pipeline doit tourner **en moins de 10 minutes sur CPU**, sans GPU, avec `scikit-learn` seul.

## Les trois temps du projet

### Bloc 1 — avant la séance 5 · *la décision et la mesure*

- **La métrique** : ce qu'elle fait des heures creuses et des pics.
- **Le coût** : une station vide contre une flotte immobilisée. Chiffrez les deux.
- **La règle métier** : que fait l'opérateur sans modèle ? Mesurez-la. C'est votre plancher.
- **Le protocole de validation** : comment vous découpez `train.csv`. Les heures se suivent —
  réfléchissez à ce que cela implique.

### Bloc 2 — avant la séance 9 · *le pipeline*

- Le pipeline complet, exécutable d'un bout à l'autre.
- **Au moins huit essais**, dont ceux qui ont échoué, chacun avec son score.
- **L'inventaire des colonnes** : pour chaque variable que vous utilisez, demandez-vous si elle est
  **connue au moment où la prévision est faite**. Faites-en la liste, et justifiez chaque ligne.
  C'est l'exercice central de ce projet.
- **L'analyse d'erreurs** : à quelles heures, dans quelles saisons votre modèle se trompe.

### Bloc 3 — avant la séance 13 · *le dépôt et la robustesse*

- Le dépôt propre, la note de 3 pages, la model card, la fiche de données.
- **Trois mutations nommées**, chacune applicable en **une cellule**.
- Le test de robustesse, et l'estimation finale des 2 178 heures.

## Comment vous êtes évalués

Voir la grille du cours. Trois points pèsent particulièrement ici :

- **la justesse du découpage** : les heures se suivent, et un découpage qui l'ignore mesure autre
  chose que ce que vous croyez ;
- **la liste des colonnes connues au moment de prévoir** : elle doit être écrite, et tenue ;
- **la vérité du journal** : le journal consigne les essais, y compris ceux qui ont échoué.

## Deux règles du cours

**Pas de fuite.** Une information qui ne serait pas connue **au moment où la prévision est faite** ne
peut pas entrer dans le modèle. Un score anormalement bon est le premier symptôme à vérifier.

**Le code doit rester réparable.** On doit pouvoir passer d'une version à l'autre en changeant une
cellule, pas en réécrivant le pipeline.
