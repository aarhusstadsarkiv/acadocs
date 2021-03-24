SAM-workflows er et lille program, som blandt andet kan generere mindre billedfiler til online visninger af de digitale filer, som vi registerer i [SAM](../registration/sam.md).

## Installation
SAM-workflows virker kun på administrative Windows-PC'er, da programmet ofte skal arbejde med de filer, som findes i vores digitale arkiv. SAM-workflows består af et lille program (```.exe```-fil) og en mappe med program-filer, som begge skal være på plads før programmet virker.

### sam_workflows.exe
Seneste udgave af [sam_workflows.exe ligger altid på denne side](https://github.com/aarhusstadsarkiv/sam-workflows/releases), hvorfra den hentes ned på en administrativ PC. Der kræves ingen installer, men blot at ```sam_workflows.exe```-filen placeres i roden af din bruge-mappen:

!!! note "Eksempel"
    ```powershell
    C:\Users\{az-ident}\sam_workflows.exe
    ```
    Erstat ```{az-ident}```med dit eget.

### Programfilerne
De tilhørende programfiler kan kun fås ved henvendelse til it-teamet, da de blandt andet indeholder identifikationskoder til Azure Blobstore.

Når mappen med programfiler er modtaget, skal den også placeres direkte i roden af din bruger-mappe:

!!! note "Eksempel"
    ```powershell
    C:\Users\{az-ident}\.sam_workflows\
    ```

!!! warning "Bemærk"
    Det er vigtigt, at man ikke ændrer navnet på mappen. Den skal hedde **```.sam_workflows```** med foranstillet punktum.

### Mappe til csv-filer
Et eller flere workflows arbejder med csv-filer, som eksporteres til og fra SAM, og for mange vil det være nemmere at oprette en fast mappe i roden af bruger-mappen til disse csv-filer:

!!! note "Eksempel"
    ```powershell
    C:\Users\{az-ident}\Workflows\
    ```
Dette er ikke et krav, men en anbefaling.

## Generering af access-filer
SAM-workflows kan aktuelt generere access-filer (jpg-filer) ud fra originale pdf'er eller billedfiler (jpg, tif, png, gif...).

### Csv-fil fra SAM
For at køre et workflow, der genererer access-filer, skal SAM-workflow bruge en csv-fil, som er eksporteret fra et SAM-job
