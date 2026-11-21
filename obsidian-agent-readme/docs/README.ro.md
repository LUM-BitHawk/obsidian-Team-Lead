# Obsidian-Agent – Ghid

Versiunea 2.0.0 · [Deutsch](README.de.md) · [English](README.en.md) · [Français](README.fr.md) · [Italiano](README.it.md) · [Español](README.es.md)

Obsidian-Agent este o interfață locală pentru managementul proiectelor cu Obsidian. Se deschide în browser la `http://127.0.0.1:5055`, dar doar pe calculatorul dvs.; nicio dată nu este trimisă pe internet.

**Principiu:** fișierele Markdown din seiful Obsidian sunt singura sursă de adevăr. Agentul le citește, afișează clar sarcinile, proiectele și ședințele și creează notele noi în formatul corect. Baza de date `planer.db` este doar un index și se reconstruiește la fiecare sincronizare, așa că puteți lucra oricând direct în Obsidian.

## Instalare

### Windows

1. Faceți dublu clic pe **`Obsidian-Agent-Setup.exe`**.
   Windows poate afișa „Windows a protejat computerul”, deoarece programul nu este semnat: **Mai multe informații → Rulează oricum**.
2. Alegeți limba și confirmați cu Enter.
3. Programul de instalare copiază aplicația în `%LOCALAPPDATA%\Programs\Obsidian-Agent` și o configurează:
   - Dacă lipsește Python 3.10 sau mai nou, este instalat pentru contul dvs. cu `winget` (altfel se deschide python.org).
   - O fereastră de dialog cere dosarul seifului.
   - De acum interfața web pornește în fundal la fiecare autentificare; sincronizarea zilnică rulează la 04:00.
   - **Obsidian-Agent** apare în meniul Start, iar o intrare de dezinstalare în *Setări → Aplicații*.

Nu sunt necesare drepturi de administrator. Rularea din nou a programului de instalare actualizează aplicația; `config.json` și `planer.db` se păstrează.

### macOS

1. Faceți dublu clic pe **`Obsidian-Agent.pkg`**.
   Pachetul nu este notarizat de Apple. Dacă macOS spune că „nu poate fi deschis”: **clic dreapta → Deschide → Deschide** (de la macOS 15: *Configurări sistem → Confidențialitate și securitate → Deschide oricum*).
2. Pachetul se instalează în `~/Applications/Obsidian-Agent` (doar pentru dvs., fără parolă de administrator).
3. Apoi se deschide o fereastră Terminal: alegeți limba, seiful și pornirea la autentificare da/nu. Sincronizarea zilnică este configurată cu launchd și recuperată când Mac-ul se trezește.

**Cerință:** Python 3.9 sau mai nou, de ex. de pe python.org sau cu `brew install python`.

Dacă seiful se află în *Documente*, pe *Desktop* sau în *iCloud Drive*, macOS poate bloca sincronizarea în fundal. Configurarea detectează acest lucru și arată ce fișier trebuie adăugat la *Configurări sistem → Confidențialitate și securitate → Acces complet la disc*.

Pornire: dublu clic pe `~/Applications/Obsidian-Agent/Obsidian-Agent.command` sau deschideți semnul de carte `http://127.0.0.1:5055`.

### Linux

```bash
./start.sh                       # creează mediul Python și pornește
.venv/bin/python agent.py init --sprache ro
```
Sincronizare zilnică cu `crontab -e`: `0 4 * * * cd /cale/obsidian-agent && .venv/bin/python agent.py sync --geplant`

## Secțiuni

| Secțiune | La ce folosește |
|---|---|
| **Astăzi** | Sarcini scadente azi și întârziate, zilele următoare, următoarele ședințe periodice. Creați direct o sarcină. |
| **Sarcini** | Toate sarcinile pe Astăzi / Întârziate / Planificate / Fără dată / Finalizate, filtru pe proiect. Bifați, schimbați starea, „+1 zi”. |
| **Proiecte** | Proiecte cu client, manager de proiect, termen limită și număr de sarcini deschise. Proiect nou cu structură fixă de dosare. |
| **Procese-verbale** | Proces-verbal, decizie și cerere de modificare din șabloane; cu un proiect ajung în dosarul potrivit. |
| **Ședințe periodice** | Ședințe recurente (de ex. echipa de proiect, comitetul de coordonare) cu frecvență și puncte deschise. Fiecare notă nouă de ședință conține punctele deschise ale seriei. |
| **Echipă** | Fișe cu rol, contact și sarcini deschise. Atribuire prin câmpul `verantwortlich` (nume sau inițiale). |
| **Căutare** | Căutare în textul complet al seifului. |
| **Setări** | Limbă, calea seifului, numele dosarelor, ora sincronizării zilnice. |

Fiecare titlu este un link `obsidian://` care deschide nota direct în Obsidian.

## Dosare în seif

La prima configurare, dosarele sunt create în limba dvs.:

| Scop | Dosar |
|---|---|
| Proiecte | `Proiecte/` |
| Sarcini fără proiect | `Sarcini/` |
| Procese-verbale fără proiect | `Procese-verbale/` |
| Membrii echipei | `Echipa/` |
| Ședințe periodice | `Sedinte/<serie>/` |
| Șabloane | `_Sabloane/` |
| Prezentarea zilei | `Agenda/Azi.md` |

Fiecare proiect primește aceste subdosare (fiecare cu un fișier readme) și o notă de prezentare `_Proiect_<nume>.md` cu client, manager de proiect și termen limită:

- `01_Mandat_si_contracte/` – Mandatul proiectului, ofertă, contracte, comenzi și acte adiționale.
- `02_Planificare/` – Plan de proiect, jaloane, resurse, buget și riscuri.
- `03_Contacte/` – Persoane de contact, roluri, părți interesate și căi de escaladare.
- `04_Procese-verbale/` – Procese-verbale de ședință, ateliere și corespondență importantă.
- `05_Decizii/` – Decizii și cereri de modificare.
- `06_Rapoarte/` – Rapoarte de stare, documente pentru comitetul de coordonare și raport final.
- `07_Documente/` – Documentație de specialitate, concepte și livrabile.

Dacă schimbați limba mai târziu, dosarele și notele existente **nu** sunt redenumite. Puteți ajusta oricând numele dosarelor în *Setări*.

## Formatul sarcinilor

```markdown
---
typ: task
titel: Pregătește testul de acceptanță
projekt: Implementare ERP
start: 2026-11-18
ende: 2026-11-20
status: offen
prioritaet: hoch
verantwortlich: AB
---
- [ ] Stabilirea cazurilor de test
- [x] Comandarea mediului de test
```

- **Numele câmpurilor și valorile sunt aceleași în toate limbile** (`status: offen | in_arbeit | wartend | erledigt`, `prioritaet: hoch | mittel | niedrig`). Interfața le afișează traduse. Astfel, un seif rămâne lizibil chiar dacă îl folosesc persoane cu limbi diferite.
- Astăzi = `start ≤ azi ≤ ende`, întârziată = `ende < azi`, planificată = `start > azi`.
- `projekt` este numele dosarului proiectului ca text simplu, `verantwortlich` un nume sau inițiale din echipă (mai multe separate prin virgulă).
- Agentul nu recunoaște sarcinile în stilul pluginului Tasks (`- [ ] … #task 📅 …`). Tot ce trebuie urmărit devine un fișier de sarcină separat.

## Prezentarea zilei

În fiecare dimineață la 04:00 (sau la următoarea pornire dacă calculatorul era oprit), agentul citește seiful, salvează un instantaneu zilnic în baza de date și scrie `Agenda/Azi.md` plus o arhivă. Astfel vedeți prezentarea și în Obsidian pe telefon.

## Adaptarea șabloanelor

Șabloanele se află în `_Sabloane/` și pot fi editate în Obsidian; agentul folosește apoi versiunea dvs. Șabloanele șterse sunt recreate la următoarea sincronizare. Substituenți precum `{{title}}` și `{{date}}` funcționează și cu pluginul de bază „Șabloane” din Obsidian.

## Dezinstalare, actualizare, mutare

- **Windows:** *Setări → Aplicații → Obsidian-Agent → Dezinstalare* sau `deinstallieren.bat` din dosarul programului.
- **macOS:** `~/Applications/Obsidian-Agent/mac/deinstallieren_mac.command`.

Dezinstalarea elimină pornirea automată și sincronizarea zilnică, salvează `config.json` și `planer.db` într-un dosar `obsidian-agent-backup_<dată>` pe desktop și, dacă doriți, șterge dosarul programului. **Seiful nu este niciodată modificat.**

Actualizare: instalați pur și simplu noul program de instalare sau pachet. Mutare pe alt calculator: instalați-l acolo și copiați `config.json` și `planer.db` din copia de rezervă în dosarul programului (baza de date este opțională; fără ea lipsește doar istoricul zilnic).

## Protecția datelor

Seiful conține date personale și informații confidențiale despre proiecte. Agentul rulează doar local (`127.0.0.1`) și nu trimite nimic în exterior. Configurați o copie de rezervă a seifului pe o unitate securizată a firmei și folosiți doar spații de stocare în cloud aprobate.

## Linia de comandă

```
python agent.py sync               # citește seiful acum
python agent.py heute              # sarcinile de azi și cele întârziate
python agent.py projekt "Nume"     # creează un proiect
python agent.py init --sprache ro  # setează limba (de, en, fr, it, es, ro)
```
