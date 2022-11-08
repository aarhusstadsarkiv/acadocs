Pipx er en wrapper rundt om pip, og bruges til at installere Python-baserede commandline værktøjer. Læs alt om Pipx på deres [dokumentationsportal]([Pipx](https://pypa.github.io/pipx/).


## Installation og opdatering
Pipx skal naturligvis installeres før alle de python-baserede commandline værktøjer, der installeres via pipx.

```
python -m pip install --user pipx
pipx ensurepath

```
Den sidste kommando tilføjer både `<USER folder>\AppData\Roaming\Python\Python3x\Scripts` og `%USERPROFILE%\.local\bin` mappen til din `path` miljøvariabel. Genstart termionalen for at teste om ´pipx´ virker.


Upgrade pipx med pip:
```
python -m pip install --user pipx
```
