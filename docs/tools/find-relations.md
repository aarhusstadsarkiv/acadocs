---
date: 2023-09-07
---

# find-relations

Dette program konverterer SQLite-databaser til specielle filer for hurtigere søgning af relationer mellem tabeller.

## Installation

find-relations skal installeres via master branch med [pipx](pipx.md):

```powershell
pipx install git+https://github.com/aarhusstadsarkiv/find-relations.git
```

## Brug

```
find-relations [OPTIONS] COMMAND [ARGS]...

Options:
  --help  Show this message and exit.

Commands:
  encode  Encode a database.
  search  Search an encoded database.
```

Det første trin er at konvertere databasen ved hjælp af kommandoen `encode`.

Når den nye fil er klar, kan `search`-kommandoen søge efter specifikke værdier.

### encode

```
find-relations encode [OPTIONS] FILE [OUTPUT]

Options:                                                                      
  --hash NAME     The hash algorithm to use.  [default: md5]
  --sample ROWS   Encode a random sample of ROWS rows for each table.  [x>=1]
  --ignore-types  Do not encode type information.
  --help          Show this message and exit.
```

Konvert en SQLite-database `FILE` til et søgbart format, der indeholder hasherne for hver celles værdi.

Resultatfilen vil blive gemt som `OUTPUT` eller som `FILE`.dat.

Altid tilgængelige hash-algoritmer er: blake2b, blake2s, md5, sha1, sha224, sha256, sha384, sha3_224, sha3_256,
sha3_384, sha3_512, sha512, shake_128, shake_256.

## search

```
find-relations search [OPTIONS] FILE

Options:
  --value <SQL-TYPE JSON-VALUE>  Search for a specific value.
  --cell <TABLE ROW COLUMN>      Search for the value in a cell.
  --column <TABLE COLUMN>        Search for all values in column.
  --max-results INTEGER          Stop after INTEGER results.  [x>=1]
  --include-null                 Do not skip null values.
  --show-all-results             Do not aggregate results.
  --help                         Show this message and exit.
```

Søg efter specifikke værdier, celler eller kolonner i en konverteret `FILE`.

Se [`find-relations encode`](#encode) for hjælp til at konvertere en database.