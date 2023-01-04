# VSCode

- [VSCode](#vscode)
  - [Documentation officielle](#documentation-officielle)
  - [Plugins intéressants](#plugins-intéressants)
    - [Plugins généralistes](#plugins-généralistes)
      - [EditorConfig for VS Code](#editorconfig-for-vs-code)
      - [Error Lens](#error-lens)
      - [Bracket Pair Colorizer](#bracket-pair-colorizer)
      - [Code Spell Checker](#code-spell-checker)
    - [Plugins spécifiques à Python](#plugins-spécifiques-à-python)
      - [Python](#python)
      - [Pylance](#pylance)
  - [Configuration](#configuration)
    - [Config généralistes](#config-généralistes)
    - [Config spécifiques à Python](#config-spécifiques-à-python)
      - [Formater le text avec Black](#formater-le-text-avec-black)
      - [Trier les imports avec Isort](#trier-les-imports-avec-isort)
      - [Utiliser PyCodeStyle pour le style d'écriture de Python](#utiliser-pycodestyle-pour-le-style-décriture-de-python)
      - [Utiliser PyDocStyle pour le style de documentation de Python](#utiliser-pydocstyle-pour-le-style-de-documentation-de-python)
      - [Utiliser MyPy](#utiliser-mypy)

## Documentation officielle

La [documentation officielle](https://code.visualstudio.com/docs/languages/python) de VSCode avec Python

## Plugins intéressants

### Plugins généralistes

#### EditorConfig for VS Code

EditorConfig aide à maintenir des styles de codage cohérents pour plusieurs développeurs travaillant sur le même projet à travers différents éditeurs et IDE.

Le fichier de configuration est à déployer à la racine de chaque projet.

Le [plugin](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig) pour VSCode l'utilisera comme règle par défaut.

- [Documentation officielle](https://editorconfig.org/)

Exemple

```bash
# EditorConfig is awesome: https://EditorConfig.org

# Active EditorConfig file
root = true

# ALL
[*]
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true
charset = utf-8
max_line_length = 120
#indent_style = space
#indent_size = 2

# 4 space indentation
# Python, Jinja
[*.{py,jinja,j2}]
indent_style = space
indent_size = 4

# 2 space indentation
# Ruby, Yaml
[*.{rb,yaml, yml}]
indent_style = space
indent_size = 2

# 2 tab indentation
# Makefile
[Makefile]
indent_style = tab
indent_size = 4
```

#### Error Lens

[Plugin](https://marketplace.visualstudio.com/items?itemName=usernamehw.errorlens) permettant de mettre en surbrillance les erreurs dans le code, augmentant très fortement leur visibilité !

#### Bracket Pair Colorizer

[Plugin](https://marketplace.visualstudio.com/items?itemName=CoenraadS.bracket-pair-colorizer-2) permettant de colorer les parenthèses, accolades, crochets, ... facilitant la lecture.

#### Code Spell Checker

[Plugin](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker) permettant de vérifier l'orthographe Anglaise et [Française](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker-french) du code et d'éviter les fautes de frappes.

### Plugins spécifiques à Python

#### Python

Le [plugin officiel](https://marketplace.visualstudio.com/items?itemName=ms-python.python) pour le support de Python.

#### Pylance

[Plugin](https://marketplace.visualstudio.com/items?itemName=ms-python.vscode-pylance) surchargeant le support de Python et d'IntelliSense (completion, linting, static type checking, ...).

Utilisant [PyRight](https://github.com/microsoft/pyright), la configuration se fait dans le fichier *pyrightconfig.json* à mettre à la racine du projet.

Exemple

```bash
{
    "reportMissingTypeStubs": false,
    "reportUnknownMemberType": false,
    "reportFunctionMemberAccess": false,
    "reportGeneralTypeIssues": false
}
```

## Configuration

### Config généralistes

Exemple de paramètres pour faciliter le développement.

- Formater le code en sauvegardant ou en copiant
- Couper le texte à partir d'un certain nombre de colonne
- Afficher des lignes verticales sur les limites de colonne

Ces paramètres sont à mettre dans *settings.json*.

```bash
{
    ...
    "editor.formatOnSave": true
    "editor.formatOnPaste": true,
    "editor.wordWrap": "on",
    "editor.wordWrapColumn": 100,
    "editor.rulers": [
        100,
        120
    ],
    ...
}
```

### Config spécifiques à Python

Pour Python, nous pouvons déclarer des formater, des linter et des types checker respectant la PEP8 de Python.

#### Formater le text avec Black

[Black](https://github.com/psf/black) permet de formatter automatiquement le code sans que l'équipe n'est à se poser des questions.

```bash
{
    ...
    "python.formatting.provider": "black",
    "python.formatting.blackArgs": [
        "--line-length",
        "120"
    ],
    ...
```

Voir aussi la documentation de VSCode sur [l'édition de code](https://code.visualstudio.com/docs/python/editing)

#### Trier les imports avec Isort

[Isort] permet de trier les importations par ordre alphabétique et de les séparer automatiquement en sections et par type.

```bash
{
    ...
    "[python]": {
        "editor.codeActionsOnSave": {
            "source.organizeImports": true
        }
    }
    ...
}
```

#### Utiliser PyCodeStyle pour le style d'écriture de Python

[PyCodeStyle](https://pycodestyle.pycqa.org/en/latest/) permet de vérifier que le code respecte bien le style d'écriture de la [PEP8](https://www.python.org/dev/peps/pep-0008/).

```bash
{
    ...
    "python.linting.pycodestyleEnabled": true,
    "python.linting.pycodestyleArgs": [
        "--max-line-length=1220"
    ],
    ...
}
```

Vous pouvez si vous le souhaiter remplacer PyCodeStyle par Flake8 qui l'intègre dans ses tests.

Voir aussi la documentation VSCode sur le [Linting](https://code.visualstudio.com/docs/python/linting)

Pour empêcher un linter de remonter une ligne en erreur, vous pouvez utiliser **# noqa** ou **# noqa: EXXX** pour ignorer une erreur spécifique.

#### Utiliser PyDocStyle pour le style de documentation de Python

[PyDocStyle](http://www.pydocstyle.org/en/stable/) permet de vérifier que le code respecte bien le style de documentation de la [PEP8](https://www.python.org/dev/peps/pep-0008/).

```bash
{
    ...
    "python.linting.pydocstyleEnabled": true,
    ...
}
```

#### Utiliser MyPy

[MyPy](http://mypy-lang.org/) permet de vérifier les types statiques et dynamiques de votre code.

```bash
{
    ...
    "python.linting.mypyEnabled": true,
    ...
}
```
