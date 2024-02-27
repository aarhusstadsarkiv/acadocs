# Python repositories

- download python from python homepage

- all python-repo use pyproject.toml and poetry (remember to add [tool.poetry.scripts] for pipx to work)
- black, flake8 and mypy are not added as dev-dependencies, but installed globally.
- write a lint-script the calls the globally installed black, flake8 and mypy commands.
- use github actions to check linting
- use pytest for testing


- use pipx to install all global clis:
  - poetry
  - black
  - flake8
  - mypy
  - renamer
  - unarchiver
  - digiarch
  - symphovert
  - pytest