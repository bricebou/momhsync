---
title: 'Téléchargez les torrents de YIFY / YTS en ligne de commande'
date: '06-04-2020 00:00'
taxonomy:
    category:
        - Linux
    tag:
        - torrent
        - console
        - 'Raspberry Pi'
twitterenable: true
twittercardoptions: summary
facebookenable: true
---

Grâce à un petit utilitaire en Python, [yifi, initialement écrit par vitamin c / akhilmaskerie](https://github.com/akhilmaskeri/yifi), il est possible de découvrir les dernières releases proposées par YIFY / YTS, de faire des recherches et surtout de télécharger les fichiers .torrent ou .magnet pour les télécharger ensuite avec votre client Bittorent préféré (dans mon cas désormais, transmission-daemon tourne sur mon NAS en remplacement de rtorrent).

J'ai [forké](https://github.com/bricebou/yify) son dépôt afin de faire un peu de ménage, de renommer l'ensemble de yifi en yify et surtout de permettre de retrouver l'identifiant IMDB des films, afin de faciliter [la recherche des sous-titres avec le module NodeJS _getsubtitle_](/blog/telechargez-vos-sous-titres-en-ligne-de-commande#getsubtitle).

Pour l'installer&nbsp;:

```bash
sudo pip3 install git+https://github.com/bricebou/yify
```

Nous avons désormais accès à la commande `yify` qui renvoie les dernières sorties&nbsp;:

```bash
$ yify
16500 Ordinary Love
16499 Jimmy P.
16497 American Masters Jimi Hendrix: Hear My Train a Comin'
16496 Fantastic Return to Oz
16495 Batman: Return of the Caped Crusaders
16494 Cardcaptor Sakura: The Movie
16492 Detroit Driller Killer
16490 Let's Kill Grandpa This Christmas
16489 All the Queen's Horses
16488 Lone Wolf and Cub: Baby Cart to Hades
16487 Depth Charge
16486 Mysterious Island
16484 That's the Way of the World
16483 Timerider: The Adventure of Lyle Swann
16482 Shackleton's Captain
16481 Seven Stages to Achieve Eternal Bliss
16479 Ghost Town
16478 The Winner
16476 DWB: Dating While Black
16475 Touched by Grace
```

Pour avoir plus d'informations sur un film&nbsp;:

```bash
$ yify -id 16497 --details
title  &nbsp;: American Masters Jimi Hendrix: Hear My Train a Comin' (2013)
year   &nbsp;: 2013
rating &nbsp;: 7.7
runtime&nbsp;: 0 min
lang   &nbsp;: English
genres &nbsp;: Biography,Documentary
mpa    &nbsp;: 
quality&nbsp;: ['720p']
descr  &nbsp;:  Jimi Hendrix was the greatest single rock star of it's era and
      remains so today. No one before or since has come [... snap ...]
imdb_id&nbsp;: tt3181314
```

Pour télécharger le torrent ou le magnet (lien copié dans un fichier)&nbsp;:

```bash
$ yify -id 16497 --torrent
$ yify -id 16497 --magnet
```

Ensuite, si vous utiliser transmission-daemon accompagné de transmission-remote, il vous suffit de fournir à ce dernier le fichier .torrent&nbsp;:

```bash
$ transmission-remote 192.168.1.21:8091 -a 16497.torrent
```

ou le contenu du fichier .magnet&nbsp;:

```bash
$ transmission-remote 192.168.1.21:8091 -a `cat 16497.magnet`
```

ou&nbsp;:

```bash
$ cat 16497.magnet | xargs transmission-remote 192.168.1.21:8091 -a 
```