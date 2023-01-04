# CI

Voici un exemple de CI Github Actions pour un module en Python, avec :

- Tests sur Python 3.7, 3.8 et 3.9
- Utilisation de Poetry
- Tests avec MyPy, Flake8, Pylint, Pytest
- MAJ de la version et création d'une release dans Github
- Création d'un paquet

```bash
name: CI
on:
  push:
  pull_request:
    branches: [main]

jobs:
  Quality:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python: [3.7, 3.8, 3.9]
    steps:
      - uses: actions/checkout@v2

      - name: Import Secrets
        uses: hashicorp/vault-action@v2.3.0
        with:
          url: ${{secrets.VAULT_URL}}
          method: approle
          namespace: ${{secrets.VAULT_NAMESPACE}}
          roleId: ${{secrets.VAULT_ROLE_ID}}
          secretId: ${{secrets.VAULT_SECRET_ID}}
          secrets: |
            secret/data/<REGISTRY_NAME> username | POETRY_HTTP_BASIC_<REGISTRY_NAME>_USERNAME;
            secret/data/<REGISTRY_NAME> password | POETRY_HTTP_BASIC_<REGISTRY_NAME>_PASSWORD;

      - uses: actions/setup-python@v2
        with:
          python-version: ${{ matrix.python }}

      - name: Install Python Poetry
        uses: abatilo/actions-poetry@v2.1.0
        with:
          poetry-version: 1.1.10

      - name: Configure poetry
        run: |
          poetry config virtualenvs.in-project true

      - name: Cache dependencies
        uses: actions/cache@v2
        id: cache-dependencies
        with:
          path: |
            .venv
            ~/.cache/pypoetry
            .tox/py
          key: ${{ runner.os }}-python-${{ steps.setup-python.outputs.python-version }}-venv-${{ hashFiles('**/poetry.lock') }}

      - name: Install dependencies
        run: poetry update

      - name: Flake8
        run: poetry run flake8 --max-line-length 120 <code_folder>

      - name: Pylint
        run: poetry run python -m pylint -vv <code_folder>

      - name: Mypy
        run: poetry run python -m mypy --install-types --non-interactive <code_folder>

      - name: Pytest
        run: |
          echo $PING_URL
          poetry run python -m pytest -vv tests --cov=<code_folder> -s

  Release:
    needs: Quality
    # https://github.community/t/how-do-i-specify-job-dependency-running-in-another-workflow/16482
    if: github.event_name == 'push' && github.ref == 'refs/heads/main' && !contains(github.event.head_commit.message, 'chore(release):')
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2

      - name: Import Secrets
        uses: hashicorp/vault-action@v2.3.1
        with:
          url: ${{secrets.VAULT_URL}}
          method: approle
          namespace: ${{secrets.VAULT_NAMESPACE}}
          roleId: ${{secrets.VAULT_ROLE_ID}}
          secretId: ${{secrets.VAULT_SECRET_ID}}
          secrets: |
            secret/data/<REGISTRY_NAME> username | POETRY_HTTP_BASIC_<REGISTRY_NAME>_USERNAME;
            secret/data/<REGISTRY_NAME> password | POETRY_HTTP_BASIC_<REGISTRY_NAME>_PASSWORD

      - uses: actions/setup-python@v2
        with:
          python-version: 3.8

      - name: Install Python Poetry
        uses: abatilo/actions-poetry@v2.1.0
        with:
          poetry-version: 1.1.10

      - name: Semantic Release
        run: |
          pip install python-semantic-release
          git config user.name github-actions
          git config user.email github-actions@github.com
          semantic-release publish

      - name: Configure poetry
        run: |
          poetry config virtualenvs.in-project true

      - name: Publish to Registry
        run: |
          poetry publish --build -r <registry_name>
```

La CI dans Github Actions

![CI](img/CI.png)
