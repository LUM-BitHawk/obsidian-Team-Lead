# Obsidian-Agent – Guide

Version 2.0.0 · [Deutsch](README.de.md) · [Français](README.fr.md) · [Italiano](README.it.md) · [Español](README.es.md) · [Română](README.ro.md)

The Obsidian-Agent is a local interface for project management with Obsidian. It runs in your browser at `http://127.0.0.1:5055`, but only on your own computer; no data is sent to the internet.

**Core principle:** the Markdown files in your Obsidian vault are the single source of truth. The agent reads them, shows tasks, projects and meetings in a clear overview and creates new notes in the right format. The database `planer.db` is only an index and is rebuilt on every sync, so you can keep working directly in Obsidian at any time.

## Installation

### Windows

1. Double-click **`Obsidian-Agent-Setup.exe`**.
   Windows may show “Windows protected your PC” because the program is not signed: **More info → Run anyway**.
2. Choose a language and press Enter.
3. Setup copies the program to `%LOCALAPPDATA%\Programs\Obsidian-Agent` and configures it:
   - If Python 3.10 or newer is missing, it is installed for your user account with `winget` (otherwise python.org opens).
   - A dialog asks for your vault folder.
   - From now on the web interface starts in the background at every sign-in; the daily sync runs at 04:00.
   - **Obsidian-Agent** appears in the Start menu, and an uninstall entry under *Settings → Apps*.

No administrator rights are needed. Running setup again updates the program; `config.json` and `planer.db` are kept.

### macOS

1. Double-click **`Obsidian-Agent.pkg`**.
   The package is not notarized by Apple. If macOS says it “can’t be opened”: **right-click → Open → Open** (macOS 15 and later: *System Settings → Privacy & Security → Open Anyway*).
2. The package installs to `~/Applications/Obsidian-Agent` (for you only, no administrator password).
3. A Terminal window then opens: choose the language, choose the vault, start at sign-in yes/no. The daily sync is set up with launchd and caught up after the Mac wakes.

**Requirement:** Python 3.9 or newer, e.g. from python.org or with `brew install python`.

If your vault is in *Documents*, on the *Desktop* or in *iCloud Drive*, macOS may block the background sync. The setup detects this and shows which file to add under *System Settings → Privacy & Security → Full Disk Access*.

Start: double-click `~/Applications/Obsidian-Agent/Obsidian-Agent.command` or open the bookmark `http://127.0.0.1:5055`.

### Linux

```bash
./start.sh                       # create the Python environment and start
.venv/bin/python agent.py init --sprache en
```
Daily sync via `crontab -e`: `0 4 * * * cd /path/obsidian-agent && .venv/bin/python agent.py sync --geplant`

## Sections

| Section | Purpose |
|---|---|
| **Today** | Tasks due today and overdue, the next days, the next recurring meetings. Create a task directly. |
| **Tasks** | All tasks by Today / Overdue / Planned / No date / Done, filter by project. Tick off, change status, “+1 day”. |
| **Projects** | Projects with client, project lead, deadline and number of open tasks. New project with a fixed folder structure. |
| **Minutes** | Meeting minutes, decisions and change requests from templates; with a project they go into the matching project folder. |
| **Recurring meetings** | Regular meetings (e.g. project team, steering committee) with frequency and action items. Each new meeting note lists the open action items of the series. |
| **Team** | Person cards with role, contact and open tasks. Assignment via the field `verantwortlich` (name or initials). |
| **Search** | Full-text search across the whole vault. |
| **Settings** | Language, vault path, folder names, time of the daily sync. |

Every title is an `obsidian://` link and opens the note directly in Obsidian.

## Folders in the vault

During the first setup the folder names are created in your language:

| Purpose | Folder |
|---|---|
| Projects | `Projects/` |
| Tasks without a project | `Tasks/` |
| Minutes without a project | `Minutes/` |
| Team members | `Team/` |
| Recurring meetings | `Meetings/<series>/` |
| Templates | `_Templates/` |
| Daily overview | `Planner/Today.md` |

Each project gets these subfolders (each with a readme file) and an overview note `_Project_<name>.md` with client, project lead and deadline:

- `01_Mandate_and_contracts/` – Project mandate, quote, contracts, orders and amendments.
- `02_Planning/` – Project plan, milestones, resources, budget and risks.
- `03_Contacts/` – Contacts, roles, stakeholders and escalation paths.
- `04_Minutes/` – Meeting minutes, workshops and important correspondence.
- `05_Decisions/` – Decisions and change requests.
- `06_Reports/` – Status reports, steering committee papers and final report.
- `07_Documents/` – Specifications, concepts and deliverables.

If you change the language later, existing folders and notes are **not** renamed. You can adjust the folder names at any time under *Settings*.

## Task format

```markdown
---
typ: task
titel: Prepare acceptance test
projekt: ERP rollout
start: 2026-11-18
ende: 2026-11-20
status: offen
prioritaet: hoch
verantwortlich: AB
---
- [ ] Agree test cases
- [x] Order test environment
```

- **Field names and values are the same in every language** (`status: offen | in_arbeit | wartend | erledigt`, `prioritaet: hoch | mittel | niedrig`). The interface shows them translated. This keeps a vault readable even when people with different languages work on it.
- Today = `start ≤ today ≤ ende`, overdue = `ende < today`, planned = `start > today`.
- `projekt` is the name of the project folder as plain text, `verantwortlich` a name or initials from the team (several separated by commas).
- The agent does not recognise Tasks-plugin style items (`- [ ] … #task 📅 …`). Anything that needs follow-up becomes its own task file.

## Daily overview

Every morning at 04:00 (or at the next start if the computer was off) the agent reads the vault, stores a daily snapshot in the database and writes `Planner/Today.md` plus an archive. This lets you see the overview in Obsidian on your phone too.

## Customising templates

The templates are in `_Templates/` and can be edited in Obsidian; the agent then uses your version. Deleted templates are recreated at the next sync. Placeholders such as `{{title}}` and `{{date}}` also work with Obsidian’s core “Templates” plugin.

## Uninstall, update, moving

- **Windows:** *Settings → Apps → Obsidian-Agent → Uninstall* or `deinstallieren.bat` in the program folder.
- **macOS:** `~/Applications/Obsidian-Agent/mac/deinstallieren_mac.command`.

Uninstalling removes the autostart and the daily sync, backs up `config.json` and `planer.db` to a folder `obsidian-agent-backup_<date>` on the desktop and deletes the program folder if you want. **The vault is never changed.**

Update: simply install the new setup or package. Moving to another computer: install there and copy `config.json` and `planer.db` from the backup into the program folder (the database is optional; without it only the daily history is missing).

## Privacy

The vault contains personal data and confidential project information. The agent runs only locally (`127.0.0.1`) and sends nothing outside. Set up a backup of the vault on a secure company drive and use only approved cloud storage.

## Command line

```
python agent.py sync               # read the vault now
python agent.py heute              # today's and overdue tasks
python agent.py projekt "Name"     # create a project
python agent.py init --sprache en  # set the language (de, en, fr, it, es, ro)
```
