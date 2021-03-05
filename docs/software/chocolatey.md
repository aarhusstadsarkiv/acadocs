# Chocolatey
Chocolatey er en såkaldt package manager til Windows. Formålet er, at gøre det lettere at installere og opdatere software.

## Installation
Denne installationsguide er skrevet med udgangspunkt i [den officielle guide](https://chocolatey.org/install).

1. Tryk på Access Director
2. Start PowerShell som administrator (højreklik -> Kør som administrator)
3. Kør følgende kommando (kopier og indsæt i PowerShell): 
```powershell 
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```
4. Lad installationen køre færdig
5. Luk PowerShell og kør en ny som administrator
6. Skriv `choco` for at tjekke, at installationen er gået som forventet

Når programmer installeres via Chocolatey, skal det altid foregå via en administrativ PowerShell. Programmer kan findes ved at søge i [packages](https://chocolatey.org/packages).

### Eksempel på installation
Følgende kommando installerer den seneste version af Python.

```powershell 
choco install python
```

## Opdatering 
Alle programmer installeret via Chocolatey skal opdateres via Chocolatey, igen i en administrativ PowerShell. Følgende kommando opdaterer alle installerede programmer.

```powershell 
choco upgrade all
```

Hvis man blot vil opdatere et program, skal man bare skrive navnet. Her bruges Python igen som eksempel.

```powershell 
choco upgrade python
```

Den officielle dokumentation af `upgrade` kan findes [her](https://docs.chocolatey.org/en-us/choco/commands/upgrade).
