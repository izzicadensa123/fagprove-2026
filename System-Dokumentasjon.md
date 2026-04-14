# System Dokumentasjon

Dette er systemdokumentasjonen til fagprøve oppgaven min som er å lage en Låneapplikasjon for bøker. Her dekkes hvilke teknologier som blir brukt, hvordan datamodellen ser ut, hvordan sikkerheten fungerer, hvordan frontenden fungerer, hvilke problemstillinger som ble møtt på, planen videre, og hvilke avvik jeg gjorde fra den originale planen.

## Innhold
- Oppsummering applikasjon
- Teknologier
- Database modell
- Sikkerhet
- Frontend
- Problemstillinger
- Planen videre
- Avvik fra plan


<details>
<summary>
    Oppsummering applikasjon
</summary>
<p>Oppgaven til denne applikasjonen skal la brukere registrere bøker og holde oversikt over utlån og innleveringer og en generell oversikt over bøkene som ligger inne. </p>
</details>
<details>
    <summary>
        Teknologier
    </summary>
    <ul>
        <li>Omega 365 CTP</li>
        <li>Microsoft SQL Server</li>
        <li>Vue.js 3</li>
        <li>Bootstrap 5.3</li>
    </ul>
</details>
<details>
    <summary>
        Database modell
    </summary>
  <img width="1465" height="736" alt="image" src="https://github.com/user-attachments/assets/ea7469a6-1d88-42c3-b28e-6b2305f9a5b9" />
  <p>Datamodellen her er egentlig ganske rett frem. Den består av 6 custom tabeller og 1 av Omega 365 sine system tabeller ( system_persons ).</p>
    <!-- <small>FK - Foreign Key </small> -->
    <details>
        <summary>Tabeller</summary>
        <ul>
            <li>Books - dette er tabellen der alle bøkene lagres med cover til boka, tittel, description, release date, total kvantitet av boka og den tilgjengelige kvantitetet.</li>
            <li>BookLoans - dette er historikk tabellen der alle utlån og innlevering man oppretter blir lagret ned med til og fra dato, book_id som er tilknyttingen til selve boka, personen som lånte boka, hvor mange av denne boka de lånte, notified date som er datoen det ble sendt ut en varsel om utgått innlevering ble sist sendt</li>
            <li>Authors - dette er historikk tabellen der alle forfatterne blir lagret med fornavn og etternavn</li>
            <li>Genres - dette er historikk tabellen der alle sjangreme blir lagret ned med sjangre</li>
            <li>BookAuthors - dette er historikk tabellen der alle bok forfatterne blir lagret ned med bok_id og author_id for å knytte flere authors til en bok viss det er flere forfattere på en bok.</li>
            <li>BookGenres - dette er historikk tabellen der alle bok sjangre blir lagret ned med bok_id og genre_id for å knytte flere sjangre til en bok viss det er flere sjangre på en bok.</li>
            <li>Persons - dette er tabellen der alle brukere i Omega 365 instanset er lagret.</li>
        </ul>
    </details>
    <details>
        <summary>Views</summary>
        <ul>
            <li>BooksOverview - view som henter ut metadata til Books og joiner på linke tabeller for å hente data som sjangre til en bok og forfatterne, den viser også ut tilgjengeligheten til boka ( hvor mange bøker som er available, unavailable, og overdue )  </li>
            <li>BorrowedBooksOverview - view som henter ut metadata til utlån på bøker og joiner seg med linke tabellene for å hente ut data som bok informasjonen, sjanger og forfattere og statusen på utlånet.</li>
            <li>MyAccessRolesCheck - view som henter ut brukerns roller slik at vi kan bruke det til å sjekke hvilken permissions en bruker har. </li>
        </ul>
    </details>
    <details>
        <summary>Procedures</summary>
        <ul>
            <li>CancelAddingBook - dette er prosedyren som kjøres når en kansellerer eller fjerner en bok, dersom du oppretter en bok og trykker ut av den dialogen blir denne kjørt for og kansellere den/ fjerne den. Denne fjerner bare alle tilknyttningene den boka har og sletter den. </li>
            <li>LoanBook - dette er prosedyren som kjøres når en tar uttak på en bok, den tar inn parameterene: Book_ID, Person_ID, Qty, FromDate, ToDate for å lagre ned når man lagrer den til og boka som blir tatt ut på hvilken person.</li>
            <li>ReturnBook - dette er prosedyren som kjøres når en leverer en bok tilbake, den tar inn parameterene: Book_ID, Person_ID, Qty for å lagre ned returdato og oppdatere availableqty på selve boka siden den blir levert tilbake.</li>
            <li>AddAuthors - dette er prosedyren som kjøres når en knytter flere forfattere til en bok, den tar inn parameterene: Book_ID, Author_ID for å lagre ned hvilken forfattere som er knyttet til denne boka.</li>
            <li>AddGenres - dette er prosedyren som kjøres når en knytter flere sjangre til en bok, den tar inn parameterene: Book_ID, Genre_ID for å lagre ned hvilken sjangre som er knyttet til denne boka.</li>
            <li>NotifyOverdueBookLoans - dette er prosedyren som kjøres når du skal minne på alle bøker som er overdue på innleveringsdatoen, denne tar bare og samler alle som har overdue book loans og sender en melding/mail til de som har overdue book loans med informasjon om hviket bøker de har lånt og at de må vennligst levere dem inn raskest mulig.</li>
            <li>BookDeliveryReminder - dette er en prosedyre som blir kjørt i en jobb som blir kjørt hver dag for å minne på at bøkene de har lånt når snart deres overdue date ( blir sendt når det er 2 dager igjen til den skal leveres tilbake ), denne tar bare og samler alle som snart har overdue book loans og sender en melding/mail til de og sendes med informasjon om hviket bøker de har lånt for å minne dem på det.</li>
        </ul>
    </details>
</details>
<details>
    <summary>
        Sikkerhet
    </summary>
      <p>Hele løsningen støtter seg på sikkerheten i Omega 365 CTP. Hoved konseptet i sikkerheten er moduler, capabilities, og roller. En modul en der man definerer tilgang på tabeller og apper. Roller kan da videre kobles opp til disse modulene, men en rolle er ikke begrenset til bare en modul, den kan ha flere forskjellige moduler på en rolle. Capabilities er "spesialtilganger" som blir gitt til noen individe roller for enda strengere sjekker. </p>
  <p>I Omega 365 CTP har vi flere måter å autentisere på, mest vanlig er SQL login eller Microsoft login. Med en SQL login som består av ett brukernavn og passord, kjøres ett sikkert API kall til SQL serveren for å autentisere brukeren. Med Microsoft login blir dette sendt til Microsoft og blir sjekket av dem. Om du blir autentisert av Microsoft returnerer de en token slik at Omega 365 vet at du er autentisert.</p>
  <p>Utover dette er to faktor autentisering (2FA) høyst anbefalt. Det er flere forskjellige tilgjengelige valg som: SMS, Email, Time-based One Time Password (TOTP), og Passkey.</p>
  <p>Sikkerheten i views er egentlig ganske enkel siden den sjekker bare om du lesetilgang på tabellen. For økt sikkerhet har brukere aldri direkte tilgang til tabeller. I stedet brukes views, ofte via en atbv som er automatisk generert for alle nye tabeller. "atbv" står for Application Table View og brukes for å kontrollere hva en bruker kan se.</p>
  <p>Sikkerheten i triggere er litt mer avansert, siden her må det sjekkes på om du har redigering/slette tilganger. </p>
  <br/>
  <details>
      <summary>Triggers</summary>
      <ul>
          <li>Standard Insert Permission Sjekk </li>
          <img width="829" height="333" alt="image" src="https://github.com/user-attachments/assets/f513aee4-595d-47b7-af62-f7b501e22f02" />
          <li>Standard Update Permission Sjekk </li>
          <img width="887" height="317" alt="image" src="https://github.com/user-attachments/assets/6303f0a1-3e38-430e-83cd-04f9069fd707" />
          <li>Standard Delete Permission Sjekk </li>
          <img width="869" height="260" alt="image" src="https://github.com/user-attachments/assets/708f5878-10d2-4311-a953-16bbc21d4dcd" />
      </ul>
  </details>
  <details>
      <summary>Roles</summary>
      <ul>
          <img width="1093" height="613" alt="image" src="https://github.com/user-attachments/assets/f53b8f63-48f4-4ee9-81e2-893b4a506fb1" />
          <li>Her er et bilde av hvordan strukturen på vår access modell i Omega 365, En rolle kan ha flere moduler og capabilities på seg, moduler er hvor du kan definere app tilganger og tabell permissions på mens capabilities blir brukt til og definere mer spesifike tilgangs sjekker. </li>
          <li>I Library appen min har jeg laget til 2 roller, en for vanlige brukerer som kun skal ha tilgang til og kunne se bøker og se sin oversikt over deres egne utlån så er det bare det de har tilgang til, den andre rollen er for administratorer/employees de har da full tilgang til og låne returnere legge til og slette bøker. </li>
      </ul>
  </details>
</details>
<details>
    <summary>
        Frontend
    </summary>
    <p>
        Denne løsningen består av 3 skjermbilder; Library, Genres og Authors
    </p>
    <ul>
        <!-- <img width="2557" height="1170" alt="image" src="https://github.com/user-attachments/assets/83f08dec-b66a-47c7-b862-d60b0debfbb7" /> -->
        <li>Library (Books Overview Tab) - Denne tabben viser deg et rask overblikk over alle bøkene som ligger inne på dette biblioteket hvor du får muligheten til å søke på bøker og åpne mer detaljer om boka, for ansatte så gjør de uttak via dette bilde på bøkene. </li>
        <!-- <img width="2546" height="1271" alt="image" src="https://github.com/user-attachments/assets/b7fc9a86-53ff-4fc0-b0a2-e8b8f2b4ab20" /> -->
        <li>Library (My Borrowed Books Tab) - Denne tabben viser oversikt over alle dine egne lånte bøker og historikk over bøker du har lånt, for ansatte så får de også mulighet til og levere inn bøker gjennom dette bildet som gjør det litt raskere for dem og holde styr på sine egne bøker de har lånt og kan leve inn kjapt.</li>
        <!-- <img width="2542" height="1270" alt="image" src="https://github.com/user-attachments/assets/94dad0c1-57b5-4fb0-9f7f-556ba67fceb0" /> -->
        <li>Library (Borrowed Books Overview Tab) - Denne tabben viser oversikt over alle bøkene som er utlånt generelt hvor de får muligheten til å sende en purre melding på de som er overdue men også levere inn bøker som er utlånt, de får også en kort oversikt på toppen over alle bøkene som er tilgjengelige/ utlånt ut/ og de som er overdue. Denne tabben er det bare ansatte som har tilgang til som gjør slik at vanlige brukere ikke kan redigere på dem.</li>
        <!-- <img width="2542" height="1270" alt="image" src="https://github.com/user-attachments/assets/94dad0c1-57b5-4fb0-9f7f-556ba67fceb0" /> -->
        <li>Library (Edit Books Tab) - Denne tabben viser deg oversikt over alle bøkene som ligger inne og gir deg muligheten til og redigere informasjonen til bøkene men også fjerne og legge til nye bøker. Denne tabben er det bare ansatte som har tilgang til som gjør slik at vanlige brukere ikke kan redigere på dem.</li>
        <img width="1269" height="1264" alt="image" src="https://github.com/user-attachments/assets/fec81a21-e0ae-4dcd-a7a0-c26841f6fa23" />
        <li>Authors - denne appen er en setup app som ligger i O365 Setup som vil da bare si en app for og se informasjon/ redigere/ legge til forfattere ( dette er noe som bare de me rollen Library Administrator skal kunne se/ redigere ). I denne appen ser du oversikt over hvilket forfattere som eksisterer.</li>
        <img width="864" height="1235" alt="image" src="https://github.com/user-attachments/assets/933c0635-da7b-4fd0-b47d-f0f9c30a0c3a" />
        <li>Genres -  denne appen er en setup app som ligger i O365 Setup som vil da bare si en app for og se informasjon/ redigere/ legge til sjangre ( dette er noe som bare de me rollen Library Administrator skal kunne se/ redigere ). I denne appen ser du oversikt over hvilket sjangre som eksisterer.</li>
        <br/>
        <p>Siden disse appen bruker Omega 365 CTP, håndteres mye av kommunikasjonen med SQL Server via et mellomlag ved hjelp av DataSources. Enkelt forklart er DataSources JSON-objekter som definerer et view. Disse sendes til .NET Core API-et for verifisering. Hvis verifiseringen godkjennes, sendes forespørselen videre til SQL Server. Ved å bruke dette konseptet håndterer API-et sikkerhetstiltak som beskyttelse mot SQL Injection, slik at dette ikke trenger å implementeres direkte i frontend.</p>
   </ul>
</details>
<!-- <details>
    <summary>
        Info Til Neste Utvikler
    </summary>
    <ul>
        <li>Koden er vell strukturert så det skal være lett og finne fram hvilken fil som hører til hvor og følger naming på variabler er utrolig viktig at du fortsetter med slik</li>
    </ul>
</details> -->
<details>
    <summary>
        Problemstillinger under utvikling
    </summary>
    <ol>
        <!-- <li>Monthly Balance Overview Chart - Charten var litt av en challenge siden jeg ikke har vært borti det så mye, så og formattere datoer og verdiene som skulle bli sendt in var en ting jeg brukte en del tid på</li>
        <li>Overview Layout - Dette var noe jeg slet litt med og endret et par ganger med tanke på hva som hadde blitt mest intuitivt for bruker så det ble et par endringer her og endring på formattering og hadde en del problemer med reaktivitet når det kom til charten siden den ikke ville endre seg basert på screen width, til slutt fikk jeg det til men det tok litt tid på og debugge dette og landet til slutt på en layout som jeg syntes var mest oversiktlig for bruker.  </li> -->
    </ol>
</details>
<details>
    <summary>
        Planen videre
    </summary>
    <ul>
        <p>Her er tingene jeg hadde planlagt videre, men som jeg ikke fikk tid til:</p>
        <!-- <li>Muligheten for og legge til flere kategorier på et budsjett ( per nå så er det bare mulig og legge til en kategori på et budsjett ) </li>
        <li>Fjerning av transaksjoner ( dette er egentlig relativt lett men var usikker på om jeg ville la bruker få muligheten til og slette transaksjoner men heller få dem til og legge inn en linje som en korrigering av feil transaksjoner ) </li>
        <li>Legging inn av transaksjoner bak i tid ( dette ville krevd omskriving i flere views og direkte i app og procedure og endring av flere tabeller men er en drit fin funksjon siden per nå så baserer den seg på Created feltet som er da transaksjonen blir lagt inn ( opprettet ) men vil da si at det ikke er mulighet for å legge inn en bak i tid eller fram i tid ) dette tenkte jeg egentlig på og endre siste dagen men dersom det ble for kort tid så legger jeg det heller til i lista som en ToDo </li>
        <li>Mulighet til og legge inn flere Accounts typ Sparekonto, Brukskonto BSU konto. Det dette ville innebært er og lagd en ny tab for administrering av Kontoer ( dette er noe som tabellstrukturen min støtter men har ikke fått tid til og implementere )</li>
        <li>Mulighet til og endre valuta og basert på valutaen skal exchangeraten endre prisen ( dette er noe datamodellen min støtter per nå men har ikke lagt mulighet for convertering men skal ikke være så altfor vanskelig og implementere ) </li> -->
    </ul>
</details>
<details>
    <summary>
        Avvik fra plan
    </summary>
    <ul>
        <p>Her er avvikene jeg har hatt fra den originale planen med begrunnelse:</p>
        <li>Det ble et par avvik i planen når det kom til layout på appen dersom jeg fant underveis ut at jeg ville endre fra grid display til kort dersom det så bedre ut og lettere for brukere og bruke så det ble brukt mer tid på å endre litt frem og tilbake på layouten av de kortene.</li>
        <li>Jeg endret også litt på tab navna og hva de skulle inneholde som f.eks dashboardet endret jeg bare til borrowed books overview der de får oversikt over alle utlån men funksjonalitet i bildet som å levere tilbake og purre på innleveringer.</li>
        <li>Jeg la også til en tab for redigerings mulighet på bøkene som ligger inne hvor de får muligheten til å slette/ legge til og endre de bøkene som ligger inne.</li>
        <li>Jeg la også merke til at tidsestimate ble litt stramt når disse tilpasningene ble opprettet som gjorde at jeg ikke var helt i mål når jeg var planlagt til å være i mål men det motiverte bare mer til å få ting på plass.</li>
        </ul>
</details>
