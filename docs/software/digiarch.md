# Digiarch
Digiarch er en CLI (Command-Line Interface), som benyttes til at indeksere og identificere digitale filer, vi modtager til bevaring. CLI'en producerer en [SQLite](https://www.sqlite.org/index.html) database kaldet `files.db`, som indeholder oversigter og informationer om de relevante processerede filer.



## Installation
Digiarch er tilgængelig på [PyPI](https://pypi.org/project/digiarch) og kan derfor installeres direkte via `pip`, såfremt man har en fungerende Pythoninstallation. 

```powershell
pip install --user digiarch
```

Når man skal opdatere Digiarch, bruges `pip` med `--upgrade`

```powershell
pip install --user --upgrade digiarch
```

!!! attention "Bemærk"
    `pip` kan også bruges uden `--user`. Det anbefales dog på det kraftigste at bruge `--user`, da man så undgår at forurene systemets Pythonmiljø. Når man laver brugerinstallationer på denne måde, er det vigtigt at sikre sig, at man har brugerens Python-sti i sine systemmiljøvariable. Denne vil typisk være 
    ```
    C:\Users\[bruger]\AppData\Roaming\Python\Python[version]\Scripts
    ```



## Forudsætninger
Digiarch bruger [Siegfried](https://github.com/richardlehane/siegfried) til identifikation af filer, og det er derfor en forudsætning for fuld brug af Digiarch, at Siegfried er installeret og tilgængelig i `PATH` på systemet. Dette opnås nemmest ved at installere Siegfried via Go. Det anbefales, at Go installeres med [Chocolatey](chocolatey.md). 

Givet en fungerende version af Go, foregår installation af Siegfried som følger.

```powershell
go get github.com/richardlehane/siegfried/cmd/sf
sf -update
```
Alternative installationsmetoder kan findes på [Siegfrieds GitHub](https://github.com/richardlehane/siegfried#install).

## Opbygning
Digiarch er en CLI, og skal derfor benyttes direkte i for eksempel PowerShell. CLI'en er opbygget som følger.

```powershell
digiarch [OPTIONS] [ARGS] [COMMAND] 
```

Argumenter, optioner, og kommandoer er beskrevet i det følgende.

### Argumenter
Digiarch har ét inputargument: stien til filerne, som skal behandles. Dette argument er påkrævet, og Digiarch kan køres med kun dette som input. Det forventes, at stien har et AARS-id.

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

## Processering

## Fildatabase

## Fejlrettelser
