# Convert files

Konvertering af indkomne original-filer fra myndigheder eller private parter er en kompleks proces, som involverer en del viden, værktøjer og procestrin.

Forudsætningen for filkonvertering er [`identifikation`](../processes/identify-files.md) med digiarch og tilhørende værktøjer.

## Opsætning

For at opsætte og forstå convertool, læs da guiden [her](../tools/convertool.md). Der gennemgås hvordan man bruger convertool i docker, og for hentet det dockerbillede ned man skal brug til at konveterer. Når det billede er på den maskine der skal køre conveteringerne, så kan du forsætte med resten af opsætningen.

### Åben en docker container med convertool billedet til en aflevering

For at begynde at konvetere en aflevering skal man åbne en convertool container, hvor man har mounted den aflevering man gerne vil konveterer ind. den nemmeste måde at gøre dette på er ved at åbne en ny PowerShell terminal i roden af den aflvering man gerne vil arbejde med og køre følgende kommando i en PowerShell terminal:

`docker run -v ${pwd}:/root/handins --it [BILLEDE-ID]`

Denne kommando gør flere ting på en gang, så her er en gennemgang af hvad de forskellige dele af den gør:

- `-v ${pwd}:/root/handins` mounter den directory som ens shell er i lige nu ind i docker containeren som man starter op i folderen `/root/handins` i containeren. Dvs. at selvom containeren køre i sit eget, lukkede miljø, skabes der her en 'åbining' hvor den har direkte adgang til præcis denne mappe, som er lokalt på din egen maskine. De programmer der er i Docker containeren kan så arbejde på data i denne mappe, dvs. afleveringen.
- `--it` er to flag, som fortæller docker at den skal åben en **i**nteractiv **t**erminal, dvs. den terminal du køre kommandoen i. Uden denne kommando vil man skulle åbne en ny terminal og køre en anden kommando for at komme ind i containeren.
- `BILLEDE-ID` eller `IMAGE-ID` er et unikt ID som docker tildeler ethvert billede. Det kan findes ved at kalde kommandoen `docker image ls`. Kommandoen giver en liste af alle billeder som er lokalt på ens maskine. Alternativt kan man også åbne 'docker desktop', gå under 'images' og i 'name' kollonen er ID'et til bileldet angivet under navnet på bileldet, samt med en genvej til at kopierer det fulde ID til udklipsholderen.

Når kommandoen er kørt vil den åbne terminal være din eneste indgang ind i containeren. Bemærk at hvis du lukker terminallen uden at afslutte containeren med `exit()`, så køre den videre i baggrunden og sluger resourcer. For at lukke den igen skal du åbne docker desktop og finde den under 'containers' her vil den stå som værende kørende, og de kan lukke den herinde.

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
