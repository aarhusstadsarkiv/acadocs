# Digiarch
Digiarch er en CLI (Command-Line Interface), som benyttes til at indeksere og identificere digitale filer, vi modtager til bevaring. CLI'en producerer en [SQLite](https://www.sqlite.org/index.html) database kaldet `files.db`, som indeholder oversigter og informationer om de relevante processerede filer.


## Installation og opdatering
Digiarch skal installeres via master branch med pipx[pipx.md]:

```powershell
pipx install git+https://github.com/aarhusstadsarkiv/digiarch.git
```

Test efterfølgende installationen med følgende kommando:

```powershell
digiarch --version
```

Når man skal opdatere Digiarch, bruges `pipx` med `--upgrade`:

```powershell
pipx upgrade git+https://github.com/aarhusstadsarkiv/digiarch.git
```


## Forudsætninger
Digiarch bruger [Siegfried](https://github.com/richardlehane/siegfried) til identifikation af filer, og det er derfor en forudsætning for fuld brug af Digiarch, at Siegfried er installeret og tilgængelig i `PATH` på systemet. Dette opnås nemmest ved at installere Siegfried via Go. Det anbefales, at Go installeres med [Chocolatey](chocolatey.md) som følger.

```powershell
choco install golang
```
!!! attention "Bemærk"
    Chocolatey skal **altid** bruges via en administrativ PowerShell.

Givet en fungerende version af Go, foregår installation af Siegfried som følger.

```powershell
go install github.com/richardlehane/siegfried/cmd/sf@latest
sf -update
```
Hvis Siegfried skal opdateres, foregår dette også gennem Go:

```powershell
go install github.com/richardlehane/siegfried/cmd/sf@latest
sf -update
```

Alternative installationsmetoder kan findes på [Siegfrieds GitHub](https://github.com/richardlehane/siegfried#install). Hvis ovenstående installation virker, men Siegfried ikke bliver opdateret ordentligt, kan det skyldes, at man har en manuel installation i sin `PATH`-miljøvariabel. Placering af Siegfriedinstallationer tilgængelig i `PATH` kan findes som følger:

```powershell
where.exe sf
```


## Opbygning
Digiarch er en CLI, og skal derfor benyttes direkte i for eksempel PowerShell. CLI'en er opbygget som følger.

```powershell
digiarch [OPTIONS] [ARGS] [COMMAND] 
```

Argumenter, optioner, og kommandoer er beskrevet i det følgende.

### Argumenter
Digiarch har ét inputargument: stien til filerne, som skal behandles. Dette argument er påkrævet, og Digiarch kan køres med kun dette som input. Det forventes, at stien har et [AARS-ID](../acquisition/acquiring-digital-material.md#identifikator).

!!! hint "Eksempel"
    ```powershell
    digiarch D:\filer\AVID.AARS.3.1
    ```

Når Digiarch køres som i ovenstående, indekseres filerne uden yderligere processering. Indeksering er yderligere forklaret [her](#indeksering).

### Optioner
Digiarch har tre optioner, der, som navnet antyder, ikke er påkrævede:

1. `--reindex` genindekserer filer
2. `--help` printer hjælp
3. `--version` printer versionen af Digiarch

`--reindex` er brugbar, hvis man har lavet manuelle ændringer i filerne, som Digiarch har indekseret. I [afsnittet om fejlrettelser](#fejlrettelser) beskrives situationer, hvori denne option kan være relevant.

!!! hint "Eksempel"
    ```powershell
    digiarch --reindex D:\filer\AVID.AARS.3.1
    ```

`--help` og `--version` kan som de eneste kaldes *uden* at angive en sti, da de blot giver information om selve CLI'en.

!!! hint "Eksempel"
    ```powershell
    digiarch --help
    digiarch --version
    ```


### Kommandoer
Digiarch har to kommandoer: `process` og `fix`. Førstnævnte processerer filerne, hvor checksum udregnes og identifikation af filformat foregår, mens sidstnævnte kan fikse prædefinerede filendelsesfejl. Filidentifikation skal forelægge, før `fix` kan finde fejl at rette.

!!! hint "Eksempel"
    ```
    digiarch D:\filer\AVID.AARS.3.1 process
    digiarch D:\filer\AVID.AARS.3.1 fix
    ```

`process` og `fix` kan også kaldes i en såkaldt *chain*, således man både kan processere og rette fejl ad én gang.

!!! hint "Eksempel"
    ```
    digiarch D:\filer\AVID.AARS.3.1 process fix
    ```


## Indeksering
Indeksering af filer er en grundlæggende funktion i Digiarch. Når Digiarch kaldes første gang i en folder med arkiveringsparate filer, gennemgås folderstrukturen rekursivt, og et indeks over filerne opbygges. Dette indeks gemmes i databasen `files.db`, som er beskrevet yderligere i [afsnittet om fildatabaser](#fildatabase). 

Under indeksering tjekkes også, om der er tomme foldere, og om foldere indeholder flere filer, da vi typisk forventer af arkiveringsparate afleveringer, at dette ikke er tilfældet.

Digiarch genbruger dette filindeks, når filerne efterfølgende skal behandles. Det er derfor vigtigt, at indekset holdes opdateret ved eventuelle ændringer i filstrukturen. Man kan genindeksere ved brug af optionen `--reindex`, som beskrevet [her](#optioner).

## Processering
Processering af filer til digital arkivering er Digiarchs hovedfunktion. Under processering bliver der udregnet SHA-256 checksummer, og filerne bliver identificeret via [Siegfried](https://github.com/richardlehane/siegfried), således de kan knyttes til en [PRONOM-ID](http://www.nationalarchives.gov.uk/PRONOM/Default.aspx). 

PRONOM-ID'et er centralt for det videre arbejde med filer, da det giver et entydigt svar på, hvilken filtype, der er tale om. Under processering tjekkes også for tomme filer, som gives en fejlbeskrivelse i PRONOM-ID'et. En filidentifikation er opbygget som følger.

|              | PUID     | Signature                                  | Warning            |
| ------------ | -------- | ------------------------------------------ | ------------------ |
| **Eksempel** | `fmt/18` | Acrobat PDF 1.4 - Portable Document Format | Extension mismatch |

I ovenstående tabel bruges `fmt/18` som eksempel. Dette er et Acrobat PDF version 1.4 dokument. I dette tilfælde er der også knyttet en advarsel på, fordi filen i dette eksempel ikke har den korrekte filendelse. De fleste filer i en aflevering vil have et PUID og en signatur uden en tilknyttet advarsel. Det er derfor primært vigtigt at tage hånd om de filer, der har tilknyttede advarsler. [Fildatabasen](#fildatabase), som Digiarch genererer, indeholder derfor blandt andet brugervenlige oversigter over filer, der har tilknyttede advarsler.

## Fildatabase
Som tidligere nævnt genererer Digiarch en fildatabase, som indeholder oplysninger om de indekserede og eventuelt processerede filer. Denne database hedder `files.db` og befinder sig i `_metadata`-mappen. `files.db` er en [SQLite](https://www.sqlite.org/index.html) database, og det kan derfor anbefales at installere [DB Browser for SQLite](https://sqlitebrowser.org/), når man skal arbejde med fildatabaser fra Digiarch.

I det følgende beskrives de tabeller og views, der eksisterer i `files.db`, når Digiarch har færdiggjort en kørsel.

### Tabeller
`files.db` fødes med fire tabeller: `Files`, `Metadata`, `_EmptyDirectories`, og `_MultipleFiles`. Sidstnævnte er præfikseret med `_` for at facilitere mere logisk sortering i DB Browser for SQLite. Tabellerne er bygget op som følger.

**`Files`**

Denne tabel er det primære filindeks, og indeholder oplysninger om alle filer fundet i filstrukturen, Digiarch behandler.

| column    | required | type  | description               |
| --------- | -------- | ----- | ------------------------- |
| id        | true     | `int` | Primærnøgle               |
| uuid      | true     | `str` | Universally Unique ID 4   |
| path      | true     | `str` | Fuld sti til filen        |
| checksum  | false    | `str` | SHA-256 checksum          |
| aars_path | true     | `str` | Sti med rod i AARS-mappen |
| puid      | false    | `str` | PRONOM ID                 |
| signature | false    | `str` | Filsignatur               |
| warning   | false    | `str` | Advarsel, hvis relevant   |

`checksum`, `puid`, `signature`, og `warning` bliver udfyldt under processering. Disse vil derfor være `NULL`, hvis der kun er kørt indeksering af filer.

**`Metadata`**

Denne tabel indeholder en oversigt over, hvornår Digiarch sidst er blevet kørt på de relevante filer.

| column        | required | type       | description                        |
| ------------- | -------- | ---------- | ---------------------------------- |
| last_run      | true     | `datetime` | Tidspunkt for seneste kørsel       |
| processed_dir | true     | `str`      | Den folder hvori Digiarch er kaldt |
| file_count    | false    | `int`      | Antal indekserede filer            |
| total_size    | false    | `str`      | Samlet filstørrelse                |

**`_EmptyDirectories_`**

Denne tabel indeholder eventuelle mapper i filstrukturen, der er tomme.

| column | required | type  | description                        |
| ------ | -------- | ----- | ---------------------------------- |
| path   | true     | `str` | Den fulde sti til den tomme folder |

**`_MultipleFiles_`**

Denne tabel indeholder eventuelle mapper i filstrukturen, der indeholder mere end én fil.

| column | required | type  | description                                |
| ------ | -------- | ----- | ------------------------------------------ |
| path   | true     | `str` | Den fulde sti til folderen med flere filer |

### Views
`files.db` fødes med to views: `_IdentificationWarnings` og `_SignatureCount`. Disse er bygget op som følger.

**`_IdentificationWarnings`**

Dette view har samme opbygning som `Files`-tabellen, men indeholder kun de filer, der har værdier i `warning`-kolonnen. Viewet er brugbart i forbindelse med fejlrettelser.

| column    | type  | description               |
| --------- | ----- | ------------------------- |
| id        | `int` | Primærnøgle               |
| uuid      | `str` | Universally Unique ID 4   |
| path      | `str` | Fuld sti til filen        |
| checksum  | `str` | SHA-256 checksum          |
| aars_path | `str` | Sti med rod i AARS-mappen |
| puid      | `str` | PRONOM ID                 |
| signature | `str` | Filsignatur               |
| warning   | `str` | Advarsel                  |

**`_SignatureCount`**

Dette view viser en oversigt over antallet af signaturer og PUID'er, der findes i fildatabasen. Viewet er brugbart til at vurdere, hvilke filer der skal håndteres, og hvorledes konvertering skal foregå.

| column    | type  | description  |
| --------- | ----- | ------------ |
| puid      | `str` | PRONOM ID    |
| signature | `str` | Filsignatur  |
| count     | `int` | Samlet antal |


## Fejlrettelser
Når filer til arkivering er blevet indekserede og processerede, vil der typisk være en andel, der er fejlbehæftede. Der er overordnet fem typer fejl, som bliver gennemgået i det følgende.

### Extension mismatch
Denne fejl indikerer, at det er lykkedes at finde en filsignatur, men at den aktuelle filendelse ikke stemmer overens med den forventede. Dette kan for eksempel være en PDF-fil, der er gemt som `.tiff`. 

Digiarch-kommandoen `fix` kan med fordel bruges til at rette [visse prædefinerede](https://github.com/aarhusstadsarkiv/digiarch/blob/master/digiarch/_data/ext_map.json) *Extension mismatch* fejl. Det er dog ikke alle fejl, `fix` kan rette, og man må derfor belave sig på, at nogle filer skal rettes manuelt, hvis muligt. Dette gøres typisk ved at slå PRONOM-ID'et op i [PRONOM-databasen](http://www.nationalarchives.gov.uk/PRONOM/PUID/proPUIDSearch.aspx?status=new), hvor den korrekte filendelse vil være at finde under "Signatures".

!!! attention "Bemærk"
    Når der laves manuelle rettelser af filer, skal Digiarch køres igen med `--reindex`-optionen, således de opdaterede filer bliver indekseret korrekt.

### Match on text only; extension mismatch
Denne fejl indikerer, at der er tale om en tekstfil, men at filendelsen ikke stemmer overens med de forventede, e.g. `.txt`, `.csv` mv. Disse filer skal manuelt inspiceres, og kan typisk gemmes som `.txt` på sikker vis, med mindre det er tydeligt, at der er tale om en anden filtype.

### Match on extension only
Denne fejl bruges, når filsignaturen ikke er entydigt bestemt, men derimod primært beror på filendelsen. Dette er for eksempel tilfældet med `.csv` filer, da de bytemæssigt blot er tekstfiler, og man derfor bruger filendelsen til den endelige signatur. Der kan sagtens være tale om en korrekt identifikation, men det er vigtigt at forholde sig til. 

### No match
Denne fejl indikerer, at der ikke er fundet noget PRONOM-ID og derfor ikke nogen filsignatur. Dette kan betyde flere ting: filen er korrupt, filen er gemt med fejl men stadig konverterbar, eller filtypen findes ikke i PRONOM-databasen. I alle tilfælde er der behov for manuel inspektion. 

!!! attention "Bemærk"
    Til tider er PDF-filer gemt forkert på en måde, hvor der er en masse tomme bytes efter `%EOF`-signaturen til sidst i filen. Disse filer kan typisk reddes ved at blive åbnet i en PDF-viewer og gemt igen, hvorefter de tomme bytes er væk. 

Hvis en fil viser sig at være korrupt, kan man med fordel tildele den PUID'et `aca-error/2` med signaturen `File is corrupt`. Hermed kan Convertool automatisk indføre erstatningsfiler under konvertering. Det er vigtigt at dokumentere korrupte filer i `Konverteringsfejl`-dokumentet.
### Empty file
Denne fejl indikerer, at filen er tom, og tomme filer har per definition ikke en gyldig filsignatur. Tomme filer skal dokumenteres i `Konverteringsfejl`-dokumentet, men man behøver ikke gøre yderligere; Convertool indfører automatisk erstatningsfiler.

## Arbejdsgang
Som opsummering kommer her en oversigt over arbejdsgangen med Digiarch.

1. Åbn PowerShell
2. Skriv `cd sti\til\data` f.eks. `cd E:\batch_7\AVID.AARS.61.1`
3. Kør `digiarch . process`
4. Tjek fildatabasen `files.db` som befinder sig i `_metadata`-mappen
5. Hvis rettelse af filendelsesfejl er nødvendigt, kør da `digiarch . fix`
6. Tjek fildatabasen igen
7. Hvis der laves manuelle rettelser, kør da `digiarch --reindex . process`
   
