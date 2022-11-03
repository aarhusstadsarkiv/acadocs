# Convert-unmanaged

Convert-unmanaged er et værktøj, der bruges til at identificere, hvilke filformater der er i en given aflvering, som vores konverteringsværktøjer (endnu) ikke kan håndtere.

## Installation
Convert-unmanaged skal installeres via master branch med [pipx](pipx.md):

```powershell
pipx install git+https://github.com/aarhusstadsarkiv/convert-unmanaged.git
```

Test efterfølgende installationen med følgende kommando:

```powershell
convert-unmanaged --version
```

Når man skal opdatere convert-unmanaged, bruges `pipx` med `--upgrade`:

```powershell
pipx upgrade convert-unmanaged
```

## Brug

Convert-unmanaged køres med kommandoen:

```powershell
convert-unmanaged db_file_path
```

!!! hint "Eksempel" 
    ```powershell
    convert-unmanaged D:\AVID.AARS.84.1\OriginalFiles\_metadata\files.db```