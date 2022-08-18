# Renamer
```renamer``` er et commandline værktøj til omdøbning af filer. Oftest brugt i forbindelse med konverteringsprocesser, hvor filer med manglende eller fejlagtig filendelse ofte vil skabe problemer i den senere konverteringsproces.

Ved warningen "Extension mismatch" i DB Browser identificeres, hvilken filendelse filen burde have.

Dernæst anvendes følgende i f.eks. PowerShell til at ændre filendelsen:

```renamer db_file_path puid new_extension_without_period_sign```

# Eksempel
I nedenstående eksempel giver renamer alle puid fmt/340 filendelsen lwp

!!! hint "Eksempel"
    ```powershell
    renamer D:\AVID.AARS.77.1\OriginalFiles\_metadata\files.db fmt/340 lwp
