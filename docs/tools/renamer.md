# Renamer
```renamer``` er et commandline værktøj til omdøbning af filer. Oftest brugt i forbindelse med konverteringsprocesser, hvor filer med manglende eller fejlagtig filendelse ofte vil skabe problemer i den senere konverteringsproces.

## Installation
Renamer skal installeres via master branch med [pipx](pipx.md):

```powershell
pipx install git+https://github.com/aarhusstadsarkiv/renamer.git
```

Test efterfølgende installationen med følgende kommando:

```powershell
renamer --version
```

Når man skal opdatere renamer, bruges `pipx` med `--upgrade`:

```powershell
pipx upgrade renamer
```

## Brug

Ved warningen "Extension mismatch" i DB Browser identificeres, hvilken filendelse filen burde have.

Dernæst bruges følgende kommando til at ændre filendelsen:

 ```powershell
 renamer db_file_path puid new_extension_without_period_sign
 ```

I nedenstående eksempel giver renamer alle puid fmt/340 filendelsen lwp

!!! hint "Eksempel" 
    ```powershell
    renamer D:\AVID.AARS.77.1\OriginalFiles\_metadata\files.db fmt/340 lwp
    ```


## Filendelser
Ved extension mismatch-warnings, hvor digiarch har givet en fil et PUID der ikke stemmer overens med dens extension (eller mangel på samme), kan der opstå tvivl om hvilken extension filen burde have, dadette ikke er angivet i `files.db`. 
Her bruges National Archives' hjemmeside [`pronom`] (https://www.nationalarchives.gov.uk/PRONOM/PUID/proPUIDSearch.aspx?status=new) til at undersøge, hvilken filendelse filen skal have. I de tilfælde, hvor der er flere muligheder kan man evt. kigge på andre filer i afleveringen med samme PUID. 
For aca-PUID'er (vores egne) bruges følgende liste:

| PUID       | extension |
| ---------- | --------- |
| aca-fmt/1  | .123      |
| aca-fmt/2  | .doc      |
| aca-fmt/3  | .xls      |
| aca-fmt/4  | .mmap     |
| aca-fmt/5  | .ntf      |
| aca-fmt/6  | .adx      |
| aca-fmt/7  | .id       |
| aca-fmt/8  | .nsf      |
| aca-fmt/9  | .dat      |
| aca-fmt/10 | .mdb      |
| aca-fmt/11 | .mdb      |
| aca-fmt/12 | .mdb      |
| aca-fmt/13 | .mdb      |
| aca-fmt/14 | .mdb      |
| aca-fmt/15 | .mdb      |


