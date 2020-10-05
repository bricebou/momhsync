---
title: 'Réencoder des fichiers en UTF-8'
date: '23-03-2020 00:00'
taxonomy:
    category:
        - Linux
    tag:
        - 'Raspberry Pi'
        - console
        - encodage
        - sous-titres
twitterenable: true
twittercardoptions: summary
facebookenable: true
---

Régulièrement, les sous-titres ([téléchargés en ligne de commande](sous_titres_2020)) ne sont pas pris en compte par `omxplayer` que j'utilise sur mon Raspberry Pi&nbsp;; la raison&nbsp;? Ils ne sont tout simplement pas en UTF-8...

J'ai longtemps utilisé la commande `recode` mais depuis une réinstallation complète d'une Raspbian Buster sur mon Raspberry Pi 2, et d'une modification du montage des disques de mon NAS, je rencontre des problèmes de permissions...

Du coup, grâce à deux contributions d'une même conversation (celles de [Pierre Fabier](https://superuser.com/a/719319) et d'un [inconnu](https://superuser.com/a/1317744)), j'ai produit un petit script.

Si ce n'est pas déjà fait, on crée un répertoire `bin/` dans notre `home`&nbsp;:

```bash
mkdir ~/bin
```

puis on y crée notre script&nbsp;:

```bash
nano ~/bin/toutf8
```

dans lequel on insère&nbsp;:

```bash
#!/bin/bash
# Find the current encoding of the file
encoding=$(uchardet "$1")

if [ ! "UTF-8" == "${encoding}" ]
  then
  # Encodings differ, we have to encode
    echo "recoding from ${encoding} to UTF-8 file : $1"
    vim +'set nobomb | set fenc=utf8 | x' "$1"
else
    echo "Already utf8 encoding"
fi
```

On le rend exécutable&nbsp;:

```bash
chmod +x ~/bin/toutf8
```

Pour le faire fonctionner, il convient bien d'avoir installé les paquets `uchardet` (plus fiable que la commande `file -i` qui renvoie régulièrement des `unknown-8bit` impossible à passer en argument de `recode` ou à traiter par ce script) et bien sûr `vim`&nbsp;:

```bash
sudo apt install uchardet vim
```

Ensuite, où que vous vous trouviez, vous pourrez appeler le script simplement&nbsp;:

```bash
toutf8 nom_du_fichier
```