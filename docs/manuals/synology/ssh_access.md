---
date: 2023-08-16
---

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

4. Skift brugerens hjemmemappens adgangsniveau

```powershell
chmod 755 ~
```

5. Log ud