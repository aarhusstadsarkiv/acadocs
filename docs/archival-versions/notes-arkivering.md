I det følgende beskrives processen med arkiveringsversioner, som modtages i forbindelse med Notes-projektet.

!!! note "Bemærk"
    - I dokumentationen bruges *AVID.AARS.3.1* som eksempel. Husk at udskifte AVID "3" med det relevante id.
    - *Arkiveringsversion* betegnes i det følgende også *aflevering*.

## Modtagelse og validering

### Kopiering til lokal disk
Arkiveringsversioner fra Notes-projektet afleveres på usb-diske, hvorfra de umiddelbart efter modtagelse kopieres til en placering på vores infrastrutur (ekstern usb-harddisk, intern D-drev eller lignende).

!!! Danger "Advarsel"
    Det er vigtigt at stoppe Kaspersky så snart den modtagne usb-disk er plugget til den administrative PC, da Kaspersky i det skjulte sletter gamle .xls-filer, hvis de indeholder macroer.

Arkiveringsversionen kopieres fuldgyldigt til en mappe, som navngives med afleveringens AVID og tilføjes en "_metadata"-mappe, hvori vi efterfølgende samler diverse logs, mails, statusdokumenter m.m. med relation til afleveringen og testen:

```
/AVID.AARS.3.1
|-- _metadata/
    |-- ADA_log-leverandør.rtf
    |-- status.txt
    |-- ...
|-- ContextDocumentation/
|-- Indices/
|-- Schemas/
|-- Tables/
|-- OriginalDocuments/
    |-- docCollection1
    |-- docCollection2
    |-- ...
```

Efter fuldendt kopiering lægges den afleverede usb-disk til side. Vi skal ikke bruge den originale aflevering mere, med mindre vi senere i processen ved et uheld kommer til at slette eller ændre noget.

### Validering af dokumenter
Hvis afleveringen indeholder digitale dokumenter (OriginalDocuments), foretages nu en indledningsvis validering af disse (tjekker for tomme mappe/filer, tilstedeværelsen af *.zip*-filer, andet?).

Vi benytter **[digiarch](../../software/digiarch)** til at foretage denne indledende validering af dokumenterne og strukturen i de enkelte *docCollections*. Kald *process* på den mappe, som arkiveringsversionen er kopieret til:

```powershell
digiarch D:\filer\AVID.AARS.3.1 process
```

[Dokumentationen til digiarch](../../software/digiarch) indeholder yderligere beskrivelser af, hvordan værktøjet benyttes, men

*Digiarch* producerer sin egen "_metadata"-mappe med en "files.db"-fil, som efter den indledningsvise test skal placeres i roden af "OriginalDocuments":

```
/AVID.AARS.3.1
|-- _metadata/
|-- ...
|-- OriginalDocuments/
    |-- _metadata/
        |-- files.db
    |-- docCollection1
    |-- docCollection2
    |-- ...
```

### Kopiering til AcaStorage
Hvis resultatet af den indledende filvalidering ikke giver anledning til en genaflevering, kopieres hele arkiveringsversionsmappen inkl. *"_metadata"*-mapperne til AcaStorage:

1. Åbn Microsoft Azure Storage Explorer
2. Mappen med arkiveringsversionen kopieres nu samlet eller over flere omgange ind i følgende mappe: MKB {din mail}/Storage Accounts/acastorage/arkver/

```
/AVID.AARS.3.1
|-- _metadata/
    |-- ...
|-- ContextDocumentation/
|-- Indices/
|-- Schemas/
|-- Tables/
|-- OriginalDocuments/
    |-- _metadata/
        |-- files.db
    |-- docCollection1.zip
    |-- docCollection2.zip
```

Nu er den modtagne arkiveringsversion indledningsvis valideret og kopieret til både ekstern disk og AcaStorage. Næste trin er at teste henholdsvis filer og database. Filerne testes og fejlrettes med **[digiarch](../../software/digiarch)**, og databasen testes med **[ADA](../ada)**. De to processer er uafhængige af hinanden og kan derfor køres både separat og parallelt.


## Efter identifikation og tilretning af de originale dokumenter
Når alle fejl eller misforhold, som det har været muligt at tilrette, er afhjulpet, vil files.db-filen står man tilbage med en filliste, hvor minimum en fil almindeligvis stadig vil være fejlbehæftet og dermed have en pronomID, som indikerer en af følgende fejltyper:

- filen er tom
- filen er korrupt
- filen er kodeordsbeskyttet
- filen er ikke genkendelig og dermed ikke identificérbar

### Efter test og tilretning af databasen med ADA
Når databasen er færdigtestet i ADA, lægges testloggen i "_metadata"-mappen i roden af AVID-mappen:

```
/AVID.AARS.3.1
|-- _metadata/
    |-- ADA_log-intern.rtf
    |-- status.txt
    |-- ...
```

## Konvertering af originale dokumenter
Når både databasen og dokumenterne er gennemarbejdet og kan godkendes, kan de originale dokumenter konverteres til **[master formater](../master-formats)** med **[convertool](../../software/convertool)**. De konverterede dokumenter reproducerer strukturen fra "OriginalDocuments", men placeres i en "MasterDocuments"-mappe i roden af afleveringen:

```
/AVID.AARS.3.1
|-- _metadata/
    |-- ADA_log-leverandør.rtf
    |-- status.txt
|-- OriginalDocuments/
    |-- docCollection1/
|-- MasterDocuments/
|-- ...
```

Selve processen med brug af *convertool* er beskrevet under [dokumentationen til convertool](../../software/convertool)

## Godkendelse af arkiveringsversion
Når arkiveringversionen kan godkendes, afsluttes processen på følgende vis:

1. Det overordnede Notes-statusdokument opdateres med de relevante informationer 
2. Hele mappen med arkiveringsversionen kopieres til "arkvers"-mappen på funktionsdrevet:
    ```
    C:\Users\{az-ident}\Aarhus kommune\FNK-Aarhus Stadsarkiv - Dokumenter\arkvers\AVID.AARS.3.1\
    ```
3. Improvento informeres i en mail om godkendelsen
4. Den relevante magistratsafdeling informeres i en mail om godkendelsen