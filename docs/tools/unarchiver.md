# Unarchiver
```unarchiver``` er et værktøj til at udpakke komplekse filer som mails og arkivfiler (.zip m.fl.).

## Installation
Unarchiver skal installeres via master branch med [pipx](pipx.md):

```powershell
pipx install git+https://github.com/aarhusstadsarkiv/unarchiver.git
```

Test efterfølgende installationen med følgende kommando:

```powershell
unarchiver --version
```

Når man skal opdatere unarchiver, bruges `pipx` med `--upgrade`:

```powershell
pipx upgrade unarchiver
```

## Brug

Unarchiver kan køres ved køre følgende

```powershell 
unarchiver path_to_directory
```

eller

```powershell 
unarchiver path_to_files.db
```

Når en aflevering skal unarchives giver man stien til `files.db`.

!!! hint "Eksempel" 
    ```powershell
    unarchiver D:\AVID.AARS.77.1\OriginalFiles\_metadata\files.db
    ```
