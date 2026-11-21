# Obsidian-Agent – Guide d’installation rapide

[← Retour à la liste des guides](README.md) · [Guide complet](README.fr.md)

[![Les cinq étapes de l’assistant d’installation (schéma)](screenshots/Setup-Assistent_Vorschau.png)](screenshots/Setup-Assistent_Vorschau.png)
<sub>Les cinq étapes de l’assistant d’installation (schéma)</sub>

## 🪟 Windows

1. **Télécharger et extraire le ZIP** – Téléchargez `Obsidian-Agent-Setup-2.0.0-Windows.zip`, clic droit → *Extraire tout*, puis double-cliquez sur **`Obsidian-Agent-Setup.exe`**. Si « Windows a protégé votre ordinateur » s’affiche : *Informations complémentaires* → *Exécuter quand même*. N’utilisez **pas** « Exécuter en tant qu’administrateur », sinon l’installation se fait pour le compte administrateur.
2. **Choisir la langue** – Deutsch, English, Français, Italiano, Español ou Română. La langue de Windows est présélectionnée.
3. **Bienvenue** – Confirmez avec *Suivant*. Si l’agent est déjà installé, il est mis à jour ; paramètres et base de données sont conservés.
4. **Dossier et options** – Proposé : `%LOCALAPPDATA%\Programs\Obsidian-Agent` (aucun droit administrateur). Options : démarrage à la connexion, raccourci dans le menu Démarrer, raccourci sur le Bureau.
5. **Choisir le coffre Obsidian** – Avec *Parcourir …*, choisissez le dossier de votre coffre ou cochez « Définir plus tard dans les paramètres ». Puis *Installer*.
6. **Installation** – La barre de progression indique l’avancement, *Afficher les détails* le journal. Si Python manque, il est installé automatiquement (Internet requis).
7. **Terminer** – « Ouvrir Obsidian-Agent » lance l’interface dans le navigateur, « Afficher le guide » ouvre le guide dans votre langue.

## 🍎 macOS

1. Téléchargez `Obsidian-Agent-Mac.zip`, extrayez-le et double-cliquez sur **`Obsidian-Agent.pkg`**. La première fois : clic droit → *Ouvrir* → *Ouvrir*.
2. Le paquet s’installe dans `~/Applications/Obsidian-Agent`. Une fenêtre Terminal s’ouvre ensuite : choisissez la langue, le coffre et le démarrage automatique.
3. Prérequis : Python 3.9 ou plus récent (python.org).

## Ensuite

L’interface fonctionne sur **http://127.0.0.1:5055** – à enregistrer en signet. Journal d’installation : `setup.log` dans le dossier du programme.
