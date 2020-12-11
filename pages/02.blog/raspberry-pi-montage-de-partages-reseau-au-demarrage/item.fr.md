---
title: 'Raspberry Pi : montage de partages réseau au démarrage'
date: '11-12-2020 22:38'
taxonomy:
    category:
        - Linux
    tag:
        - 'Raspberry Pi'
        - réseau
twitterenable: true
twittercardoptions: summary
facebookenable: true
---

Sur mon Raspberry Pi sous Raspbian Buster, [mes partages cifs](/blog/partage-samba-cifs-sur-nas-d-link-dns-320) ou nfs n'étaient par montés au démarrage, malgré l'option `_netdev` dans mon `/etc/fstab` et bien que mon Raspberry soit connecté en Ethernet&nbsp;; je devais relancer la commande `sudo mount -a` après chaque démarrage pour accéder aux partages de mon NAS.     
La raison en est simplement, très schématiquement, que le démarrage se fait sans même attendre que les interfaces réseau soient opérationnelles, et donc le serveur Samba est inaccessible.

La solution est toute simple&nbsp;: faire en sorte que le Raspberry Pi attende d'avoir paramétré le réseau pour finaliser le boot.

Il suffit de lancer l'utilitaire `raspi-config`

```shell
sudo raspi-config
```

puis d'aller dans «&nbsp;1. System Options&nbsp;» > «&nbsp;S6. Network at Boot&nbsp;» et choisir l'option «&nbsp;Oui&nbsp;».