# Convertool
Convertool er en CLI (Command-Line Interface), som benyttes til at konvertere filer, således de overholder Aarhus Stadsarkivs arkiveringskrav. Der læses fra DB-filen produceret af [Digiarch](digiarch.md).

## Installation
Convertool skal installeres via [releases](https://github.com/aarhusstadsarkiv/convertool/releases) på GitHub. Download først den seneste `.whl`-fil, og installér herefter denne via `pip`.

!!! hint "Eksempel"
    ```powershell
    pip install --user C:\Downloads\convertool-0.3.5-py3-none-any.whl
    ```

!!! attention "Bemærk"
    Installation via `.whl`-filer kræver, at pakken `wheel` er installeret. Dette skal kun foregå første gang man har behov for at installere wheels, og det gøres som følger.
    ```powershell
    pip install --user wheel
    ```

Når Convertool skal opdateres, bruges igen `pip` med en ny `.whl`-fil og `--upgrade`.

!!! hint "Eksempel"
    ```powershell
    pip install --user --upgrade C:\Downloads\convertool-[new version]-py3-none-any.whl
    ```

## Forudsætninger
Convertool gør brug af [Ghostscript](ghostscript.md) og [LibreOffice](libreoffice.md), og det er derfor vigtigt at sikre sig, at disse er installeret som beskrevet i de respektive guides.

!!! attention "Bemærk"
    Det er især vigtigt, at de relevante Ghostscript- og LibreOffice-foldere eksisterer i brugerens `PATH`-miljøvariabel. Vær derfor ekstra opmærksom på, at disse trin i installationen er udført korrekt.

## Opbygning
Convertool er en CLI, og skal derfor benyttes direkte i for eksempel PowerShell. CLI'en er opbygget som følger.

```powershell
convertool [OPTIONS] [ARGS] [COMMAND] 
```

Argumenter, optioner, og kommandoer er beskrevet i det følgende.

### Argumenter
Convertool har to inputargumenter, som begge er påkrævede: `FILES` og `OUTDIR`. `FILES` skal angive stien til den `files.db` fil, der tidligere i processen er genereret af [Digiarch](digiarch.md), mens `OUTDIR` skal angive stien, hvori de konverterede filer skal gemmes.

!!! hint "Eksempel"
    ```powershell
    convertool D:\filer\AVID.AARS.3.1\_metadata\files.db D:\filer\out [COMMAND]
    ```

Convertool genererer selv en mappe baseret på [AARS-ID'et](../acquisition/acquiring-digital-material.md#identifikator), og det er derfor kun nødvendigt at angive en overordnet folder i `OUTDIR`.

### Optioner
Convertool har tre optioner, der, som navnet antyder, ikke er påkrævede:

1. `--threshold` angiver det maksimale antal fejl, der må ske under konvertering
2. `--help` printer hjælp
3. `--version` printer versionen af Convertool

`--threshold` kan bruges, hvis man ønsker, at Convertool accepterer et andet antal fejl end standardværdien. Standardværdien udregnes som kvadratroden af det totale antal filer. Hvis få filer konverteres, kan det være fordelagtigt at sætte `--threshold` til `0`, da konvertering så stopper, første gang en fejl hænder. Det er dog ikke anbefalet for større mængder filer.

!!! hint "Eksempel"
    ```powershell
    convertool --threshold=0 D:\filer\AVID.AARS.3.1\_metadata\files.db D:\filer\out [COMMAND]
    ```

`--help` og `--version` kan som de eneste kaldes *uden* at angive argumenter, da de blot giver information om selve CLI'en.

!!! hint "Eksempel"
    ```powershell
    convertool --help
    convertool --version
    ```

### Kommandoer
Convertool har pt. én kommando kaldet `main`. Denne kommando konverterer filer til Aarhus Stadsarkivs prædefinerede Master-formater. Disse er blandt andre Open Document Text for alle Word-lignende filer, PDF/A for PDF-filer, og TIFF for billedfiler.

!!! hint "Eksempel"
    ```powershell
    convertool D:\filer\AVID.AARS.3.1\_metadata\files.db D:\filer\out main
    ```
I fremtiden skal Convertool også kunne håndtere konvertering af alle Master-filer til arkiveringsformater, således der kan genereres juridiske afleveringer. Dette bør være en separat kommando.


## Konvertering

## Fejlrettelser