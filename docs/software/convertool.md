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
Convertool har 4 optioner. Som udgangspunk ter disse ikke påkrævede, bortset fra option 4, som skal bruges hvis kommandoen (se kommandoer) `replacepdf` kaldes:

1. `--threshold` angiver det maksimale antal fejl, der må ske under konvertering
2. `--help` printer hjælp
3. `--version` printer versionen af Convertool
4. `--pdf_1_7` Angiver at convertool kun skal indlæse PDF 1.7 filer. Denne Option skal bruges sammen med kammandoen `replacepdf` for at konvertere og
                overskrive de PDF 1.7 filer som er tilbage efter første gennemkørsel.

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

!!! hint "Eksempel"
```powershell
    convertool --pdf_1_7=1 D:\filer\AVID.AARS.3.1\_metadata\files.db D:\filer\out replacepdf
```

### Kommandoer
Convertool har 3 kommandoer kaldet `main`, `tiff` og `replacepdf`. 

* main

Denne kommando konverterer filer til Aarhus Stadsarkivs prædefinerede Master-formater. Disse er blandt andre Open Document Text for alle Word-lignende filer, PDF/A for PDF-filer, og TIFF for billedfiler.

!!! hint "Eksempel"
    ```powershell
    convertool D:\filer\AVID.AARS.3.1\_metadata\files.db D:\filer\out main
    ```
I fremtiden skal Convertool også kunne håndtere konvertering af alle Master-filer til arkiveringsformater, således der kan genereres juridiske afleveringer. Dette bør være en separat kommando.

* tiff

Denne kommando konverterer Master-filer til Tagged Image File Format (TIFF) i overenstemmelse med gældende lovgivning. En passende bitdybe og komprimering som overholder lovgivningen bliver valgt af værktøjet, og behøver ikke at specificeres. Syntaksen for agt bruge kommandoen er den samme som `main`, hvor `main` erstattes med `tiff`.


* replacepdf

Denne kommando køre funktionen `replace` fra replace_pdf.py på de restederende PDF 1.7 filer som indlæses ved brug af optionen `--pdf_1_7`.
Da funktionen bruger GhostScript, kan den multiprocesses og køre flere konverteringer parallelt. Et eksempel på brugen kan ses under beskrivelsen af `--pdf_1_7`.


## Konvertering
Når Convertool sættes i gang med at konvertere, er der ikke umiddelbart behov for yderligere brugerinput. Konvertering tager i gennemsnit 2s/fil, og det kan derfor være fordelagtigt at have processen kørende i baggrunden, mens man tager sig af andre opgaver.

!!! attention "Bemærk"
    Convertool gør *meget* brug af LibreOffices CLI. Det er ikke testet, hvorvidt brug af LibreOffice samtidig med konvertering kan foregå uden problemer. Vær derfor forsigtig, hvis LibreOffice skal benyttes sideløbende med konvertering.

Under konvertering skriver Convertool til tabellen `_ConvertedFiles` i den `files.db`, som Convertool læser fra. I `files.db` laves også et såkaldt view, `_NotConverted`, der på brugervenlig vis angiver de filer, der endnu ikke er konverteret. `_ConvertedFiles`-tabellen ser ud som følger.

**`_ConvertedFiles`**

| column  | required | type  | description                       |
| ------- | -------- | ----- | --------------------------------- |
| file_id | true     | `int` | Fremmednøgle til `Files`-tabellen |
| uuid    | true     | `str` | Universally Unique ID 4           |

`_NotConverted`-viewet baserer sig på `_ConvertedFiles` og `Files`. Det ser ud som følger.

**`_NotConverted`**

| column    | required | type  | description               |
| --------- | -------- | ----- | ------------------------- |
| id        | true     | `int` | ID fra `Files`-tabellen   |
| uuid      | true     | `str` | Universally Unique ID 4   |
| path      | true     | `str` | Fuld sti til filen        |
| aars_path | true     | `str` | Sti med rod i AARS-mappen |
| puid      | false    | `str` | PRONOM ID                 |
| signature | false    | `str` | Filsignatur               |
| warning   | false    | `str` | Advarsel, hvis relevant   |


I [afsnittet om fejlrettelser](#fejlrettelser) beskrives det, hvorledes `_ConvertedFiles` manuelt opdateres, hvis der er behov herfor.

Under konvertering skrives også til en logfil, `convertool.log`, der kan findes i `_metadata` folderen under `OUTDIR\AARS-ID`.

!!! hint "Eksempel"
    Antag, at originalfilerne `D:\filer\AVID.AARS.3.1` bliver konverteret som følger.
    ```powershell
    convertool D:\filer\AVID.AARS.3.1\_metadata\files.db D:\filer\out main
    ```
    `_ConvertedFiles` og `_NotConverted` kan nu findes i databasefilen `D:\filer\AVID.AARS.3.1\_metadata\files.db`, mens logfilen kan findes i mappen `D:\filer\out\AVID.AARS.3.1\_metadata`.



## Fejlrettelser
Under konvertering kan forskellige fejl opstå. Disse vil typisk ikke stoppe et konverteringsjob, med mindre antallet af fejl overskrider det tilladte. Fejl opdages ved at kigge i `convertool.log`-filen, hvori de vil stå som `WARNING`. Når et job er afsluttet, skrives også en oversigt over antallet af fejl til sidst i log-filen.

!!! hint "Eksempel"
    I det følgende ses et uddrag af en `convertool.log`-fil. Succesfuld konvertering angives med `INFO`, fejl angives med `WARNING`, og til slut ses en oversigt over antal fejl.
    ```log
    2021-02-25 19:25:48 INFO: Starting conversion of file.pdf
    2021-02-25 19:25:55 INFO: Converted file.pdf successfully.
    2021-02-26 09:19:36 WARNING: Failed to convert file.tif: Failed to save file.tif as TIF with error: Error setting from dictionary
    2021-02-25 19:25:55 INFO: Finished conversion of 11438 files with 15 issues, 7 of which were critical.
    ```

Der skelnes mellem kritiske og ikke-kritiske fejl. Ikke-kritiske fejl er typisk timeouts fra LibreOffice -- disse indikerer, at LibreOffice ikke havde tid nok til at konvertere filen, men at filen med al sandsynlighed er velformet, og kan konverteres manuelt. Nogle gange er det også fordelagtigt at give LibreOffice en chance mere, ved at køre konverteringsjobbet igen.

Når filer konverteres manuelt, er det vigtigt at opdatere `_ConvertedFiles`-tabellen, således man bevarer oversigten over, hvilke filer der er konverterede. Dette gøres ved at indsætte den manuelt konverterede fils ID og UUID, som vist i følgende eksempel.

!!! hint "Eksempel"
    Antag, at en fil med ID `144` og UUID `cd61494b-4547-43e7-97c6-adbafc6e8bd` ikke er konverteret. Den fremgår i `_NotConverted` som følger.

    | id  | uuid                                | path                        | aars_path          | puid    | signature                | warning |
    | --- | ----------------------------------- | --------------------------- | ------------------ | ------- | ------------------------ | ------- |
    | 144 | cd61494b-4547-43e7-97c6-adbafc6e8bd | D:\files\AARS.TEST\file.tif | AARS.TEST\file.tif | fmt/353 | Tagged Image File Format |         |

    Filen konverteres nu manuelt, og `_ConvertedFiles` opdateres med følgende SQL-statement.

    ```sql
    INSERT INTO _ConvertedFiles VALUES (144, "cd61494b-4547-43e7-97c6-adbafc6e8bd")
    ```

    Herefter fremgår filen ikke længere i `_NotConverted`-viewet.

Til tider kan filer ikke konverteres, fordi de viser sig at være korrupte eller på anden vis fejlbehæftede. Alle filer, der ikke kan eller skal konverteres, skal noteres i et dokument kaldet "Konverteringsfejl", og herudover skal en TIFF-fil, der forklarer den specifikke fejl, bruges som erstatning for den konverterede fil. Disse erstatningsfiler kan findes [her](https://github.com/aarhusstadsarkiv/convertool/tree/master/convertool/core/replacements). Til slut skal dette dokument gemmes som TIFF og fremgå i kontekstdokumentationen. Skabelonen til dokumentet kan findes [her](https://github.com/aarhusstadsarkiv/acadocs/blob/master/files/Konverteringsfejl.odt).

!!! hint "Eksempel"
    Dokumentet "Konverteringsfejl" kan se ud som følger. 
    ![Konverteringsfejl](../img/konverteringsfejl.png)

## Arbejdsgang
Som opsummering kommer her en oversigt over arbejdsgangen med Convertool.

1. Åbn PowerShell
2. Skriv `cd sti\til\data` f.eks. `cd E:\batch_7\AVID.AARS.61.1`
3. Kør `convertool .\_metadata\files.db OUTDIR main`, hvor `OUTDIR` f.eks. er `E:\batch_7\out`
4. Tjek fildatabasen
5. Tjek logfilen i `_metadata\convertool.log` for `WARNING`s. Ved ikke-kritiske fejl som f.eks. timeout køres convertool igen som i trin 3.
6. Tjek database og logfil igen. Referér til eksemplet i [fejlrettelser](#fejlrettelser), hvis der skal laves manuelle rettelser.
7. Hvis der er filer, som ikke kan konverteres, skal dette noteres i dokumentet "Konverteringsfejl", som beskrevet i [fejlrettelser](#fejlrettelser).