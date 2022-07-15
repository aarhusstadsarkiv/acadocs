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

### Validering af dokumentstruktur
Hvis afleveringen indeholder digitale dokumenter (OriginalDocuments), foretages nu en indledningsvis validering af disse (tjekker for tomme mappe/filer, tilstedeværelsen af *.zip*-filer, andet?).

Vi benytter **[digiarch](../../tools/digiarch)** til at foretage denne indledende validering af dokumenterne og strukturen i de enkelte *docCollections*. Kald *process* på den mappe, som arkiveringsversionen er kopieret til:

```powershell
digiarch D:\filer\AVID.AARS.3.1 process
```

[Dokumentationen til digiarch](../../tools/digiarch) indeholder yderligere beskrivelser af, hvordan værktøjet benyttes

*Digiarch* producerer sin egen "_metadata"-mappe med en "files.db"-fil, som efter den indledningsvise test skal placeres i roden af "OriginalDocuments":

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
    |-- _metadata/
        |-- files.db
    |-- docCollection1
    |-- docCollection2
    |-- ...
```

### Kopiering til AcaStorage
Hvis resultatet af dokumentvalideringen ikke nødvendiggør en genaflevering, kopieres hele arkiveringsversionsmappen inkl. *"_metadata"*-mapperne til AcaStorage:

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

!!! warning "Vigtigt"
    Al det test- og konverteringsarbejde, som følger, skal arbejde med den kopi af arkiveringsversionen, som nu ligger på AcaStorage. Den kopi af arkiveringsversionen, som først blev kopieret ind i det lokale system, skal som nævnt forblive uberørt, så vi altid kan gå tilbage til udgangspunktet.

## Identifikation af dokumenter
Inden dokumentkonvertering foretages en grundig identifikation af de originale dokumenter. Dette gøres både for at fange så mange problemer (filendelser, tomme filer...) som muligt inden konverteringsprocessen, men også for at finde ud af, hvilket master-format en given fil skal konverteres til.

Vi benytter endnu engang **digiarch** til at styre denne omstændige proces, som er nærmere beskrevet her: [dokumentation til digiarch](../../tools/digiarch).

Når alle fejl eller misforhold, som det har været muligt at tilrette, er afhjulpet, står man tilbage med en filliste, hvor minimum en fil almindeligvis stadig vil være fejlbehæftet og dermed have en pronomID, som indikerer en af følgende fejltyper:

- filen er tom
- filen er korrupt
- filen er kodeordsbeskyttet
- filen er ikke genkendelig og dermed ikke identificérbar

## Test af databasen
Afleveringen er nu klar til at blive testet i ADA. Denne proces er beskrevet under **[ADA-dokumentationen](../ada)**, og her skal blot nævnes, at de logs, notater m.m., som måtte blive produceret under testen, nu findes i *"_metadata"*-mappen i roden af den afleveringen, som ligger på AcaStorage.

Når ADA-testen er afsluttet og arkiveringsversionen kan godkendes, er det tid til at konvertere de originale dokumenter til Stadsarkivets [*"master"*-formater](../master-formats).

## Konvertering til master-formater
Vi benytter **[convertool](../../tools/convertool)** til at konvertere de originale dokumenter til master-formater. Denne proces er beskrevet i [convertool-dokumentationen](../../tools/convertool), og her skal blot nævnes, at de konverterede dokumenter lægges i samme struktur som de originale dokumenter, blot i mappen "MasterDocuments":

```
/AVID.AARS.3.1
|-- _metadata/
    |-- ADA_log-leverandør.rtf
    |-- ADA_log-test.rtf
    |-- test-notat.txt
    |-- status.txt
|-- ContextDocumentation/
|-- Indices/
|-- Schemas/
|-- Tables/
|-- OriginalDocuments/
    |-- _metadata/
        |-- files.db
    |-- docCollection1/
    |-- docCollection2/
|-- MasterDocuments/
    |-- _metadata/
        |-- files.db
    |-- docCollection1/
    |-- docCollection2/
```

Filerne testes og fejlrettes med **[digiarch](../../tools/digiarch)**, og databasen testes med **[ADA](../ada)**. De to processer er uafhængige af hinanden og kan derfor køres både separat og parallelt.

## Konvertering til arkiv-formater
Når både databasen og dokumenterne er gennemarbejdet og kan godkendes, konverteres master-dokumenterne slutteligt til arkiv-formater (tiff, jpeg2000, MPEG..). Igen benyttes **[convertool](../../tools/convertool)**, og igen lægges de konverterede filer i samme struktur som de originale dokumenter, blot i mappen "Documents":

```
/AVID.AARS.3.1
|-- _metadata/
    |-- ADA_log-leverandør.rtf
    |-- ADA_log-test.rtf
    |-- test-notat.txt
    |-- status.txt
|-- ContextDocumentation/
|-- Indices/
|-- Schemas/
|-- Tables/
|-- OriginalDocuments/
    |-- _metadata/
        |-- files.db
    |-- docCollection1/
    |-- docCollection2/
|-- MasterDocuments/
    |-- _metadata/
        |-- files.db
    |-- docCollection1/
    |-- docCollection2/
|-- Documents/
    |-- _metadata/
        |-- files.db
    |-- docCollection1/
    |-- docCollection2/
```

## Godkendelse af arkiveringsversion
Efter succesrig dokumentkonvertering afsluttes processen på følgende vis:

1. Det overordnede Notes-statusdokument på OneDrive opdateres med de relevante informationer 
2. Hele mappen med arkiveringsversionen ("AVID.AARS.3.1\") kopieres til "arkvers"-mappen på funktionsdrevet:
    ```
    C:\Users\{az-ident}\Aarhus kommune\FNK-Aarhus Stadsarkiv - Dokumenter\arkvers\
    ```
3. Improvento informeres i en mail om godkendelsen
4. Den relevante magistratsafdeling informeres i en mail om godkendelsen