---
title: 'mpdscribble : scrobbler vers last.fm et Libre.fm'
date: '20-04-2015 00:00'
taxonomy:
    category:
        - Linux
    tag:
        - mpd
        - mpdscribble
        - lastfm
---

mpdscribble, une fois installé et configuré (cf. [cet article](/blog/mpdscribble "MPD et LastFM&nbsp;: mpdscribble")), ne scrobble que vers le service [last.fm](http://www.lastfm.fr).

Si vous souhaitez scrobbler la musique que vous écoutez avec MPD également vers l'alternative libre à last.fm, [Libre.fm](https://libre.fm/), il faut alors modifier à la main le fichier de configuration de mpdscribble qui devrait ressembler à cela&nbsp;:

```bash
$ sudo cat /etc/mpdscribble.conf
```

```
## mpdscribble - an audioscrobbler for the Music Player Daemon.
## http://mpd.wikia.com/wiki/Client:mpdscribble

# HTTP proxy URL.
#proxy = http://the.proxy.server:3128

# The location of the pid file.  mpdscribble saves its process id there.
#pidfile = /var/run/mpdscribble.pid

# Change to this system user after daemonization.
#daemon_user = mpdscribble

# The location of the mpdscribble log file.  The special value
# "syslog" makes mpdscribble use the local syslog daemon.  On most
# systems, log messages will appear in /var/log/daemon.log then.
# "-" means log to stderr (the current terminal).
log = syslog

# How verbose mpdscribble's logging should be.  Default is 1.
verbose = 1

# How often should mpdscribble save the journal file? [seconds]
#journal_interval = 600

# The host running MPD, possibly protected by a password
# ([PASSWORD@]HOSTNAME).  Defaults to $MPD_HOST or localhost.
#host = localhost

# The port that the MPD listens on and mpdscribble should try to
# connect to.  Defaults to $MPD_PORT or 6600.
#port = 6600

[last.fm]
url = http://post.audioscrobbler.com/
username = my_username
password = my_password
# The file where mpdscribble should store its Last.fm journal in case
# you do not have a connection to the Last.fm server.
journal = /var/cache/mpdscribble/lastfm.journal

#[libre.fm]
#url = http://turtle.libre.fm/
#username = my_username
#password = my_password
#journal = /var/cache/mpdscribble/librefm.journal

#[jamendo]
#url = http://postaudioscrobbler.jamendo.com/
#username = my_username
#password = my_password
#journal = /var/cache/mpdscribble/jamendo.journal

#[file]
#file = /var/log/mpdscribble/log
```

__ATTENTION__&nbsp;: il semblerait que mpdscribble ne parvienne pas à scrobbler vers les deux services si vos comptes ont le même identifiant et le même mot de passe.

Avant d'éditer le fichier `/etc/mpdscribble.conf`, il faut calculer le hash de votre mot de passe&nbsp;; pour ce faire, lancer la commande&nbsp;:

```bash
$ echo -n 'PASSWORD' | md5sum | cut -f 1 -d " "
```

Copiez le résultat puis lancez l'édition du fichier de conf de mpdscribble&nbsp;:

```bash
$ sudo nano /etc/mpdscribble.conf
```

et modifiez la section `libre.fm`&nbsp;: décommentez chacune des lignes de celle-ci et saisissez votre nom d'utilisateur et copiez le hash de votre mot de passe. Cette section devrait ressembler à cela&nbsp;:

```
[libre.fm]
url = http://turtle.libre.fm/
username = USER
password = 319f4d26e3c536b5dd871bb2c52e3178					#hash de PASSWORD
journal = /var/cache/mpdscribble/librefm.journal
```

Une fois enregistré, il vous faut relancer mpdscribble&nbsp;:

```bash
$ sudo service mpdscribble restart
```