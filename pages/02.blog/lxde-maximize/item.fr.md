---
title: 'Lancer une application maximisée par défaut sous LXDE / Lubuntu'
date: '28-07-2018 00:00'
taxonomy:
    category:
        - Linux
    tag:
        - personnalisation
        - LXDE
---

Par exemple, dans mon cas, `lxterminal`, qui a chaque démarrage me frustrait du fait de la taille de sa fenêtre. Utilisant par ailleurs régulièrement `tmux` (voir à ce sujet [«&#8239;My Own Tmux Cheat Sheet&#8239;»](/tutos/Linux/tmux_cheatsheet)), je souhaitais que `lxterminal` s'ouvre par défaut maximisée.

Pour cela, on édite le fichier `~/.config/openbox/lubuntu-rc.xml` (ou son `~/.config/openbox/lxde-rc.xml`) et on ajoute juste avant la fin de la section `applications` ceci&nbsp;:

```markup
<application name="lxterminal">
  <maximized>yes</maximized>
</application>
```

Puis on force Openbox à relire sa configuration&nbsp;:

```bash
$ openbox --reconfigure
```