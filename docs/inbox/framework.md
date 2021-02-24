# Framework
**This is the birdseye-view of what we need/want to do the next three years.**

The list differentiates between **permanent services** (general public, governmental, internal) and **temporal services** (general, governmental, internal).
How and in what priority we do it, is recorded and discussed elsewhere, often on the ''Project Boards'' of the relevant repositories or directly within ''aarhusstadsarkiv''.

## Permanent general public services
These are the public-facing services (sites and tools) that we have to maintain and develop further.

### AarhusArkivet
Public frontend for our digital collections. Includes PostGRES with users, orders, inventory...

#### Milestones
|Deadline|Title| 
|--------|:----|
|2021-1|Opfyldelse af WASG2 (webtilgængelighed)|
|2021-1|Flytning til Azure|
|2021-1|Indpakning i Docker-kontainer|
|2021-1|Bestillingsmodul + andre prioriterede ønsker|
|2021-3|Søgemaskine|
|2021-4|Dynamisk integration af API'er (wikis, lex.dk, matrikeldata?, digdag?)|

#### Issues and development
Issues are tracket at [aarhusstadsarkiv/aarhusarkivet/issues](https://github.com/aarhusstadsarkiv/aarhusarkivet/issues)

### Smartarkivering 2.0
NEMID-integration, inkl. medarbejder-login,

#### Milestones
|Deadline|Title| 
|--------|:----|
|2021-1|Ensure funding|
|2021-1|Hire developer|
|2021-1|Dockerize|
|2021-1|Deploy to Azure|
|2021-1|Formalize GDPR and permissions-setup|
|2021-2|Launch|

#### Issues and development
Issues are tracket at [aarhusstadsarkiv/aarhusarkivet/issues](https://github.com/aarhusstadsarkiv/aarhusarkivet/issues)

### Byrådsarkivet
Hjemmeside.

#### Milestones
|Deadline|Title| 
|--------|:----|
|2021-1|Opfyldelse af WASG2 (webtilgængelighed)|
|2021-1|Flytning til Azure|
|2021-1|Indpakning i Docker-kontainer|
|2021-1|Bestillingsmodul + andre prioriterede ønsker|
|2021-3|Søgemaskine|
|2021-4|Dynamisk integration af API'er (wikis, lex.dk, matrikeldata?, digdag?)|

#### Issues and development
Issues are tracket at [aarhusstadsarkiv/aarhusarkivet/issues](https://github.com/aarhusstadsarkiv/aarhusarkivet/issues)

### AarhusWiki (AcaWiki)
Flyttes til mediawiki-service? Flyttes til AarhusArkiet?

#### Milestones
|Deadline|Title| 
|--------|:----|
|2021-1|Opfyldelse af WASG2 (webtilgængelighed)|
|2021-1|Flytning til Azure|
|2021-1|Indpakning i Docker-kontainer|
|2021-1|Bestillingsmodul + andre prioriterede ønsker|
|2021-3|Søgemaskine|
|2021-4|Dynamisk integration af API'er (wikis, lex.dk, matrikeldata?, digdag?)|

#### Issues and development
Issues are tracket at [aarhusstadsarkiv/aarhusarkivet/issues](https://github.com/aarhusstadsarkiv/aarhusarkivet/issues)

## Permanent governmental services
These are the permanent services that we have to maintain and/or develop with relation to the municipality

### Services regarding the production of archival versions
Conversion, import...

### Services regarding test and preservation of archival versions
Finish Notes, eliminate backlog, finish all major systems
- Notesprojekt afsluttes
- KMD Sag
- CSC Vitae Omsorg
- CSC Social
- KMD fagsystemer bagkatalog gennemarbejdes

### Service with regards to access to municipality-data
Blanketter og workflows, hvor magistraterne fungerer som filtre

## Permanent Internal services and tools
These are our permanent internal tools used by all employees

### AcaDocs
Hjemmeside med alverdens dokumentation omkring services, processer, blanketter...

### AcApi (metadata-store)
Backend-PostGRES med records, entities, orders...

### AcaIndex (Index-store)
Elasticsearch søgemaskine.

### AcArchive (file-store)
Digitalt arkiv for alle filer som vi er ansvarlige for. Separat blobstore for access og masterfiler.
Inklusiv PostGRES med metadata om filer

### SAM
Registreringssystem. Skal ændres til webbaseret værktøj og gentænkes.

### COMMANDLINE TOOLS
Konvertering    ConvertTool
Identifikation  Digiarc
Kopiering       Værktøj til kopiering af filer efter ingest
Datafetcher     Værktøj til hentning af data fra arkiveringsversioner

### WORKFLOW-ENGINE
Checksumtest    
QA-flows        Af registreringer
Imports         Flow til regelmæssig hentning af data fra eks. DBC, WikiPEdia, MTM, 


### Import-jobs
- Kort
- Family Search
- Biblioteksbasen
- Bennys skanninger
- Poders skanninger

### 

## Temporal Projects and services

### NotesArkivering

### DSG19

### Machinelearning-Project
Nina

### Web-based crowdsourcing-tool
What??
