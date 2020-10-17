---
title: 'Monter son Google Drive sous Debian Testing'
date: '17-10-2020 09:17'
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

Puis on lance la commande&nbsp;:

```shell
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
