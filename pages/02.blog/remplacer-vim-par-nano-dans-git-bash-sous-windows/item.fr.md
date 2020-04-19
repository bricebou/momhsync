---
title: 'Remplacer vim par nano dans Git Bash sous Windows'
date: '12-08-2014 00:00'
taxonomy:
    category:
        - Windows
    tag:
        - terminal
        - git
        - console
---

Vous êtes linuxien, incapable de comprendre __vim__ (hormis le _:quit_...) et utilisez Git Bash sous Windows qui propose cet éditeur par défaut&nbsp;? Faites comme moi, passez à __nano__&nbsp;!

## Installation de nano

Commencez par vous rendre sur la [page de téléchargement de nano](http://www.nano-editor.org/download.php) et télécharger l'archive au format zip.

Décompressez l'ensemble des fichiers de l'archive vers le répertoire `C:\Program Files (x86)\Git\share\nano\` (en fonction du chemin d'installation de Git).

## Nano dans Git Bash

Créez un fichier avec l'éditeur de votre choix (Sublime Text, Notepad++) avec ceci dedans&nbsp;:

```bash
#!/bin/sh
exec /share/nano/nano.exe "$@"
```

Enregistrez-le sous le nom __nano__ (sans extension) dans un répertoire qui se trouve dans le Path ou ajouter le répertoire dans le Path.

### Connaître et modifier le Path sous Windows

Dans l'explorateur de fichier, faites un clic droit sur `Ordinateur`, sélectionnez `Propriétés`. Accédez aux `Paramètres système avancés` et allez dans `Variables d'environnement`.
Dans la seconde partie de la fenêtre se trouvent les "Variables système". Double cliquez sur `Path` et vous aurez la liste des dossiers pris en compte par cette variable.
Vous pouvez donc facilement ajouter le dossier où vous avez enregistré le fichier _nano_ créé précédemment.

### Prise en compte par Git Bash

Une fois cela fait, relancer Git Bash s'il était ouvert et vous devriez pouvoir lancer la commande nano directement&nbsp;:

```bash
$ nano
```

Si ce n'est pas le cas et que vous avez modifié le Path, ouvrez une invite de commande (Ctrl+R puis tapez cmd) et vérifiez la valeur de PATH&nbsp;:

```bash
> PATH
```

Si votre répertoire n'apparaît pas dans la liste, lancez la commande&nbsp;:

```bash
SET PATH=%PATH%;C:\VOTRE\REPERTOIRE
```

Relancez Git Bash et testez à nouveau de lancer nano.

### Utiliser nano pour les _commits_

Vous devez configurer Git pour qu'il appelle nano plutôt que vim pour les _commits_&nbsp;:
```bash
$ git config --global core.editor "nano"
```

Lors de votre prochain _commit_ vous devriez donc bénéficier de nano.