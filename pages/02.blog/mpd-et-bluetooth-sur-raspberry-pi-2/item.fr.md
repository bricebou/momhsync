---
title: 'MPD et Bluetooth sur Raspberry Pi 2'
date: '03-05-2020 10:11'
taxonomy:
    category:
        - Linux
    tag:
        - mpd
        - 'Raspberry Pi'
        - bluetooth
twitterenable: true
twittercardoptions: summary
facebookenable: true
recaptchacontact:
    enabled: false
---

Afin d'utiliser pleinement mon «&nbsp;système multimédia&nbsp;» à base d'un NAS et d'un Raspberry Pi &ndash;&nbsp;qui commence à se faire vieillissant puisque dans sa vesion 2&nbsp;&ndash;sur lequel tourne un serveur MPD, je souhaitais connecter via Bluetooth ce dernier à mon petit amplificateur Tangent Ampster BT.

## Connexion Bluetooth

J'ai fait l'aquisition d'un dongle USB/Bluetooth TP-Link reconnu, dans mon souvenir, nativement sous Raspbian Buster, sous réserve d'avoir bien installé le paquet _pi-bluetooth_&nbsp;:

```shell
sudo apt install pi-bluetooth
```

Pour connecter l'ampli au Raspberry Pi, on utilise l'utilitaire en ligne de commande `bluetoothctl`&nbsp;: on commence par rechercher les périphériques disponibles (`scan on`), puis on fait l'appairage avec le bon device (`pair`) et on lui accorde notre confiance (pas d'authentification nécessaire, `trust`) avant de nous y connecter (`connect`) et de cesser la recherche (`scan off`)&nbsp;:

```shell
$ bluetoothctl 
Agent registered
[bluetooth]# scan on
Discovery started
[CHG] Controller 00:1A:7D:DA:71:15 Discovering: yes
[CHG] Device FC:58:FA:14:27:BC RSSI: -71
[CHG] Device FC:58:FA:14:27:BC TxPower: 4
[...]
[bluetooth]# pair FC:58:FA:14:27:BC 
Attempting to pair with FC:58:FA:14:27:BC
[CHG] Device FC:58:FA:14:27:BC Connected: yes
[...]
[CHG] Device FC:58:FA:14:27:BC ServicesResolved: yes
[CHG] Device FC:58:FA:14:27:BC Paired: yes
Pairing successful
[CHG] Device FC:58:FA:14:27:BC ServicesResolved: no
[CHG] Device FC:58:FA:14:27:BC Connected: no
[bluetooth]# trust FC:58:FA:14:27:BC 
[CHG] Device FC:58:FA:14:27:BC Trusted: yes
Changing FC:58:FA:14:27:BC trust succeeded
[bluetooth]# connect FC:58:FA:14:27:BC 
Attempting to connect to FC:58:FA:14:27:BC
[CHG] Device FC:58:FA:14:27:BC Connected: yes
Connection successful
[CHG] Device FC:58:FA:14:27:BC ServicesResolved: yes
[Tangent Ampster BT]# scan off
Discovery stopped
[...]
[Tangent Ampster BT]# exit
```

Cependant, si à l'étape de la connexion à l'ampli, vous obtenez une erreur, il convient alors d'installer le paquet _pulseaudio-module-bluetooth_ &nbsp;:

```shell
$ sudo apt install pulseaudio-module-bluetooth
```

Ouvez ensuite le fichier `/etc/pulse/default.pa` et vérifiez aux alentours des lignes 65-67 que vous avez bien&nbsp;:

```plain
.ifexists module-bluetooth-discover.so
load-module module-bluetooth-discover
.endif
```

et ajoutez à la fin du fichier&nbsp;:

```plain
# automatically switch to newly-connected devices
load-module module-switch-on-connect
```

Lancez alors `pulseaudio` avec la commande `pulseaudio -D` après l'avoir tué si besoin (`pulseaudio -k`) puis relancez la commande `connect` dans l'outil `bluetoothctl`.

## Configurer de nouvelles sorties dans MPD

Une nouvelle fois, je ne suis pas parvenu à configurer MPD pour qu'il utilise PulseAudio... On s'en tient donc à ALSA et au «&nbsp;Bluetooth Audio ALSA Backend&nbsp;» _bluealsa_ que l'on commence par installer&nbsp;:

```shell
$ sudo apt install bluealsa
```

Puis on «&nbsp;crée&nbsp;» de nouveaux périphériques audio pour l'ensemble du système et de ses utilisateurs&nbsp;:

```shell
sudo nano /etc/asound.conf
```

```plain
pcm.tangent {
	type plug
	slave {
		pcm {
			type bluealsa
			device FC:58:FA:14:27:BC
			profile "a2dp"
		}
  }
  hint {
    show on
    description "Tangent Ampster BT"
	}
}

pcm.jblgo {
  type plug
  slave {
    pcm {
      type bluealsa
      device 78:44:05:61:4C:17
      profile "a2dp"
    }
  }
  hint {
    show on
    description "JBL Go"
  }
}
```

On en vient ensuite à la configuration de MPD auquel on doit déclarer les nouvelles sorties à utiliser&nbsp;:

```plain
audio_output {
        type            "alsa"
        name            "Tangent Ampster BT - ALSA Bluetooth"
        device          "tangent"
        mixer_type	"software"
	format          "44100:16:2"
}
audio_output {
        type            "alsa"
        name            "JBL Go - ALSA Bluetooth"
        device          "jblgo"
        mixer_type      "software"
}
```

Et on relance le service&nbsp;:

```shell
$ sudo service mpd restart
```