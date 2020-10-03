---
title: 'ncspot, un client ncurse pour Spotify'
published: false
date: '03-10-2020 12:56'
taxonomy:
    category:
        - Linux
    tag:
        - 'Raspberry Pi'
        - Spotify
twitterenable: true
twittercardoptions: summary
facebookenable: true
---

_ncspot_ est un client ncurse pour Spotify écrit en Rust et hautement inspiré des clients ncmpc ou ncmpcpp pour MPD. 

## Installation

On commence par installer les dépendances&nbsp;:

```shell
sudo apt install libncursesw5-dev libdbus-1-dev libpulse-dev libssl-dev libxcb1-dev libxcb-render0-dev libxcb-shape0-dev libxcb-xfixes0-dev
```

On installe ensuite une « instance » de Rust, grâce au script _rustup_&nbsp;:

```shell
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

avant de lancer l'installation à proprement parler de _ncspot_&nbsp;:

```shell
cargo install ncspot
```

## Usage et configuration

Il suffit alors de lancer la commande `ncspot`.



## Scrobbling

Si l'on souhaite scrobbler ce que l'on écoute avec _ncspot_, il convient d'installer _rescrobbled_&nbsp;:

```shell
wget https://github.com/InputUsername/rescrobbled/releases/download/v0.2.0/rescrobbled
tar xvzf v0.2.0
cd rescrobbled-0.2.0/
cargo install --path .
```

La configuration se fait à travers le fichier `~/.config/rescrobbled/config.toml` mais nécessite d'avoir généré au préalable un couple clé-secret via cette page de [Last.fm](https://www.last.fm/api/account/create)&nbsp;:

```shell
mkdir ~/.config/rescrobbled/
nano ~/.config/rescrobbled/config.toml
```

```
lastfm-key = "Last.fm API key"
lastfm-secret = "Last.fm API secret"
listenbrainz-token = "ListenBrainz API token"
enable-notifications = false
min-play-time = 0 # in seconds
player-whitelist = [ "Player MPRIS identity" ] # if empty or ommitted, will allow all players
```

Il faut ensuite lancer la commande `rescrobbled` alors que le lecteur ncspot fonctionne&nbsp;; il vous sera alors demander votre identifiant et votre mot de passe.     
Pour lancer le service en tant que démon, il faut placer le fichier `~/rescrobbled-0.2.0/rescrobbled.service` dans votre répertoire `~/.config/systemd/user/`&nbsp;:

```shell
mkdir -p ~/.config/systemd/user
cp ~/rescrobbled-0.2.0/rescrobbled.service ~/.config/systemd/user/
```