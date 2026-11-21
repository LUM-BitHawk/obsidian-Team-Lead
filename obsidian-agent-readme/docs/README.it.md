# Obsidian-Agent – Guida

Versione 2.0.0 · [Deutsch](README.de.md) · [English](README.en.md) · [Français](README.fr.md) · [Español](README.es.md) · [Română](README.ro.md)

L’Obsidian-Agent è un’interfaccia locale per la gestione dei progetti con Obsidian. Si apre nel browser all’indirizzo `http://127.0.0.1:5055`, ma solo sul tuo computer; nessun dato viene inviato in Internet.

**Principio:** i file Markdown del tuo vault Obsidian sono l’unica fonte di verità. L’agente li legge, mostra attività, progetti e riunioni in modo chiaro e crea le nuove note nel formato corretto. Il database `planer.db` è solo un indice e viene ricostruito a ogni sincronizzazione; puoi quindi continuare a lavorare direttamente in Obsidian in qualsiasi momento.

## Installazione

### Windows

1. Fai doppio clic su **`Obsidian-Agent-Setup.exe`**.
   Windows può mostrare «PC protetto da Windows» perché il programma non è firmato: **Ulteriori informazioni → Esegui comunque**.
2. Scegli la lingua e conferma con Invio.
3. Il setup copia il programma in `%LOCALAPPDATA%\Programs\Obsidian-Agent` e lo configura:
   - Se manca Python 3.10 o successivo, viene installato per il tuo account con `winget` (altrimenti si apre python.org).
   - Una finestra di dialogo chiede la cartella del vault.
   - D’ora in poi l’interfaccia web si avvia in background a ogni accesso; la sincronizzazione giornaliera avviene alle 04:00.
   - **Obsidian-Agent** compare nel menu Start e una voce di disinstallazione in *Impostazioni → App*.

Non servono diritti di amministratore. Eseguire di nuovo il setup aggiorna il programma; `config.json` e `planer.db` vengono mantenuti.

### macOS

1. Fai doppio clic su **`Obsidian-Agent.pkg`**.
   Il pacchetto non è autenticato da Apple. Se macOS dice che «non può essere aperto»: **clic destro → Apri → Apri** (da macOS 15: *Impostazioni di Sistema → Privacy e sicurezza → Apri comunque*).
2. Il pacchetto si installa in `~/Applications/Obsidian-Agent` (solo per te, senza password di amministratore).
3. Poi si apre una finestra del Terminale: scegli la lingua, il vault e se avviare all’accesso. La sincronizzazione giornaliera viene configurata con launchd e recuperata al risveglio del Mac.

**Requisito:** Python 3.9 o successivo, ad es. da python.org o con `brew install python`.

Se il vault si trova in *Documenti*, sulla *Scrivania* o in *iCloud Drive*, macOS può bloccare la sincronizzazione in background. La configurazione lo rileva e indica quale file aggiungere in *Impostazioni di Sistema → Privacy e sicurezza → Accesso completo al disco*.

Avvio: doppio clic su `~/Applications/Obsidian-Agent/Obsidian-Agent.command` oppure apri il segnalibro `http://127.0.0.1:5055`.

### Linux

```bash
./start.sh                       # crea l’ambiente Python e avvia
.venv/bin/python agent.py init --sprache it
```
Sincronizzazione giornaliera con `crontab -e`: `0 4 * * * cd /percorso/obsidian-agent && .venv/bin/python agent.py sync --geplant`

## Sezioni

| Sezione | A cosa serve |
|---|---|
| **Oggi** | Attività in scadenza oggi e in ritardo, i prossimi giorni, le prossime riunioni periodiche. Crea un’attività direttamente. |
| **Attività** | Tutte le attività per Oggi / In ritardo / Pianificate / Senza data / Completate, filtro per progetto. Spunta, cambia stato, «+1 giorno». |
| **Progetti** | Progetti con cliente, capoprogetto, scadenza e numero di attività aperte. Nuovo progetto con struttura di cartelle fissa. |
| **Verbali** | Verbale, decisione e richiesta di modifica da modelli; con un progetto finiscono nella cartella giusta. |
| **Riunioni periodiche** | Riunioni ricorrenti (ad es. team di progetto, comitato guida) con frequenza e punti aperti. Ogni nuova nota di riunione riporta i punti aperti della serie. |
| **Team** | Schede con ruolo, contatti e attività aperte. Assegnazione tramite il campo `verantwortlich` (nome o sigla). |
| **Ricerca** | Ricerca full-text in tutto il vault. |
| **Impostazioni** | Lingua, percorso del vault, nomi delle cartelle, orario della sincronizzazione giornaliera. |

Ogni titolo è un link `obsidian://` e apre la nota direttamente in Obsidian.

## Cartelle nel vault

Alla prima configurazione le cartelle vengono create nella tua lingua:

| Scopo | Cartella |
|---|---|
| Progetti | `Progetti/` |
| Attività senza progetto | `Attivita/` |
| Verbali senza progetto | `Verbali/` |
| Membri del team | `Team/` |
| Riunioni periodiche | `Riunioni/<serie>/` |
| Modelli | `_Modelli/` |
| Panoramica del giorno | `Agenda/Oggi.md` |

Ogni progetto riceve queste sottocartelle (ognuna con un file readme) e una nota di riepilogo `_Progetto_<nome>.md` con cliente, capoprogetto e scadenza:

- `01_Mandato_e_contratti/` – Mandato di progetto, offerta, contratti, ordini e integrazioni.
- `02_Pianificazione/` – Piano di progetto, milestone, risorse, budget e rischi.
- `03_Contatti/` – Referenti, ruoli, stakeholder e percorsi di escalation.
- `04_Verbali/` – Verbali di riunione, workshop e corrispondenza importante.
- `05_Decisioni/` – Decisioni e richieste di modifica.
- `06_Rapporti/` – Rapporti di stato, documenti del comitato guida e rapporto finale.
- `07_Documenti/` – Documentazione tecnica, concetti e risultati.

Se in seguito cambi lingua, le cartelle e le note esistenti **non** vengono rinominate. Puoi modificare i nomi delle cartelle in qualsiasi momento nelle *Impostazioni*.

## Formato delle attività

```markdown
---
typ: task
titel: Preparare il collaudo
projekt: Introduzione ERP
start: 2026-11-18
ende: 2026-11-20
status: offen
prioritaet: hoch
verantwortlich: AB
---
- [ ] Concordare i casi di test
- [x] Ordinare l’ambiente di test
```

- **Nomi dei campi e valori sono uguali in tutte le lingue** (`status: offen | in_arbeit | wartend | erledigt`, `prioritaet: hoch | mittel | niedrig`). L’interfaccia li mostra tradotti. Così un vault resta leggibile anche se lo usano persone con lingue diverse.
- Oggi = `start ≤ oggi ≤ ende`, in ritardo = `ende < oggi`, pianificata = `start > oggi`.
- `projekt` è il nome della cartella del progetto come testo semplice, `verantwortlich` un nome o una sigla del team (più valori separati da virgola).
- L’agente non riconosce le attività nello stile del plugin Tasks (`- [ ] … #task 📅 …`). Tutto ciò che va seguito diventa un file attività a sé.

## Panoramica del giorno

Ogni mattina alle 04:00 (o al prossimo avvio se il computer era spento) l’agente legge il vault, salva un’istantanea giornaliera nel database e scrive `Agenda/Oggi.md` più un archivio. Così vedi la panoramica anche in Obsidian sul telefono.

## Personalizzare i modelli

I modelli si trovano in `_Modelli/` e si modificano in Obsidian; l’agente usa poi la tua versione. I modelli eliminati vengono ricreati alla sincronizzazione successiva. Segnaposto come `{{title}}` e `{{date}}` funzionano anche con il plugin di base «Modelli» di Obsidian.

## Disinstallazione, aggiornamento, trasferimento

- **Windows:** *Impostazioni → App → Obsidian-Agent → Disinstalla* oppure `deinstallieren.bat` nella cartella del programma.
- **macOS:** `~/Applications/Obsidian-Agent/mac/deinstallieren_mac.command`.

La disinstallazione rimuove l’avvio automatico e la sincronizzazione giornaliera, salva `config.json` e `planer.db` in una cartella `obsidian-agent-backup_<data>` sulla scrivania ed elimina la cartella del programma, se lo desideri. **Il vault non viene mai modificato.**

Aggiornamento: installa semplicemente il nuovo setup o pacchetto. Trasferimento su un altro computer: installalo lì e copia `config.json` e `planer.db` dal backup nella cartella del programma (il database è facoltativo; senza di esso manca solo lo storico giornaliero).

## Protezione dei dati

Il vault contiene dati personali e informazioni di progetto riservate. L’agente funziona solo in locale (`127.0.0.1`) e non invia nulla all’esterno. Configura un backup del vault su un’unità aziendale sicura e usa solo servizi cloud autorizzati.

## Riga di comando

```
python agent.py sync               # leggere il vault ora
python agent.py heute              # attività di oggi e in ritardo
python agent.py projekt "Nome"     # creare un progetto
python agent.py init --sprache it  # impostare la lingua (de, en, fr, it, es, ro)
```
