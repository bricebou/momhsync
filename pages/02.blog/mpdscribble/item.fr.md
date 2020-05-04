---
title: 'mpdscribble : MPD et LastFM'
date: '13-09-2011 00:00'
taxonomy:
    category:
        - Linux
    tag:
        - mpd
        - mpdscribble
        - lastfm
        - console
---

Utilisateur de [LastFM](https://www.last.fm/) et de [MPD](/blog/mpd-music-player-daemon "Installation et configuration de MPD") pour écouter ma musique, je dois pouvoir scrobbler les titres que j'écoute&nbsp;: _mpdscribble_ est la solution, particulièrement simple à installer et configurer.

===

Il faut donc commencer par installer le paquet `mpdscribble`&nbsp;:

```bash
$ sudo apt install mpdscribble
```

Une fois cela fait, il faut le configurer&nbsp;:

```bash
$ sudo dpkg-reconfigure mpdscribble
```

Il faut alors accepter de lancer mpdscribble comme daemon, puis saisir ses identifiants LastFM. Une fois cela fait, le service devrait démarrer et les titres écoutés apparaîtront sur LastFM.