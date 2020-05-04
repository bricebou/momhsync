---
title: 'MPD (Music Player Daemon)'
date: '02-09-2011 00:00'
taxonomy:
    category:
        - Linux
    tag:
        - mpd
        - console
twitterenable: true
twittercardoptions: summary
facebookenable: true
---

MPD (Music Player Daemon) est un lecteur audio fonctionnant selon l'architecture client-serveur. Nous n'allons aborder ici que l'aspect serveur, pris en charge par MPD, et verrons ultérieurement le côté client (en l'occurrence [ncmpcpp](/blog/ncmpcpp "ncmpcpp, un client mpd complet")). Si MPD peut apparaître au premier abord assez complexe, il n'en demeure pas moins un outil extrêmement intéressant ne serait-ce que pour sa légèreté et la flexibilité qu'un tel système propose. Sans chercher à pénétrer trop avant dans les arcanes de sa configuration, il est possible d'obtenir un outil fonctionnel très rapidement et finalement assez facilement. La preuve…

===

Pour commencer, il faut installer le paquet _mpd_&nbsp;:

```bash
$ sudo apt install mpd
```

puis lancer la commande suivante afin de créer le _daemon_&nbsp:

```bash
$ sudo dpkg-reconfigure mpd
```

C'est à ce moment-là que les choses semblent se corser, mais rien de bien alarmant non plus&nbsp;: il va nous falloir modifier le fichier de configuration de MPD. Commençons&nbsp;:

```bash
$ sudo nano /etc/mpd.conf
```

Il faut commencer par renseigner les chemins vers les répertoires où se trouvent notre musique et où seront stockées les playlists&nbsp;:

```
music_directory "/Multimedia/Musique"
playlist_directory "/home/user/.mpd/playlists"
```

On commente (ajout d'un # en début de ligne) la ligne suivante&nbsp;:

```
user "mpd"
```

Que vous utilisiez ALSA ou PulseAudio, les sorties audio sont déjà configurées et fonctionnent par défaut chez moi&nbsp;:

```
audio_output {
        type            "alsa"
        name            "My ALSA Device"
        device          "hw:0,0"        # optional
        mixer_type      "hardware"      # optional
        mixer_device    "default"       # optional
        mixer_control   "PCM"           # optional
        mixer_index     "0"             # optional
}

audio_output {
        type            "pulse"
        name            "My MPD Pulse Output"
        server          "127.0.0.1"             # optional
#       sink            "remote_server_sink"    # optional
}
```

Avec PulseAudio, lancez l'utilitaire _paprefs_ et vérifiez dans l'onglet «&nbsp;Network Server » que «&nbsp;Activer l'accès réseau aux périphériques de son locaux&nbsp;» et «&nbsp;Don't require authentification&nbsp;» soient bien cochés.

Le plus dur est fait. Il ne nous reste plus qu'à relancer MPD avec l'une des deux commandes suivantes :

```bash
$ sudo service mpd restart
```

et à créer la base de données recensant l'ensemble des morceaux disponibles&nbsp;:

```bash
$ sudo mpd --create-db
```

Pour suivre la progression de cette dernière opération (qui peut s'avérer longue si vous avez beaucoup de morceaux) vous pouvez utiliser la commande suivante&nbsp;:

```bash
$ tail -f ~/.mpd/mpd.log
```

Bien que le client soit facultatif, nous vous présentons [ncmpcpp](/blog/ncmpcpp "ncmpcpp, un client mpd complet"), un client en ncurse extrêmement léger, facile à utiliser et à configurer, «&nbsp;grand frère&nbsp;» de _ncmpc_.      
De plus si vous êtes un utilisateur de [LastFM](http://www.lastfm.fr/), vous souhaitez bien évidemment pouvoir scrobbler la musique que vous écoutez avec MPD&nbsp;; pour cela il existe [mpdscribble](/blog/mpdscribble "mpdscribble&nbsp;: mpd et lastfm").