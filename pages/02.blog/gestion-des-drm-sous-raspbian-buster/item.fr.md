---
title: 'Gestion des DRM sous Raspbian Buster'
published: true
date: '29-09-2020 08:06'
taxonomy:
    category:
        - Linux
    tag:
        - 'Raspberry Pi'
twitterenable: true
twittercardoptions: summary
facebookenable: true
---

Le passage de mon Raspberry Pi dans sa vieillissante version 2 à une version 4 m'a permis d'étendre les usages que je fais de ce nano-ordinateur, et notamment de pouvoir regarder _myCanal_  &ndash; sans avoir à brancher un autre ordinateur sur la télé... &ndash; ou encore de basculer sur [le lecteur web de Spotify](https://open.spotify.com/) lorsque ma discothèque personnelle ne me suffit plus.       
Mais ces plateformes, comme tant d'autres, protègent leurs contenus avec des DRM qui ne sont pas pris en charge sur l'architecture ARM...

===

La solution la plus simple à mettre en œuvre, sous réserve que vous ayiez opté pour une version 32 bit de Raspbian, c'est encore de délaisser Chromium pour [Vivaldi](https://vivaldi.com/fr/) et de suivre [leurs astuces](https://help.vivaldi.com/fr/article/raspberry-pi-astuces-pour-utiliser-vivaldi/) pour une utilisation sur le Raspberry Pi, qui renvoie notamment à [ce script](https://gist.github.com/ruario/19a28d98d29d34ec9b184c42e5f8bf29).

On commence par récupérer le contenu du script&nbsp;:

```bash
$ wget https://gist.githubusercontent.com/ruario/19a28d98d29d34ec9b184c42e5f8bf29/raw/6ff95fa30a291319700b5a75dd558038d3e202c5/widevine-flash_armhf.sh
```

On le rend exécutable et on le lance&nbsp;:

```bash
$ chmod +x widevine-flash_armhf.sh && ./widevine-flash_armhf.sh
```

avant de suivre les instructions fournies&nbsp;:

```bash
$ sudo tar Cfx / widevine-flash-20200124_armhf.tgz
$ mkdir -p ~/.config/vivaldi{,-snapshot}/WidevineCdm
$ echo '{"Path":"/opt/WidevineCdm"}' | tee ~/.config/vivaldi/WidevineCdm/latest-component-updated-widevine-cdm > ~/.config/vivaldi-snapshot/WidevineCdm/latest-component-updated-widevine-cdm
```


