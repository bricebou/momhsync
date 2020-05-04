---
title: 'mpdscribble : conserver les morceaux scrobblés dans un log'
date: '15-08-2014 00:00'
taxonomy:
    category:
        - Linux
    tag:
        - mpd
        - mpdscribble
        - lastfm
        - console
---

Par défaut, il semblerait que [mpdscribble](/blog/mpdscribble) ne conserve pas dans un log les morceaux scrobblés (sauf lorsque l'on n'a pas de connexion au service de scrobbling de LastFM).      
Il est évidemment possible de garder la trace localement de tout ce que MPD lit, simplement en modifiant le fichier de configuration `/etc/mpdscribble.conf`. Cependant, il faut bien garder à l'esprit qu'un tel fichier de log ne fait que grossir, posant d'éventuels problèmes sur le long terme. Ce n'est donc pas forcément une bonne idée...

===

Mais pour ce faire, il suffit donc de modifier le fichier de configuration `/etc/mpdscribble.conf`&nbsp;:

```shell
$ sudo nano /etc/mpdscribble.conf
```       
et de décommentez les dernières lignes du fichier, pour obtenir ceci&nbsp;:

```plain
[file]
file = /var/log/mpdscribble/log
```

Puis on crée le répertoire `/var/log/mpdscribble` et on lui donne pour propriétaire l'"utilisateur" `mpdscribble`&nbsp;:

```shell
$ sudo mkdir /var/log/mpdscribble && sudo chown mpdscribble /var/log/mpdscribble
```

On relance le service `mpdscribble`&nbsp;:

```shell
$ sudo service mpdscribble restart
```

Pour vérifier que tout fonctionne, on lance la lecture et à la fin de la lecture de la première piste, vous devriez la voir apparaître dans le log que l'on affiche ainsi&nbsp;:

```shell
$ tail -f /var/log/mpdscribble/log
```