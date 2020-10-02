---
title: 'ncmpcpp, un client pour mpd'
media_order: config.txt
date: '04-09-2011 00:00'
taxonomy:
    category:
        - Linux
    tag:
        - mpd
twitterenable: true
twittercardoptions: summary
facebookenable: true
---

ncmpcpp est un client pour [MPD](/blog/mpd-music-player-daemon "Installation et configuration de MPD"), plus complet que _ncmpc_, dont il reprend le mécanisme de navigation, l'interface en ncurse, apportant de nouvelles fonctionnalités parmi lesquelles la visualisation, l'édition des tags, la recherche avec des expressions régulières, une véritable navigation au sein de votre collection… 

===

_ncmpcpp_ est (bien évidemment) disponible dans les dépôts&nbsp;:

```shell
sudo apt install ncmpcpp
```

Pour le lancer, la commande suivante suffit&nbsp;:

```shell
ncmpcpp
```

La configuration se fait, comme pour ncmpc, à la main dans le fichier _~/.ncmpcpp/config_. On commence par créer le répertoire ~/.ncmpcpp puis on copie le ficher d'exemple avant de l'éditer&nbsp;:

```shell
mkdir ~/.ncmpcpp
cp /usr/share/doc/ncmpcpp/config.gz ~/.ncmpcpp/
gzip -d ~/.ncmpcpp/config.gz
nano ~/.ncmpcpp/config
```

Si vous avez des difficultés pour configurer ncmpcpp, penchez-vous sur la documentation&nbsp;:

```shell
man ncmpcpp
```

ll faut relancer ncmpcpp pour que les modifications du fichier de configuration soient prises en compte. Rien de particulier si ce ne sont ces deux points&nbsp;:

1. pour pouvoir éditer les tags depuis ncmpcpp, il vous faut impérativement renseigner le champ _mpd\_music\_dir_ (ligne 14) qui correspond au répertoire de votre musique (celui que vous avez indiqué dans la configuration de MPD)&nbsp;;
2. pour profiter de la visualisation, il faut commencer par modifier la configuration de MPD et ajouter dans la section Audio Output ces lignes&nbsp;:  

```
audio_output {
 type "fifo"
 name "My FIFO"
 path "/tmp/mpd.fifo"
 format "44100:16:1"
}
```

Il faut ensuite éditer certaines options dans le fichier `~/.ncmpcpp/config`&nbsp;; les voici&nbsp;:

```
visualizer_fifo_path = "/tmp/mpd.fifo"
visualizer_output_name = "My MPD PulseAudio Output"
visualizer_sync_interval = "30"
visualizer_type = "spectrum" (spectrum/wave)
```

Pensez à relancer mpd&nbsp;:

```shell
sudo service mpd restart
```

Un fichier de configuration est téléchargeable [ici](config.txt).