## Arkiveringsversioner
### Intern produktion
- Kingo PPR
- Aarhus Havn
- eDoc
- DAOS
- AffaldVarme
- Aarhus Vand


### Ekstern levering
- CSC Social
- CSC Vitae Omsorg
- TM Sund
- KMD Sag
- Kingo Kommunikation


### Dokumentation
Færdiggør dokumentation m.m. for både eksterne leverancer og interne produktioner.
- contextdocumentationindex
- archiveindex
- sql-forespørgsler for alle AVID'er (gennemtestede)
  - inkl. forespørgsel, der henter AL data på en borger
- slettelister (til forvaltningerne)


### Test
ADA test af alle FÆRDIGE arkiveringsversioner.


### Registrering
1. Lave og publicere lister over indhold (avid, avid-titel, org. enhed, startdato, tildato)
2. Registrere alle AVID'er


### Tilgængeliggørelse
1. Blanketter/formularer til udfyldelse ved data-forespørgsler.
2. Html-pakker eller lignende til udlevering ved forespørgsler, både personlige og emnemæssige.


### Scripts
- add_context_doc.py (inkl. mulighed for at indsætte på en bestemt plads (id))
- remove_context_doc.py
- convert_single_file_to_compressed_tif.py
- copy files from docCollections in reponse to data-export from an avid
- upload/download files to azure
- generate json/xml files from sql-query output from a request for data in an AVID


## Digiarch
Skal være dokumenteret omkring: hvad det er, installation, files.db, warnings-betydninger, hvad gøres ved fejl af alle type, hvordan fejlrettes, hvordan dokumenteres processen i status.txt, hvordan og hvornår opdateres digiarch, hvordan opdateres json-filerne, hvordan publiceres nye releases?


## Convertool
1. Integration af GIS- og CAD-konvertering
2. Skal være dokumenteret omkring: hvad det er, installation, files.db, warnings-betydninger, hvad gøres ved fejl af alle type, hvordan fejlrettes, hvordan dokumenteres processen i status.txt, hvordan og hvornår opdateres digiarch, hvordan opdateres json-filerne, hvordan publiceres nye releases?

## Digitalt Arkiv
1. Webservice, hvorigennem ALLE kald (med tid) skal foretages (FastAPI)
  - database (PG)
  - søgemaskine (ElasticSearch)
  - filserver 
  - brugerfunktionalitet
  - lager- og ordrestyring
- Storage (diskarray)


## Projekter
### Smartarkivering
Lancering af smartarkivering.dk med mulighed for aflevering til både os, Aalborg og Kolding.


### Byrådsarkivet
Bearbejdning og integration af 2022-data og -dokumenter.


## Adhoc
- overfør billeder til Aarhusbilleder via API
  - https://www.e-pics.ethz.ch/CIP/doc/CIP.htm (CIP Documentation)
