---
title: 'Masquer des partitions dans votre gestionnaire de fichiers'
date: '15-10-2014 00:00'
taxonomy:
    category:
        - Linux
    tag:
        - personnalisation
        - partition
---

Sur mon netbook ASUS F200M, sous Xubuntu mais avec un dual-boot, certaines partitions «&nbsp;Windows&nbsp;» apparaissaient dans le volet de Thunar alors que je n'avais nul besoin de les monter.     
La situation est assez logiquement la même sous Debian Buster avec Caja, le gestionnaire de fichier de Mate.

La solution pour les faire disparaître consiste à créer de nouvelles règles udev ([documentation Ubuntu](http://doc.ubuntu-fr.org/udev) / [documentation Debian](https://wiki.debian.org/fr/udev)).

Pour ce faire, on commence par se rendre dans le bon répertoire&nbsp;:

```bash
$ cd /etc/udev/rules.d/
```

Puis, on créée un nouveau fichier&nbsp;:

```bash
$ sudo nano 99-hide-disks.rules
```

dans lequel nous allons indiquer les partitions à ignorer&nbsp;; pour les identifier, vous pouvez utiliser soit `blkid` soit `fdisk`&nbsp;:

```bash
$ sudo blkid
$ sudo fdisk -l
```

Ainsi, dans mon cas, voici à quoi ressemble le fichier `/etc/udev/rules.d/99-hide-disks.rules`&nbsp;:
```
KERNEL=="sda1", ENV{UDISKS_IGNORE}="1"
KERNEL=="sda2", ENV{UDISKS_IGNORE}="1"
KERNEL=="sda4", ENV{UDISKS_IGNORE}="1"
KERNEL=="sda6", ENV{UDISKS_IGNORE}="1"
```