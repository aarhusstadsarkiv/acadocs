# At udvikle på convertool
Convertool er vores eneste værktøj som køre inde i en Docker container. Denne guide vil
beskrive hvordan man nemmest tilknytter VS Code sådan at man kan udvikle, teste og debugge
værktøjet mens det køre inde i containeren, uden at man behøver at genbygge containeren o.l.

## Executive summary: 
1. Start docker containeren med kommandoen: `docker run -it -v [sti_til_lokal_kopi_convertool]:/root/convertool --name convertool --rm convertool`
2. Åben et tomt VS Code vindue
3. Åben command-pallette (CTRL+SHIFT+p)
4. Vælg kommandoen `Dev containers: Attach to running container...` 
5. Den kørende convertool container skulle kunne findes. Vælg den. 

Du har nu alt VS Code functionalitet inde i en container, og dine lokale ændringer opdateres på din egen maskines lokale kopi.


## Udvikle inde i en Docker container, den lange forklaring
Først skal Docker billedet bygges. Se [Docker guiden](docker.md)
for at se hvordan man bygger et billede og bruger docker på et basalt niveau.


Når billedet er bygget skal starter vi en container baseret på det. Her skal vi bruge `docker run` kommandoen, men vi skal sikre os at vi for indsat (mounted) de rigtige mapper fra vores egen maskine ind i containeren. Mappen med convertool koden vigtig at få mounted, da det ellers bliver bøvlet at rette i koden efter at containeren er startet. Vi mounter mapper med `-v` flaget, det tillader os at lave et link mellem en af vores lokale mapper og en mappe i docker containeren. Vi giver den disse parametre ved at skrive 

`docker run -v [abselout_sti_til_mappe_på_pc]:[mappe_i_container]`

Hvis vi f.eks. havde vores lokale kopi af convertool gemt under stien `C:\development\github\convertool`, så kan vi lavet et link til mappen i containeren ved at skrive 

`docker run -v C:\development\github\convertool:/root/convertool`.

Hvis vi ønsker at havde adgang til flere mapper, f.eks. en mappe med data til manuelle test o.l. så skriver vi blot flere `-v` flag i `docker run` kaldet.


[TBD]


## At debugge i en docker container
VS Codes inbyggede debugger er et stærkt værktøj til at finde fejl i ens kode. For at bruge den i en dokker container skal din VS Code være tilknyttet containeren som beskrevet før.

Da de fleste af vores værktøjer er CLI-baseret, kan det gøre en debug sessions svær. Det nemmeste er følgende:
1. Lav en `debug.py` file i test mappen hvor de kalder CLI'en med de parametre du gerne vil teste. Se convertool's `debug.py` for et eksempel
2. Åben vscodes `debug` panel. Her vil der være en prompt der spørger dig om du vil lave en launch.json fil, gør det
3. I den nye prompt, vælg python file. Du for så en standard python file template du kan definere en dbug session i.
4. Definer følgende attributer i filen således:
    ```
    "name": "Python: debug",
    "type": "python",
    "request": "launch",
    "program": "AbseloutStiTilTestMappen/debug.py",
    "justMyCode": true
    ```
5. Kør debug scriptet i vscode

Der er andre mere modelerbare måder at køre det på, men erfaring fortæller at de er mindre robuste og er mere error-prone i et container-enviroment.