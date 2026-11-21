# Obsidian-Agent – Ghid rapid de instalare

[← Înapoi la lista ghidurilor](README.md) · [Ghid complet](README.ro.md)

[![Cei cinci pași ai asistentului de instalare (schemă)](screenshots/Setup-Assistent_Vorschau.png)](screenshots/Setup-Assistent_Vorschau.png)
<sub>Cei cinci pași ai asistentului de instalare (schemă)</sub>

## 🪟 Windows

1. **Descărcați și dezarhivați ZIP-ul** – Descărcați `Obsidian-Agent-Setup-2.0.0-Windows.zip`, clic dreapta → *Extragere totală* și dublu clic pe **`Obsidian-Agent-Setup.exe`**. Dacă apare „Windows a protejat computerul”: *Mai multe informații* → *Rulează oricum*. **Nu** folosiți „Executare ca administrator”, altfel instalarea se face pentru contul de administrator.
2. **Alegeți limba** – Deutsch, English, Français, Italiano, Español sau Română. Limba Windows este preselectată.
3. **Bun venit** – Confirmați cu *Înainte*. Dacă agentul este deja instalat, este actualizat; setările și baza de date se păstrează.
4. **Dosar și opțiuni** – Propus: `%LOCALAPPDATA%\Programs\Obsidian-Agent` (fără drepturi de administrator). Opțiuni: pornire la autentificare, comandă rapidă în meniul Start, comandă rapidă pe desktop.
5. **Alegeți seiful Obsidian** – Cu *Răsfoire …* alegeți dosarul seifului sau bifați „Setez mai târziu în Setări”. Apoi *Instalează*.
6. **Instalare** – Bara arată progresul, *Afișează detalii* jurnalul. Dacă lipsește Python, este instalat automat (necesită internet).
7. **Terminare** – „Deschide Obsidian-Agent” pornește interfața în browser, „Afișează ghidul” deschide ghidul în limba dvs.

## 🍎 macOS

1. Descărcați `Obsidian-Agent-Mac.zip`, dezarhivați-l și faceți dublu clic pe **`Obsidian-Agent.pkg`**. Prima dată: clic dreapta → *Deschide* → *Deschide*.
2. Pachetul se instalează în `~/Applications/Obsidian-Agent`. Apoi se deschide o fereastră Terminal: alegeți limba, seiful și pornirea automată.
3. Cerință: Python 3.9 sau mai nou (python.org).

## După aceea

Interfața rulează la **http://127.0.0.1:5055** – salvați-o ca semn de carte. Jurnalul instalării: `setup.log` în dosarul programului.
