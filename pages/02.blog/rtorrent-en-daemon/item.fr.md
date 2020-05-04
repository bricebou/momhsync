---
title: 'rTorrent en daemon'
date: '16-10-2014 00:00'
taxonomy:
    category:
        - Linux
    tag:
        - NAS
        - fun_plug
        - torrent
---

Pour mes téléchargements, notamment de séries, j'utilise le logiciel [rTorrent](http://rakshasa.github.io/rtorrent/) installé sur mon NAS (un D-Link DNS-320 avec [fun_plug](http://nas-tweaks.net/371/hdd-installation-of-the-fun_plug-0-7-on-nas-devices/) par-dessus).    
Cependant il convient de pouvoir le laisser tourner en tâche de fond, d'autant plus que je récupère les torrents des derniers épisodes de séries avec [FlexGet](http://flexget.com/).

## Configuration de rTorrent

Il s'agit ici de présenter simplement les éléments de configuration nécessaires au téléchargement des nouveaux torrents, au déplacement des fichiers vers un dossier surveillé par mon serveur media [MediaTomb (désormais Gerbera)](https://gerbera.io)...

### Surveillance des nouveaux fichiers torrent

```plain
# Watch a directory for new torrents, and stop those that have been deleted.
schedule = watch_directory,5,5,load_start=/mnt/HD/HD_b2/torrent/*.torrent
schedule = untied_directory,5,5,stop_untied=
```

### Déplacement des fichiers téléchargés

```plain
# Répertoire pour téléchargement
directory = /mnt/HD/HD_b2/torrent/incomplete/

# On déplace les fichiers dont le téléchargement est fini
system.method.set_key = event.download.finished,move_complete,"execute=mv,-u,$d.get_base_path=,/mnt/HD/HD_b2/torrent/complete/;d.set_directory=/mnt/HD/HD_b2/torrent/complete/"
```

## Daemon rTorrent

Il s'agit de pouvoir lancer rTorrent "encapsulé" dans un "multiplexeur de terminaux" comme `screen`&nbsp;; le script est basé sur celui proposé dans [la documentation d'ubuntu-fr](http://doc.ubuntu-fr.org/rtorrent#rtorrent_en_daemon) et [celui-ci](http://utbm.gesc.free.fr/Wiki/doku.php?id=informatique:linux:seedbox#lancer_rtorrent_dans_un_screen). Cependant, j'ai décidé de ne pas lancer rTorrent au démarrage du NAS mais à la demande.

J'ai donc créé le fichier `/bin/drtorrent`&nbsp;:

```bash
$ sudo nano /bin/drtorrent
```

avec ce script&nbsp;:
```bash
#!/ffp/bin/sh
# Start/Stop rtorrent sous forme de daemon.

NAME=drtorrent
SCRIPTNAME=/ffp/bin/$NAME
PATH=/ffp/bin:/ffp/sbin:/usr/sbin:/usr/bin:/bin:/opt/bin:/opt/sbin

case $1 in
        start)
                echo "Starting rtorrent... "
                /ffp/bin/screen -fn -dmS rtd /opt/bin/rtorrent
                echo "Terminated"
        ;;
        stop)
                if [ "$(ps aux | grep -e '.*rtorrent$' -c)" != 0  ]; then
                {
                        echo "Shutting down rtorrent... "
                        killall -r "^.*rtorrent$"
                        echo "Terminated"
                }
                else
                {
                        echo "rtorrent not yet started !"
                        echo "Terminated"
                }
                fi
        ;;
        restart)
                if [ "$(ps aux | grep -e '.*rtorrent$' -c)" != 0  ]; then
                {
                        echo "Shutting down rtorrent... "
                        killall -r "^.*rtorrent$"
                        echo "Starting rtorrent... "
            /ffp/bin/screen -fn -dmS rtd /opt/bin/rtorrent
                        echo "Terminated"
                }
                else
                {
                        echo "rtorrent not yet started !"
                        echo "Starting rtorrent... "
            /ffp/bin/screen -fn -dmS rtd /opt/bin/rtorrent
                        echo "Terminated"
                }
                fi
        ;;
        *)
                echo "Usage: $SCRIPTNAME {start|stop|restart}" >&2
                exit 2
        ;;
esac
```

__Attention&nbsp;:__ il vous faudra sans doute modifier quelques lignes, notamment pour ce qui est des chemins vers les exécutables (remplacer `/ffp/bin/` par `/bin/` par exemple).

Une fois enregistré, il doit falloir le rendre exécutable&nbsp;:
```bash
sudo chmod +x /bin/drtorrent
```

## Usage

Pour lancer rTorrent en background, il suffit de lancer la commande&nbsp;:
```bash
$ drtorrent start
```

Vous devriez avoir ce message&nbsp;:
```bash
Starting rtorrent... 
Terminated
```

Pour suivre nos téléchargement, il faut alors ouvrir le `screen` dans lequel le script a lancé rTorrent&nbsp;:
```bash
$ screen -x
```

Si vous avez plusieurs sessions screen, il convient de préciser le nom de la session utilisée par le script, `rtd`&nbsp;:
```bash
$ screen -x rtd
```

[En savoir plus sur screen sur [la documentation d'ubuntu-fr](http://doc.ubuntu-fr.org/screen)].

Pour "quitter" cet écran, ou plutôt pour le "détacher", il faut utiliser la combinaison de touches `Ctrl+a d`. Pour véritablement quitter, c'est-à-dire arrêter rTorrent, on peut utiliser la combinaison `Ctrl+q` ou après l'avoir détaché, lancer la commande&nbsp;:
```bash
$ drtorrent stop
```