---
title: 'Monter son Google Drive sous Debian Testing'
media_order: google-drive-ocamlfuse-startup.png
date: '17-10-2020 09:17'
taxonomy:
    category:
        - Linux
twitterenable: true
twittercardoptions: summary
facebookenable: true
---

Pour accéder à ses documents stockés sur Google Drive depuis votre explorateur de fichiers préféré sous Debian, le mieux semble être d'utiliser [google-drive-ocamlfuse](https://github.com/astrada/google-drive-ocamlfuse) d'Alessandro Strada.

## Installation

On se réfère au [wiki](https://github.com/astrada/google-drive-ocamlfuse/wiki/Installation) mais on adapte quelque peu...

On crée le fichier&nbsp;:
```shell
sudo nano /etc/apt/sources.list.d/alessandro-strada-ubuntu-ppa.list
```
au sein duquel on ajoute ces lignes&nbsp;:
```
deb http://ppa.launchpad.net/alessandro-strada/ppa/ubuntu focal main 
deb-src http://ppa.launchpad.net/alessandro-strada/ppa/ubuntu focal main 
```

(Sous Debian Buster, il est nécessaire de basculer sur les dépôts de la précédentes version LTS d'Ubuntu et donc de remplacer `focal` par `bionic`.)

Puis on récupère la signature du dépôt avant de lancer l'installation&nbsp;:

```shell
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys AD5F235DF639B041
sudo apt update && sudo apt install -y google-drive-ocamlfuse
```

## Lancement

Le premier lancement se fait sans argument afin de générer les autorisations d'accès&nbsp;:

```shell
google-drive-ocamlfuse
```

Ensuite, on crée le répertoire où sera monté notre Google Drive et on passe ce dernier en argument à la commande précédente&nbsp;:

```shell
mkdir ~/GoogleDrive
google-drive-ocamlfuse ~/GoogleDrive
```

## Montage à l'ouverture d'une session graphique

Sous Mate, il faut ouvrir le Centre de contrôle depuis le menu `Système` (ou via la commande `mate-control-center`) puis accéder à «&nbsp;Applications au démarrage&nbsp;» et créer une nouvelle entrée avec la commande suivante&nbsp;:

```shell
sh -c "google-drive-ocamlfuse ~/GoogleDrive"
```

![](google-drive-ocamlfuse-startup.png)
