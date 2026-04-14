# Test Rapport

## Plan:
Utføre grundig test av alle funksjonene i appene og beskrive hvordan de burde oppføre seg, og om de ikke oppfører seg rett, dokumenter hva som ikke fungerer og eventuelle fikser.

### Library
<table>
    <th>Funksjon</th>
    <th>Forventet oppførsel</th>
    <th>Resultat</th>
    <th>Bilde</th>
    <th>Eventuelle fikser</th>
    <th>Status</th>
    <tr>
        <td>Opprette Bøker</td>
        <td>Opprette Bok</td>
        <td>Opprettet bok</td>
        <td><img width="2542" height="1227" alt="image" src="https://github.com/user-attachments/assets/2c427fb7-fdc3-43ce-a0bc-34bed8292d6c" /></td>
        <td>Oppdaterte ikke Availability slik den skulle så fikset slik at den oppdaterte den og ble satt til samme qty som total books, ser også at lookup ikke clearer seg når lookup blir åpnet på ny så må fikse det slik at den clearer seg når den                 lukkes, måtte også skru opp nvarchar verdien opp dersom jeg hadde en litt for lang tittel så kappet den seg ned så fikset det i samme slengen.
        </td>
        <td>✅</td>
    </tr>
    <tr>
        <td>Fjerne Bøker</td>
        <td>Fjerne Bøker</td>
        <td>Fjerne Bok</td>
        <td><img width="2527" height="785" alt="image" src="https://github.com/user-attachments/assets/b9259a88-a5e8-42e9-ab81-8c871ff62f13" /></td>
        <td>Fjernet Bok som forventet.</td>
        <td>✅</td>
    </tr>
    <tr>
        <td>Redigere Bøker</td>
        <td>Redigere Bøker</td>
        <td>Redigere Bøker</td>
        <td><img width="1228" height="726" alt="image" src="https://github.com/user-attachments/assets/d3f00ae4-5ba2-40cd-9397-4ecd5a11bfe6" /></td>
        <td>Det funker å redigere boken men noe som kan være misleading er at når du setter genres og authors blir det lagret med en gang uten å måtte trykke save siden det er en funksjon som blir kjørt på binden som er noe jeg kan tenke på i                     fremtiden og fikse når jeg får tid</td>
        <td>✅</td>
    </tr>
    <tr>
        <td>Låne bok</td>
        <td>Låne bok</td>
        <td>Låne bok</td>
        <td><img width="2535" height="640" alt="image" src="https://github.com/user-attachments/assets/863cc91f-d534-479d-9f35-d5d38abb8481" /> <img width="772" height="148" alt="image" src="https://github.com/user-attachments/assets/7d3de0d9-f5a6-477a-9b04-a57e00a72188" />
</td>
        <td>Funket å låne bok, oppdaterte available Qty også. Bestemte meg for å disable Total Books kolonna siden jeg då at den var redigerbar og får heller bare tenke på dette i en senere tid om vi skal legge til funksjonalitet for å legge til flere av samme bok</td>
        <td>✅</td>
    </tr>
    <tr>
        <td>Levere inn bok</td>
        <td>Levere inn bok</td>
        <td>Levere inn bok </td>
        <td><img width="2453" height="473" alt="image" src="https://github.com/user-attachments/assets/7f175092-61ce-435e-bb16-fd6d0cdcfac7" /></td>
        <td>Funket som forventet</td>
        <td>✅</td>
    </tr>
    <tr>
        <td>Purre på lånte bøker </td>
        <td>Purre på lånte bøker </td>
        <td>Purre på lånte bøker</td>
        <td><img width="2465" height="738" alt="image" src="https://github.com/user-attachments/assets/6299d98e-fd6e-45a4-83cc-489e56eaa4b1" /> <img width="510" height="1022" alt="image" src="https://github.com/user-attachments/assets/9c7a14b8-e86d-4d97-acc3-2a20bae3f999" />
</td>
        <td>Funket som forventet men ser at statusen på den ene boken var feil så måtte fikse det</td>
        <td>✅</td>
    </tr>
    <tr>
        <td>Legge til Sjangre og forfattere i setup app </td>
        <td>Legge til Sjangre og forfattere i setup app </td>
        <td>Legge til Sjangre og forfattere i setup app </td>
        <td><img width="1033" height="1258" alt="image" src="https://github.com/user-attachments/assets/0604227e-7529-4f7a-b523-fe257a77c31d" /><img width="737" height="1245" alt="image" src="https://github.com/user-attachments/assets/3a847d24-ed02-4f16-a61f-4b1df8db6ee6" />

</td>
        <td>Funket som forventet. </td>
        <td>✅</td>
    </tr>
    <tr>
        <td>Jobb Notifikasjon</td>
        <td>Jobb Notifikasjon</td>
        <td>Jobb Notifikasjon</td>
        <td><img width="804" height="1018" alt="image" src="https://github.com/user-attachments/assets/4468e0de-813a-45a2-9b0e-3aff0efef83f" />
</td>
        <td>Funket som forventet</td>
        <td>✅</td>
    </tr>
</table>
