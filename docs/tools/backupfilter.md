# Backupfilter
The user needs the backupdatabase as a csv-file to input to this cli.


## Installation
```powershell
pipx install git+https://github.com/aarhusstadsarkiv/filter_csv_generic.git
```

Test efterfølgende installationen med følgende kommando:

```powershell
backupfilter --version
```

eller

```powershell
backupfilter --list
```

eller 

```powershell
backupfilter --help
```
for eksempler.

Opdatér med:
```
pipx update backupfilter
```

Afinstallation
```
pipx uninstall backupfilter
```


## Brug
Filtrerer en backupdatabase som csv-formatteret fil.

Angiv et antal --filter for at filtrere efter det ønskede:
### --filter [field] [operator] [value]:
hvor value er den ønskede søgeværdi:

ex. --filter Samling contains Salling.

### --list:
Viser de for søgning tilgængelige felter samt deres operatorer.

### --filename [filename]
hvis der ønskes et specielt filnavn til filerne med de fundne indgange, kan det angives her. Nyttigt hvis der laves flere søgninger i streg
til samme bibliotek, således forhindres overskrivning af tidligere resultater.l

### --or_
Hvis der er flere filtre, "and"'es de sammen som standard, men hvis denne option angives, bliver de "or"'ede sammen i stedet.

### --version
Printer den aktuelle version af programmet.

### --help
```
Filters the csv formatted backup database:
Apply any number of --filter
Use --list for allowed fields and operators.

ex.1 field is "id-field"
--filter Samling equalTo 1
or
--filter Samling contains Salling

ex.2 if the field is a dictionary, and target search is in a key's value:
--filter "Administrative data" contains "Bestillingsinformation:negativsamlingen 1970"

ex.3 if the field is a dictionary, and filter after target has a certain key:
--filter "Administrative data" hasKey Bestillingsinformation

ex.4 the field is a dictionary and the key is known and filter on the value:
--filter "Beskrivelsesdata" contains Typer:Farve

positional arguments:
  input_csv_file_path   Path to the backup database csv file.
  output_dir            Path to the output/result directory for csv file(s).

options:
  -h, --help            show this help message and exit
  --filter FILTER FILTER FILTER
                        Adds a filter to limit the results
  --version             States the current version of this CLI.
  --list                Lists the allowed fields and their operators.
  --filename FILENAME   specify the output filename(s).
  --or_                 the filters are or'ed together.
```