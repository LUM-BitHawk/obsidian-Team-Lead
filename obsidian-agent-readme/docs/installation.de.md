# Obsidian-Agent – Kurzanleitung Installation

[← Zur Übersicht aller Anleitungen](README.md) · [Ausführliche Anleitung](README.de.md)

[![Die fünf Schritte des Setup-Assistenten (schematische Darstellung)](screenshots/Setup-Assistent_Vorschau.png)](screenshots/Setup-Assistent_Vorschau.png)
<sub>Die fünf Schritte des Setup-Assistenten (schematische Darstellung)</sub>

## 🪟 Windows

1. **ZIP herunterladen und entpacken** – `Obsidian-Agent-Setup-2.0.0-Windows.zip` herunterladen, mit Rechtsklick → *Alle extrahieren* entpacken und **`Obsidian-Agent-Setup.exe`** doppelklicken. Erscheint „Der Computer wurde durch Windows geschützt“: *Weitere Informationen* → *Trotzdem ausführen*. Bitte **nicht** „Als Administrator ausführen“ – sonst wird für das Administrator-Konto installiert.
2. **Sprache wählen** – Deutsch, English, Français, Italiano, Español oder Română. Vorgewählt ist die Sprache von Windows.
3. **Willkommen** – Mit *Weiter* bestätigen. Ist der Agent schon installiert, wird er aktualisiert; Einstellungen und Datenbank bleiben erhalten.
4. **Zielverzeichnis und Optionen** – Vorgeschlagen ist `%LOCALAPPDATA%\Programs\Obsidian-Agent` (keine Administratorrechte nötig). Optionen: Autostart bei der Anmeldung, Verknüpfung im Startmenü, Verknüpfung auf dem Desktop.
5. **Obsidian-Vault wählen** – Mit *Durchsuchen …* den Ordner deines Vaults wählen oder „Später in den Einstellungen festlegen“ ankreuzen. Dann *Installieren*.
6. **Installieren** – Der Fortschrittsbalken zeigt den Stand, *Details anzeigen* das Protokoll. Fehlt Python, wird es automatisch installiert (Internet nötig).
7. **Fertigstellen** – „Obsidian-Agent öffnen“ startet die Oberfläche im Browser, „Anleitung anzeigen“ öffnet die Anleitung in deiner Sprache.

## 🍎 macOS

1. `Obsidian-Agent-Mac.zip` herunterladen, entpacken und **`Obsidian-Agent.pkg`** doppelklicken. Beim ersten Mal: Rechtsklick → *Öffnen* → *Öffnen*.
2. Das Paket installiert nach `~/Applications/Obsidian-Agent`. Danach öffnet sich ein Terminal-Fenster: Sprache, Vault und Autostart wählen.
3. Voraussetzung: Python 3.9 oder neuer (python.org).

## Danach

Die Oberfläche läuft unter **http://127.0.0.1:5055** – am besten als Lesezeichen speichern. Protokoll der Installation: `setup.log` im Programmordner.
