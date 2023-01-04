# POETRY

Poetry est un outil de gestion des dépendances et de packaging en Python. \
Il permet de déclarer les bibliothèques dont dépend le projet et il les gérera (installer/mettre à jour) pour nous.

[Documentation officielle](https://python-poetry.org/docs/)

- [POETRY](#poetry)
  - [PRÉREQUIS](#prérequis)
    - [Installation](#installation)
    - [Mise à jour de Poetry](#mise-à-jour-de-poetry)
    - [Complétion](#complétion)
    - [Configuration](#configuration)
      - [Création du dossier de l'environnement virtuel dans le projet](#création-du-dossier-de-lenvironnement-virtuel-dans-le-projet)
  - [UTILISATION](#utilisation)
    - [Créer un nouveau projet](#créer-un-nouveau-projet)
    - [Entrer dans le nouvel environnement](#entrer-dans-le-nouvel-environnement)
    - [Gestion des dépendances](#gestion-des-dépendances)
      - [Ajouter une dépendance](#ajouter-une-dépendance)
        - [Dépendance applicative](#dépendance-applicative)
        - [Dépendance de développement](#dépendance-de-développement)
        - [Dépendance dans une version spécifique](#dépendance-dans-une-version-spécifique)
        - [Dépendance avec option](#dépendance-avec-option)
      - [Supprimer une dépendance](#supprimer-une-dépendance)
      - [Voir les dépendances](#voir-les-dépendances)
      - [Mettre à jour les dépendances](#mettre-à-jour-les-dépendances)
      - [Installer les dépendances "verrouillées"](#installer-les-dépendances-verrouillées)
      - [Exporter vers le fichier requirements.txt](#exporter-vers-le-fichier-requirementstxt)
    - [Création d'un binaire ou d'une archive et la pousser dans une registry](#création-dun-binaire-ou-dune-archive-et-la-pousser-dans-une-registry)

## PRÉREQUIS

### Installation

- Pour MacOS et Linux:

```bash
curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python3 -
```

- Pour Windows:

```bash
(Invoke-WebRequest -Uri https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py -UseBasicParsing).Content | python -
```

### Mise à jour de Poetry

```bash
poetry self update
```

### Complétion

Voir la [documentation](https://python-poetry.org/docs/#enable-tab-completion-for-bash-fish-or-zsh) suivant votre environnement.

### Configuration

#### Création du dossier de l'environnement virtuel dans le projet

```bash
poetry config virtualenvs.in-project true
```

## UTILISATION

### Créer un nouveau projet

```bash
poetry new <project_name>
```

### Entrer dans le nouvel environnement

```bash
poetry shell
```

### Gestion des dépendances

#### Ajouter une dépendance

##### Dépendance applicative

```bash
poetry add <non_du package>
```

##### Dépendance de développement

```bash
poetry add --dev <non_du package>
> poetry add --dev icecream loguru
```

##### Dépendance dans une version spécifique

```bash
poetry add "<non_du package=version>"
> poetry add "google-cloud-bigquery>=1.21.1"
```

##### Dépendance avec option

```bash
poetry add <non_du package> -E <option>
> poetry add uvicorn -E standard
```

#### Supprimer une dépendance

```bash
poetry remove <non_du package>
```

#### Voir les dépendances

Poetry vous montrera les dépendances actuelles installées et les MAJ possibles. \
(status vert, jaune, orange, rouge)

```bash
poetry show -l
```

Il peut aussi vous montrer l'arbre des dépendances

```bash
poetry show -t
```

#### Mettre à jour les dépendances

```bash
poetry update
```

#### Installer les dépendances "verrouillées"

Utiliser et n'installer que les dépendances avec les versions spécifiées dans le poetry.lock (il est mis à jour à chaque installation de dépendances) \
Utile pour les CIs par exemple.

```bash
poetry install
```

#### Exporter vers le fichier requirements.txt

Nous allons exporter vers le fichier requirements.txt. \
(utile pour les services cloud par exemple) \
Seul des dépendances non-dev seront exportées.

```bash
poetry export -f requirements.txt --without-hashes -o <path/requirements.txt>
```


### Création d'un binaire ou d'une archive et la pousser dans une registry

Nous allons construire et publier l'archive dans une registry. \
Cela peut se faire en une seule commande.

```bash
poetry publish --build -r <registry_name>
```
