# Convert files

Konvertering af indkomne original-filer fra myndigheder eller private parter er en kompleks proces, som involverer en del viden, værktøjer og procestrin.

Forudsætningen for filkonvertering er [`identifikation`](../processes/identify-files.md) med digiarch og tilhørende værktøjer.

## Opsætning

For at opsætte og forstå convertool, læs da guiden [her](../tools/convertool.md). 

## 1. Kør convertool

Inden `convertool` køres, så opret en ny mappe ved navnet `MasterDocuments` i roden af afleveringen. I denne mappe gemmes de konverteret filer.

```Bash
convertool /path/to/Original-files/_metadata/files.db /path/to/MasterDocuments ammendmaster
```

## 2. Kør Symphovert

`Symphovert` konverterer Lotus WordPerfect (lwp)-filer. Bemærk at programmet skal køres på en computer med IBM Symphony installeret.

!!! attention "Bemærk"
    Mens symphovert kører, kan man ikke foretage andet på pc'en, som heller ikke må låses. Da der under processen kan komme personfølsomme oplysninger til syne på skærmen, skal pc'en stilles en på læsesalen, hvis programmet køres om natten eller i weekenden.

Når programmet har kørt vil der typisk være en mængde fejl, der kan løses ved blot at lade programmet køre igen. Symphovert køres derfor gentagne gange, indtil antallet af fejl har været det samme to gange i træk.
