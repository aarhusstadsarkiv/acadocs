# Convert-qa

Convert-qa er et værktøj der bruges til undersøge om filerne er konverteret korrekt, ved at sammenligne originale filer med de konverterede filer.

## Installation
Convert-qa skal installeres via master branch med [pipx](pipx.md):

```powershell
pipx install git+https://github.com/aarhusstadsarkiv/convert-qa.git
```

Test efterfølgende installationen med følgende kommando:

```powershell
convert-qa --version
```

Når man skal opdatere convert-qa, bruges `pipx` med `--upgrade`:

```powershell
pipx upgrade convert-qa
```

## Brug