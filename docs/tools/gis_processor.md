# Gisprocessor

gis_processor bruges til at flytte alle filerne fra de enkelte GIS-projekter sammen i en mappe for hvert GIS-projekt, så de kan identificeres og konverteres rigtigt.

## Installation
Gisprocessor skal installeres via master branch med [pipx](pipx.md):

```powershell
pipx install git+https://github.com/aarhusstadsarkiv/gis_processor.git
```

Test efterfølgende installationen med følgende kommando:

```powershell
gisprocessor --version
```

Når man skal opdatere gisprocessor, bruges `pipx` med `--upgrade`:

```powershell
pipx upgrade gisprocessor
```

## Brug
Gisprocessor har to kommandoer, nemlig `g-json` og `move`. Når man er igang med at klargøre en aflevering, skal begge kommandoer køres (først `g-json` dernæst `move`)

`g-json` genererer en json-fil der viser, hvad værktøjet flytter.

`move` flytter GIS-filerne i json-filen.

Når man kører `g-json`-kommandoen spørger den om stien til av.db-filen. Den kan findes i den _metadata-mappe der ligger i roden af afleveringen. Når j-son-filen er dannet inspiceres denne før move-kommandoen køres, for at sikre at de rette filer bliver flyttet.

!!! hint "Eksempel" 
    ```powershell
    gisprocessor g-json

    Enter full path to av.db file: D:\AVID.AARS.84.1\_metadata\AVID.AARS.84.av.db
    ```

Den af g-json-kommandoen genererede json-fil findes også i denne _metadata mappe. move-kommandoen spørger om stien dertil.