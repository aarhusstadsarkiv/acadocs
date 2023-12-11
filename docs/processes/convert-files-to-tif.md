# Convert files to tif
I dette sidste trin skal afleveringen køres gennem en række kommandoer før slutresultat af afleveringen vil være i et myndighedsgodkendt format.

Målet for det sidste trin er at køre kommandoen ```statutory```<br>
Forudsætningen er at der allerede er kørt kommandoen ```master```


## 1. Kør ```ammendmaster```
```ammendmaster``` løber afleveringen igennem og ved mapper med filer af formaten ```.odt```, ```.odp```, ```.ods``` og/eller ```.odg``` og uden en pdf, så vil der blive dannet pdf til pågældende filer.

```
convertool /path/to/MasterDocuments/_metadata/files.db /path/to/MasterDocuments ammendmaster
```

## 2. Kør ```replacepdf```
For at sikre at ```statutory``` ikke giver en error, så konverters pdf'erne med versionerne 1.4, 1.5, 1.6 og 1.7 til pdf/a.

```
convertool /path/to/MasterDocuments/_metadata/files.db /path/to/MasterDocuments replacepdf
```

## 3. Kør ```statutory```
Inden statutory køres, så opret en ny mappe ved navnet ```Statutory``` i roden af afleveringen.

```
convertool /path/to/MasterDocuments/_metadata/files.db /path/to/Statutory statutory
```

Nu er afleveringen færdig.