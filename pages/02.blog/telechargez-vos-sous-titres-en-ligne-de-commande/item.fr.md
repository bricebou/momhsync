---
title: 'Téléchargez vos sous-titres en ligne de commande'
date: '06-04-2020 00:00'
taxonomy:
    category:
        - Linux
    tag:
        - console
        - 'Raspberry Pi'
        - sous-titres
twitterenable: true
twittercardoptions: summary
facebookenable: true
---

## addic7ed-cli

Des deux alternatives repérées en 2014 pour télécharger ses sous-titres depuis le site [addic7ed.com](http://www.addic7ed.com/), celle de Michael Baudino ([addic7ed-ruby](https://github.com/michaelbaudino/addic7ed-ruby)) n'est plus maintenue.      
Celle qui avait notre préférence, addic7ed-cli de Benoit Zugmeyer, disponible sur [GitHub](https://github.com/BenoitZugmeyer/addic7ed-cli), reste quant à elle un tant soit peu maintenue. 

Pour installer [addic7ed-cli](https://github.com/BenoitZugmeyer/addic7ed-cli), le plus simple reste de commencer par installer l'utilitaire Pip ainsi que la librairie _libxslt-dev_&nbsp;:

```bash
$ sudo apt install python3-pip libxslt-dev
```

Une fois cela fait, lancez la commande&nbsp;:

```bash
$ sudo pip3 install https://github.com/BenoitZugmeyer/addic7ed-cli/archive/master.zip
```

Vous pouvez dès lors utilisez cet utilitaire avec la commande `addic7ed`&nbsp;:

```bash
$ addic7ed -l french The.Walking.Dead.S05E01.720p.HDTV.x264-KILLERS.mkv
```

Pour en savoir plus&nbsp;:

```bash
$ addic7ed --help
```

## subdl

Pour télécharger des sous-titres depuis [opensubtitles.org](http://www.opensubtitles.org), le script proposé par akexakex et disponible sur [GitHub](https://github.com/akexakex/subdl) est toujours aussi simple, efficace et rapide.

Pour l'installer&nbsp;:

```bash
$ sudo pip3 install git+https://github.com/alexanderwink/subdl
```

La commande `subdl` est dès lors disponible&nbsp;:
```bash
$ subdl -h
```

Personnellement, je me suis créé un alias Bash avec identification auprès d'opensubtitles.org et l'argument de la langue notamment&nbsp;:

```bash
$ nano ~/.bash_alias
```

```bash
alias sos='subdl --username USERNAME --password "PASSWORD" --interactive --lang=fre'
```

## Subliminal

Très complet, mais aussi plus lourd et plus lent &ndash; du moins sur mon Raspberry Pi 2... &ndash; [Subliminal](https://github.com/Diaoul/subliminal)  permet de chercher des sous-titres depuis de nombreux sites, dont [addic7ed.com](http://www.addic7ed.com/) et [opensubtitles.org](http://www.opensubtitles.org).

Pour l'installer&nbsp;:

```bash
sudo pip3 install subliminal
```

<!--
Par contre, cette version est désormais particulièrement ancienne&nbsp;; en cas de besoin, on peut se reporter sur la version de développement (non testée)&nbsp;:

```bash
$ sudo pip3 install git+https://github.com/Diaoul/subliminal@develop
```
-->

Pour télécharger des sous-titres&nbsp;:

```bash
subliminal download -l fr The.Big.Bang.Theory.S05E18.HDTV.x264-LOL.mp4
```

L'on peut aussi spécifier les "providers" auxquels on souhaite limiter la recherche, ainsi que l'encodage du fichier .srt ainsi que son format (avec ou sans suffixe de langue &ndash; sans, dans mon cas, sinon omxplayer ne prend pas en compte le sous-titre).      
Je me suis donc créé un alias afin de m'en faciliter l'usage&nbsp;:

```bash
$ nano ~/.bash_alias
```

```bash
alias psubliminal='subliminal download -p addic7ed -p opensubtitles -l fr -e UTF-8 -s'
```

## getsubtitle {#getsubtitle}

Si comme moi vous êtes un utilisateur régulier de [YIFY / YTS](https://yts.mx/) &ndash;&nbsp;dont l'accès est rendu possible [en changeant ses serveurs DNS](/blog/changer-ses-dns-sous-raspbian-buster)&nbsp;&ndash;, et dont [on peut télécharger les fichiers _.torrent_ en ligne de commande](/blog/yify_yts_command_line), vous pourriez vouloir récupérer les sous-titres depuis le site [yifysubtitles.com](https://www.yifysubtitles.com/).

Pour cela, on peut utiliser le module pour NodeJS [_getsubtitle_](https://www.npmjs.com/package/getsubtitle)&nbsp;:

```bash
$ sudo npm install -g getsubtitle
```

L'idéal dans son utilisation est de lui fournir l'identifiant IMDB du film dont on recherche les sous-titres, par exemple&nbsp;:

```bash
$ getsubtitle tt0054756
```

Pour connaître cet identifiant IMDB, il convient d'utiliser [le petit utilitaire yify](/blog/yify_yts_command_line), que j'ai forké justement pour récupérer cette donnée.