---
title: 'Transmission sur NAS D-Link DNS-320 avec fun_plug'
date: '24-04-2020 16:23'
taxonomy:
    category:
        - Linux
    tag:
        - NAS
        - fun_plug
        - torrent
twitterenable: true
twittercardoptions: summary
facebookenable: true
recaptchacontact:
    enabled: false
---

Si je gère toujours mes téléchargements de torrents sur mon NAS «&nbsp;augmenté&nbsp;» de [fun_plug](https://nas-tweaks.net/371/hdd-installation-of-the-fun_plug-0-7-on-nas-devices/), je n'utilise plus [rTorrent en daemon](/blog/rtorrent-en-daemon) mais Transmission. Plusieurs raisons à ce changement&nbsp;: la plus importantes étant la non disponibilité de certaines dépendances du fait de la disparition de plusieurs dépôts pour fun_plug&nbsp;; et puis, rTorrent présentait certaines limitations, notamment l'absence de file d'attente...

## Installation

L'installation de Transmission ne peut se faire par le biais de _slacker_, l'outil de gestion de paquets de fun_plug, encore une fois pour des questions de dépendances non satisfaites...     
Il faut commencer par [installer _Optware_](https://nas-tweaks.net/219/installation-of-optware-on-the-d-link-dns-320-dns-325-dns-343-and-conceptronic-ch3mnas/), qui fournit de nombreux logiciels&nbsp;:

```shell
wget http://wolf-u.li/u/233 -O /ffp/start/optware.sh
chmod a+x /ffp/start/optware.sh
/ffp/start/optware.sh start
```

L'on dispose alors de la commande `ipkg`&nbsp;

```shell
ipkg update
ipkg list
ipkg list transmission
ipkg install transmission
```

## Préparation

On commence par créer les répertoires de téléchargement et de configuration&nbp;:

```shell
mkdir -p /mnt/HD/HD_b2/torrent/{.transmission,complete,incomplete}
```

Ensuite, pour bénéficier du script de lancement de transmission-daemon, nous installons puis désinstallons dans la foulée via slacker le paquet transmission&nbsp;:

```shell
slacker -a transmission
slacker -r transmission
```

Puis on édite le fichier `/ffp/start/transmission.sh` en modifiant les chemins des exécutables (qui se trouvent dans `/opt/bin`) et des fichiers de configuration de Transmission&nbsp;:

```shell
nano /ffp/start/transmission.sh 
```

Voici mon script&nbsp;:
```bash
#!/ffp/bin/sh

# PROVIDE: Transmission

. /ffp/etc/ffp.subr

TRANSMISSION_HOME=/mnt/HD/HD_b2/torrent/.transmission

name="transmission-daemon"
command="/opt/bin/$name"
start_cmd="transmission_start"
stop_cmd="transmission_stop"
status_cmd="transmission_status"
user=root
su_cmd="/ffp/bin/su"

transmission_start()
{
   if [ ! -d ${TRANSMISSION_HOME} ]; then
      $su_cmd $user -c "mkdir ${TRANSMISSION_HOME}"
   fi
   echo "Starting $name"
      $su_cmd $user -c "$command -g ${TRANSMISSION_HOME} -e ${TRANSMISSION_HOME}/$name.log"
}

transmission_stop()
{
   echo "Stopping $name"
      /ffp/bin/killall -SIGINT $name
}

transmission_status()
{
   _pids=$(pidof $name)
   if test -n "$_pids"; then
      echo "$name is running, pid:"
      pidof $name
   else
      echo "$name not running"
   fi
}

run_rc_command "$1"
```

On le rend exécutable&nbsp;:

```shell
chmod a+x /ffp/start/transmission.sh
```

## Premier lancement

Afin de générer le fichier de configuration, il convient de lancer une première fois `transmission-daemon` avec les arguments suivants&nbsp;:

- `-g`&nbsp;: le chemin vers le répertoire de configuration et autres fichiers générés par Transmission&nbsp;;
- `-w`&nbsp;: le répertoire où seront stockés les fichiers téléchargés&nbsp;;
- `-T`&nbsp;: je n'ai pas besoin d'authentification puisque je me limite à mon réseau local (option `-a`).

```shell
/opt/bin/transmission-daemon -f -g /mnt/HD/HD_b2/torrent/.transmission -w /mnt/HD/HD_b2/torrent/complete -T -a 127.0.0.1,192.168.1.*
```
On peut interrompre le daemon (Ctrl+C) puis éditer le fichier de configuration qui a été créé&nbsp;:

```shell
nano /mnt/HD/HD_b2/torrent/.transmission/settings.json 
```

Ensuite, on peut lancer Transmission simplement avec&nbsp;:

```shell
/ffp/start/transmission.sh
```

**Références&nbsp;:**
- [_[DNS-323] Tutoriel Installation Transmission_ par Sil51](http://www.bernaerts-nicolas.fr/nas/71-dns325-ffp07/226-dns325-ffp7-transmission)
- [_DNS 325 - Funplug 0.7 : Install Transmission p2p client_ par Nicolas Bernaerts](http://www.sil51.com/informatique/dns-323/7-dns-323-tutoriel-installation-transmission.html)