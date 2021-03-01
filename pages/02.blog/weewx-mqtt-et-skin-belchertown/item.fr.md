---
title: 'WeeWX, MQTT et skin Belchertown'
date: '20-12-2020 18:17'
twitterenable: true
twittercardoptions: summary
facebookenable: true
taxonomy:
    category:
        - Linux
    tag:
        - 'Raspberry Pi'
        - WeeWX
        - MQTT
        - IoT
---

Le [skin Belchertown](https://github.com/poblabs/weewx-belchertown) pour WeeWX proposé par Pat O'Brien est extrêmement complet (et complexe, du moins pour moi) et est particulièrement intéressant pour ses graphiques dynamiques en JavaScript.

## Installation et configuration de base

Son installation n'est pas particulièrement difficile puisqu'il suffit de suivre les instructions fournies&nbsp;; ainsi, à ce jour&nbsp;:

```shell
wget https://github.com/poblabs/weewx-belchertown/releases/download/weewx-belchertown-1.2/weewx-belchertown-release-1.2.tar.gz
sudo wee_extension --install weewx-belchertown-release-1.2.tar.gz
```

Ensuite, il convient de passer à sa configuration en ayant pris soin, afin de bénéficier de certaines options dont le module de prévisions à sept jours, de [remplir certaines conditions](https://github.com/poblabs/weewx-belchertown#requirements).

La configuration de base est la suivante&nbsp;:
```shell
sudo nano /etc/weewx/weewx.conf
```
```
[StdReport]
    ...
    [[Belchertown]]
        skin = Belchertown
        HTML_ROOT = /var/www/html/weewx/
```

## MQTT

Nous allons surtout nous intéresser à la mise en place du rafraîchissement automatique des graphiques et de l'affichage des données permises par ce skin grâce au protocole MQTT (Message Queuing Telemetry Transport). Il convient d'abord d'installer et de configurer un «&nbsp;serveur&nbsp;», un _broker_, en l'occurrence `mosquitto` tel que nous l'avons vu dans [un précédent article](/blog/mosquitto-un-broker-mqtt) à partir du post de Pat O'Brien [sur son blog](https://obrienlabs.net/how-to-setup-your-own-mqtt-broker/).

Bien sûr, dans mon cas, WeeWX ne récupérant des données de la station météo que toutes les cinq voire dix minutes, l'utilisation du plugin weewx-mqtt ne se justifie pas forcément... 

Une fois le _broker_ Mosquitto installé et configuré, il faut installer le plugin [_weewx-mqtt_](https://github.com/matthewwall/weewx-mqtt)&nbsp;:

```shell
sudo pip3 install paho-mqtt
wget -O weewx-mqtt.zip https://github.com/matthewwall/weewx-mqtt/archive/master.zip
wee_extension --install weewx-mqtt.zip
```

On modifie ensuite sa configuration qui dépend de la section [Restful] dans le fichier `/etc/weewx/weewx.conf` :

```
[Restful]
    ...
    [[MQTT]]
        server_url = mqtt://USER:PASSWORD@SERVER:PORT/
        topic = weewx
        unit_system = METRIC
        binding = archive, loop
        aggregation = aggregate
        [[[tls]]]
            tls_version = tlsv12
            ca_certs = /etc/ssl/certs/ca-certificates.crt
```

On en profite pour éditer la configuration du skin Belchertown de la sorte&nbsp;:

```
[StdReport]
    ...
    [[Belchertown]]
        skin = Belchertown
        HTML_ROOT = /var/www/html/weewx/
        mqtt_websockets_enabled = 1
        mqtt_websockets_host = "SERVER"
        mqtt_websockets_port = 9001
        mqtt_websockets_ssl = 1
        mqtt_websockets_topic = "weather/loop"
        disconnect_live_website_visitor = 1800000
```

Il ne nous reste plus qu'à relancer weewx&nbsp;:

```shell
sudo service restart weewx
```

On peut vérifier le bon fonctionnement du plugin weewx-mqtt en lançant la commande `mosquitto_sub` pour suivre le topic `weewx/loop` ou en épluchant `/var/log/syslog` à la recherche d'une ligne de ce type&nbsp;:

```
$ tail -f /var/log/syslog | grep weewx
[snip]
Mar  1 06:48:23 raspberrypi weewx[3998] INFO weewx.restx: MQTT: Published record 2021-03-01 06:45:00 CET (1614588300)
[snip]  
```