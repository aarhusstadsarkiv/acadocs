# Identifikation af filer
Identifikation af indkomne filer fra myndigheder eller private parter er en kompleks proces, som involverer en del viden, værktøjer og procestrin.

Hver gang et værktøj har kørt inspiceres nogle af de berørte filer manuelt i stifinderen, for at sikre sig at værktøjet har kørt succesfuldt.

Detaljerede guides til installation og brug af de enkelte værktøjer findes [`her`](https://aarhusstadsarkiv.github.io/acadocs/tools/).


## 1. Klargørelse af identifikation af filer
Alle sti henvisningerne tager udgangspunkt fra `AVID.AARS.XX`, hvoraf `XX` er nummeret på afleveringen.

Opret `status.txt` i mappen ` .\OriginalFiles\_metadata`. Heri noteres hvor langt man er kommet i processen, hvilke fejl og problemer man er stødt på undervejs, og hvilke manuelle rettelser man foretaget.


## 2. Identifikation af indkomne filer
```
digiarch .\OriginalFiles process
```

Noter i `status.txt` hvor mange `warnings` der findes i `files.db`

Se [digiarch](../tools/digiarch.md) for mere information


## 3. Omdøbning af komplekse filer
I dette trin skal komplekse filer med advarlsen `Extension mismatch` i `_identificationWarnings` omdøbes vha. `renamer` således, at de får den korrekte extension.

Se komplet og opdateret liste [her](https://github.com/aarhusstadsarkiv/reference-files/blob/main/to_extract.json)

```
renamer .\OriginalFiles\_metadata\files.db puid new_extension_without_period_sign
```

Se [renamer](../tools/renamer.md) for mere information


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