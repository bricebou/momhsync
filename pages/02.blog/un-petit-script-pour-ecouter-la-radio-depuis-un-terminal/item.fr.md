---
title: 'Un petit script pour écouter la radio depuis un terminal'
date: '22-07-2018 00:00'
taxonomy:
    category:
        - Linux
    tag:
        - bash
        - console
---

Afin de faciliter l'écoute de certaines radio en streaming, et surtout sans utiliser un navigateur web, de plus en plus gourmand en ressources, j'utilise désormais un petit script bash qui utilise `mplayer`.

```bash
$ mkdir /home/$USER/bin
$ touch /home/$USER/bin/radio
$ chmod +x /home/$USER/bin/radio
$ nano /home/$USER/bin/radio
```

```bash
#!/bin/bash

usage ()
{
    echo '*****************************************************'
    echo '******************** Radio **************************'
    echo '*****************************************************'
    echo '  - beaub    &nbsp;: BeaubFM, radio associative de Limoges'
    echo '  - culture  &nbsp;: France Culture'
    echo '  - info     &nbsp;: France Info'
    echo '  - inter    &nbsp;: France Inter'
    echo '*****************************************************'
}

case $1 in
    beaub )         mplayer http://beaubfm2.ice.infomaniak.ch/beaubfm2-96.mp3
                   &nbsp;;;
    culture )       mplayer http://direct.franceculture.fr/live/franceculture-midfi.mp3
                   &nbsp;;;
    info )          mplayer http://direct.franceinfo.fr/live/franceinfo-midfi.mp3
                   &nbsp;;;
    inter )         mplayer http://direct.franceinter.fr/live/franceinter-midfi.mp3
                   &nbsp;;;
    -h | --help )   usage
                    exit
                   &nbsp;;;
    * )             usage
                    exit 1
esac
```

Ensuite, il suffit de choisir sa radio et de lancer la commande suivante (par exemple)&nbsp;:

```bash
$ radio culture
```

### Auto-complétion des stations

On crée le fichier `radio-completion.bash`&nbsp;:

```bash
$ nano /home/$USER/bin/radio-completion.bash
```

dans lequel on colle les deux lignes suivantes&nbsp;:

```bash
#/usr/bin/env bash
complete -W "beaub culture info inter" radio
```

lancez la commande suivante&nbsp;:

```bash
$ source /home/$USER/bin/radio-completion.bash
```

Et vous devriez avoir les stations qui vous sont proposées lorsque vous pressez la touche `Tab` à la suite de la commande `radio` et au fur et à mesure de votre saisie.