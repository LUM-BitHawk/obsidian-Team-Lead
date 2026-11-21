# Obsidian-Agent – Guida rapida all’installazione

[← Torna all’elenco delle guide](README.md) · [Guida completa](README.it.md)

[![I cinque passaggi della procedura di installazione (schema)](screenshots/Setup-Assistent_Vorschau.png)](screenshots/Setup-Assistent_Vorschau.png)
<sub>I cinque passaggi della procedura di installazione (schema)</sub>

## 🪟 Windows

1. **Scaricare ed estrarre lo ZIP** – Scaricare `Obsidian-Agent-Setup-2.0.0-Windows.zip`, clic destro → *Estrai tutto* e doppio clic su **`Obsidian-Agent-Setup.exe`**. Se compare «PC protetto da Windows»: *Ulteriori informazioni* → *Esegui comunque*. **Non** usare «Esegui come amministratore», altrimenti l’installazione avviene per l’account amministratore.
2. **Scegliere la lingua** – Deutsch, English, Français, Italiano, Español o Română. È preselezionata la lingua di Windows.
3. **Benvenuto** – Confermare con *Avanti*. Se l’agente è già installato viene aggiornato; impostazioni e database restano.
4. **Cartella e opzioni** – Proposta: `%LOCALAPPDATA%\Programs\Obsidian-Agent` (nessun diritto di amministratore). Opzioni: avvio all’accesso, collegamento nel menu Start, collegamento sul desktop.
5. **Scegliere il vault Obsidian** – Con *Sfoglia …* scegliere la cartella del vault oppure spuntare «Impostare più tardi nelle Impostazioni». Poi *Installa*.
6. **Installazione** – La barra mostra l’avanzamento, *Mostra dettagli* il registro. Se manca Python, viene installato automaticamente (serve Internet).
7. **Fine** – «Apri Obsidian-Agent» avvia l’interfaccia nel browser, «Mostra la guida» apre la guida nella vostra lingua.

## 🍎 macOS

1. Scaricare `Obsidian-Agent-Mac.zip`, estrarlo e fare doppio clic su **`Obsidian-Agent.pkg`**. La prima volta: clic destro → *Apri* → *Apri*.
2. Il pacchetto si installa in `~/Applications/Obsidian-Agent`. Poi si apre una finestra del Terminale: scegliere lingua, vault e avvio automatico.
3. Requisito: Python 3.9 o successivo (python.org).

## Poi

L’interfaccia è disponibile su **http://127.0.0.1:5055** – conviene salvarla come segnalibro. Registro dell’installazione: `setup.log` nella cartella del programma.
