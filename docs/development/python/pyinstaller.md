# PyInstaller
Herunder følger en kort guide til [PyInstaller](https://www.pyinstaller.org/), som på Aarhus Stadsarkiv bruges til at generere eksekverbare filer af forskellige værktøjer.

## Installation
`PyInstaller` skal installeres på lige fod med andre dependencies via `poetry`. Der er tale om en development dependency, og derfor er kaldet som følger.

```powershell
poetry add pyinstaller --dev
```

## Kompilering
Når `PyInstaller` skal kompilere, skal den kaldes i det virtuelle environment, således alle dependencies kan findes.

```powershell
poetry run pyinstaller [OPTIONS] [ARGS]
```

De oftest brugte optioner er `-F` og `-w`. `-F` angiver, at der skal kompileres til en enkelt fil, mens `-w` angiver windowed mode. 

```powershell
poetry run pyinstaller -F -w [ARGS]
```

Det er vigtigt, at PyInstaller kaldes direkte fra det directory, hvori `main.py` eller ækvivalent befinder sig.

```powershell
cd path\to\main
poetry run pyinstaller -F -w main.py
```

Når PyInstaller har kørt første gang, genereres en `.spec`-fil. Denne fil kan benyttes til finpudsning af kompileringsprocessen: det er blandt andet muligt at angive specifikke ikoner mv. Hvis man beslutter sig for at bruge `.spec`-filen fremover, skal den under source control, og PyInstaller skal kaldes med filen som følger.

```powershell
cd path\to\specfile
poetry run pyinstaller specfile.spec
```


## Debugging
Generelt er det nemmest at debugge ved at undgå at bruge windowed mode, og køre den resulterende `.exe`-fil via PowerShell. 

```powershell
## debug
cd path\to\main
poetry run pyinstaller -F main.py
dist\main.exe
```

Til tider virker windowed mode ikke på Windows overhovedet. I så tilfælde må man nøjes med at kompilere til én fil, og leve med, at et lille PowerShell vindue dukker op bag programmet.