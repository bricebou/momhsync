---
title: 'Raspbian, Spotify et PulseAudio '
media_order: 'spotify_select.jpeg,spotify_select_raspotify.jpeg'
date: '03-10-2020 10:31'
taxonomy:
    category:
        - Linux
    tag:
        - 'Raspberry Pi'
        - PulseAudio
        - Spotify
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

Il est possible d'utiliser son Raspberry Pi sous Raspbian comme client Spotify Connect, c'est-à-dire d'en faire une sorte de module de sortie audio, et ce grâce à [Raspotify](https://dtcooper.github.io/raspotify/).

## Installation

Pour l'installer, rien de plus simple&nbsp;:

```shell
# Install curl and https apt transport
sudo apt-get -y install curl apt-transport-https

# Add repo and its GPG key
curl -sSL https://dtcooper.github.io/raspotify/key.asc | sudo apt-key add -v -
echo 'deb https://dtcooper.github.io/raspotify raspotify main' | sudo tee /etc/apt/sources.list.d/raspotify.list

# Install package
sudo apt-get update
sudo apt-get -y install raspotify
```

Ensuite, depuis votre application Spotify, il suffit de sélectionner la sortie « Raspotify » dans les options proposées dans le menu accessible depuis l'icone dans le bas inférieur gauche de l'écran de lecture&nbsp;:

<img src="/blog/raspbian-spotify-et-pulseaudio/spotify_select.jpeg" alt="spotify select out" style="width:200px; float: left; margin-right: 24px;" />
<img src="/blog/raspbian-spotify-et-pulseaudio/spotify_select_raspotify.jpeg" alt="spotify select out raspotify" style="width:200px; float: left" />

## Configuration

Raspotify est fonctionnel _out of the box_ mais, s'il en est besoin, il est possible de jouer sur certains paramètres dans le fichier `/etc/default/raspotify`.

En cas de modification, il convient alors de relancer le service `raspotify`&nbsp;:

```shell
sudo service raspotify restart
```

## Raspotify & PulseAudio

Pour que Raspotify utiliser PulseAudio, il suffit de suivre [la démarche proposée par Marc Fauvain ](https://github.com/dtcooper/raspotify/issues/154#issuecomment-442299432)&nbsp;:

```shell
cd /var/cache/raspotify
sudo mkdir .pulse
sudo sh -c 'echo "default-server = 127.0.0.1" > .pulse/client.conf'
sudo chown -R raspotify:raspotify .pulse
```

Il faut également éditer le fichier `/etc/asound.rc`&nbsp;:

```shell
sudo nano /etc/asound.rc
```

```
pcm.!default {
    type pulse
}

ctl.!default {
    type pulse
}
```
