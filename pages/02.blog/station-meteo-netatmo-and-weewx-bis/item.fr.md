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
content:
    items: '@self.children'
    limit: '5'
    order:
        by: date
        dir: desc
    pagination: '1'
    url_taxonomy_filters: '1'
twitterenable: true
twittercardoptions: summary
facebookenable: true
---

Suite à [mon premier post sur le sujet](/blog/station-meteo-netatmo-and-weewx), je me suis attelé plus longuement à la compatibilité du [plugin weewx-netatmo de Matthew Wall](https://github.com/matthewwall/weewx-netatmo) avec les versions 4 de WeeWX, sous Python 3.

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

L'installation de WeeWX