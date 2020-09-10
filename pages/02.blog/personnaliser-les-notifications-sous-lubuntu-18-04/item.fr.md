---
title: 'Personnaliser les notifications sous Xfce'
date: '14-06-2018 00:00'
taxonomy:
    category:
        - Linux
    tag:
        - Xfce
        - personnalisation
twitterenable: true
twittercardoptions: summary
facebookenable: true
---

Pour pouvoir personnaliser les notifications système, il convient de commencer par installer le paquet `xfce4-notifyd`&nbsp;:

```bash
$ sudo apt-get install xfce4-notifyd
```

Ensuite, vous pouvez lancer l'utilitaire `xfce4-notifyd-config` pour configurer la disposition des notifications, leur durée, le thème utilisé...

## Créer son propre thème de notification

En se référant à la [documentation de XFCE](https://docs.xfce.org/apps/notifyd/theming), il faut que notre thème, pour qu'il apparaisse dans le sélecteur de `xfce4-notifyd-config`, soit nommé `gtk.css` soit dans le répertoire `/usr/share/themes/$yourtheme/xfce-notify-4.0` ou `~/.themes/$yourtheme/xfce-notify-4.0`.

Pour se faire une idée des éléments à cibler, de leurs propriétés, on peut se référer aux thèmes proposés de base et dont le code est disponible [ici](https://git.xfce.org/apps/xfce4-notifyd/tree/themes/gtk-3.20/), et plus particulièrement au thème `Retro`.

Voici un exemple de thème que j'utilise&nbsp;:

```css
#XfceNotifyWindow {
    background-color: #3c3c3c;
    border-radius: 10px;
    border: 1px solid #4C4C4C;
    padding: 20px;
}

#XfceNotifyWindow:hover {
    background-color: shade(#3c3c3c, 0.95);
}

#XfceNotifyWindow label,
#XfceNotifyWindow image {
    color: #e4e4e4;
}

#XfceNotifyWindow label#summary {
    font-weight: Bold;
}

#XfceNotifyWindow button {
    font-weight: Bold;
    box-shadow: none;
    background-image: none;
    background-color: #3c3c3c;
    color: #e4e4e4;
}

#XfceNotifyWindow button:hover {
    box-shadow: none;
    background-image: none;
    background-color: #4C4C4C;
}

#XfceNotifyWindow progressbar {
    min-height: 10px;
}

#XfceNotifyWindow progressbar progress {
    background-image: linear-gradient(to right, shade(#CD2D25,0.75), shade(#CD2D25,1));
}

#XfceNotifyWindow progressbar trough {
    background-color: #333333;
}
```