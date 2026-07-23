Dokumentmodul

Frontend:
    index.html

Backend:
    docs.php

Endpoints

GET    docs.php
POST   docs.php?upload=1
POST   docs.php?delete=1&id=...

Adgang

Admin
Trainer

Dokumenter lagres pr. klub.

Metadata:
    docs.json

Filer:
    /clubs/{clubId}/documents/

Funktioner:

- Upload af dokumenter
- Download/visning
- Sletning
- Dokumenter vises kun for den klub den loggede bruger tilhører
- Login kræves

SENEST RETTELSER TIL MODULET:

- Dokumentupload fungerer igen.
- Dokumenthentning fungerer igen.
- Dokumentsletning fungerer igen.
- Dokumenter er nu isoleret pr. klub.
- Frontend synkroniseret mellem NAS og iOS.
- Fjernet dubleret JavaScript i index.html.
- Rettet JavaScript-syntaksfejl.
