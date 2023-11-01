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
        - File med puid 111-1112323-3423423 havde 3 extensions, jeg slettede alle bortset fra den første (.pdf) for at sikre den blev markeret korrekt.

Her betaler det sig at være udførlig.

## 1. Identifikation af indkomne filer
Før alt andet skal de indkomne filer så vidt muligt identificeres. 
Især ved store afleveringer, kan identifiaktionen dog tage lang tid. Man kan derfor med fordel køre den i baggrunden, mens andre opgaver løses, eller natten over.

Selve identifikationen af filer gøres med [`digiarch's`](../tools/digiarch.md) `process`-kommando. Man skal huske at køre denne på `Original-files`. Hvis man står i roden på en aflevering så køre digiarch på 'Original-files' mappen ved følgende kommando:

`digiarch .\Original-files\ process`

> #### **BEMÆRK**: Hvis der allerede er en `files.db` file i `_metadata` mappen, så vil digiarch opdaterer og overskrive denne. Omdøb den til et andet navn for at undgå dette hvis den skal gemmes.

Den af 'digiarch' producerede 'files.db'-fil inspiceres herefter i DB Browser. Det er vigtigt at danne sig et overblik over følgende:

- Hvilke filetyper der er i `_SignatureCount`-viewet (Er det primært .docx? .pdf? Forskellige typer af GIS filer?) Dette har betydning for resten af processen, da store mængder 'eksotiske' filtyper eller filer som ikke akn identificeres oftest skal løses manuelt
- Hvilke forskellige problemer der er i `_IdentificationWarnings`-viewet (Er der mange extension mismatch? Eller mange filer der skal erstattes af en template?). Bemærk at der ikke altid (dog for det meste) er nogle _IdentificationWarnings, men prøv alligevel at få et overblik over afleveringen.

Man noterer sine tanker i `status.txt`. 


## 2. Omdøbning af komplekse filer
Efter identifikationsprocessen vil der almindeligvis være filer, der enten ikke kunne identificeres, eller hvor filendelsen enten mangler eller er uforenelig med det filformat, som [`digiarch`](../tools/digiarch.md) har fundet. Alle tilfælde, hvor puid ikke kunne determineres eller hvor filendelsen ikke stemmer overens med det af digiarch fundne filformat, vil være oplistet i _IdentificationWarnings-viewet i DB Browser. 
I dette trin skal man som minimum omdøbe alle komplekse filer (se endenfor), som [`unarchiver`](../tools/unarchiver.md) skal udpakke i næste trin. Dette gøres med værktøjet [`renamer`](../tools/renamer.md).
Vi behøver ikke identificere filerne igen umiddelbart efter omdøbning, da [`renamer`](../tools/renamer.md) opdaterer både database og filnavne.

I dette trin skal komplekse filer, identificeret af digiarch som havende følgende PUID og med advarslen "Extension mismatch" i _IdentificationWarnings, omdøbes så de får tilsvarende extensions:

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

For mere deltajeret beskrivelse af brugen af [`renamer`](../tools/renamer.md), se trin 5.

## 4. Udpak alle komplekse filer
Opret en mappe med navnet `save_dir` i `.\OriginalFiles\_metadata`. I mappen `save_dir` vil de originale filer blive lagt efter udvinding.

```
unarchiver .\_metadata\files.db .\_metadata\save_dir\
```

Se [unarchiver](../tools/unarchiver.md) for mere information


## 5. Identificér filer igen
I `.\OriginalFiles\_metadata` omdøb `files.db` til `files_preunwrap.db`.

```
digiarch .\OriginalFiles process
```

Noter i `status.txt` hvor mange `warnings` der findes i `files.db`


## 6. Omdøbning af udpakkede og andre resterende filer, samt identifikation af andre problemer
I dette skridt bruges [`renamer`](../tools/renamer.md) igen til at omdøbe de filer der ikke blev omdøbt før, samt de i trin 3 udpakkede filer. I dette trin identificeres også hvilke fejl og mangler der ellers er i afleveringen. Det er kun binære fejl extension mismatches og andre fejl der skal løses/identificeres.

Vær opmærksom på, at der ikke sker en ændring på GIS-filer. 

I `_IdentificationWarnings`-viewet filtreres derfor det er kun følgende warnings, der skal løses:

- `Extension mismatch`
- (Binær fil) `No match` 
- (Binær fil) `No match; possibilities based on extension are …`
- (Tekst fil) `Match on extension only`
- (Tekst fil) `Match on extension only; extension mismatch`


Alle filer med advarslen `Extension mismatch` bruges `renamer` til omdøbning af filerne extension. Ved tvivl om extension kan man ved fordel se på andre filer med samme `puid` i afleveringen.

```
renamer .\OriginalFiles\_metadata\files.db puid new_extension_without_period_sign
```

Notere hvilke binære filer der ikke kan identificeres. Disse har advarslen `No match` eller `No match; possibilities based on extension are …`. Bemærk at nogle af disse filer kan være GIS-filer.

Tekstfiler med advarslen `Match on extension only` eller `Match on text only; extension mismatch` skal noteres, men herefter ikke gøres yderligere ved. Convert-værktøjet `plaintextconverter` vil kopiere teksten direkte over.


## 7. Identificér problematiske filformater
I dette trin identificere hvilke filformater vores konverteringsværktøjer aktuelt ikke kan håndtere.

```
convert-unmanaged .\ OriginalFiles\_metadata\files.db
```

Kopier ouput fra `convert-unmanaged` til ‘status.txt`

Se [convert-unmanaged](../tools/convert-unmanaged.md) for mere information


## 8. GIS-filer skal merges
Gisprocessor skal køres over to omgange. `j-json` genererer en json-fil, der viser, hvad værktøjet flytter. `move` flytter GIS-filerne i json-filen. Efter værktøjet har kørt, inspiceres den af værktøjet skabte log-fil.

#### Trin 1
```
gisprocessor g-json
```

Hertil skal der udfyldes den fulde sti til av.db. Dette gøres med følgende hvoraf XX byttes ud med afleveringsnummeret. 
```
.\_metadata\AVID.AARS.XX.av.db
```

#### Trin 2
```
gisprocessor move
```

#### Typer af GIS-projekter:

| Navn           | Obligatoriske filformater | Mulige filformater |
| -------------- | ------------------------- | ------------------ |
| ESRI Shapefile | shp, shx, dbf | sbn, sbx, atx, fbn. fbx, ain, aih, ixs, mxs, prj, xml, cpg|
| MapInfo dataset format  | tab | dat, map, ud, ind |
| MapInfo Interchange Format | mif | mid |

Se [gisprocessor](../tools/gis_processor.md) for mere information


## 9. Udpakkede filer skal rearranges
I dette skridt skal de filer, der blev udpakket i trin 3, flyttes til nye mapper i en ny `docCollection`, således at de ligger i hver deres mappe.

```
rearranger .\OriginalFiles newDocCollectionnumer dIDCounter newTableNumer
```

`newDocCollectionnumer` er nummeret på den nye docCollection-mappe, hvortil filerne flyttes (hvis der i forvejen er 3 docCollections vil den nye mappes nummer være 4).

`dIDCounter` er den sidste fil i den sidste docCollections nummer + 1 (hvis der er 34875 filer vil nummeret være 34876).

`newTableNumber` er antallet af eksisterende tables (i tables-mappen) + 1 (dvs. at hvis der er 10 tables vil tallet være 11)

Se [rearranger](../tools/rearranger.md) for mere information

## 10. Identificér filer igen
I `.\ OriginalFiles\_metadata` omdøb ‘files.db` til ‘files_preGISRearrange.db`.

```
digiarch .\OriginalFiles process
```

Noter i `status.txt` hvor mange `warnings` der findes i `files.db`.

Når afleveringen findes klar til konvertering, opdateres status for afleveringen i excel-arket `Arkiveringsversioner (notes)` på funktionsdrevet til "Klar til konvertering".