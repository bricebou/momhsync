---
title: 'MPD, PulseAudio et Bluetooth sous Raspbian Buster'
published: true
date: '29-09-2020 20:36'
taxonomy:
    category:
        - Linux
    tag:
        - mpd
        - 'Raspberry Pi'
        - PulseAudio
        - bluetooth
twitterenable: true
twittercardoptions: summary
facebookenable: true
---

Alors que je venais juste de mettre en place la cohabitation entre MPD et mon ampli connecté en Bluetooth sur mon Raspberry Pi 2 (voir [ici](/blog/mpd-et-bluetooth-sur-raspberry-pi-2)), je basculais sur un modèle 4, modifiant quelque peu mes usages et utiisant cette fois-ci l'interface graphique (afin de profiter notamment de Spotify, [une fois la gestion des DRM ajoutée](/blog/gestion-des-drm-sous-raspbian-buster)). Et du coup, afin d'avoir une maîtrise plus fine de mes sorties audio, j'ai opté pour l'utilisation de PulseAudio.

===

On commence par purger `bluealsa` s'il est installé&nbsp;:

```shell
sudo apt purge bluealsa
```

puis on installe tout ce qui concerne le Bluetooth et PulseAudio&nbsp;:

```shell
 sudo apt install pi-bluetooth pulseaudio pulseaudio-module-bluetooth paprefs pavumeter pavucontrol pasystray
 ```

On vérifie la configuration de PulseAudio (`/etc/pulse/default.pa`) notamment pour ce qui est du Bluetooth&nbsp;:

```
### Automatically load driver modules for Bluetooth hardware
.ifexists module-bluetooth-policy.so
load-module module-bluetooth-policy
.endif

.ifexists module-bluetooth-discover.so
load-module module-bluetooth-discover
.endif
```

et on ajoute ces éléments&nbsp;:

```
# automatically switch to newly-connected devices
load-module module-switch-on-connect

load-module module-native-protocol-tcp auth-ip-acl=127.0.0.1 # IP of localhost
```

On tue PulseAudio&nbsp;:

```shell
pulseaudio -k
```

S'il ne redémarre pas tout seul, il suffit de lancer la commande&nbsp;:

```shell
pulseaudio -D
```

On veille à bien connecter notre enceinte Bluetooth via l'utilitaire en ligne de commande `bluetoothctl`&nbsp;: on commence par rechercher les périphériques disponibles (`scan on`), puis on lui accorde notre confiance (pas d'authentification nécessaire, `trust`) avant de faire l'appairage avec le bon device (`pair`) et de nous y connecter (`connect`)&nbsp;; on peut alors cesser la recherche (`scan off`)&nbsp;:

```shell
bluetoothctl 
```
```shell
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

<!--

Pour la configuration de MPD, il est nécessaire d'identifier alors les sorties disponibles grâce à la commande suivante&nbsp;:

```shell
pacmd list-sinks
```

```shell
3 sink(s) available.
    index: 0
	name: <alsa_output.platform-bcm2835_audio.analog-mono>
	driver: <module-alsa-card.c>
	flags: HARDWARE DECIBEL_VOLUME LATENCY FLAT_VOLUME 
	state: SUSPENDED
	suspend cause: IDLE
	priority: 9009
	volume: mono: 38011 /  58% / -14,19 dB
	        balance 0,00
	base volume: 65536 / 100% / 0,00 dB
	volume steps: 65537
	muted: no
	current latency: 0,00 ms
	max request: 0 KiB
	max rewind: 0 KiB
	monitor source: 0
	sample spec: s16le 1ch 44100Hz
	channel map: mono
	             Mono
	used by: 0
	linked by: 0
	fixed latency: 99,95 ms
	card: 0 <alsa_card.platform-bcm2835_audio>
	module: 6
	properties:
		alsa.resolution_bits = "16"
		device.api = "alsa"
		device.class = "sound"
		alsa.class = "generic"
		alsa.subclass = "generic-mix"
		alsa.name = "bcm2835 HDMI 1"
		alsa.id = "bcm2835 HDMI 1"
		alsa.subdevice = "0"
		alsa.subdevice_name = "subdevice #0"
		alsa.device = "0"
		alsa.card = "0"
		alsa.card_name = "bcm2835 HDMI 1"
		alsa.long_card_name = "bcm2835 HDMI 1"
		alsa.driver_name = "snd_bcm2835"
		device.bus_path = "platform-bcm2835_audio"
		sysfs.path = "/devices/platform/soc/fe00b840.mailbox/bcm2835_audio/sound/card0"
		device.form_factor = "internal"
		device.string = "hw:0"
		device.buffering.buffer_size = "8816"
		device.buffering.fragment_size = "2208"
		device.access_mode = "mmap"
		device.profile.name = "analog-mono"
		device.profile.description = "Mono analogique"
		device.description = "Audio interne Mono analogique"
		alsa.mixer_name = "Broadcom Mixer"
		module-udev-detect.discovered = "1"
		device.icon_name = "audio-card"
	ports:
		analog-output: Sortie analogique (priority 9900, latency offset 0 usec, available: unknown)
			properties:
				
	active port: <analog-output>
    index: 1
	name: <alsa_output.platform-bcm2835_audio.analog-mono.2>
	driver: <module-alsa-card.c>
	flags: HARDWARE HW_MUTE_CTRL HW_VOLUME_CTRL DECIBEL_VOLUME LATENCY FLAT_VOLUME 
	state: SUSPENDED
	suspend cause: IDLE
	priority: 9009
	volume: mono: 7864 /  12% / -55,25 dB
	        balance 0,00
	base volume: 56210 /  86% / -4,00 dB
	volume steps: 65537
	muted: no
	current latency: 0,00 ms
	max request: 0 KiB
	max rewind: 0 KiB
	monitor source: 1
	sample spec: s16le 1ch 44100Hz
	channel map: mono
	             Mono
	used by: 0
	linked by: 0
	fixed latency: 99,95 ms
	card: 1 <alsa_card.platform-bcm2835_audio.2>
	module: 7
	properties:
		alsa.resolution_bits = "16"
		device.api = "alsa"
		device.class = "sound"
		alsa.class = "generic"
		alsa.subclass = "generic-mix"
		alsa.name = "bcm2835 Headphones"
		alsa.id = "bcm2835 Headphones"
		alsa.subdevice = "0"
		alsa.subdevice_name = "subdevice #0"
		alsa.device = "0"
		alsa.card = "1"
		alsa.card_name = "bcm2835 Headphones"
		alsa.long_card_name = "bcm2835 Headphones"
		alsa.driver_name = "snd_bcm2835"
		device.bus_path = "platform-bcm2835_audio"
		sysfs.path = "/devices/platform/soc/fe00b840.mailbox/bcm2835_audio/sound/card1"
		device.form_factor = "internal"
		device.string = "hw:1"
		device.buffering.buffer_size = "8816"
		device.buffering.fragment_size = "2208"
		device.access_mode = "mmap"
		device.profile.name = "analog-mono"
		device.profile.description = "Mono analogique"
		device.description = "Audio interne Mono analogique"
		alsa.mixer_name = "Broadcom Mixer"
		module-udev-detect.discovered = "1"
		device.icon_name = "audio-card"
	ports:
		analog-output-headphones: Casque audio (priority 9000, latency offset 0 usec, available: unknown)
			properties:
				device.icon_name = "audio-headphones"
	active port: <analog-output-headphones>
  * index: 2
	name: <bluez_sink.FC_58_FA_14_27_BC.a2dp_sink>
	driver: <module-bluez5-device.c>
	flags: HARDWARE DECIBEL_VOLUME LATENCY FLAT_VOLUME 
	state: RUNNING
	suspend cause: (none)
	priority: 9550
	volume: front-left: 65536 / 100% / 0,00 dB,   front-right: 65536 / 100% / 0,00 dB
	        balance 0,00
	base volume: 65536 / 100% / 0,00 dB
	volume steps: 65537
	muted: no
	current latency: 58,01 ms
	max request: 3 KiB
	max rewind: 0 KiB
	monitor source: 2
	sample spec: s16le 2ch 44100Hz
	channel map: front-left,front-right
	             Stéréo
	used by: 1
	linked by: 1
	fixed latency: 45,32 ms
	card: 2 <bluez_card.FC_58_FA_14_27_BC>
	module: 28
	properties:
		bluetooth.protocol = "a2dp_sink"
		device.description = "Tangent Ampster BT"
		device.string = "FC:58:FA:14:27:BC"
		device.api = "bluez"
		device.class = "sound"
		device.bus = "bluetooth"
		device.form_factor = "speaker"
		bluez.path = "/org/bluez/hci0/dev_FC_58_FA_14_27_BC"
		bluez.class = "0x240414"
		bluez.alias = "Tangent Ampster BT"
		device.icon_name = "audio-speakers-bluetooth"
	ports:
		speaker-output: Haut-parleur (priority 0, latency offset 0 usec, available: yes)
			properties:
				
	active port: <speaker-output>
```

De là, on peut configurer les sorties dans MPD&nbsp;:

```shell
sudo nano /etc/mpd.conf
```

```
audio_output {
        type            "pulse"
        name            "Tangent Ampster BT"
        server          "127.0.0.1"            
        sink            "bluez_sink.FC_58_FA_14_27_BC.a2dp_sink"       
        mixer_type      "software"
}
audio_output {
        type            "pulse"
        name            "HDMI 1 Output"
        server          "127.0.0.1"            
        sink            "alsa_output.platform-bcm2835_audio.analog-mono"     
        mixer_type      "software"
}
```
-->

On configure une sortie PulseAudio dans la configuration de mpd, `/etc/mpd.conf`&nbsp;:

```shell
sudo nano /etc/mpd.conf
```
audio_output {
        type            "pulse"
        name            "My Pulse Output"
        server          "localhost"
}
```

On relance MPD et l'affaire est jouée&nbsp;:

```shell
sudo service mpd restart
```