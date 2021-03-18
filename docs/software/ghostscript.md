# Ghostscript
Ghostscript er en såkaldt *interpreter* til PostScript-sproget og PDF-filer. Et typisk brugsscenarie på Aarhus Stadsarkiv er konvertering af PDF'er til PDF/A-format.

## Installation
Ghostscript kan installeres via [Chocolatey](chocolatey.md)

```powershell
choco install ghostscript.app
```
eller via download af en `.exe`-fil [her](https://www.ghostscript.com/download/gsdnld.html). 

!!! attention "Husk"
    Chocolatey skal **altid** bruges via en administrativ PowerShell.


## Systemmiljøvariable

Det er nødvendigt at tilføje `bin`-folderen i installationslokationen til brugeres `PATH`-miljøvariabel. Den typiske sti er `C:\Program Files\gs\gs[version]\bin`.

!!! hint "Eksempel"
    Antag, at Ghostscript version 9.53.3 er installeret. Følgende sti skal nu tilføjes til `PATH`.
    ```powershell
    C:\Program Files\gs\gs9.53.3\bin
    ```

Systemmiljøvariable opdateres som følger.

1. Find indstillingen "Rediger systemmiljøvariablerne"

    ![img](../img/find_env.png)
2. 