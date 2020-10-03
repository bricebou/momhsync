---
title: 'Raspbian, Spotify et PulseAudio '
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
---

Il est possible d'utiliser son Raspberry Pi sous Raspbian comme client Spotify Connect, c'est-à-dire d'en faire une sorte de module de sortie audio, et ce grâce à [Raspotify](https://dtcooper.github.io/raspotify/).

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