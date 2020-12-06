---
title: 'Sublime Text via SSH'
date: '06-12-2020 11:56'
taxonomy:
    category:
        - Linux
    tag:
        - 'Sublime Text'
        - SSH
content:
    items:
        - '@self.children'
    limit: 5
    order:
        by: date
        dir: desc
    pagination: true
    url_taxonomy_filters: true
twitterenable: true
twittercardoptions: summary
facebookenable: true
---

Sur le client, on commence par installer via le gestionnaire de paquet le plugin [RemoteSubl](https://github.com/randy3k/RemoteSubl)&nbsp;: <kbd>Ctrl+Shift+p</kbd> > «&nbsp;Package Control: Install Package.&nbsp;».

Sur le serveur, il faut installer [`rmate`](https://github.com/aurora/rmate)&nbsp;:

```shell
curl -o /usr/local/bin/rmate https://raw.githubusercontent.com/aurora/rmate/master/rmate
sudo chmod +x /usr/local/bin/rmate
```

Depuis le client, il faut ensuite ouvrir une connection SSH avec un routage de port&nbsp;:

```shell
ssh -R 52698:localhost:52698 user@example.com
```

puis de lancer la commande&nbsp;:

```shell
rmate file.txt
```

et ce fichier doit s'ouvrir dans Sublime Text.