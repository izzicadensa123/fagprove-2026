# Planlegging Prøve Fagprøve
  <p>07/04/2026 - 15/04/2026</p>

## Målet med oppgaven:
Målet med denne oppgaven er å utvikle en enkel applikasjon hvor brukere kan registrere bøker og holde oversikt over utlån og innlevering.

- **Applikasjonskomponenter:** Det skal være en klient side og en server side hvor bøker og utlånsdata er lagret trygt, og disse blir servert til klient via sikret API.
- **Bokhåndtering:** Mulighet for å legge til, vise, endre og slette bøker, hver med tittel, forfatter, sjanger og status.
- **Utlånshåndtering (admin):** Mulighet for å registrere utlån og innlevering med dato og navn på låner. Mulighet for å sende purring på ikke-innleverte bøker.
- **Oversikt (admin):** Vis en enkel oversikt over tilgjengelige bøker, utlånte bøker og forfalte utlån.
- **Oversikt (låner):** Vis en enkel oversikt over tilgjengelige bøker. Vis om en bok er utlånt eller ikke.
- **Lagring:** Bok- og utlånsdata med metadata lagres på server.
- **Sikkerhet:** Sikre at data ikke er tilgjengelig for uvedkommende. 
- **Design:** Design som møter grunnleggende krav til universell utforming og tilgjengelighet.

## Sjekkliste kjernefunksjonalitet:
- Lag datamodell ved bruk av DrawSQL
- Lag skisse av app ved bruk av Figma som designverktøy
  
## Sjekkliste ekstrafunksjonalitet:
- Sortering bok sjanger

## Fremgangsmåte:
1. Jeg skal begynne ved å planlegge datamodellen, layout på app, og hvordan sikkerheten skal virke
2. Lage SQL Objekter
   - Tabeller og trigger
   - Views
   - Procedures
3. Lage app visuelt etter layout plan
4. Legge til planlagt kjernefunksjonalitet i appen, som f.eks. mulighet til å legge inn bøker, redigere bøker, låne bøker o.l.
5. Finpusse og teste at all kjernefunksjonalitet fungerer som forventet
6. Vurdere ekstra funksjonalitet og om jeg har tid til å implementere det
7. Kjøre testing av appen og lage rapport på det
9. Skrive system dokumentasjon
10. Skrive bruker dokumentasjon om hvordan appen fungerer
11. Gå gjennom presentasjon og evt presentere for andre lærlinger for å finne ut hvilken flyt som passer


## Skisse:
### Data model:
<img width="1764" height="673" alt="image" src="https://github.com/user-attachments/assets/b369c2f9-197c-4842-8ebf-6ca761ebdcb8" />

- **Universal Columns**: Standard kolonner som kommer i alle Omega365 CTP tabeller.
- **Persons**: En system tabell som kommer ferdig definert i et Omega365 instans, har en rad per bruker i systemet og lagrer diverse metadata som navn, epost etc.
- **OrgUnits**: En system tabell som kommer ferdig definert i et Omega365 instans, hjelper med tilgangsfordeling.
- **Rolle / Modul tabeller**: De er system tabeller som kommer ferdig definert i et Omega365 instans, disse blir brukt for tilgangsstyring.
- **Books**: En tabell som lagrer alle bøkene man oppretter.
- **Authors**: En tabell som lagrer alle forfatterene til bøkene.
- **Genres**: En tabell som lagrer alle sjangerene til bøkene.
- **BookLoans**: En tabell som lagrer alle uttak på bøker ( historikk for lån og levering tilbake ).
- **BookGenres**: En tabell som lagrer sjangerene til bøkene, grunnen til at det er en egen tabell er fordi det kan være en bok med flere sjangre.
- **BookAuthors**: En tabell som lagrer forfatterne til bøkene, grunnen til at det er en egen tabell er fordi det kan være en bok med flere forfattere.

### Grov skisse:

<p>
  <img width="493" height="420" src="https://github.com/user-attachments/assets/6357699c-5804-4219-8b31-48f152a6f27f" /><br/>
  På dette bildet så ser du tabben "Dashboard" dette vil da bare være tilgjengelig for admin. Dette er da bare en kjapp oversikt over bøkene.
</p>

<p>
  <img width="494" height="420" src="https://github.com/user-attachments/assets/d16a1805-d048-4d3d-959b-dfcfa63b95b2" /><br/>
  På dette bildet så ser du tabben "Alle Bøker" hvor du får en oversikt over alle bøker og kan filtrere å søke på bøkene som ligger inne. Her viss du trykker view knappen får du en større oversikt over boken hvor du da åpner et detaltje vindu hvor du kan lere mer om boka men også låne den. Dette får dere se lengre nede. Du får også muligheten til å legge til nye bøker i dette bildet men er usikker på om dette bare skal bli gjort av administratorer eller om vanlige brukere skal kunne gjøre dette også.
</p>

<p>
  <img width="491" height="418" src="https://github.com/user-attachments/assets/61577da8-3f47-4cfa-8d78-a1833ac2f4a4" /><br/>
  På dette bildet så ser du tabben "Utlånte Bøker" hvor du får en oversikt over alle utlpånte bøker og når de ble lånt ut/ forfallsdatoen og en kolonne som heter actions hvor du skal få muligheten til å purre på personen som har lånt boka. Denne visningen blir også for administratorer foprdi det er en generell oversikt over alle bøker som er lånt ut.
</p>

<p>
  <img width="494" height="416" src="https://github.com/user-attachments/assets/f262eb8c-ec5e-43af-b617-72ebfe0169dc" /><br/>
  På dette bildet så ser du tabben "Mine Lånte Bøker" hvor du får en oversikt over alle dine egne lånte bøker hvor du da har oversikt over bøker som du selv har lånt ut og hvor lenge du har igjen før du må levere den tilbake/ og muligheten til å levere inn boka ( dette vil da bli mulig å gjøre enten i actions kolonna eller kanskje muligvis en knapp ).
</p>

<p>
  <img width="311" height="322" src="https://github.com/user-attachments/assets/a2beef16-d4f8-42dd-90ac-742924cd8a1f" /><br/>
  På dette bilde så ser du en modal som åpnes opp når du trykker på "View" i Alle bøker viewet hvor du da får en oversikt over boka men også muligheten for å låne den / litt usikker på om jeg skal legge til funksjonaliteten for å slette bøker i samme modal eller om jeg skal flytte denne funksjonaliteten ut.
</p>

<p>
  <img width="310" height="329" src="https://github.com/user-attachments/assets/f2e0a95c-e07c-4e47-9dd8-20104f4017cf" /><br/>
  På dette bilde så ser du en modal som åpnes opp når du trykker på "+ Legg Til Bok" hvor du får muligheten til å legge til bøker.
</p>
  
## Tidsskjema:
<details>
  <summary>
      Tirsdag <sub>07/04</sub>
  </summary>
  <ul>
    <li> Gjennomgang fagprøve - 1t </li>
    <li> Lage datamodell, layout og skrive plandokument - 6.25t </li>
    <li> Levere plan - Før 17:00</li>
    <li> Logging - 0.25t </li>
  </ul>
</details>
<details>
  <summary>
    Onsdag <sub>08/04</sub>
  </summary>
  <ul>
    <li>Fortsette med tabellene, triggers, views, og procedures - 2t</li>
    <li>Opprette appen og begynne og legge inn design - 5.25t</li>
    <li>Logging - 0.25t</li>
  </ul>
</details>
<details>
  <summary>
    Torsdag <sub>09/04</sub>
  </summary>
  <ul>
    <li>Fortsette med appen sin kjernefunksjonalitet - 6.25t</li>
    <li>Fortsette med SQL biten til appen ( tabellene, triggers, views, og procedures )  - 1t</li>
    <li>Logging - 0.25t</li>
  </ul>
</details>
<details>
  <summary>
    Fredag <sub>10/04</sub>
  </summary>
  <ul>
    <li>Nærme seg inn på å få alle kravene til appen sin kjernefunksjonalitet og beynne vurdering av ekstrafunksjonalitet - 7.25t</li>
    <li>Logging - 0.25t</li>
  </ul>
</details>
<details>
  <summary>
    Mandag <sub>13/04</sub>
  </summary>
  <ul>
    <li>Fullføre appen sin kjernefunksjonalitet og implementere ekstrafunksjonalitet - 7.25t</li>
    <li>Logging - 0.25t</li>
  </ul>
</details>
<details>
  <summary>
    Tirsdag <sub>14/04</sub>
  </summary>
  <ul>
    <li>Ferdigstille appen og kjøre testing for å plukke opp noen bugs - 2t</li>
    <li>Skriv dokumentasjon - 3t</li>
    <li>Utfør og skriv test rapport - 1.25t</li>
    <li>Lag presentasjon - 1t</li>
    <li>Logging - 0.25t</li>  </ul>
</details>
<details>
  <summary>
    Onsdag <sub>15/04</sub>
  </summary>
  <ul>
    <li>Presentere løsning for sensor og prøvenemd</li>
  </ul>
</details>

## Teknologi:

- Omega 365 CTP (Core Technology Platform)
  - Jeg har valgt Omega 365-rammeverket for å bygge applikasjonen. Det som gjør CTP så bra er at det rammeverket håndterer mye av SQL og sikkerhetsfunksjonaliteten noe som gjør det mye mer effektivt og sikrere og utvikle raskt. Omega 365 bruker SQL for databehandling, data lagring og oppbygning av tilgangsstyring. CTP håndterer alt som har med å få data fra SQL serveren til frontend. Det som gjør CTP enda bedre er at det har masse innebygd funksjonalitet som diverse utviklerverktøy, innebygde tabeller og innebygd sikkerhetsstyring
  - VueJS 
  - Bootstrap 
  - Microsoft SQL Server
- GitHub
  - For dokumentasjon som er lett å dele med andre
- DrawSQL
  - For visualisering av datamodell
- Figma
  - For skisse av layout på applikasjonen
- Excel
  - For skisse av layout på applikasjonen

## Kostnader:
- Timerate lærling: 225,-
  - Estimert total pris lærling: 7 arbeidsdager * 7.5 timer * 225 = 11 812.5,-

- Estimert pris for meg som utvikler ( hosting kostnader ) = 10 000,-

## Kilder:
- Vidar Nordnes (Sjef)
- Tor Halvorsen Aasheim (faglig leder)
- [ChatGPT](https://chatgpt.com)
- [OmegaDocs](https://docs.omega365.com/?AreaType=10001&Area-ID=10004)
