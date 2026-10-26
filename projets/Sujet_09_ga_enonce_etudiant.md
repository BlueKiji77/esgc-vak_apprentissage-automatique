# Projet — Estimer le revenu d'un visiteur d'un site marchand

**Équipe de 3 · 6 semaines · rendu par dépôt**

---

## Le contexte

Un site marchand veut savoir, **pour chaque visiteur**, combien il va rapporter. C'est ce chiffre qui
décide où placer le budget d'acquisition, et quels visiteurs relancer.

## La question à laquelle le projet répond

> Peut-on estimer le revenu d'un visiteur à partir de son comportement de navigation — et de combien
> une méthode soignée bat-elle l'estimation actuelle, qui traite tous les visiteurs de la même façon ?

**Et avant cela, une question d'ingénierie** : les données arrivent **telles qu'un outil de mesure les
produit** — une ligne par session, avec des objets JSON imbriqués. Les mettre en état d'être
exploitées fait partie du travail.

## Ce que vous recevez

Deux archives, à décompresser vous-mêmes.

| Fichier | Contenu | Compressé | Décompressé |
|---|---|---|---|
| **`train.zip`** | les sessions des **571 333 visiteurs** dont le revenu est connu — une ligne par **session** | **48,5 Mo** | **1 204,7 Mo** |
| **`a_estimer.zip`** | les sessions des **142 834 visiteurs** à estimer — mêmes colonnes, **`transactionRevenue` retiré** | **12,2 Mo** | **298,1 Mo** |

**1,40 % des visiteurs seulement ont généré un revenu.** Les autres n'ont rien acheté — et ce n'est
pas une anomalie : c'est la réalité d'un site marchand.

## Ce que contient chaque colonne

> **Texte officiel de la compétition**, recopié de la page *Data* de Kaggle.

Trois avertissements officiels, en majuscules dans le texte d'origine :

- « Each row in the dataset is **one visit** to the store. » — la source est donc à la **session**,
  et la cible est **par visiteur** : l'agrégation est à votre charge ;
- « There are multiple columns which contain **JSON blobs of varying depth**. In one of those JSON
  columns, `totals`, the sub-column `transactionRevenue` contains the revenue information we are
  trying to predict. **This sub-column exists only for the training data.** »
- « **IMPORTANT:** Due to the formatting of `fullVisitorId` you must load the Id's as **strings** in
  order for all Id's to be properly unique! »

Et la définition de la cible : `y_user = Σ transaction_i`, puis `target_user = ln(y_user + 1)`.

**Les 12 colonnes de chaque fichier** — quatre d'entre elles contiennent un objet JSON :

| Colonne | Type | Ce que c'est |
|---|---|---|
| `fullVisitorId` | texte | l'identifiant du visiteur — **à lire comme une chaîne**, jamais comme un nombre |
| `visitId` | entier | l'identifiant de la session |
| `visitNumber` | entier | numéro de la visite dans l'historique du visiteur |
| `visitStartTime` | entier | horodatage Unix du début de session |
| `date` | entier | la date, au format `AAAAMMJJ` |
| `channelGrouping` | texte | le canal d'acquisition |
| `socialEngagementType` | texte | type d'engagement social |
| **`totals`** | **JSON** | `visits`, `hits`, `pageviews`, `bounces`, `newVisits`, `transactions`, **`transactionRevenue`** |
| **`device`** | **JSON** | `browser`, `operatingSystem`, `deviceCategory`, `isMobile`, `browserVersion`… |
| **`geoNetwork`** | **JSON** | `continent`, `subContinent`, `country`, `region`, `city`, `networkDomain`… |
| **`trafficSource`** | **JSON** | `source`, `medium`, `campaign`, `keyword`, `isTrueDirect`, `adContent`… |
| `sessionId` | entier | identifiant technique de session |

**`transactionRevenue` est en micro-unités** : un revenu de `48 360 000` vaut `48,36`. Et il est
**absent du JSON de `a_estimer.zip`** — c'est ce que vous devez estimer.

## Ce que vous rendez

Un **dépôt de code** :

1. un **pipeline exécutable** qui prend `a_estimer.zip` et produit `visiteur,revenu` — il dézippe,
   parse le JSON, agrège par visiteur, entraîne et prédit ;
2. le **protocole de validation** retenu, justifié ;
3. les **essais**, y compris ceux qui ont échoué, avec leur score ;
4. une **note de 3 pages** : décision, métrique, méthode, résultat chiffré, ce qui a échoué ;
5. une **model card** ;
6. la **fiche de données** du S13.

Le pipeline doit tourner **en moins de 10 minutes sur CPU**, **décompression et parsing compris**,
sans GPU, avec `scikit-learn` seul.

## Ce que l'enseignant garde — et ce que vous pouvez demander

**Vous recevez toutes les lignes, sauf mention contraire ci-dessous.** Ce qui reste chez l'enseignant
n'est **jamais** une partie des données : c'est **la cible** des lignes que vous devez estimer, et
elle seule. Sans elle, il n'y aurait rien à prédire.

| | |
|---|---|
| Fichier retenu | **`verite.csv`** |
| Lignes | **142 834** |
| Colonnes | `visiteur`, `revenu` |
| Contenu | le revenu réel des visiteurs de `a_estimer.zip` |

**Vous n'y avez pas accès, et aucune copie ne doit circuler.** C'est l'enseignant qui mesure votre
rendu contre ce fichier.

### La table source complète est chez l'enseignant

Les données viennent d'une **compétition publique**. La source est la même que celle que vous
recevez — **ses 903 653 sessions et son JSON** — mais **avec `transactionRevenue` sur toutes les
lignes**. Si vous voulez la consulter pour comprendre la structure, **demandez-la** : elle est chez
l'enseignant.

**Deux précautions si vous la demandez :** elle pèse **1,50 Go décompressée** (64,1 Mo compressée),
donc **ne l'extrayez pas sur un disque plein** et **ne la chargez jamais d'un bloc** — lisez-la par
morceaux. Et **restez sur la même découpe** : le rendu porte sur les visiteurs de `a_estimer.zip`.

## Travailler : récupérer les données, coder, sauvegarder

Les données du cours sont sur le **Drive du cours**, en lecture seule — **dossier du sujet :**
**https://drive.google.com/drive/folders/1MuDHUzHFCg0rPegRgHsoI5wP2417G2VT?usp=sharing**. Vous téléchargez **le dossier de votre sujet** — pas les autres — et vous choisissez
ensuite **où coder**. Le lien est rappelé au **premier écran du carnet de démarrage**.

Le dossier contient **trois choses** : cet énoncé, le **carnet de démarrage** (`Carnet_de_demarrage.ipynb`)
et les fichiers de données.

### 1. Récupérer votre dossier

| | |
|---|---|
| Dossier du sujet | **`Sujet_09_GA_Customer_Revenue/`** |
| Poids attendu | **60,7 Mo** (les deux archives) |
| Contenu | `train.zip`, `a_estimer.zip` |

### 2. Option A — travailler en local, dans VS Code

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

**Décompressez dans un dossier de travail**, pas dans le dossier partagé : comptez **1,50 Go** pour les
deux fichiers décompressés. Puis ouvrez le dossier dans VS Code, écrivez votre pipeline et lancez-le.

**La contrainte des 10 minutes s'applique à votre machine**, décompression et parsing compris.
Mesurez-la : elle fait partie du livrable.

### 3. Option B — travailler dans un carnet Kaggle

Kaggle ne voit pas votre disque : créez **votre propre jeu de données** avec les deux archives.

1. **kaggle.com/datasets** → **New Dataset**.
2. Déposez `train.zip` et `a_estimer.zip`, nommez le jeu `esgc-vak-sujet-09-…`, puis **Create**.
3. Dans votre carnet : **Add Input** → onglet **Datasets** → filtre **Your Datasets** → **Add**.
4. Vérifiez : `!ls /kaggle/input/<nom-de-votre-jeu>/`.
5. **Session options** → **Accelerator : None**, **Internet désactivé**.

> **La compétition d'origine n'est pas la source.** Ses fichiers diffèrent des vôtres : pas de jeu à
> estimer sans revenu, et plusieurs compétitions de ce type ne sont plus rejoignables.

### 4. Sauvegarder, et réutiliser votre travail

**En local.** Votre dossier de travail est la sauvegarde ; gardez les sorties dans `sorties/`.

**Sur Kaggle.** `/kaggle/input` est en **lecture seule** : écrivez dans **`/kaggle/working/`**, puis
**Save Version → Save & Run All (Commit)** pour conserver les sorties. Pour les reprendre ailleurs :
**Add Input → Notebook Output**. ⚠️ `/kaggle/working` est éphémère : une session inactive est coupée
au bout de 20 minutes.

## Les trois temps du projet

### Bloc 1 — avant la séance 5 · *la décision et la mesure*

- **La métrique** : laquelle, et **sur quelle échelle**. Le revenu va de zéro à plusieurs millions —
  réfléchissez à ce que cela implique avant de mesurer quoi que ce soit.
- **Le coût** : un acheteur manqué contre un visiteur relancé pour rien.
- **La règle métier** : que fait le site sans modèle ? Mesurez-la. C'est votre plancher.
- **Le protocole de validation** : comment vous découpez les visiteurs connus, sachant que 1,40 %
  seulement ont acheté.

### Bloc 2 — avant la séance 9 · *le pipeline*

- Le pipeline complet, **de l'archive au fichier de prédiction**.
- **Au moins huit essais**, dont ceux qui ont échoué, chacun avec son score.
- **Le passage du JSON à la table** : quels champs gardez-vous, comment agrégez-vous les sessions d'un
  visiteur, et que perdez-vous en le faisant ? Écrivez-le.
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
