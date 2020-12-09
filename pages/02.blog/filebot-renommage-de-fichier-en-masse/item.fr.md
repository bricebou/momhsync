---
title: 'Renommage de films en masse'
date: '01-10-2020 15:02'
content:
    items: '- ''@self.children'''
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

L'idée n'est pas ici de présenter [Filebot](https://www.filebot.net/), outil entre autres de renommage en masse pour tout ce qui est films et séries, notamment, mais bien de garder une trace des « patterns » qui me sont utiles.

Ainsi, pour renommer mes films, j'utilise l'expression suivante&nbsp;:

```
{director} - {y} - {if (localize.fr.n.lower() == primaryTitle.lower()) "$primaryTitle" else "$primaryTitle ($localize.fr.n)"} [{hd}]
```

qui donne ce type de nommage&nbsp;:

```
Jean Renoir - 1937 - La Grande Illusion [HD].mkv
Jean Renoir - 1938 - La Bête humaine [SD].avi
Jim Jarmusch - 1999 - Ghost Dog The Way of the Samurai (Ghost Dog, la voie du samouraï) [HD].mkv
Jim Jarmusch - 2003 - Coffee and Cigarettes [HD].mp4
```

Si l'on ne souhaite obtenir une liste que des titres de films, on peut alors utiliser l'une de ces commandes&nbsp;:

```shell
ls | sed 's/[ ]-[ ]/\t/g' | cut -f3
ls | tr "-" "\t" | cut -f3
```