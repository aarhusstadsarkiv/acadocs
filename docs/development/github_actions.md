# Githubs `actions` og CI
Her kan du finde nogle korte beskrivelser af:
- Hvad man kan bruge GH `actions` til
- Hvordan man kan tilpasse dem sådan de passer til det specifikke repo
- Hvad og hvordan vi betaler for at bruge actions
- etc.
## Hvad gør en github action?
Med en action på GH er det muligt at køre kode direkte på  GitHub. Vi benytter primært `actions` til at køre CI (continuos development) test, der sikre os at vores ændringer ikke ødelægger funktionaliteten i koden. Vi køre os andre tjek såsom lintin, der sørger for koden er skrevet efter best-practices og codecoverage, der sørger for at vores test tester så høj en del af koden som muligt. Ud over det er det også muligt at:
- Automatisere pull-request og review requests.
- Skabe overblik med automatisk logning og monitorering af projekter.
- etc.

## Tilpas GH actions
En GH `actions` køre i et GH __workflow__. Et __workflow__ kan bestå af en eller flere actions/mindre scripts. Dette Workflow køre hvergang en predefinetet __event__ bliver triggered i et repository. Disse __events__ kan være alle former for ændringer som kan finde sted på GH. F.eks. kan man sætte `actions` op til at køre hvergang en pull-request bliver åbnet/lukket, hvergang en pull-request bliver godkendt (og inden den bliver merged) eller bare hvergang der pushes ændringer op til branchen.
For mere læsning se GitHubs egen [documentation](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions)
Hver workflow køre en eller flere __runners__ som er hver sin VM. Disse runners køre et eller flere __job__, som er en samling af `actions` og scripts. Det er muligt at betinge kørslen af et job, sådan det enten afhænger af at et andet job er kørt først, eller at jobbet kun køre på en bestemt branch.
Det er muligt at bruge et workflow i et andet workflow [[TODO]]: Beskriv hvordan

## Hva koster'ed?
Vi har omkring 3.000 minuters computertid til rådighed. Hver pull-request vi laver koster mellem 5-10 minutter, og hvis vi bruger det koster hvert ekstra minut i 'overage' 0.008 dollars. Vi kan altid tjekke vores forbrug under settings, se denne [guide](https://docs.github.com/en/billing/managing-billing-for-github-actions/viewing-your-github-actions-usage) for hvordan. 

## Side spring: Kort introduktion til YAML
`actions` skrives YAML formatet (enten .yml eller .yaml). Det er et meget bredt format, som har en meget human-readable syntax, men hvis man ikke er vant til det kan man finde introduktioner her:
- [Lær yaml på 5 min](https://www.codeproject.com/Articles/1214409/Learn-YAML-in-five-minutes)
- [Githubs egen documentation for deres brug](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions), med mange gode eksempler.

## Kendte problemer
### Et `required` GitHub check bliver ikke kørt, på trods af at der køre actions
Her er der 2 mulige fejl: 
Enten bliver der pågældende check ikke kørt i de `actions` man har, og derfor venter GH på rm form for respons på checket, som aldrig kommer. Det kan løses ved at tilføje checket til de `actions` man køre.
Hvis checket allerede køre som en del af de `actions` manj køre, kan fejlen være at GH ikke genkender det chek man køre som det `required` check den leder efter. Se følgende [issue](https://github.com/orgs/community/discussions/25720) for en forklaring. Man kan løse det ved at:
- Gå ind i settings på repo'et (kræver admin privilegier)
- Gå under `branches` -> `branch protection rules`. Her er en liste af de `required` checks der skal til for at godkende en `pull-request`
- Fjern det check GH ikke kan få godkendt, og søg efter nylige kørte `actions`. Vælg den `action` du mener køre det pågældende check.
GH er meget emsig med hvilke `actions` der opfylde hvilke checks, så selv små ændringer i indhold eller navn kan skabe problemer


