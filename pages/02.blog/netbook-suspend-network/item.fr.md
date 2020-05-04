---
title: 'Réseau filaire ou Wifi au retour de veille sur Xubuntu 15.04'
date: '11-05-2015 00:00'
taxonomy:
    category:
        - Linux
    tag:
        - 'ASUS F200M'
        - wifi
        - réseau
twitterenable: true
twittercardoptions: summary
facebookenable: true
---

Sous Xubuntu 15.04, si j'utilise le raccourci clavier `Fn+F1` pour mettre en veille mon netbook ASUS F200M, je rencontre un problème à la sortie de veille&nbsp;: le réseau ne fonctionne plus, bien que les connexions soient bien actives dans le NetworkManager...

Pour retrouver une connexion en état de fonctionnement, qu'elle soit filaire ou via Wifi, il suffit de saisir ces trois commandes&nbsp;:

```bash
$ sudo service NetworkManager stop
$ sudo rm /var/lib/NetworkManager/NetworkManager.state
$ sudo service NetworkManager start
```

On commence par stopper le service NetworkManager, on efface le fichier dans lequel est stocké le statut des connexion, et enfin on relance le NetworkManager.