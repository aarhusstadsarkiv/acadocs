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

### Eksempel på pakkeinstallation
Følgende kommando installerer den seneste version af Python.
```powershell 
choco install python
```