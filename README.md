# IAA1302.1 — Apprentissage Automatique · dépôt du cours

> ESGC-VAK · Licence Professionnelle Génie Informatique · Semestre 3 · 2026-2027

**Ce dépôt contient les carnets du cours, les devoirs et les énoncés des sujets de projet.** Les
données, elles, sont sur la compétition Kaggle.

## Ce qu'il y a ici

| Fichier | À quoi il sert |
|---|---|
| `notebook_J0.ipynb` | **Les fondamentaux Python.** À faire en premier si vous n'avez jamais écrit de Python : la syntaxe dont tous les autres carnets ont besoin, et rien d'autre |
| `notebook_J0.5.ipynb` | **Le test de plomberie.** À exécuter avant la première séance : s'il tourne de bout en bout, tout le reste du cours tournera |
| `notebook_J1.ipynb` | **Séance 1 — du fichier brut à la soumission.** Lire, explorer, préparer la table de travail, situer la référence, entraîner, régler, écrire `submission.csv`, tenir le tableau d'essais, exposer l'interface `predict(df)` |
| `notebook_J2.ipynb` | **Séance 2 — ce que la table cache.** La régression logistique, une feature de votre cru, la cible comme objet suspect, la colonne qui disparaît en production, les colonnes connues après l'issue, le rapport. Sa mise en place est extraite de J1 : il n'y a qu'une seule table de travail dans le cours |
| `notebook_J3.ipynb` | **Séance 3 — comparer sans se mentir, et ce qu'on fait des défauts trouvés.** Les baselines, le piège du public, la variance d'un score, le plafond, les deux sortes de trous, le journal de nettoyage, le pipeline |
| `notebook_J4.ipynb` | **Séance 4 — la mesure.** La matrice de confusion recalculée à la main, les six métriques, l'exactitude qui ment, ROC contre PR, le coût et le seuil, et l'échelle « quand ne pas faire de ML » |
| `Devoir_maison_1.ipynb` | **Devoir 1 — les carnets J1 et J2** (questions 1 à 10 sur J1, 11 à 16 sur J2), à faire seul, dans un carnet auto-suffisant : la mise en place est fournie. **À rendre avant le 19 octobre**, dans votre dépôt, avec la déclaration d'usage de l'IA (`declaration_DM1.md`) |
| `Devoir_maison_2.ipynb` | **Devoir 2 — les carnets J3 et J4.** Il porte aujourd'hui les quatre questions de J3 ; celles de J4 les rejoindront. À rendre avec `declaration_DM2.md` |
| `DECLARATION_modele.md` | **Le modèle de déclaration d'usage de l'IA.** À recopier dans votre dépôt à chaque rendu : `declaration_DM1.md`, `declaration_DM2.md`, `declaration_E1.md`, `declaration_E2.md`, `declaration_E3.md` — et `declaration_B3.md` sur le dépôt d'équipe. Elle se dépose **avec** le carnet, dans le même commit |
| `projets/` | **Les énoncés des sujets de projet.** Un fichier par sujet : le problème, la décision à servir, ce que vous recevez, ce qu'on attend, et rien de plus. Le sujet de votre équipe vous sera indiqué |
| `carnets/` | **Neuf carnets de démarrage, un par sujet.** Ils trouvent les données, les chargent (cache Parquet pour les tables lourdes), les profilent, écrivent une soumission au bon format — puis s'arrêtent : métrique, découpage et modèle restent à votre charge |

## Comment s'en servir

1. **Le chemin normal** : compétition → onglet **Code** → **New Notebook**. Les données de la
   compétition sont **montées automatiquement** — rien à attacher.
2. **Importer le carnet** : `File → Import Notebook`, puis coller l'URL de ce dépôt. Le contenu est
   remplacé, **l'attachement des données reste**.
3. *(Seulement dans un carnet créé depuis votre page Kaggle personnelle)* **attacher la
   compétition** : panneau de droite → `Add Input` → onglet **Competitions**. Il faut l'avoir
   rejointe au préalable.
4. **`Save & Run All`** — pas `Save`. J0.5, J1, J2 et J3 écrivent `submission.csv` dans les sorties
   du carnet.

> Le détail de chaque étape est dans le wiki du cours, section 12 « Guide de démarrage ».

## Les copies exécutées — pour relire seul

Chaque carnet existe aussi en **`<nom>_execute.html`** : la même chose, **déjà exécutée**, avec les
tableaux et les chiffres visibles. Les `.ipynb`, eux, partent **sans sorties** (une sortie
enregistrée voyage avec le fichier). Pour **travailler**, importez le `.ipynb` ; pour **relire** un
passage seul, ouvrez le `.html` — si un chiffre diffère, c'est le `.html` qui a raison : il a tourné.

## L'ordre des carnets, et ce que chaque devoir couvre

```
J0 → J0.5 → J1 ─┐
                 ├─ Devoir 1
         J2 ─────┘
         J3 ─┐
              ├─ Devoir 2  (J4 les rejoindra)
         J4 ─┘
```

## Les énoncés des sujets de projet

Ils sont dans **`projets/`**, un fichier Markdown par sujet. Un énoncé dit **ce que vous savez, ce
qu'on vous donne et ce qu'on attend — et rien de plus** : il ne nomme aucun des pièges de la table.
Les trouver fait partie du travail, et c'est ce qui est évalué.

Lisez l'énoncé avant d'ouvrir les données, et gardez-le sous les yeux pendant la conception : c'est
lui qui fixe la décision à servir.

## Les données

Elles vivent sur la **compétition Kaggle**, onglet *Data* — **une seule source, jamais recopiée**.

## Licence

Documents pédagogiques, ESGC-VAK. Usage réservé aux étudiants inscrits à l'ECUE IAA1302.1.
