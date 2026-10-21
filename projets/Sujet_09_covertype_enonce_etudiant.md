# Projet — Identifier le type de couvert forestier d'une parcelle

**Équipe de 3 · 6 semaines · rendu par dépôt**

---

## Le contexte

Un service forestier doit **cartographier la couverture végétale** d'un massif. Envoyer une équipe sur
chaque parcelle coûte cher ; les relevés topographiques et pédologiques, eux, sont déjà disponibles
pour l'ensemble du territoire.

## La question à laquelle le projet répond

> Peut-on déduire, des seules variables de relief et de sol, **l'espèce qui domine** sur une parcelle
> de 30 × 30 mètres — et de combien une méthode soignée bat-elle le classement actuel, qui applique la
> même espèce partout ?

## Ce que vous recevez

| Fichier | Contenu |
|---|---|
| `train.csv` | **464 809 parcelles** dont l'espèce est connue — 55 colonnes, dont `couvert` |
| `a_classer.csv` | **116 203 parcelles** — les mêmes 54 variables, **sans `couvert`** |

**Sept espèces, très inégalement réparties** — de 48,76 % pour la plus fréquente à **0,47 %** pour la
plus rare. Les sept sont présentes dans les deux fichiers.

## Ce que contient chaque colonne

> **Documentation officielle du jeu de données** (UCI Machine Learning Repository, `covtype.info`),
> traduite et mise en tableau. « Number of Attributes: *12 measures, but 54 columns of data* (10
> quantitative variables, 4 binary wilderness areas and 40 binary soil type variables) ».

### Les 10 mesures quantitatives

| Nom | Type | Unité | Description officielle |
|---|---|---|---|
| `elevation_m` | quantitative | mètres | Elevation in meters |
| `aspect_deg` | quantitative | azimut | Aspect in degrees azimuth |
| `slope_deg` | quantitative | degrés | Slope in degrees |
| `distance_hydrology_m` | quantitative | mètres | Horz Dist to nearest surface water features |
| `vertical_hydrology_m` | quantitative | mètres | Vert Dist to nearest surface water features |
| `distance_road_m` | quantitative | mètres | Horz Dist to nearest roadway |
| `hillshade_9h` | quantitative | indice 0 à 255 | Hillshade index at 9am, summer solstice |
| `hillshade_midi` | quantitative | indice 0 à 255 | Hillshade index at noon, summer solstice |
| `hillshade_15h` | quantitative | indice 0 à 255 | Hillshade index at 3pm, summer solstice |
| `distance_fire_m` | quantitative | mètres | Horz Dist to nearest wildfire ignition points |

### Les 4 zones sauvages — binaires

« Wilderness area designation », 0 (absence) ou 1 (présence). **Une seule vaut 1 par parcelle.**

| Colonne | Zone officielle |
|---|---|
| `zone_sauvage_1` | Rawah Wilderness Area |
| `zone_sauvage_2` | Neota Wilderness Area |
| `zone_sauvage_3` | Comanche Peak Wilderness Area |
| `zone_sauvage_4` | Cache la Poudre Wilderness Area |

### Les 40 types de sol — binaires

« Soil Type designation », 0 (absence) ou 1 (présence), d'après les **USFS Ecological Landtype Units**
(ELU). **Un seul vaut 1 par parcelle.** Les 40 sont présents dans les données ; leurs libellés
officiels vont de « Cathedral family - Rock outcrop complex, extremely stony » à « Krummholz »-type
d'altitude, et sont donnés dans `covtype.info`.

### La cible

| Valeur | Espèce officielle |
|---|---|
| 1 | Spruce/Fir |
| 2 | Lodgepole Pine |
| 3 | Ponderosa Pine |
| 4 | Cottonwood/Willow |
| 5 | Aspen |
| 6 | Douglas-fir |
| 7 | Krummholz |

## Ce que vous rendez — attention, c'est une **distribution**

Votre pipeline produit, pour chaque parcelle, **sept probabilités** :

```
id,couverture_1,couverture_2,couverture_3,couverture_4,couverture_5,couverture_6,couverture_7
1,0.02,0.91,0.01,0.00,0.01,0.04,0.01
```

Chaque ligne doit sommer à 1. **Ce n'est pas une étiquette que vous rendez, c'est une distribution.**

Le dépôt contient en outre le protocole de validation justifié, les essais (dont ceux qui ont échoué),
une note de 3 pages, une model card, et la fiche de données du S13.

Le pipeline doit tourner **en moins de 10 minutes sur CPU**, sans GPU, avec `scikit-learn` seul.
**581 012 lignes et 54 variables : la contrainte de temps est réelle**, et c'est une contrainte du
projet, pas un détail.

## Les trois temps du projet

### Bloc 1 — avant la séance 5 · *la décision et la mesure*

- **La métrique** : ce qu'elle juge exactement, et pourquoi elle **pèse la confiance** de chaque
  réponse.
- **Le coût** : que coûte une parcelle mal classée ?
- **La règle métier** : que fait le service sans modèle ? Mesurez-la. C'est votre plancher.
- **Le protocole de validation** : comment vous découpez `train.csv`. Les sept espèces, dont une à
  0,47 %, rendent ce choix décisif.

### Bloc 2 — avant la séance 9 · *le pipeline*

- Le pipeline complet, exécutable d'un bout à l'autre, produisant **les sept probabilités**, **dans la
  contrainte de temps**.
- **Au moins huit essais**, dont ceux qui ont échoué.
- **La question de ce sujet** : que se passe-t-il si vous rendez seulement l'espèce gagnante ?
  Mesurez-le.
- **Le rappel par espèce** : la moyenne cache les classes rares. Montrez-le.

### Bloc 3 — avant la séance 13 · *le dépôt et la robustesse*

- Le dépôt propre, la note de 3 pages, la model card, la fiche de données.
- **Trois mutations nommées**, chacune applicable en **une cellule**.
- Le test de robustesse, et le classement final des 116 203 parcelles.

## Comment vous êtes évalués

Voir la grille du cours. Trois points pèsent particulièrement ici :

- **la sortie probabiliste** : répondre une seule espèce n'est pas ce qui est demandé ;
- **le chiffre contre le plancher** : votre score se rapporte à une règle sans modèle, mesurée ;
- **la vérité du journal** : le journal consigne les essais, y compris ceux qui ont échoué.

## Deux règles du cours

**Pas de fuite.** Une information inconnue **au moment de la cartographie** ne peut pas entrer dans le
modèle.

**Le code doit rester réparable.** On doit pouvoir passer d'une version à l'autre en changeant une
cellule, pas en réécrivant le pipeline.
