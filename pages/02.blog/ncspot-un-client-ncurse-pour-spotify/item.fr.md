---
title: 'ncspot, un client ncurse pour Spotify'
published: true
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

[_ncspot_](https://github.com/hrkfdn/ncspot) est un client ncurse pour Spotify écrit en Rust et hautement inspiré des clients ncmpc ou ncmpcpp pour MPD. 

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

Il suffit alors de lancer la commande `ncspot` et l'on peut alors naviguer entre trois écrans grâce aux touches `F1`, `F2` et `F3`&nbsp;: queue, recherche, bibliothèque.     

Pour en savoir plus, il suffit d'accéder à l'écran d'aide avec la touche `?`.

La configuration se fait dans le fichier `~/.config/ncspot/config.toml` avec a possibilité de définir soi-même des raccourcis clavier au sein d'une section `[keybindings]` et un thème au sein d'une section `[theme]` ([un générateur](https://ncspot-theme-generator.vaa.red) vous en facilite la création)&nbsp;:

```toml
backend = "pulseaudio"

[saved_state]
volume = 70
repeat = "no"
shuffle = true

[keybindings]

"Shift+p" = "pause"

[theme]
background = "black"
primary = "green"
secondary = "cyan"
title = "magenta"
playing = "black"
playing_selected = "blue"
playing_bg = "magenta"
highlight = "black"
highlight_bg = "green"
error = "white"
error_bg = "red"
statusbar = "magenta"
statusbar_progress = "magenta"
statusbar_bg = "black"
cmdline = "cyan"
cmdline_bg = "light black"
```


## Scrobbling

Si l'on souhaite scrobbler ce que l'on écoute avec _ncspot_, il convient d'installer [_rescrobbled_](https://github.com/InputUsername/rescrobbled), écrit lui aussi en Rust&nbsp;:

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

```toml
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