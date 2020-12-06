---
title: 'Station météo Netatmo & WeeWX [bis]'
date: '05-11-2020 08:18'
taxonomy:
    category:
        - Linux
    tag:
        - 'Raspberry Pi'
        - météo
        - WeeWX
twitterenable: true
twittercardoptions: summary
facebookenable: true
content:
    items: '@self.children'
    limit: '5'
    order:
        by: date
        dir: desc
    pagination: '1'
    url_taxonomy_filters: '1'
---

Suite à [mon premier post sur le sujet](/blog/station-meteo-netatmo-and-weewx), je me suis attelé plus longuement à la compatibilité du [plugin _weewx-netatmo_ de Matthew Wall](https://github.com/matthewwall/weewx-netatmo) avec les versions 4 de WeeWX sous Python 3.

## Installation

De ce fait, on n'a plus besoin d'installer une ancienne version de WeeWX et l'on peut se reporter sur la version disponible dans les dépôts&nbsp;:

```shell
sudo apt install weewx
```

Désormais, la configuration et autres skins ne se trouvent plus dans `/home/weewx` mais dans `/etc/weewx`. L'installation du plugin _weewx-netatmo_ peut se faire depuis [mon fork du plugin](https://github.com/bricebou/weewx-netatmo)&nbsp;:

```shell
wget -O weewx-netatmo.zip https://github.com/bricebou/weewx-netatmo/archive/master.zip
sudo wee_extension --install weewx-netatmo.zip
sudo wee_config --reconfigure
```
On ouvre ensuite le fichier de configuration `/etc/weewx/weewx.conf` pour vérifier que nos identifiants Netatmo soient bien pris en compte. Ensuite, on redémarre WeeWX&nbsp;:

```shell
sudo service weewx restart
```


## Modifications apportées

Pour ce faire, on utilise l'outil `2to3` :

```shell
sudo pip3 install 2to3
```

puis on lance la commande :

```shell
2to3 -w netatmo.py
```

Une erreur subsistait mais le problème avait déjà été résolu par [kwalker05](https://github.com/matthewwall/weewx-netatmo/issues/15#issuecomment-628994957)&nbsp;; il suffit, à la ligne 521, de remplacer&nbsp;:

```python
params = urlencode(params)
```

par 

```python
params = urlencode(params).encode("utf-8")
```