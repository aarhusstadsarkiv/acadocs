SAM-workflows er et lille program, som blandt andet kan generere mindre billedfiler til online visninger af de digitale masterfiler, som vi registerer i [SAM](../registration/sam.md).

## Fejlmeldinger og spørgsmål
Indberetning af fejl, spørgsmål med mere foretages gennem issues, [som beskrevet under brugen af Github](../development/github.md#indberetninger).

## Installation
SAM-workflows består af 1) et lille program (```sam_workflows.exe```-fil) og 2) en mappe med program-filer, som begge skal være på plads før programmet virker.

!!! warning "Bemærk"
    SAM-workflows virker kun på administrative Windows-PC'er, da programmet ofte skal arbejde med de filer, som findes i vores digitale arkiv. 

### sam_workflows.exe
Seneste udgave af [sam_workflows.exe ligger altid på denne side](https://github.com/aarhusstadsarkiv/sam-workflows/releases), hvorfra den kan downloades. Der kræves ingen installationsproces, men blot at ```sam_workflows.exe```-filen placeres i roden af din bruger-mappen:

!!! note "Eksempel"
    ```powershell
    C:\Users\{az-ident}\sam_workflows.exe
    ```
    Erstat ```{az-ident}```med dit eget.

### Programfilerne
Mappen med de tilhørende programfiler kan kun fås ved henvendelse til it-teamet, da de blandt andet indeholder autorisationskoder til vores filservice.

Når mappen med programfiler er modtaget, skal den også placeres direkte i roden af din bruger-mappe:

!!! note "Eksempel"
    ```powershell
    C:\Users\{az-ident}\.sam_workflows\
    ```

!!! warning "Bemærk"
    Det er vigtigt, at man ikke ændrer navnet på mappen. Den skal hedde **```.sam_workflows```** med foranstillet punktum.

### Mappe til csv-filer
Et eller flere workflows arbejder med csv-filer, som henholdsvis importeres fra eller eksporteres til  SAM, og for mange vil det være nemmere at oprette en fast mappe i roden af bruger-mappen til disse csv-filer:

!!! note "Eksempel"
    ```powershell
    C:\Users\{az-ident}\Workflows\
    ```
Dette er ikke et krav, men en anbefaling.

## Generering af access-filer
SAM-workflows kan aktuelt generere access-filer (jpg-filer) ud fra originale pdf'er eller billedfiler (jpg, tif, png, gif...).

### Csv-fil fra SAM
For at køre et SAM-relateret workflow, skal SAM-Workflow bruge den csv-fil, som er [eksporteret fra det relevante SAM-job](../registration/sam.md#eksport-af-metadata).

