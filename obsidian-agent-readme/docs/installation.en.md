# Obsidian-Agent – Quick installation guide

[← Back to the overview of all guides](README.md) · [Full guide](README.en.md)

[![The five steps of the setup wizard (schematic)](screenshots/Setup-Assistent_Vorschau.png)](screenshots/Setup-Assistent_Vorschau.png)
<sub>The five steps of the setup wizard (schematic)</sub>

## 🪟 Windows

1. **Download and extract the ZIP** – Download `Obsidian-Agent-Setup-2.0.0-Windows.zip`, right-click → *Extract All* and double-click **`Obsidian-Agent-Setup.exe`**. If “Windows protected your PC” appears: *More info* → *Run anyway*. Please do **not** use “Run as administrator” – otherwise it installs for the administrator account.
2. **Choose a language** – Deutsch, English, Français, Italiano, Español or Română. Windows’ language is preselected.
3. **Welcome** – Confirm with *Next*. If the agent is already installed it is updated; settings and database are kept.
4. **Install location and options** – The suggestion is `%LOCALAPPDATA%\Programs\Obsidian-Agent` (no administrator rights needed). Options: start at sign-in, Start menu shortcut, desktop shortcut.
5. **Choose the Obsidian vault** – Use *Browse …* to select your vault folder, or tick “Set later in Settings”. Then *Install*.
6. **Installing** – The progress bar shows the status, *Show details* the log. If Python is missing, it is installed automatically (internet required).
7. **Finish** – “Open Obsidian-Agent” starts the interface in your browser, “Show guide” opens the guide in your language.

## 🍎 macOS

1. Download `Obsidian-Agent-Mac.zip`, extract it and double-click **`Obsidian-Agent.pkg`**. First time: right-click → *Open* → *Open*.
2. The package installs to `~/Applications/Obsidian-Agent`. A Terminal window then opens: choose language, vault and start at sign-in.
3. Requirement: Python 3.9 or newer (python.org).

## Then

The interface runs at **http://127.0.0.1:5055** – best saved as a bookmark. Installation log: `setup.log` in the program folder.
