# Obsidian-Agent – Guide

Version 2.0.0 · [Deutsch](README.de.md) · [English](README.en.md) · [Italiano](README.it.md) · [Español](README.es.md) · [Română](README.ro.md)

L’Obsidian-Agent est une interface locale de gestion de projet avec Obsidian. Il s’ouvre dans le navigateur à l’adresse `http://127.0.0.1:5055`, mais uniquement sur votre ordinateur ; aucune donnée n’est envoyée sur Internet.

**Principe :** les fichiers Markdown de votre coffre Obsidian sont la seule référence. L’agent les lit, présente tâches, projets et réunions de façon claire et crée les nouvelles notes au bon format. La base de données `planer.db` n’est qu’un index, reconstruit à chaque synchronisation ; vous pouvez donc toujours continuer à travailler directement dans Obsidian.

## Installation

### Windows

1. Double-cliquez sur **`Obsidian-Agent-Setup.exe`**.
   Windows peut afficher « Windows a protégé votre ordinateur », car le programme n’est pas signé : **Informations complémentaires → Exécuter quand même**.
2. Choisissez la langue et validez avec Entrée.
3. Le programme d’installation copie l’application dans `%LOCALAPPDATA%\Programs\Obsidian-Agent` et la configure :
   - Si Python 3.10 ou plus récent manque, il est installé pour votre compte avec `winget` (sinon python.org s’ouvre).
   - Une boîte de dialogue demande le dossier de votre coffre.
   - L’interface web démarre désormais en arrière-plan à chaque connexion ; la synchronisation quotidienne a lieu à 04:00.
   - **Obsidian-Agent** apparaît dans le menu Démarrer, et une entrée de désinstallation sous *Paramètres → Applications*.

Aucun droit administrateur n’est nécessaire. Relancer le programme d’installation met l’application à jour ; `config.json` et `planer.db` sont conservés.

### macOS

1. Double-cliquez sur **`Obsidian-Agent.pkg`**.
   Le paquet n’est pas notarié par Apple. Si macOS indique qu’il « ne peut pas être ouvert » : **clic droit → Ouvrir → Ouvrir** (macOS 15 et plus : *Réglages Système → Confidentialité et sécurité → Ouvrir quand même*).
2. Le paquet s’installe dans `~/Applications/Obsidian-Agent` (pour vous seul, sans mot de passe administrateur).
3. Une fenêtre Terminal s’ouvre ensuite : choisir la langue, choisir le coffre, démarrage à la connexion oui/non. La synchronisation quotidienne est configurée avec launchd et rattrapée au réveil du Mac.

**Prérequis :** Python 3.9 ou plus récent, p. ex. depuis python.org ou avec `brew install python`.

Si le coffre se trouve dans *Documents*, sur le *Bureau* ou dans *iCloud Drive*, macOS peut bloquer la synchronisation en arrière-plan. La configuration le détecte et indique quel fichier ajouter sous *Réglages Système → Confidentialité et sécurité → Accès complet au disque*.

Démarrage : double-cliquez sur `~/Applications/Obsidian-Agent/Obsidian-Agent.command` ou ouvrez le signet `http://127.0.0.1:5055`.

### Linux

```bash
./start.sh                       # créer l’environnement Python et démarrer
.venv/bin/python agent.py init --sprache fr
```
Synchronisation quotidienne avec `crontab -e` : `0 4 * * * cd /chemin/obsidian-agent && .venv/bin/python agent.py sync --geplant`

## Rubriques

| Rubrique | Usage |
|---|---|
| **Aujourd’hui** | Tâches du jour et en retard, les prochains jours, les prochaines réunions régulières. Créer une tâche directement. |
| **Tâches** | Toutes les tâches par Aujourd’hui / En retard / Planifiées / Sans date / Terminées, filtre par projet. Cocher, changer le statut, « +1 jour ». |
| **Projets** | Projets avec client, chef de projet, échéance et nombre de tâches ouvertes. Nouveau projet avec une arborescence fixe. |
| **Procès-verbaux** | Procès-verbal, décision et demande de modification à partir de modèles ; avec un projet, ils sont rangés dans le bon dossier. |
| **Réunions régulières** | Réunions récurrentes (p. ex. équipe projet, comité de pilotage) avec fréquence et points ouverts. Chaque nouvelle note de réunion reprend les points ouverts de la série. |
| **Équipe** | Fiches avec rôle, contact et tâches ouvertes. Attribution via le champ `verantwortlich` (nom ou initiales). |
| **Recherche** | Recherche plein texte dans tout le coffre. |
| **Paramètres** | Langue, chemin du coffre, noms des dossiers, heure de la synchronisation quotidienne. |

Chaque titre est un lien `obsidian://` qui ouvre la note directement dans Obsidian.

## Dossiers du coffre

Lors de la première configuration, les dossiers sont créés dans votre langue :

| Usage | Dossier |
|---|---|
| Projets | `Projets/` |
| Tâches sans projet | `Taches/` |
| Procès-verbaux sans projet | `Proces-verbaux/` |
| Membres de l’équipe | `Equipe/` |
| Réunions régulières | `Reunions/<série>/` |
| Modèles | `_Modeles/` |
| Vue du jour | `Planning/Aujourdhui.md` |

Chaque projet reçoit ces sous-dossiers (chacun avec un fichier readme) et une note de synthèse `_Projet_<nom>.md` avec client, chef de projet et échéance :

- `01_Mandat_et_contrats/` – Mandat de projet, offre, contrats, commandes et avenants.
- `02_Planification/` – Plan de projet, jalons, ressources, budget et risques.
- `03_Contacts/` – Interlocuteurs, rôles, parties prenantes et voies d’escalade.
- `04_Proces-verbaux/` – Procès-verbaux, ateliers et correspondance importante.
- `05_Decisions/` – Décisions et demandes de modification.
- `06_Rapports/` – Rapports d’avancement, documents du comité de pilotage et rapport final.
- `07_Documents/` – Documents techniques, concepts et livrables.

Si vous changez de langue plus tard, les dossiers et notes existants ne sont **pas** renommés. Vous pouvez adapter les noms des dossiers à tout moment dans les *Paramètres*.

## Format des tâches

```markdown
---
typ: task
titel: Préparer le test de recette
projekt: Déploiement ERP
start: 2026-11-18
ende: 2026-11-20
status: offen
prioritaet: hoch
verantwortlich: AB
---
- [ ] Valider les cas de test
- [x] Commander l’environnement de test
```

- **Les noms de champs et les valeurs sont identiques dans toutes les langues** (`status: offen | in_arbeit | wartend | erledigt`, `prioritaet: hoch | mittel | niedrig`). L’interface les affiche traduits. Ainsi, un coffre reste lisible même si des personnes de langues différentes l’utilisent.
- Aujourd’hui = `start ≤ aujourd’hui ≤ ende`, en retard = `ende < aujourd’hui`, planifiée = `start > aujourd’hui`.
- `projekt` est le nom du dossier du projet en texte simple, `verantwortlich` un nom ou des initiales de l’équipe (plusieurs séparés par des virgules).
- L’agent ne reconnaît pas les tâches au format du plugin Tasks (`- [ ] … #task 📅 …`). Tout ce qui doit être suivi devient un fichier de tâche à part.

## Vue du jour

Chaque matin à 04:00 (ou au prochain démarrage si l’ordinateur était éteint), l’agent lit le coffre, enregistre un instantané quotidien dans la base de données et écrit `Planning/Aujourdhui.md` ainsi qu’une archive. Vous voyez ainsi la vue d’ensemble aussi dans Obsidian sur votre téléphone.

## Adapter les modèles

Les modèles se trouvent dans `_Modeles/` et se modifient dans Obsidian ; l’agent utilise ensuite votre version. Les modèles supprimés sont recréés à la synchronisation suivante. Les variables comme `{{title}}` et `{{date}}` fonctionnent aussi avec le plugin de base « Modèles » d’Obsidian.

## Désinstallation, mise à jour, déménagement

- **Windows :** *Paramètres → Applications → Obsidian-Agent → Désinstaller* ou `deinstallieren.bat` dans le dossier du programme.
- **macOS :** `~/Applications/Obsidian-Agent/mac/deinstallieren_mac.command`.

La désinstallation supprime le démarrage automatique et la synchronisation quotidienne, sauvegarde `config.json` et `planer.db` dans un dossier `obsidian-agent-backup_<date>` sur le bureau et supprime le dossier du programme si vous le souhaitez. **Le coffre n’est jamais modifié.**

Mise à jour : installez simplement le nouveau programme d’installation ou paquet. Changement d’ordinateur : installez-le sur le nouvel ordinateur et copiez `config.json` et `planer.db` de la sauvegarde dans le dossier du programme (la base de données est facultative ; sans elle, seul l’historique quotidien manque).

## Protection des données

Le coffre contient des données personnelles et des informations de projet confidentielles. L’agent fonctionne uniquement en local (`127.0.0.1`) et n’envoie rien à l’extérieur. Mettez en place une sauvegarde du coffre sur un lecteur d’entreprise sécurisé et n’utilisez que des stockages cloud autorisés.

## Ligne de commande

```
python agent.py sync               # lire le coffre maintenant
python agent.py heute              # tâches du jour et en retard
python agent.py projekt "Nom"      # créer un projet
python agent.py init --sprache fr  # définir la langue (de, en, fr, it, es, ro)
```
