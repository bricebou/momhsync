---
title: 'Générer des playlists pour MPD avec les dernières pistes ajoutées'
date: '09-05-2020 08:23'
taxonomy:
    category:
        - Linux
    tag:
        - mpd
        - bash
twitterenable: true
twittercardoptions: summary
facebookenable: true
---

Afin de retrouver facilement dans [ncmpcpp](/blog/ncmpcpp) les dernières pistes ajoutées à ma bibliothèque, j'ai trouvé et adapté le script suivant.

```shell
$ nano ~/bin/lastaddedmpd
```

```bash
#!/bin/bash

cd /home/$USER/NAS/Musique
find . -type f -ctime -1 | egrep '\.mp3$|\.flac$' | awk '{ sub(/^\.\//, ""); print }' | sort -n > /home/$USER/NAS/Musique/playlists/$(date +%d-%m-%Y)_last_$1_days.m3u
```

```shell
$ chmod +x ~/bin/lastaddedmpd
```

Et on lance la commande `lastaddedmpd` avec pour argument le nombre de jours à prendre en compte.