---
title: 'Station météo Netatmo & WeeWX'
date: '03-09-2020 09:16'
taxonomy:
    category:
        - Linux
    tag:
        - 'Raspberry Pi'
        - météo
twitterenable: true
twittercardoptions: summary
facebookenable: true
content:
    items: '- ''@self.children'''
    limit: '5'
    order:
        by: date
        dir: desc
    pagination: '1'
    url_taxonomy_filters: '1'
---

Dans le cadre de la «digitalisation» de [_La Sculpture : les Pluies_](http://patrickdubrac.fr/-La-Sculpture-les-Pluies-) de Patrick Dubrac, nous cherchons à aller au-delà de ce que nous avons mis en place avec _Le Calendrier des pluies_, mis à jour mensuellement à partir des données météorologiques quotidiennes. D'où l'idée de développer un prototype associant [une station météo Netatmo](https://www.netatmo.com/fr-fr/weather/weatherstation) à un Raspberry Pi sur lequel serait installé [WeeWX](http://weewx.com/), un petit programme en Python qui permet d'interagir avec de multiples modèles de stations météo, de publier les données sur de multiples sites, de conserver les données dans des bases de données...

Cependant, le pilote pour les stations Netatmo ([https://github.com/matthewwall/weewx-netatmo](https://github.com/matthewwall/weewx-netatmo)) n'est pas compatible avec la version 4 de WeeWX... On bascule donc sur la dernière des versions 3 de WeeWX disponibles, la 3.9.2&nbsp;:

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

On crée une base de données avec les mêmes identifiants que ceux que nous avons indiqués dans la sous-section `[[archive_mysql]]` de la secion `[Databases]`, par défaut&nbsp;:

```
    # MySQL
    [[archive_mysql]]
        database_name = weewx
        database_type = MySQL
```

```bash
$ sudo mysql -u root -p
```

```sql
MariaDB [(none)]> CREATE DATABASE weewx;
MariaDB [(none)]> CREATE USER 'weewx'@localhost IDENTIFIED BY 'weewx';
MariaDB [(none)]> GRANT ALL PRIVILEGES ON weewx.* TO 'weewx'@localhost;
MariaDB [(none)]> FLUSH PRIVILEGES;
```

Enfin, on fait en sorte que WeeWX soit lancé au démarrage comme un service :

```bash
$ cd /home/weewx
$ sudo cp util/init.d/weewx.debian /etc/init.d/weewx
$ sudo chmod +x /etc/init.d/weewx
$ sudo update-rc.d weewx defaults 98
$ sudo /etc/init.d/weewx start
```