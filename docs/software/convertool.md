# Convertool
Convertool er en CLI (Command-Line Interface), som benyttes til at konvertere filer, således de overholder Aarhus Stadsarkivs arkiveringskrav. Der læses fra DB-filen produceret af [Digiarch](digiarch.md).

## Installation
Convertool skal installeres via [releases](https://github.com/aarhusstadsarkiv/convertool/releases) på GitHub. Download først den seneste `.whl`-fil, og installér herefter denne via `pip`.

!!! hint "Eksempel"
    ```powershell
    pip install --user C:\Downloads\convertool-0.3.4-py3-none-any.whl
    ```

!!! attention "Bemærk"
    Installation via `.whl`-filer kræver, at pakken `wheel` er installeret. Dette skal kun foregå første gang man har behov for at installere wheels, og det gøres som følger.
    ```powershell
    pip install --user wheel
    ```

Når Convertool skal opdateres, bruges igen `pip` med en ny `.whl`-fil og `--upgrade`.

!!! hint "Eksempel"
    ```powershell
    pip install --user --upgrade C:\Downloads\convertool-0.3.5-py3-none-any.whl
    ```

## Forudsætninger

## Opbygning

## Konvertering

## Fejlrettelser