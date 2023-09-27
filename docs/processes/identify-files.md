# Identifikation af filer
Identifikation af indkomne filer fra myndigheder eller private parter er en kompleks proces, som involverer en del viden, værktøjer og procestrin.

Ved processens begyndelse oprettes en `status.txt` i den relevante `_metadata`-mappe (eg. `AVID.AARS.78.1\OriginalFiles\_metadata`). I dette dokument noteres hvor langt man er kommet i processen, hvilke fejl og problemer man er stødt på undervejs, og hvilke manuelle rettelser man har foretaget.

Hver gang et værktøj har kørt inspiceres nogle af de berørte filer manuelt i stifinderen, for at sikre sig at værktøjet har kørt succesfuldt.

Detaljerede guides til installation og brug af de enkelte værktøjer findes [`her`](https://aarhusstadsarkiv.github.io/acadocs/tools/).

!!! attention "Bemærk"
    Før hvert værktøj køres, skal man sikre sig at man benytter den nyeste version af værktøjet. Dette kan evt. gøres med kommandoen 
    
    ```
    pipx upgrade-all
    ```

## 0. Forbered status.txt
Som sagt før er processen at konvetere en aflevering lang og fyldt med faldgrupper. For at gøre det muligt for en selv at spore sine tanker og handlinger over de dage, uger og (nogle gange desværre) måneder man arbejder med afleveringen, er det vigtigt at man altid noterer sine tanker i `status.txt` filen.

Vi anbefaler at man benytter den som logbog og åbner den hvergang man arbejder med en aflevering. Man laver så en overskrift med dagens dato og noterer ens handlinger og observationer derunder. Evt. kan man havde en TODO sektion med ting man skal gøre / holde øje med. Et eksempel på hvordan `status.txt` kunne være opsat på ses herunder:

    D. 2/10/2023
        Kørte Digiarch første gang på afleveringen
        - Der ser ud til at være mange GIS relaterede filer. Hver opmærksom når rearranger skal køres.
        - Flere .pdf filer har et extension mismatch. Det ligner .docx filer der ved en fejl er gemt som .pdf ved første øjekast
    
    TODO:
        - Kig på et par stykker af .pdf filerne med extension mismatch og se om det er en kategorisk fejl

    Manuelle rettelser
        - File med puid 111-1112323-3423423 havde 3 extensions, jeg slettede alle bortset fra den første (.pdf) for at sikre den blev identificeret korrekt.

Her betaler det sig at være udførlig.

## 1. Identifikation af indkomne filer
Før alt andet skal de indkomne filer så vidt muligt identificeres. 
Især ved store afleveringer, kan identifiaktionen dog tage lang tid. Man kan derfor med fordel køre den i baggrunden, mens andre opgaver løses, eller natten over.

Selve identifikationen af filer gøres med [`digiarch's`](../tools/digiarch.md) `process`-kommando. Man skal huske at køre denne på `Original-files`. Hvis man står i roden på en aflevering så køre digiarch på 'Original-files' mappen ved følgende kommando:

```bash
digiarch .\Original_files\ process
```

> #### **BEMÆRK**: Hvis der allerede er en `files.db` file i `_metadata` mappen, så vil digiarch opdaterer og overskrive denne. Omdøb den til et andet navn for at undgå dette hvis den skal gemmes.

Den af 'digiarch' producerede 'files.db'-fil inspiceres herefter i DB Browser. Det er vigtigt at danne sig et overblik over følgende:

- Hvilke filetyper der er i `_SignatureCount`-viewet (Er det primært .docx? .pdf? Forskellige typer af GIS filer?) Dette har betydning for resten af processen, da store mængder 'eksotiske' filtyper eller filer som ikke akn identificeres oftest skal løses manuelt
- Hvilke forskellige problemer der er i `_IdentificationWarnings`-viewet (Er der mange extension mismatch? Eller mange filer der skal erstattes af en template?). Bemærk at der ikke altid (dog for det meste) er nogle _IdentificationWarnings, men prøv alligevel at få et overblik over afleveringen.

Man noterer sine tanker i `status.txt`. 


## 2. Omdøbning af zip o.l. filer
Digiarch benytter [Siegfried](https://www.itforarchivists.com/siegfried/) til at identificere langt størstedelen af vores filer. Den bygger så selv på PRONOM databasen og andre lignende databaser over fil typer.

I sin identifikation af fil typer kan Siegfried godt være lidt pedantisk. Derfor sker det at den særligt identificerer filer i et zip-format forkert. Mange filer i zip-formater vil derfor havde en warning der siger `Extension_Mismatch`.

For at rette op på det skal man gøre følgende:

1. Sørg for først for at `renamer` er installeret og opdateret
2. Dernærst, kig på de filer som har et `Extension_Mismatch`. Hvis nogle af dem har et af de PUID'er som er markeret nede i tabellen, så skal deres extension erstattes af den pågældende extension i tabellen.

    | PUID      | extension |
    | --------- | --------- |
    | fmt/411   | .rar      |
    | fmt/484   | .7z       |
    | fmt/613   | .rar      |
    | x-fmt/263 | .zip      |
    | x-fmt/264 | .rar      |
    | x-fmt/265 | .tar      |
    | x-fmt/266 | .gz       |
    | x-fmt/430 | .msg      |
    | aca-fmt/9 | .dat      |

    Der vil også være tidspunkter hvor at Digiarch har markeret nogle filer som værende .zip filer, men hvor at de ikke skal bevares af os af forskellige grunde. Hvis man møder sådan en type fil, skal den markeres som ikke bevaringsværdi manuelt i 'action' feltet. De filtyper der typisk er tale om er:
    
    | PUID      | extension |
    | --------- | --------- |
    | x-fmt/412 | .jar      |


3. For at erstatte en specifik filtype angives den PUID og hvilken extension man øsnker at give alle filer med  `Extension_Mismatch` således:
    ```Bash
    renamer db_file_path puid new_extension_without_period_sign
    ```



For mere deltajeret beskrivelse af brugen af [`renamer`](../tools/renamer.md), se trin 5.

## 3. Udpak alle komplekse filer
Brug [`unarchiver`](../tools/unarchiver.md) til at udpakke komplekse filer, hvor man bruger stien til den i trin 1 producerede files.db. `Unarchiver` laver en .log-fil i `_metadata`-mappen, som inspiceres efter værktøjet har kørt. 

!!! Status

    === "Erfaringer og Tips"

        * Det er en god idé at foretage stikprøver i stifinderen for de enkelte filtyper, for at sikre sig at unarchiver har kørt succesfuldt.


## 4. Identificér filer igen
Efter unarchiver, skal ```digiarch``` køres igen, så alle filer i alle underliggende mapper identificeres.
OBS! Før værktøjet køres, omdøbes den gamle `files.db` til `files_preunwrap.db` el., da digiarch nu laver en ny `files.db`

## 5. Omdøbning af udpakkede og andre resterende filer, samt identifikation af andre problemer

I dette skridt bruges [`renamer`](../tools/renamer.md) igen til at omdøbe de filer der ikke blev omdøbt før, samt de i trin 3 udpakkede filer. I dette trin identificeres også hvilke fejl og mangler der ellers er i afleveringen. Det er kun binære fejl extension mismatches og andre fejl der skal løses/identificeres. I _IdentificationWarnings-viewet filtreres derfor 
Det er kun følgende warnings, der skal løses:

- "Extension mismatch"
- "puid is Null"
- "Match on text only; extension mismatch"

Derudover skal man i dette trin i `status.txt` notere, hvilke binære filer der ikke kan identificeres, dvs. har advarslerne "no match" eller "match on extension only" (Ignorér her GIS-filer, som der vendes tilbage til i trin 7). Nogle af disse filer vil muligvis kunne reddes.

!!! attention "Bemærk"
    Lad være med at omdøbe .eml, identificeret som .html


Nogle gange vil det være nødvendigt at ændre PUID'et, som identificeret af digarch. Dette gøres ved at lave et "update-statement" i `Execute SQL`-fanen i DB Browser. I nedenstående eksempel ændres PUID'et for winmail.dat-filer, fejlalgtigt identificeret som fmt/1600, til vores eget PUID, aca-fmt/9:

!!! hint "Eksempel"
    UPDATE Files  
        SET puid = "aca-fmt/9",  
    signature = "Microsoft email attachments archive (winmail)"  
    where puid = "fmt/1600"  
    and is_binary = 1

!!! Status

    === "Erfaringer og Tips"

        * Filer .wmz- og .emz-extensions der tidligere er blevet identificeret som og omdøbt til .gz og udpakket af 'unarchiver', har nu igen .wmz- og .emz-extensions. Denne gang skal de omdøbes henholdsvis .wmf og .emf, ganske som digiarch siger.
        * Filer, man er i tvivl om, hvad er, kan inspiceres i en hex editor.
        * Brug ctrl + z til at tilbage-navngive filer i stifinderen.
        * Brug /Ex/ (regex som filterer efter stort E) som filter i DB for at filtrere filer der stadig har "Extension mismatch".
        * Bemærk at Extension mismatches både gælder filer med forkert extension, men også filer helt uden extension.
        * Brug [`PRONOM`](https://www.nationalarchives.gov.uk/pronom/BasicSearch/proBasicSearch.aspx?status=new) til at finde ud af, hvilke extensions de enkelte PUID-burde have. Pronom kan dog ikke følges blindt, da der er mange undtagelser i processen.
        * Ved `aca-fmt`-PUID'er bruges listen i [`renamer`](../tools/renamer.md)-guiden.
        * Hvis flere extensions kan bruges til det samme PUID, kan man evt. se hvilke extensions andre filer med samme PUID i afleveringen har.
        * Der vil ofte være korrupte pdf'er, der ikke kan identificeres, men alligevel kan åbnes i browseren. Disse er ofte bevaringsværdige, og kan evt. gemmes manuelt i et nyt pdf-format, som så skal erstatte den korrupte fil.
        * Filer med "No match; possibilities based on extension [etc.]"-warnings er sandsynligvis korrupte.

    === "Problemer/mangler"

        * I db-filen kan man ikke se, hvilken extension man burde benytte. Det er til tider svært, at vide, hvilken extension et givet filformat bruger.
        * I db-filen kan man ikke altid se, hvilken version af et givet filformat en fil måtte tilhøre. Det skal eventuelt bruges til at vælge extension.
        * Vi skal gerne have default extension ud af Siegfried. Hvordan laves ellers et dryrun?

    === "Roadmap"

        * `Renamer` burde have et default kald, hvor alle identificerede filnavne UDEN extension blev omdøbt.
        * Man bør kunne køre et dry-run, hvor alle blev omdøbt og lagt i foldere, navngivet efter deres puid. Det ville speede processen op.
        * Man kunne have en liste af extension-renames, som altid vil kunne køres med eks. 'rename-all'. Der er for mange undtagelser til at dette kan gøres på nuværende tidspunkt. 

## 6. Identificér problematiske filformater
Brug [`convert-unmanaged`](../tools/convert-unmanaged.md), med stien til files.db'en til at identificere, hvilke filformater i indkomsten, som vores konverteringsværktøjer aktuelt ikke kan håndtere, så vi allerede på dette tidspunkt i processen ved, hvor problemerne findes.

## 7. GIS-filer skal merges
Eter omdøbning og udpakning, skal eventuelle GIS-filer merges, så de identificeres og konverteres korrekt. Dette gøres med værktøjet [`gis_processor`](../tools/gis_processor.md), der flytter alle GIS-filerne i de enkelte GIS-projekter hen i mappen, hvor det enkelte projekts masterfil ligger, og erstatter de flyttede med templates (følg guiden på værktøjets egen [`side`](../tools/gis_processor.md)). Efter værktøjet har kørt, inspiceres den af værktøjet skabte log-fil.

Typer af GIS-projekter:

| Navn           | Obligatoriske filformater | Mulige filformater |
| -------------- | ------------------------- | ------------------ |
| ESRI Shapefile | shp, shx, dbf | sbn, sbx, atx, fbn. fbx, ain, aih, ixs, mxs, prj, xml, cpg|
| MapInfo dataset format  | tab | dat, map, ud, ind |
| MapInfo Interchange Format | mif | mid |


!!! Status

    === "Problemer"

        * Flere identiske GIS-projekter, der ligger ved siden af hinanden skaber problemer (måske løst?)

    === "Erfaringer"

        * Det nogle gange være svært at finde ud af, om gis_processor har kørt korrekt - hvis der er filer den ikke har flyttet, fordi de af den ene eller den anden grund ikke er identificeret korrekt (i dette eller tidligere trin), kommer de heller ikke med i log-filen, osv. Her kan det være behjælpeligt at kigge på de forskellige GIS-projekter i DB browser og se på, hvilke filer der ligger i "området".

## 8. Udpakkede filer skal rearranges
I dette skridt skal de filer, der blev udpakket i trin 3, flyttes til nye mapper i en ny `docCollection`, således at de ligger i hver deres mappe. Dette gøres med værktøjet [`rearranger`] (../tools/rearranger.md), som også opdaterer/opretter de relaterede xml-filer fra en myndighedsaflevering. `DoxIndex.xml` opdateres med de udpakkede filer; der oprettes en ny tabel med relationerne mellem de originale filer og de udpakkede "child"-filer; tableIndex.xml opdateres med information om den nye tabel.

## 9. Identificér filer igen
Til sidst køres digiarch endnu engang for at afleveringen klar til konvertering. Før digiarch køres skal den gamle `files.db` omdøbes til `files_preGISRearrange.db` el. 
Den nye `files.db` inspiceres en sidste gang for fejl.

Når afleveringen findes klar til konvertering, opdateres status for afleveringen i excel-arket `Arkiveringsversioner (notes)` på funktionsdrevet til "Klar til konvertering".