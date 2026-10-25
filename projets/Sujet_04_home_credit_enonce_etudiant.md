# Projet — Décider d'accorder ou de refuser un crédit

**Équipe de 3 · 6 semaines · rendu par dépôt**

---

## Le contexte

Un établissement de crédit reçoit des demandes de prêt. Pour chacune, il doit décider s'il
**accorde** ou s'il **refuse**.

Accorder un crédit qui finira en défaut coûte de l'argent. Refuser un bon client coûte un client —
et il ira voir ailleurs. Les deux erreurs n'ont pas le même prix, et c'est à vous de dire lequel.

## La question à laquelle le projet répond

> Parmi 61 503 demandes, lesquelles vont se terminer en défaut — et **où placer la barre** pour que le
> coût total des erreurs soit le plus faible possible ?

## Ce que vous recevez

| Fichier | Contenu |
|---|---|
| `train.csv` | **246 008 demandes** dont l'issue est connue — 122 colonnes, dont `TARGET` (1 = défaut, 0 = remboursé) |
| `a_decider.csv` | **61 503 demandes** — les 121 mêmes colonnes, **sans `TARGET`** |
| `HomeCredit_columns_description.csv` | **le dictionnaire des 122 colonnes** — lisez-le avant tout le reste |

**8,07 % des demandes de `train.csv` sont des défauts.**

Deux avertissements, et ils comptent :

- **les noms de colonnes sont opaques.** `EXT_SOURCE_2`, `DAYS_BIRTH`, `AMT_ANNUITY`, `NAME_EDUCATION_TYPE` —
  rien n'est devinable sans le dictionnaire. Lisez-le en premier ;
- **les valeurs manquantes sont nombreuses, et elles n'ont pas été comblées.** 50 colonnes dépassent
  30 % de manquants, certaines atteignent 70 %. **Ce n'est pas un défaut du fichier** : c'est une
  information, et vous devrez décider, colonne par colonne, ce que vous en faites.

Aucune donnée ne permet d'identifier une personne : l'identifiant `SK_ID_CURR` est un numéro interne,
et les montants sont ceux du dossier.


## Ce que contient chaque colonne

**La description officielle est un fichier, pas une page.** Contrairement aux autres compétitions,
Kaggle ne publie pas ici une page de description : il livre **`HomeCredit_columns_description.csv`**,
que vous avez reçu avec les données. Ce fichier **est** la description officielle des 121 colonnes —
c'est votre première lecture, et elle est exhaustive.

Il y a **121 colonnes par demande**, et leurs noms sont explicites mais techniques. **Le fichier
`HomeCredit_columns_description.csv`, livré avec les données, donne la définition de chacune** —
c'est votre première lecture, avant toute analyse.

Les familles, et ce qu'elles mesurent :

| Préfixe | Nombre | Ce que c'est |
|---|---|---|
| `AMT_` | ~20 | **montants** : crédit demandé, annuité, revenu du demandeur, prix du bien, valeur des biens |
| `DAYS_` | ~15 | **délais en jours**, souvent **négatifs** (nombre de jours *avant* la demande) : âge, ancienneté d'emploi, ancienneté du dernier changement de téléphone |
| `FLAG_` / `FLAG_DOCUMENT_` | ~20 | **indicateurs 0/1** : pièces fournies, mode de contact, situation documentaire |
| `NAME_` | ~15 | **catégories** : type de contrat, sexe, niveau d'éducation, situation familiale, type de revenu, profession, organisation |
| `CNT_` | ~10 | **comptages** : nombre d'enfants, de membres du ménage, de demandes antérieures |
| `EXT_SOURCE_1/2/3` | 3 | **scores externes** fournis par un organisme tiers — les trois variables les plus prédictives du dossier |
| `REGION_` / `REG_CITY_` / `LIVE_` | ~10 | **géographie** : région, ville, type de logement |
| `CODE_` / `OCCUPATION_` / `ORGANIZATION_` | 3 | catégories administratives |

**`a_decider.csv`** reprend ces **121 colonnes, sans `TARGET`** — et avec la clé `SK_ID_CURR`.

## Ce que vous rendez

**Un dépôt de code**, pas un notebook :

1. un **pipeline exécutable** qui prend `a_decider.csv` et produit un fichier
   `SK_ID_CURR,TARGET` — une **décision**, pas seulement un score ;
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
| Dossier du sujet | **`Sujet_04_Home_Credit/`** |
| Poids attendu | **165,4 Mo** |
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
2. Déposez les fichiers du dossier `Sujet_04_Home_Credit/` (glisser-déposer), nommez le jeu de données
   `esgc-vak-sujet-04-…`, puis **Create**.
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

- **Le coût** : chiffrez ce que coûte un défaut accordé et ce que coûte un bon client refusé. Les
  deux n'ont pas le même prix, et le rapport que vous choisissez engage toute la suite.
- **La métrique** : laquelle mesurera votre modèle ? Dites précisément **ce qu'elle récompense** et
  **ce qu'elle ignore**. Une métrique peut être satisfaite par un modèle inutile.
- **La règle métier** : quelle règle simple un analyste appliquerait-il sans modèle ? Mesurez-la.
  C'est votre plancher.
- **Le protocole de validation** : comment vous découpez `train.csv` pour estimer honnêtement votre
  performance sur `a_decider.csv` — et pourquoi ce découpage-là.

### Bloc 2 — avant la séance 9 · *le pipeline*

- Le pipeline complet, exécutable d'un bout à l'autre.
- **Au moins huit essais**, dont ceux qui ont échoué, chacun avec son score.
- Le traitement des **50 colonnes incomplètes** : ce que vous faites, colonne par colonne, et
  pourquoi.
- **La question qui compte** : votre modèle produit un **score**. À partir de quel score
  **refusez-vous** ? Ce seuil est une décision, et il n'a rien d'évident.

### Bloc 3 — avant la séance 13 · *le dépôt et la robustesse*

- Le dépôt propre, la note de 3 pages, la model card, la fiche de données.
- **Trois mutations nommées**, chacune applicable en **une cellule**.
- Le test de robustesse, et la décision finale sur `a_decider.csv`.

## Comment vous êtes évalués

Voir la grille du cours. Trois points pèsent particulièrement ici :

- **le seuil** : un bon modèle mal utilisé ne rapporte rien. Vous devez pouvoir dire **ce que votre
  seuil coûte** et **pourquoi celui-là** ;
- **le chiffre contre le plancher** : votre score se rapporte à la règle métier, pas à zéro ;
- **la vérité du journal** : le journal consigne les essais, y compris ceux qui ont échoué.

## Deux règles du cours

**Pas de fuite.** Une information qui ne serait pas connue **au moment où la demande est instruite**
ne peut pas entrer dans le modèle.

**Le code doit rester réparable.** On doit pouvoir passer d'une version à l'autre en changeant une
cellule, pas en réécrivant le pipeline.
