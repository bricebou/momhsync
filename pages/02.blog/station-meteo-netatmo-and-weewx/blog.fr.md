---
title: 'Station météo Netatmo & WeeWX'
date: '03-09-2020 09:16'
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

Dans le cadre de la "digitalisation" de [_La Sculpture : les Pluies_](http://patrickdubrac.fr/-La-Sculpture-les-Pluies-) de Patrick Dubrac, nous cherchons à aller au-delà de ce que nous avons mis en place avec _Le Calendrier des pluies_, mis à jour mensuellement à partir des données météorologiques quotidiennes. D'où l'idée de développer 

Le pilote Netatmo pour WeeWX ([https://github.com/matthewwall/weewx-netatmo](https://github.com/matthewwall/weewx-netatmo)) n'est pas compatible avec la version 4 de WeeWX...
Donc on se reporte sur la dernière des versions 3 de WeeWX disponibles, la 3.9.2&nbsp;:

```bash
$ wget http://weewx.com/downloads/released_versions/weewx-3.9.2.tar.gz
```

On installe les dépendances nécessaires :

```bash
$ sudo apt install python-configobj python-pil python-serial python-usb python-pip python-cheetah python-ephem mariadb-client python-mysqldb
```

Puis on lance l'installation :

```bash
$ tar xvfz weewx-3.9.2.tar.gz
$ cd weewx-3.9.2
$ python2 setup.py build
$ sudo python setup.py install
```

L'ensemble des exécutables se trouve dans `/home/weewx/bin/` ; le fichier de configuration est à l'emplacement `/home/weewx/weewx.conf`


On installe ensuite le pilote weewx-netatmo (https://github.com/matthewwall/weewx-netatmo)&nbsp;:

```bash
$ wget -O weewx-netatmo.zip https://github.com/matthewwall/weewx-netatmo/archive/master.zip
$ sudo /home/weewx/bin/wee_extension --install weewx-netatmo.zip
```

Puis on configure weewx :

```bash
$ sudo /home/weewx/bin/wee_config --reconfigure
```

On édite ensuite le fichier de configuration à la main &nbsp;:

```bash
$ sudo nano /home/weewx/weewx.conf
```

On s'assure que nos identifiants Netatmo sont bien pris en compte et on bascule, dans la sous-section `[[wx_binding]]` de la section `[DataBindings]` sur une base de données MySQL&nbsp;:

```
database = archive_mysql
```

Enfin, on fait en sorte que WeeWX soit lancé au démarrage comme un service :

```bash
$ cd /home/weewx
$ sudo cp util/init.d/weewx.debian /etc/init.d/weewx
$ sudo chmod +x /etc/init.d/weewx
$ sudo update-rc.d weewx defaults 98
$ sudo /etc/init.d/weewx start
```