# Identifikation af filer
Identifikation af indkomne filer fra myndigheder eller private parter er en kompleks proces, som involverer en del viden, værktøjer og procestrin.

## 1. Identifikation af indkomne filer
Før alt andet skal de indkomne filer så vidt muligt identificeres. Dette gøres med [`digiarch`](../tools/digiarch.md).

!!! Status

    === "Problemer"

        * Sed sagittis eleifend rutrum

    === "Mangler"

        * Sed sagittis eleifend rutrum

    === "Erfaringer"

        * Sed sagittis eleifend rutrum


## 2. Omdøbning af filer
Efter identifikationsprocessen vil der almindeligvis være filer, der enten ikke kunne identificeres, eller hvor filendelsen enten mangler eller er uforenelig med det filformat, som `digiarch` har fundet. Alle tilfælde, hvor puid ikke kunne determineres eller hvor der er mismatch til filendelsen, vil være oplistet i _IdentificationWarnings-viewet. Det er kun følgende warnings, der skal løses:

- "Extension mismatch"
- "puid is Null"
- "Match on text only; extension mismatch"

Inden næste trin skal man som minimum omdøbe alle komplekse filer (.msg, winmail.dat, .7z, .zip ...), som [`unarchiver`](../tools/unarchiver.md) skal udpakke. Vi behøver ikke identificere filerne igen umiddelbart efter omdøbning, da [`renamer`](../tools/renamer.md) opdaterer både database og filnavne.

!!! Status

    === "Problemer/mangler"

        * I db-filen kan man ikke se, hvilken extension man burde benytte. Det er til tider svært, at vide, hvilken extension et givet filformat bruger.
        * I db-filen kan man ikke se, hvilken version af et givet filformat en fil måtte tilhøre. Det skal eventuelt bruges til at vælge extension.
        * Vi skal gerne have default extension ud af Siegfried. Hvordan ellers lave et dryrun?

    === "Roadmap"

        * Burde `renamer` have et default kald, hvor alle identificerede filnavne UDEN extension blev omdøbt?
        * Burde man køre et dry-run, hvor alle blev omdøbt og lagt i foldere, navngivet efter deres puid? Ville speede processen op.
        * 'rename-all' 

    === "Tips"

        * brug ctrl + z til at tilbage-navngive filer i stifinderen
        * brug /Ex/ (regex som filterer efter stort E) som filter i DB for at filtrere filer som stadig har "Extension mismatch"


## 3. Udpak alle komplekse filer
Brug ```unarchiver``` til at udpakke komplekse filer.

!!! Status

    === "Problemer"

        * Sed sagittis eleifend rutrum
        * Donec vitae suscipit est
        * Nulla tempor lobortis orci

    === "Mangler"

        1. Sed sagittis eleifend rutrum
        2. Donec vitae suscipit est
        3. Nulla tempor lobortis orci

    === "Erfaringer"

        * Sed sagittis eleifend rutrum
        * Donec vitae suscipit est
        * Nulla tempor lobortis orci


## 4. Identificér filer igen
Efter unarchiver, skal ```digiarch``` køres igen, så alle filer i alle underliggende mapper identificeres.

!!! Status

    === "Problemer"

        * Sed sagittis eleifend rutrum
        * Donec vitae suscipit est
        * Nulla tempor lobortis orci

    === "Mangler"

        1. Sed sagittis eleifend rutrum
        2. Donec vitae suscipit est
        3. Nulla tempor lobortis orci

    === "Erfaringer"

        * Sed sagittis eleifend rutrum
        * Donec vitae suscipit est
        * Nulla tempor lobortis orci


## 5. Identificér problematiske filformater
Brug Jespers værktøj til at identificere, hvilke filformater i indkomsten, som vi aktuelt ikke kan håndtere, så vi allerede på dette tidspunkt i processen ved hvor problemerne findes.

!!! Status

    === "Problemer"

        * Sed sagittis eleifend rutrum
        * Donec vitae suscipit est
        * Nulla tempor lobortis orci

    === "Mangler"

        1. Sed sagittis eleifend rutrum
        2. Donec vitae suscipit est
        3. Nulla tempor lobortis orci

    === "Erfaringer"

        * Sed sagittis eleifend rutrum
        * Donec vitae suscipit est
        * Nulla tempor lobortis orci


## 6. GIS-filer skal merges
Eter omdøbning og udpakning, skal eventuelle GIS-filer merges, så de identificeres og konverteres korrekt.

!!! Status

    === "Problemer"

        * Sed sagittis eleifend rutrum
        * Donec vitae suscipit est
        * Nulla tempor lobortis orci

    === "Mangler"

        1. Sed sagittis eleifend rutrum
        2. Donec vitae suscipit est
        3. Nulla tempor lobortis orci

    === "Erfaringer"

        * Sed sagittis eleifend rutrum
        * Donec vitae suscipit est
        * Nulla tempor lobortis orci

