AarhusArkivet er den primære online indgang til vores samlinger.

## Fejlmeldinger og udviklingsønsker
Indberetning af fejl, udviklingsønsker, spørgsmål med mere foretages gennem issues, [som beskrevet under brugen af Github](../inbox/github.md#indberetninger).


## Teknisk information
AarhusArkivet er aktuelt en Flask-applikation uden integreret database, som hostes på Heroku.

Alle kald til backends foregår gennem et api-modul, som videresender requests til følgende services:

- **Auth0**. Authentication
- **DynamoDB** (AWS). Brugerstyring og Authorization
- **Cloudsearch** (AWS). Fuldtekst søgeindeks
- **Oaws** (GCP). Registreringer af materialer og entiteter
- **Aarhusiana** (GCP). Autosuggest

