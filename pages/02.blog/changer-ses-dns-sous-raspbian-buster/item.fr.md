---
title: 'Changer ses DNS sous Raspbian Buster'
date: '23-03-2020 00:00'
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

Afin de pouvoir accéder à certains sites tels [EZTV](https://eztv.io/) ou [YTS / YIFY](https://yts.mx/) &ndash; que mon opérateur bloque &ndash;, il convient de passer par des serveurs DNS qui ne soient pas «&nbsp;menteurs&nbsp;»... On peut par exemple utiliser les serveurs bien connus de Google, mais on peut aussi privilégier ceux proposés par FDN (French Data Network), un fournisseur d'accès à Internet associatif sur [cette page](https://www.fdn.fr/actions/dns/).

Sous Raspbian Buster, il faut éditer le fichier `/etc/dhcpcd.conf`&nbsp;:

```bash
sudo nano /etc/dhcpcd.conf
```

et décommenter la ligne 

```
#static domain_name_servers=192.168.0.1 8.8.8.8 fd51:42f8:caae:d92e::1
```

et la remplacer par

```
static domain_name_servers=80.67.169.12 80.67.169.40 2001:910:800::12 2001:910:800::40
```

Une fois le fichier enregistré, il faut relancer le service `dhcpcd`

```bash
sudo service dhcpcd restart
```

Pour vérifier que nos modifications ont bien été prises en compte, on peut regarder les serveurs affichés dans le fichier `/etc/resolv.conf`&nbsp;:

```bash
cat /etc/resolv.conf
```