# Rearranger
Rearranger bruges til at flytte udpakkede filer over i nye mapper, så de ligger i hver deres mappe.


## Installation
Rearranger skal installeres via master branch med [pipx](pipx.md):

```powershell
pipx install git+https://github.com/aarhusstadsarkiv/rearranger.git
```

Test efterfølgende installationen med følgende kommando:

```powershell
rearranger --version
```

Når man skal opdatere rearranger, bruges `pipx` med `--upgrade`:

```powershell
pipx upgrade rearranger
```

## Brug
Rearranger bruges ved følgende kommando:

```powershell
rearranger docCollection_dir newDocCollectionNumber dIDCounter newTableNumber
```
Hvor "docCollection_dir" er den fulde sti til den mappe, hvori `docCollection1`-mapperne ligger (eg. `OriginalFiles`); "newDocCollectionNumber" er nummeret på den nye docCollection-mappe, hvortil filerne flyttes (hvis der i forvejen er 3 docCollections vil den nye mappes nummer være `4`); "dIDCounter" er den sidste fil i den sidste docCollections nummer + 1 (hvis der er 34875 filer vil nummeret være `34876`), og "newTableNumber" er antallet af eksisterende tables (i tables-mappen) + 1 (dvs. at hvis der er 10 tables vil tallet være `11`).

!!! hint "Eksempel" 
    ```powershell
    rearranger D:\AVID.AARS.84.1\OriginalFiles 4 34876 11```
