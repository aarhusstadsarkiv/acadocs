---
date: 2023-08-16
---

# Opret ny bruger

1. Åbn http://nas.home.arpa:5000 i browseren.
2. Login som administrator (spørge efter adgangskode)
3. Åbn "Kontrolpanel"<br/>
![](new_user%20(1).png)
4. Åbn "Bruger og gruppe"<br/>
![](new_user%20(2).png)
5. Klik på "Opret" > "Opret bruger"<br/>
![](new_user%20(3).png)
6. Indtast oplysningerne<br/>
![](new_user%20(4).png)
     * Navn: azID
     * Beskrivelse: fuldt navn
     * E-mail: aarhus.dk mail
     * Adgangskode: password
7. Tilføj gruppe "administrators" og "stadsarkivet"<br/>
![](new_user%20(5).png)
8. Tilføj "Læse/skriv" adgang til stadsarkivet<br/>
![](new_user%20(6).png)
9. Spring over "Brugerkvote" side
10. Tilføj alle tilladelser<br/>
![](new_user%20(7).png)
11. Spring over brugerhastighedsgrænse
12. Bekræft oplysningerne og klik på "Udført"

# SSH-adgang

1. Åbn terminalen
2. Kopi personlig SSH-nøgle til NAS (brug brugeren azID)

```powershell
ssh-copy-id az00000@nas.home.arpa
```

3. Login med SSH (brug brugeren azID)


```powershell
ssh az00000@nas.home.arpa
```

4. Skift burgerens hjemmemappens adgangsniveau

```powershell
chmod 755 ~
```

5. Log ud
