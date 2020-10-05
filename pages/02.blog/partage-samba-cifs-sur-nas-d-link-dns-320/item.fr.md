---
title: 'Partage Samba / CIFS depuis NAS D-Link DNS-320'
date: '02-05-2020 09:36'
taxonomy:
    category:
        - Linux
    tag:
        - 'Raspberry Pi'
        - NAS
        - réseau
twitterenable: true
twittercardoptions: summary
facebookenable: true
recaptchacontact:
    enabled: false
---

Un déménagement m'ayant amené à réinstaller l'ensemble de mon «&nbsp;système multimédia&nbsp;» constitué d'un NAS et d'un Raspberry Pi, je me suis trouvé une nouvelle fois à avoir le plus grand mal à mettre en place le partage des partitions de mon NAS vers l'ensemble de mes terminaux... et impossible de réutiliser de manière satisfaisante le montage NFS et ses options que j'utilisais jusqu'alors.      
Je me suis rabattu alors vers une solution de partage unique via le protocole CIFS proposé par défaut par le NAS.

On commence par créer un nouvel utilisateur dans la section _Management > Account Management_ à qui l'on donne les droits de lecture et d'écriture si les partages ont déjà été défini dans _Management > Network Shares_&nbsp;; sinon, les permissions seront définies lors de la création des partages.

Ensuite, on crée, sur les clients un fichier `~/.nascredentials` qui va contenir les identifiants de connexion de cet utilisateur&nbsp;:

```plain
username=USER
password=PASSWORD
```
On fait en sorte que seul le propriétaire de ce fichier puisse le lire et l'écrire&nbsp;:

```shell
chmod 600 ~/.nascredentials
```

On crée des points de montage pour les partages du NAS&nbsp;:

```shell
mkdir ~/NAS ~/NAS_DOWN
```

Enfin, on édite le fichier `/etc/fstab` (en spécifiant bien l'emplacement du fichier `.nascredentials`)&nbsp;:

```plain
//192.168.1.42/Contents /home/bbrice/NAS 		cifs 	_netdev,vers=1.0,credentials=/home/bbrice/.nascredentials,rw,iocharset=utf8 0 0
//192.168.1.42/torrent /home/bbrice/NAS_DOWN 	cifs 	_netdev,vers=1.0,credentials=/home/bbrice/.nascredentials,rw,iocharset=utf8 0 0
```

Puis on monte nos partitions&nbsp;:

```shell
sudo mount -a
```

Par contre, il est sans doute nécessaire d'installer le paquet `cifs-utils`&nbsp;:

```shell
sudo apt install cifs-utils
```