I det følgende beskrives strukturen og flows i arbejdet med arkiveringsversioner, som modtages i forbindelse med Notes-projektet.

!!! note
    I dokumentationen bruges AVID.AARS.3.1 som eksempel. Husk at udskifte AVID 3 med det relevante id. 
## Modtagelse
Hvis virusscan (Kaspersky) fejler, bedes Improvento genaflevere hele arkiveringsversionen eller blot de berørte filer.

## Kopiering af arkiveringsversionen
Når virusscan er veloverstået, skal den modtagne arkiveringsversion kopieres ind på vores systemer.

Først kopieres til AcaStorage (Azure Blobstorage) med Microsoft Azure Storage Explorer:
  1. Åbn Microsoft Azure Storage Explorer
  2. Arkiveringsversionen kopieres nu samlet eller over flere omkgange ind i følgende mappe: MKB {din mail}/Storage Accounts/acastorage/stage/, så mappeindholdet eks. ser således ud:

```
/AVID.AARS.3.1
|-- _metadata/
    |-- ADA_log-leverandør.rtf
    |-- status.txt (se næste afsnit)
|-- ContextDocumentation/
|-- Indices/
|-- Schemas/
|-- Tables/
|-- OriginalDocuments/
```

### Tilføj metadata
I mappen med arkiveringsversionen tilføjes en mappe kaldet "_metadata", hvori senere procesdokumenter kan lægges. Eks. ADA-log fra leverandøren, vores ADA-log, et statusdokument, interne notater eller andet.


## Identifikation af de originale dokumenter
Identifikation af originalfiler gøres med **digiarch**
Skal vi køre identifikation af dokumenter inden vi flytter skidtet til Azure?


### Fejlretning ved identifikation
...


### Afslutning af identifikation
Når alle mulige fejl eller misforhold er afhjulpet, står man tilbage med en filliste, hvor minimum en fil almindeligvis stadig vil være fejlbehæftet og dermed have en pronomID, som indikerer en af følgende fejltyper:

- filen er tom
- filen er korrupt
- filen er kodeordsbeskyttet
- filen er ikke genkendelig og dermed ikke identificérbar



## Konvertering af de originale dokumenter
Konvertering til master-formater gøres med **convertool**. De konverterede dokumenter tildeles samme struktur som OriginalDocuments, men placeres i en MasterDocuments-mappe i roden af afleveringen:

```
/AVID.AARS.3.1
|-- _metadata/
    |-- ADA_log-leverandør.rtf
    |-- status.txt (se næste afsnit)
|-- OriginalDocuments/
|-- MasterDocuments/
|-- ...
```


## Godkendelse af arkiveringsversion
Når arkiveringversionen kan godkendes, afsluttes processen på følgende vis:

1. Det overordnede Notes-statusdokument opdateres med de relevante informationer 
2. Hele mappen med arkiveringsversionen kopieres til "arkvers"-mappen på funktionsdrevet:
    ```
    C:\Users\{az-ident}\Aarhus kommune\FNK-Aarhus Stadsarkiv - Dokumenter\arkvers\AVID.AARS.3.1\
    ```
3. Improvento informeres i en mail om godkendelsen
4. Den relevante magistratsafdeling informeres i en mail om godkendelsen