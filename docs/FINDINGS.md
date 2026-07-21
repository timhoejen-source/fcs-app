# FCS Platform – Findings Register

Dette dokument indeholder tekniske fund, observationer, risici og forbedringsforslag for FCS-platformen.

Formålet er at sikre, at tekniske problemer bliver dokumenteret, undersøgt, prioriteret og afsluttet på en kontrolleret måde.

## Statusser

- 🔍 Under undersøgelse
- ⚠️ Verificeret
- 🛠 Planlagt
- 🚧 I gang
- 🧪 Klar til test
- ✅ Løst
- ❌ Afvist
- 🟦 Accepteret risiko

## Prioriteter

- Kritisk
- Høj
- Normal
- Lav

## Arbejdsgang

1. Et nyt fund oprettes som **Under undersøgelse**.
2. Fundet undersøges og dokumenteres med konkrete beviser.
3. Hvis fundet bekræftes, ændres status til **Verificeret**.
4. Når en løsning er besluttet, ændres status til **Planlagt**.
5. Under implementering bruges status **I gang**.
6. Efter implementering bruges status **Klar til test**.
7. Fundet markeres først som **Løst**, når løsningen er verificeret.

---

## FCS-001 – Dokumentisolering mellem klubber

**Status: ⚠️ Verificeret
**Prioritet: Høj  
**Område:** Backend / Dokumenter  
**Fundet:** 20. juli 2026  
**Ansvarlig:** ChatGPT / teknisk review

### Observation

Dokumentmodulet ser umiddelbart ud til at benytte en fælles dokumentmappe og en fælles metadatafil.

Det skal verificeres, om dokumenter reelt er isoleret pr. klub, eller om en bruger fra én klub potentielt kan se, hente, ændre eller slette dokumenter fra en anden klub.

### Skal verificeres

- Hvordan `docs.php` identificerer brugerens klub.
- Om klub-id indgår i dokumenternes filsti.
- Om metadata filtreres eller opdeles pr. klub.
- Om upload, hentning, visning og sletning håndhæver klubadgang.
- Om dokumentlinks kan bruges uden korrekt autentificering.
- Om eksisterende dokumenter allerede er opdelt i undermapper.

### Bevis

Afventer detaljeret gennemgang af dokumentmodulet.

### Anbefaling

Afventer verificering.

---

## FCS-002 – Manglende versionsstyrede databasemigrationer

**Status:** ⚠️ Verificeret  
**Prioritet:** Høj  
**Område:** Database  
**Fundet:** 20. juli 2026  
**Ansvarlig:** Ikke tildelt

### Observation

Klubbernes SQLite-databaser har ikke helt identiske databaseskemaer.

Der er blandt andet forskelle i kolonner og tabeller mellem enkelte klubdatabaser.

### Risiko

En funktion kan virke for én klub og fejle for en anden, hvis koden forventer et nyere databaseskema.

Manuelle eller automatiske databaseændringer kan desuden blive udført forskelligt på tværs af klubber.

### Anbefaling

Etabler et enkelt versionsstyret migrationssystem med:

- Et `schema_version`-felt pr. klubdatabase.
- Nummererede migrationer.
- Automatisk eller kontrolleret opgradering.
- Validering af alle klubdatabaser ved deployment.
- Backup før migration.

---

## FCS-003 – API-tokens har ingen tydelig udløbsdato

**Status:** ⚠️ Verificeret  
**Prioritet:** Normal  
**Område:** Sikkerhed / Autentificering  
**Fundet:** 20. juli 2026  
**Ansvarlig:** Ikke tildelt

### Observation

iOS-klientens API-tokens har et oprettelsestidspunkt, men der er ikke identificeret en maksimal gyldighedsperiode i tokenkontrollen.

### Risiko

Et kopieret eller kompromitteret token kan potentielt anvendes i lang tid.

### Anbefaling

Overvej at indføre:

- Udløbstidspunkt for tokens.
- Tilbagekaldelse ved logout.
- Mulighed for at se og lukke aktive sessioner.
- Eventuel rotation eller fornyelse af tokens.

---

## FCS-004 – Logout ser ikke ud til at tilbagekalde app-token

**Status:** 🔍 Under undersøgelse  
**Prioritet:** Normal  
**Område:** Sikkerhed / Autentificering  
**Fundet:** 20. juli 2026  
**Ansvarlig:** Ikke tildelt

### Observation

Logout-flowet ser ud til at afslutte PHP-sessionen, men det er endnu ikke verificeret, om iOS-tokenet samtidig slettes eller deaktiveres.

### Skal verificeres

- Om klienten sender tokenet ved logout.
- Om tokenet slettes i databasen.
- Om tokenet fortsat virker efter logout.

### Anbefaling

Afventer verificering.

---

## FCS-005 – Test- og administrationsfiler i serverpakken

**Status:** 🔍 Under undersøgelse  
**Prioritet:** Høj  
**Område:** Sikkerhed / Deployment  
**Fundet:** 20. juli 2026  
**Ansvarlig:** Ikke tildelt

### Observation

Serverpakken indeholder filer med navne som:

- `auth_test.php`
- `login_test.php`
- `test_login.php`
- `load_debug.php`
- `save_debug.php`
- `phpliteadmin.php`
- `dbadmin.php`
- `super_admin.php`

### Risiko

Hvis filerne er tilgængelige på produktionsserveren uden tilstrækkelig beskyttelse, kan de udvide angrebsfladen eller give adgang til følsomme funktioner.

### Skal verificeres

- Om filerne er deployet i produktion.
- Om de kan tilgås fra internettet.
- Om de kræver autentificering.
- Om de stadig bruges.

### Anbefaling

Fjern ubrugte filer fra produktion, eller beskyt dem med stærk autentificering og netværksbegrænsning.

---

## FCS-006 – Produktionsdata og backups kan være blandet med kildekoden

**Status:** 🔍 Under undersøgelse  
**Prioritet:** Høj  
**Område:** Repository / Databeskyttelse  
**Fundet:** 20. juli 2026  
**Ansvarlig:** Ikke tildelt

### Observation

Den gennemgåede serverpakke indeholder klubdatabaser, backups, brugerdata og aktivitetsdata sammen med kildekoden.

Det er endnu ikke verificeret, om disse filer faktisk er committed til GitHub, eller kun indgik i den ZIP-fil, der blev delt til review.

### Risiko

Hvis produktionsdata ligger i Git-historikken, kan følsomme oplysninger være tilgængelige for alle med adgang til repositoriet, også efter at filerne slettes i en senere commit.

### Skal verificeres

- Om databasefiler findes i GitHub.
- Om backupfiler findes i Git-historikken.
- Om hemmeligheder eller credentials er committed.
- Om roden har en korrekt `.gitignore`.

### Anbefaling

Hold kode, produktionsdata, backups og hemmeligheder strengt adskilt.

---

## FCS-007 – Manglende maintenance- og statusmekanisme

**Status:** 🔍 Under undersøgelse  
**Prioritet:** Normal  
**Område:** Drift / Web / iOS  
**Fundet:** 21. juli 2026  
**Ansvarlig:** Ikke tildelt

### Observation

Platformen har ikke en fælles mekanisme til at vise en kontrolleret statusside i webappen og iOS-appen under planlagte opdateringer, patches eller midlertidig utilgængelighed af backenden.

### Risiko

Brugere kan opleve generelle fejl, tomme sider eller loginproblemer uden forklaring, når backenden er utilgængelig.

### Ønsket løsning

- En ekstern statusfil eller statustjeneste, som ikke afhænger af den normale backend.
- En vedligeholdelsesside til webappen.
- En indbygget fallback-skærm i iOS-appen.
- Understøttelse af både planlagt vedligeholdelse og uventet nedbrud.
- Mulighed for at vise titel, besked og forventet sluttidspunkt.

### Næste skridt

- Fastlæg hvor statusfilen skal hostes.
- Gennemgå webappens opstartsflow.
- Gennemgå iOS-appens API- og fejlhåndtering.
- Beskriv implementeringen til Claude.

### Anbefaling

Ingen kodeændringer før den samlede løsning er designet og gennemgået.

## Skabelon til nye fund

Kopiér sektionen nedenfor, når et nyt fund oprettes.

```markdown
## FCS-XXX – Kort titel

**Status:** 🔍 Under undersøgelse  
**Prioritet:** Normal  
**Område:**  
**Fundet:** ÅÅÅÅ-MM-DD  
**Ansvarlig:** Ikke tildelt

### Observation

Beskriv, hvad der er observeret.

### Risiko

Beskriv den mulige konsekvens.

### Bevis

Indsæt relevante filnavne, funktioner, kodehenvisninger eller testresultater.

### Anbefaling

Beskriv den anbefalede handling.

### Løsning

Udfyldes, når løsningen er implementeret.

**Version:**  
**Commit eller pull request:**  
**Løst dato:**  
```
