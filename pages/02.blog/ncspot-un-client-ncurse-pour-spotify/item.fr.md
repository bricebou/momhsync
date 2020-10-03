---
title: 'ncspot, un client ncurse pour Spotify'
published: false
date: '03-10-2020 12:56'
taxonomy:
    category:
        - Linux
    tag:
        - Spotify
        - 'Raspberry Pi'
twitterenable: true
twittercardoptions: summary
facebookenable: true
---

On commence par installer les dépendances&nbsp;:

```shell
sudo apt install libncursesw5-dev libdbus-1-dev libpulse-dev libssl-dev libxcb1-dev libxcb-render0-dev libxcb-shape0-dev libxcb-xfixes0-dev
```

On installe ensuite une « instance » de Rust, grâce au script _rustup_&nbsp;:

```shell
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```