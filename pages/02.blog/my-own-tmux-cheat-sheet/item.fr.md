---
title: 'My Own Tmux Cheat Sheet'
taxonomy:
    category:
        - Linux
    tag:
        - keybinding
        - console
twitterenable: true
twittercardoptions: summary
facebookenable: true
---

Il s'agit ici pour moi de réunir en un lien unique, clairement identifiable &mdash; pour moi du moins &mdash; un certain nombre de ressources consacrées à Tmux, collant le plus possible à mes besoins et à mon usage de ce «&#8239;multiplexeur de terminaux&#8239;».

## Installation et préparation de base

Tmux est disponible dans les dépôts et il suffit pour l'installer d'un&nbsp;:

```bash
sudo apt install tmux
```

Ensuite, on récupère et on place dans notre `/home` les fichiers de configuration proposé par [Gregory Pakosz sur GitHub](https://github.com/gpakosz/.tmux)&nbsp;:

```bash
cd /home/$USER
git clone https://github.com/gpakosz/.tmux.git
ln -s -f .tmux/.tmux.conf
cp .tmux/.tmux.conf.local .

```

Pour profiter pleinement du copier-coller, il convient d'installer le paquet `xclip`.

## Configuration personnelle

...

## Raccourcis claviers

`Ctrl+b "`&nbsp;: partager le panneau horizontalement      
`Ctrl+b %`&nbsp;: partager le panneau verticalement      
`Ctrl+b c`&nbsp;: créer un nouveau panneau     
`Ctrl+b x`&nbsp;: détruire le panneau actif    

`Ctrl+b m`&nbsp;: (dés)activer le mode souris     