# Unarchiver
```unarchiver``` er et commandline værktøj til at udpakke komplekse filer som mails og arkivfiler (.zip m.fl.).

## Installation
Unarchiver skal installeres via master branch med [pipx](pipx.md):

```powershell
pipx install git+https://github.com/aarhusstadsarkiv/unarchiver.git
```

Test efterfølgende installationen med følgende kommando:

```powershell
unarchiver --version
```

## Opdatering
Når man skal opdatere unarchiver, bruges `pipx` med `--upgrade`:

```powershell
pipx upgrade unarchiver
```

## Afinstallation
```powershell
pipx uninstall unarchiver
```

## Forudsætninger
For at identificere alle de udpakkede dokumenter rekursivt, har unarchiver brug for at commandline værktøjet 
`Siegfried`er installeret og tilgængelig i path. Test med ``$ sf -v``. Siegfried er et eksternt Go-program, som benytter Pronom-registraturen til filidentifikation. Vi bruger det også bagved `digiarch`.

## Brug
Unarchiver tager en sti til en mappe eller en sti til en `files.db`-fil, genereret af `digiarch`.


```powershell 
unarchiver path_to_directory
```

eller

```powershell 
unarchiver path_to_files.db
```

Når en myndighedsaflevering skal unarchives giver man stien til `files.db`-filen.

!!! hint "Eksempel" 
    ```powershell
    unarchiver D:\AVID.AARS.77.1\OriginalFiles\_metadata\files.db
    ```

