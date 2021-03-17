# Digitalt Materiale
Modtagelse af digitalt materiale

## Identifikator
Når digitalt materiale modtages til bevaring hos Aarhus Stadsarkiv, skal afleveringen gives et AARS-ID med følgende mønster.

```regex
^(\w+\.)?(AARS\.).*
```

Dette mønster forstås som følger.

1. `^(\w+\.)?` angiver, at der kan være et valgfrit alfanumerisk præfiks afsluttet med punktum, for eksempel `AVID.`
2. `(AARS\.)` angiver, at strengen `AARS.` er påkrævet som del af navnet
3. `.*` angiver, at der kan være et valgfrit postfiks

Derfor er følgende eksempler valide AARS-ID'er

!!! hint "Eksempel"
    `AVID.AARS.3.1`

    `AARS.TEST`

    `123bc.AARS.2s`

mens disse ikke er

!!! hint "Eksempel"
    `AVID.3.1`

    `AARSTEST`

    `123.bc.AARS.2s`
