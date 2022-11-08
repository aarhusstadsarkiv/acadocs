# Git og Github workflow
Denne side vil gennemgå hvordan man arbejder med Git og GitHub. Gennemgangen er delt op i tre dele:  
1. En basal gennemgang af Git og GitHub     
2. Hvordan man arbejder med kode fra github. Det inkluderer hvordan man cloner et repository fra GitHub, laver en ny branch til de ændringer og modifikationer man laver, commiter og pusher ens kode op til github, og laver en pull-request, så koden er klar til gennemgang, inden den bliver en integreret del af projektet.  

3. Hvordan man gennemgår (reviewer) andres pull-request, kommer med kommentare og ændringsforslag.


## Git og GitHub
Git er et værktøj, som hjælper med versionskontrol af softwareudvikling. Git gør det muligt altid at havde et klart overblik over hvor man er i udviklingen af ens produkt. Den tillader også en altid at går tilbage til tidligere versioner af ens kode og lave forskellige 'forgreninger' (branches) af ens kode, som kan indeholde forskellige features, ændringer og forbedringer, som ikke er klar til at blive en del af det endelige produkt.  
GitHub er en online tjeneste, som hoster ens Git repositories og tillader at man kan arbejde på samme kode samtidig. Samtidig er det muligt for andre at gennemgå ens ændringer, inden de bliver en del af det endelige produkt.  

> **Takeaway:** Git er et værktøj, som hjælper med softwareudvikling og versionskontrol. GitHub er en online overbygning på Git, som gør man kan samarbejde om at udvikle.

### Terminologi
Git og GitHub har deres egen terminologi. Herunder er en kort forklaring af de vigtigste koncepter. GitHub har også selv en ordbog, som kan findes ved dette [link](https://docs.github.com/en/get-started/quickstart/github-glossary).

- **Repository** (oftest forkortet til **Repo**): Det mest essentielle element ved GitHub. Her er alle de file som er knyttet til det projekt man arbejder på. Man kan se det som mappen projektet er gemt i.
- **Branch:** 

## Workflow: udvikling
Herunder er workflowet for hvordan vi arbejder på et repo beskrevet. Det er delt op i fem punkter, som alle beskriver en separat del af processen. Under hvert punkt er en højniveaubeskrivelse af selve processen. Derefter følger en forklaring.

### 1. Clone
Sikre dig at du altid clone'er fra 'Main'. (medmindre du skal arbejde videre på en anden branch?)
Når du har sat et repository op lokalt, opret eller skift over til den branch som du gerne vil arbejde på.  
Sørg for at branchen er navngivet hensigtsmæssigt, sådan man hurtigt kan se hvad der udvikles på den. 'feature' eller 'bug_fix' fortæller ikke nogen hvilke ændringer der laves

### 2. Code
Implementer dine ændringer/features. Her er der flere ting man skal være opmærksom på.  
- Sørg for at dine ændringer passer med den branch du er på. Hvis du går i gang med at implementerer en ny feature på en branch der er der for at fixe en bug, så enten skift branch, opret en ny branch, eller opret en feature request til senere.
- Husk at køre de relevante tests, og tjek at din kode faktisk fungere

- Hold din implementering på et minimum. Når koden virker og opfylder alle test, så lav som udgangspunkt være med at tilføje mere til den commit. Lav en pull-request på din nuværende version, og find derefter ud af om du skal arbejde videre på den branch eller en ny

- Sørg for at køre linting på koden, sådan den opfylder best-practices i forhold til stil og formatering. I Python scripts gøres det ved at køre black, flake8 og derefter mypy, hvis du har skrevet din kode med typehints (som anbefales)

### 3. Commit 
Commit derefter dine ændringer. Hold dine commits små og overskuelige. Hvis du laver flere commits på den samme pull-request, så lav commits gradvist. Det er langt mere overskueligt for revieweren med 3 små commits end 1 stor, der ændrer i flere forskellige aspekter af repo'et.

> **HUSK** at køre linting og lignende inden du commiter!


### 4. Push
Push dine ændringer op til GitHub.

### 5. Pull-request
Opret en pull-request. 
- Assign gerne med det samme en reviewer, sådan personen hurtigt kan kigge på koden. 
- Hvis man specifikt adresserer en issue eller feature request, så skriv Id'et i pull-requesten eller tulføj den under `development`

## Workflow: Review af andres kode
Herunder beskrives hvordan man mest hensigtmæssigt tjekker andres kode. 

=======
Opret en pull-request. Assign gerne med det samme en reviewer, sådan personen hurtigt kan kigge på koden. Hvis man specifikt adresserer en issue eller feature request, så skriv Id'et i pull-requesten.

## Workflow: Review af andres kode
Herunder beskrives hvordan man mest hensigtmæssigt tjekker andres kode. 
