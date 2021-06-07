# Tiffany
Tiffany er en CLI (Command-Line Interface), som benyttes til at udfylde relevante 'huller' i MasterDocuments med passende template filer, således de overholder de juridiske krav til en aflevering.

!!! warning "Bemærk"
Tiffany er specifikt tilrettet Improventos måde at aflevere originalfiler på, så skal værktøjet bruges til andre afleveringer end Notes-baser, skal man være opmærksom på dette.

!!! danger "Advarsel"
    Det er vigtigt, at man i videst mulige omfang gennemfører konverteringsprocessen med [convertool](convertool.md) inden man bruger Tiffany, da man ellers kan komme til at erstatte ikke-konverterede filer med dummyfiler.

## Installation
Tiffany skal installeres via [releases](https://github.com/aarhusstadsarkiv/tifler/releases) på GitHub. Download først den seneste `.whl`-fil, og installér herefter denne via `pip`.

!!! hint "Eksempel"
    ```powershell
    pip install --user C:\Downloads\tifler-0.1.1-py3-none-any.whl
    ```

!!! attention "Bemærk"
    Installation via `.whl`-filer kræver, at pakken `wheel` er installeret. Dette skal kun foregå første gang man har behov for at installere wheels, og det gøres som følger.
    ```powershell
    pip install --user wheel
    ```

## Opdatering
Når tifler skal opdateres, bruges igen `pip` med en ny `.whl`-fil og `--upgrade`.

!!! hint "Eksempel"
    ```powershell
    pip install --user --upgrade C:\Downloads\tifler-[new version]-py3-none-any.whl
    ```

## Brug
Som **commandline** værktøj, og skal Tiffany benyttes direkte i for eksempel PowerShell. CLI'en er opbygget som følger.

```powershell
tiffany [OPTIONS] [COMMANDS] [ARGS]
```

Tiffany har to optioner, som hverken tager efterfølgende kommmandoer eller argumenter:

1. `--help` printer hjælp
2. `--version` printer versionen af Tiffany

!!! hint "Eksempel"
    ```powershell
    tiffany --help
    tiffany --version
    ```

## Kommandoer
Tiffany har to kommandoer: `substitute` or `list`.

### Substitute
`substitute` erstatter tomme, ikke-bevaringsværdige, kodeordsbeskyttede eller korrumperede filer med dummyfiler, så arkiveringsversioner, hvor vi selv står for konvertering af filer, kan godkendes.

#### Argumenter
`substitute` tager to argumenter: `DB_FILE` og `OUT_DIR`.

- `DB_FILE` angiver stien til den `files.db` fil, der oprindeligt er genereret af [Digiarch](digiarch.md) og som ligger i '_metadata'-mappen direkte under 'OriginalDocuments'-mappen)
- `OUT_DIR` angiver stien, hvor de allerede konverterede filer fra [Convertool](convertool.md) er gemt.

!!! hint "Eksempel"
    ```powershell
    tiffany substitute .\AVID.AARS.3.1\OriginalDocuments\_metadata\files.db .\AVID.AARS.3.1\MasterDocuments
    ```
#### Proces
`substitute` gennemgår overordnet to processer i forsøget på at finde ud af, hvilke dummyfiler, der skal/kan sættes hvor. Først arbejder den med `files.db `-databasens `_EmptyDirectories`-tabel for at finde kodeordsbeskyttede filer, og dernæst arbejdes der med et view kaldet `_NotConverted`, der lister alle filer, som ikke er konverterede.

##### Kodeordsbeskyttede filer
Hvis et originalt dokument er kodeordsbeskyttet, vil dokumentet ikke blive medtaget i den oprindelige aflevering fra Improvento, og der vil således blot være en tom folder i den `docCollection`, hvor dokumentet skulle have været. Hver tom folder i de enkelte `docCollection` er registreret i `_EmptyDirectories`, så for hver række i denne tabel gøres følgende:

1. indsæt den relevante tif-template, hvor den originale fil skulle have været.
2. slet rækken i `_EmptyDirectories`-tabellen.

Efter endt proces vil `_EmptyDirectories`-tabellen være tom, og de resterende processer arbejder med `_NotConverted`-view'et.

##### Korrumperede filer
Ødelagte originalfiler, der ikke kan åbnes, figurerer i `_NotConverted` med følgende puid `aca-error/2`. Disse filer erstattes af den relevante dummyfil, og tilføjes efterfølgende til `_ConvertedFiles`-tabellen.

##### Ikke-bevaringsværdige filer
For hver række i `_NotConverted`-view'et undersøger Tiffany om filens *puid* eller filudvidelse figurerer i den interne `blacklist.py`-fil, der lister alle de puid'er og filudvidelser, som vi har vurderet til at være kassable. Hvis en fil har et *puid* eller en filudvidelse, som vi har blacklisted, erstattes den af den relevante dummyfil, og filen tilføjes til `_ConvertedFiles`-tabellen.

##### Tomme filer
[Convertool](convertool.md) håndterer aktuelt erstatningen af tomme filer med den relevante dummyfil. På sigt skal dette ændres så Tiffany også tager sig af denne opgave. Selvom `convertool` allerede har håndteret denne proces, vil eventuelle tomme filer stadig fremgå af i den id'liste, som Tiffanys `list`-kommando genererer.

### List
`list`-kommandoen oplister alle id'er på filer, der er blevet erstattet af en dummyfil, fordelt på fejltype.

#### Argumenter
`list` tager ét argument: `LOG_FILE`, der angiver stien til Tiffanys logfil (se afsnittet [logging](#logging)):

!!! hint "Eksempel"
    ```powershell
    tiffany list .\AVID.AARS.3.1\MasterDocuments\_metadata\tiffany.log
    ```

#### Proces
 `list` parser hele logfilen og printer id'er på alle de filer, der på grund af en eller anden fejl, er blevet erstattet af dummyfiler:
 
!!! hint "Eksempel"
    ````cmd
    Empty files:
    23
    542
    761
    794
    Password-protected files:
    43
    321
    No corrupted files substituted with template
    Disposable files:
    907
    Finished listing templates used
    See the logfile and/or files.db for any remaining errors
    ````

Listen af id'er kan kopieres og indsættes i det kontekstdokument med oplistning af konverteringsfejl, som skal vedlægges enhver arkiveringsverion.

## Logging
Hver gang man kører `substitute` vil Tiffany oprette (eller skrive til en eksisterende) logfil i `_metadata`-mappe i roden af `OUT_DIR`. Logformatet er følgende: timestamp, loglevel, description, path to relevant file/folder. Eks.:

```log
2021-06-03 15:39:44 INFO: Started substituting relevant files with tif-templates
2021-06-03 15:39:44 INFO: Empty file substituted with template PATH: .AVID.AARS.1000.1\MasterDocuments\AVID.AARS.1000.1\OriginalDocuments\docCollection1\1\text.txt
2021-06-03 15:39:44 INFO: Replaced password-protected file with template PATH: .AVID.AARS.1000.1\MasterDocuments\AVID.AARS.1000.1\OriginalDocuments\docCollection1\4
2021-06-03 15:39:44 INFO: Replaced disposable file with template PATH: .AVID.AARS.1000.1\OriginalDocuments\docCollection1\8\gs919w64.exe
2021-06-03 15:39:44 INFO: Finished substituting with template-files
```

