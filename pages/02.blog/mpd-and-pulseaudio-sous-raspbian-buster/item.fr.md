---
title: 'MPD, PulseAudio et Bluetooth sous Raspbian Buster'
published: false
date: '28-09-2020 20:36'
taxonomy:
    category:
        - Linux
    tag:
        - mpd
        - 'Raspberry Pi'
        - PulseAudio
twitterenable: true
twittercardoptions: summary
facebookenable: true
---

Alors que je venais juste de mettre en place la cohabitation entre MPD et mon ampli connecté en Bluetooth sur mon Raspberry Pi 2 (voir [ici](/blog/mpd-et-bluetooth-sur-raspberry-pi-2)), je basculais sur un modèle 4, modifiant quelque peu mes usages et utiisant cette fois-ci l'interface graphique (afin de profiter notamment de Spotify, [une fois la gestion des DRM ajoutée](/blog/gestion-des-drm-sous-raspbian-buster)). Et du coup, afin d'avoir une maîtrise plus fine de mes sorties audio, j'ai opté pour l'utilisation de PulseAudio.

===

On commence par purger `bluealsa` s'il est installé&nbsp;:

```bash
$ sudo apt purge bluealsa
```

puis on installe tout ce qui concerne PulseAudio&nbsp;:

```bash
 $ sudo apt install pulseaudio pulseaudio-module-bluetooth paprefs pavumeter pavucontrol pasystray
 ```

On vérifie la configuration de PulseAudio (`/etc/pulse/default.pa`) notamment pour ce qui est du Bluetooth&nbsp;:

```
### Automatically load driver modules for Bluetooth hardware
.ifexists module-bluetooth-policy.so
load-module module-bluetooth-policy
.endif

.ifexists module-bluetooth-discover.so
load-module module-bluetooth-discover
.endif
```

et on ajoute ces éléments&nbsp;:

```
# automatically switch to newly-connected devices
load-module module-switch-on-connect

load-module module-native-protocol-tcp auth-ip-acl=127.0.0.1 # IP of localhost
```



Pour lister les sorties disponibles, on lance la commande suivante&nbsp;:

```bash
$ pacmd list-sinks
```

De là, on peut configure
