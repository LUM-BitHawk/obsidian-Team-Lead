# Obsidian-Agent – Anleitung

Version 2.0.0 · [Übersicht](README.md) · [English](README.en.md) · [Français](README.fr.md) · [Italiano](README.it.md) · [Español](README.es.md) · [Română](README.ro.md)

Der Obsidian-Agent ist eine lokale Oberfläche für Projektmanagement mit Obsidian. Er läuft im Browser unter `http://127.0.0.1:5055`, aber nur auf deinem Rechner; es werden keine Daten ins Internet übertragen.

**Grundprinzip:** Die Markdown-Dateien in deinem Obsidian-Vault sind die einzige Wahrheit. Der Agent liest sie ein, zeigt Aufgaben, Projekte und Termine übersichtlich an und legt neue Notizen im richtigen Format an. Die Datenbank `planer.db` ist nur ein Index und wird bei jedem Sync neu aufgebaut. Du kannst also jederzeit direkt in Obsidian weiterarbeiten.

## Installation

### Windows

1. **`Obsidian-Agent-Setup.exe`** doppelklicken.
   Windows zeigt eventuell „Der Computer wurde durch Windows geschützt“, weil das Programm nicht signiert ist: **Weitere Informationen → Trotzdem ausführen**.
2. Sprache wählen und mit Enter bestätigen.
3. Das Setup kopiert das Programm nach `%LOCALAPPDATA%\Programs\Obsidian-Agent` und richtet es ein:
   - Fehlt Python 3.10 oder neuer, wird es mit `winget` für dein Benutzerkonto installiert (sonst öffnet sich python.org).
   - Ein Dialog fragt nach deinem Vault-Ordner.
   - Die Weboberfläche startet ab jetzt bei jeder Anmeldung im Hintergrund, der Tages-Sync läuft täglich um 04:00.
   - Im Startmenü erscheint **Obsidian-Agent**, unter *Einstellungen → Apps* ein Eintrag zum Deinstallieren.

Keine Administratorrechte nötig. Ein erneutes Ausführen des Setups aktualisiert das Programm; `config.json` und `planer.db` bleiben erhalten.

### macOS

1. **`Obsidian-Agent.pkg`** doppelklicken.
   Das Paket ist nicht von Apple beglaubigt. Meldet macOS „kann nicht geöffnet werden“: **Rechtsklick → Öffnen → Öffnen** (ab macOS 15: *Systemeinstellungen → Datenschutz & Sicherheit → Trotzdem öffnen*).
2. Das Paket installiert nach `~/Applications/Obsidian-Agent` (nur für dich, ohne Administratorpasswort).
3. Danach öffnet sich ein Terminal-Fenster: Sprache wählen, Vault wählen, Autostart ja/nein. Der tägliche Sync wird per launchd eingerichtet und nach dem Aufwachen nachgeholt.

**Voraussetzung:** Python 3.9 oder neuer, z.B. von python.org oder mit `brew install python`.

Liegt der Vault in *Dokumente*, auf dem *Schreibtisch* oder in *iCloud Drive*, kann macOS den Hintergrund-Sync blockieren. Die Einrichtung erkennt das und zeigt, welche Datei du unter *Systemeinstellungen → Datenschutz & Sicherheit → Festplattenvollzugriff* hinzufügen musst.

Starten: `~/Applications/Obsidian-Agent/Obsidian-Agent.command` doppelklicken oder das Lesezeichen `http://127.0.0.1:5055` öffnen.

### Linux

```bash
./start.sh                       # Python-Umgebung anlegen und starten
.venv/bin/python agent.py init --sprache de
```
Täglicher Sync per `crontab -e`: `0 4 * * * cd /pfad/obsidian-agent && .venv/bin/python agent.py sync --geplant`

## Bereiche

| Bereich | Wofür |
|---|---|
| **Heute** | Heute fällige und überfällige Tasks, die nächsten Tage, die nächsten Jour Fixes. Neuer Task direkt erfassen. |
| **Tasks** | Alle Tasks nach Heute / Überfällig / Geplant / Ohne Datum / Erledigt, Filter nach Projekt. Abhaken, Status ändern, „+1 Tag“. |
| **Projekte** | Projekte mit Kunde, Projektleitung, Deadline und Anzahl offener Tasks. Neues Projekt mit fester Ordnerstruktur. |
| **Protokolle** | Meeting-Protokoll, Entscheidung und Änderungsantrag aus Vorlagen; mit Projekt landen sie im passenden Projektordner. |
| **Jour Fixe** | Wiederkehrende Sitzungen (z.B. Projektteam, Lenkungsausschuss) mit Turnus und Pendenzen. Jede neue Sitzungsnotiz enthält die offenen Pendenzen der Serie. |
| **Team** | Personenkarten mit Rolle, Kontakt und offenen Tasks. Zuordnung über das Feld `verantwortlich` (Name oder Kürzel). |
| **Suche** | Volltextsuche über den ganzen Vault. |
| **Einstellungen** | Sprache, Vault-Pfad, Ordnernamen, Zeit des Tages-Syncs. |

Jeder Titel ist ein `obsidian://`-Link und öffnet die Notiz direkt in Obsidian.

## Ordner im Vault

Bei der Ersteinrichtung werden die Ordnernamen in deiner Sprache angelegt:

| Zweck | Ordner |
|---|---|
| Projekte | `Projekte/` |
| Tasks ohne Projekt | `Tasks/` |
| Protokolle ohne Projekt | `Protokolle/` |
| Teammitglieder | `Team/` |
| Jour Fixe | `JourFixe/<Serie>/` |
| Vorlagen | `_Vorlagen/` |
| Tagesübersicht | `Planer/Heute.md` |

Jedes Projekt bekommt diese Unterordner (jeweils mit einer readme-Datei) und eine Übersichtsnotiz `_Projekt_<Name>.md` mit Kunde, Projektleitung und Deadline:

- `01_Auftrag_und_Vertraege/` – Projektauftrag, Angebot, Verträge, Bestellungen und Nachträge.
- `02_Planung/` – Projektplan, Meilensteine, Ressourcen, Budget und Risiken.
- `03_Kontakte/` – Ansprechpartner, Rollen, Stakeholder und Eskalationswege.
- `04_Protokolle/` – Meeting-Protokolle, Workshops und wichtige Korrespondenz.
- `05_Entscheidungen/` – Entscheidungen und Änderungsanträge (Changes).
- `06_Berichte/` – Statusberichte, Lenkungsausschuss-Unterlagen und Abschlussbericht.
- `07_Dokumente/` – Fachliche Unterlagen, Konzepte und Ergebnisse.

Ändert sich später die Sprache, werden bestehende Ordner und Notizen **nicht** umbenannt. Die Ordnernamen kannst du jederzeit unter *Einstellungen* anpassen.

## Task-Format

```markdown
---
typ: task
titel: Abnahmetest vorbereiten
projekt: ERP-Einführung
start: 2026-11-18
ende: 2026-11-20
status: offen
prioritaet: hoch
verantwortlich: AB
---
- [ ] Testfälle abstimmen
- [x] Testumgebung bestellen
```

- **Feldnamen und Werte bleiben in allen Sprachen gleich** (`status: offen | in_arbeit | wartend | erledigt`, `prioritaet: hoch | mittel | niedrig`). Die Oberfläche zeigt sie übersetzt an. So bleibt ein Vault lesbar, auch wenn Personen mit unterschiedlicher Sprache damit arbeiten.
- Heute = `start ≤ heute ≤ ende`, überfällig = `ende < heute`, geplant = `start > heute`.
- `projekt` ist der Name des Projektordners als einfacher Text, `verantwortlich` ein Name oder Kürzel aus dem Team (mehrere mit Komma).
- Aufgaben im Stil des Tasks-Plugins (`- [ ] … #task 📅 …`) erkennt der Agent nicht. Alles, was nachverfolgt werden soll, wird eine eigene Task-Datei.

## Tägliche Übersicht

Jeden Morgen um 04:00 (oder beim nächsten Start, falls der Rechner aus war) liest der Agent den Vault ein, speichert einen Tages-Snapshot in der Datenbank und schreibt `Planer/Heute.md` sowie ein Archiv. So siehst du die Übersicht auch in Obsidian auf dem Handy.

## Vorlagen anpassen

Die Vorlagen liegen in `_Vorlagen/` und lassen sich in Obsidian bearbeiten; der Agent verwendet danach deine Version. Gelöschte Vorlagen werden beim nächsten Sync neu angelegt. Platzhalter wie `{{title}}` und `{{date}}` funktionieren auch mit dem Obsidian-Kernplugin „Vorlagen“.

## Deinstallation, Update, Umzug

- **Windows:** *Einstellungen → Apps → Obsidian-Agent → Deinstallieren* oder `deinstallieren.bat` im Programmordner.
- **macOS:** `~/Applications/Obsidian-Agent/mac/deinstallieren_mac.command`.

Die Deinstallation entfernt Autostart und Tages-Sync, sichert `config.json` und `planer.db` in einen Ordner `obsidian-agent-backup_<Datum>` auf dem Schreibtisch und löscht auf Wunsch den Programmordner. **Der Vault wird nie verändert.**

Update: neues Setup bzw. Paket einfach installieren. Umzug auf einen anderen Rechner: dort installieren und `config.json` sowie `planer.db` aus dem Backup in den Programmordner kopieren (die Datenbank ist optional, ohne sie fehlt nur die Tageshistorie).

## Datenschutz

Der Vault enthält Personaldaten und vertrauliche Projektinformationen. Der Agent läuft ausschliesslich lokal (`127.0.0.1`) und sendet nichts nach aussen. Richte ein Backup des Vaults auf einem sicheren Firmenlaufwerk ein und verwende nur freigegebene Cloud-Speicher.

## Kommandozeile

```
python agent.py sync               # jetzt einlesen
python agent.py heute              # heutige und überfällige Tasks
python agent.py projekt "Name"     # Projekt anlegen
python agent.py init --sprache de  # Sprache festlegen (de, en, fr, it, es, ro)
```
