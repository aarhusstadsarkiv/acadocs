SAM-workflows består af 1) et lille program (```sam_workflows.exe```-filen) og 2) en mappe med program-filer, som begge skal være på plads før programmet virker.

!!! warning "Bemærk"
    SAM-workflows virker kun på administrative Windows-PC'er, da programmet ofte skal arbejde med de filer, som findes i vores digitale arkiv. 

### sam_workflows.exe
Seneste udgave af [sam_workflows.exe ligger altid på denne side](https://github.com/aarhusstadsarkiv/sam-workflows/releases), hvorfra den kan downloades. Der kræves ingen installationsproces, men blot at ```sam_workflows.exe```-filen placeres et eller andet sted i din bruger-mappe:

!!! note "Eksempel"
    ```powershell
    C:\Users\{az-ident}\Workflows\sam_workflows.exe
    ```
    Erstat ```{az-ident}```med dit eget.

### Programfilerne
Mappen med de tilhørende programfiler kan kun fås ved henvendelse til it-teamet, da de blandt andet indeholder autorisationskoder til vores filservice.

Når mappen med programfiler er modtaget, skal den placeres **direkte i roden af din bruger-mappe**:

!!! note "Eksempel"
    ```powershell
    C:\Users\{az-ident}\.sam_workflows\
    ```

!!! warning "Bemærk"
    Det er vigtigt, at man ikke ændrer navnet på mappen. Den skal hedde **```.sam_workflows```** med foranstillet punktum.

### Mappe til csv-filer
Flere workflows arbejder med csv-filer, som henholdsvis importeres fra eller eksporteres til SAM. For mange vil det være nemmest at oprette en fast mappe i bruger-mappen til disse csv-filer:

!!! note "Eksempel"
    ```powershell
    C:\Users\{az-ident}\Workflows\SAM-jobs\
    ```
Dette er ikke et krav, men en anbefaling. Når man efterfølgende bruger SAM til at eksportere eller importere csv-filer, vil programmet huske hvilken mappe csv-filerne tidligere er placeret i.

