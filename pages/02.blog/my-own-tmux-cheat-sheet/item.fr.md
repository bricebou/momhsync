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

## Configuration personnelle

...

## Raccourcis claviers

Avec cette configuration proposée par  Gregory Pakosz, le préfixe à utiliser est soit `Ctrl+b`, celui par défaut de tmux, soit `Ctrl+a`.

`<prefix> "` ou `<prefix> -`&nbsp;: partager le panneau horizontalement      
`<prefix> %` ou `<prefix> _`&nbsp;: partager le panneau verticalement      
`<prefix> c`&nbsp;: créer un nouveau panneau     
`<prefix> x`&nbsp;: détruire le panneau actif    

`<prefix> m`&nbsp;: (dés)activer le mode souris     

## Copier-Coller

Pour profiter pleinement du copier-coller, il convient d'installer le paquet `xclip`.

À la souris, il suffit de surligner le texte à copier et d'utiliser `Alt+w`.

Sinon, au clavier, il faut commencer par utiliser `<prefix> [` pour entrer en mode copie, se rendre au début du texte à copier, appuyer sur `Espace`, se rendre à la fin et simplement appuyer sur `Entrée`.