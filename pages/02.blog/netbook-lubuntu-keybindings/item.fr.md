---
title: 'Lubuntu 18.04 sur ASUS F200M : réglages système au clavier'
date: '13-06-2018 00:00'
taxonomy:
    category:
        - Linux
    tag:
        - keybinding
        - touchpad
        - Xfce
        - 'ASUS F200M'
        - PulseAudio
---

Sous Lubuntu 18.04, presque tous les réglages accessibles via la touche `Fn` de mon Asus F200M sont fonctionnels «&nbsp;_out of scratch_&nbsp;» à l'exception de celui (dés)activant le touchpad et d'un léger souci sur les touche de volume.

## Activer / désactiver le touchpad

On commence par déterminer si la touche de raccourcis `Fn+F9` est reconnue, grâce à l'outil `xev`&nbsp;:

```bash
$ xev | grep 'keycode'
    state 0x0, keycode 199 (keysym 0x1008ffa9, XF86TouchpadToggle), same_screen YES,
```

Elle l'est&nbsp;! On garde donc ce `XF86TouchpadToggle` de côté&nbsp;; si ce n'est pas le cas, on peut toujours se rabattre sur un raccourci du genre `Windows+F9`.

J'ai déjà eu l'occasion de produire deux posts consacrés à ce raccourci de (dés)activation du touchpad: [le premier pour Xubuntu 14.04](/blog/netbook-keybindings), [le second sous Xubuntu 14.10](/blog/netbook-touchpad-keybinding).

Par chance, dans mon cas, l'outil `synclient` utilisé sous Xubuntu 14.04 fonctionne bien.

On crée donc un répertoire `bin/` dans notre dossier personnel, et l'on y crée un script bash `toggle-trackpad.sh`&nbsp;:

```bash
$ mkdir ~/bin
$ nano ~/bin/toggle-trackpad.sh
```

On y colle ceci&nbsp;:

```bash
#!/bin/bash
synclient TouchpadOff=$(synclient -l | grep -c 'TouchpadOff.*=.*0')
```

On enregistre et on rend ce script exécutable&nbsp;:
```bash
$ chmod +x ~/bin/toggle-trackpad.sh
```

Il nous faut maintenant indiquer le raccourcis clavier souhaité à l'environnement de bureau LXDE. On édite le fichier `~/.config/openbox/lubuntu-rc.xml` et l'on y ajoute, à la fin de la section consacrée aux raccourcis clavier, donc juste avant `</keyboard>`, un nouveau raccourci clavier pour appeler le script `toggle-touchpad.sh`:

```markup
<keybind key="XF86TouchpadToggle">
    <action name="Execute">
        <command>~/bin/toggle-trackpad.sh</command>
    </action>
</keybind>
```

Une fois le fichier enregistré, pas la peine de redémarrer ou de quitter votre session! Il suffit de lancer la commande suivante, et vous devriez pouvoir utiliser le raccourci juste créé.

```bash
$ openbox --reconfigure
```

## Réglage du volume

### Mute/Unmute

Si le raccourci `Fn+F10` coupait bien le son, il était impossible de le restaurer à son état précédent, le son restait sur "_mute_".

Merci à _mutz_ sur [le forum anglophone d'Ubuntu](https://ubuntuforums.org/showthread.php?t=1796713&s=d2aa16450829a37b72aad43fa8b355cb&p=12655641#post12655641 "ubuntuforums.org") qui m'a permis de corriger ce petit souci avec cette petite commande&nbsp;:

```bash
$ amixer -D pulse set Master toggle
```

Elle permet de couper et de réactiver le son, alternativement. Il suffit juste alors de modifier la définition du raccourci clavier dans le fichier `~/.config/openbox/lubuntu-rc.xml`.     
Remplacer 
```markup
<keybind key="XF86AudioMute">
  <action name="Execute">
    <command>amixer -q sset Master toggle
  </action>
</keybind>
```

par ceci:
```markup
<keybind key="XF86AudioMute">
  <action name="Execute">
    <command>amixer -D pulse set Master toggle
  </action>
</keybind>
```

### Réglage du volume

Il semblerait que l'installation de certains paquets liés à PulseAudio et que l'utilisation de l'applet `pasystray` en lieu et place de l'applet fournie de base avec Lubuntu causent le non fonctionnement des raccourcis `Fn+F11` et `Fn+F12` (après une installation fraîche, aucun souci&nbsp;!).

Du coup, on suit la logique du point précédent et on ajoute l'option `-D pulse` à la commande `amixer` et tout rentre dans l'ordre&nbsp;; il suffit donc de modifier dans le fichier `~/.config/openbox/lubuntu-rc.xml` les raccourcis associés aux touches `XF86AudioRaiseVolume` et `XF86AudioLowerVolume` (notez qu'il est possible de modifier la valeur pour avoir un pas plus conséquent)&nbsp;:

```markup
<keybind key="XF86AudioRaiseVolume">
  <action name="Execute">
    <command>amixer -D pulse set Master 3%+ unmute</command>
  </action>
</keybind>
<keybind key="XF86AudioLowerVolume">
  <action name="Execute">
    <command>amixer -D pulse set Master 3%- unmute</command>
  </action>
</keybind>
```

### Mais elle est où ma notification&nbsp;?

Heu... Je ne sais pas... --' Mais grâce à [Konrad Strack](https://konradstrack.ninja/blog/volume-change-notifications-in-openbox/), on va en créer une&nbsp;! On ne touche à pas grand chose de son travail, de son script, si ce n'est qu'on remplace les commandes `amixer` par celles qui fonctionnent bien chez nous avec PulseAudio.

Il faut au préalable avoir installé le paquet `libnotify-bin` pour bénéficier de la commande `notify-send`&nbsp;:

```bash
$ sudo apt-get install libnotify-bin
```

Il est possible de modifier le pas du réglage du volume (`step=5`) ainsi que, pour la notification en elle-même (la commande `notify-send`), la durée de la notification (l'argument `-t 1000`) ou les icones appelées (`-i audio-volume-muted-panel`).

On crée donc le script `~/bin/volume.sh`&nbsp;:
```bash
#!/bin/bash
 
step=5
 
if [[ $# -eq 1 ]]; then
  case $1 in
    "up")
      amixer -D pulse set Master $step\%+;;
    "down")
      amixer -D pulse set Master $step\%-;;
    "toggle")
      amixer -D pulse set Master toggle;;
    *)
      echo "Invalid option";;
  esac
fi
 
muted=`amixer -D pulse get Master|tail -n1|sed -E 's/.*\[([a-z]+)\]/\1/'`
volume=`amixer -D pulse get Master|tail -n1|sed -E 's/.*\[([0-9]+)\%\].*/\1/'`
 
if [[ $muted == "off" ]]; then
  notify-send "Muted" -t 1000 -i audio-volume-muted-panel -h int:value:$volume
else
  notify-send "Volume" -t 1000 -i audio-volume-medium-panel -h int:value:$volume
fi
```

On le rend exécutable&nbsp;:
```bash
$ chmod +x ~/bin/volume.sh 
```

Et on n'a plus qu'à modifier la commande appelée par nos raccourcis claviers&nbsp;:
```markup
<keybind key="XF86AudioRaiseVolume">
    <action name="Execute">
        <command>volume.sh up</command>
    </action>
</keybind>
<keybind key="XF86AudioLowerVolume">
    <action name="Execute">
        <command>volume.sh down</command>
    </action>
</keybind>
<keybind key="XF86AudioMute">
    <action name="Execute">
        <command>volume.sh toggle</command>
    </action>
</keybind>
```

Sachez enfin qu'il est possible de [personnaliser l'apparence des notifications](/blog/personnaliser-les-notifications-sous-lubuntu-18-04).

### Alternative

Ce script semblait par moment rencontrer quelques bugs sur mon netbook et une alternative semble être de passer par le _daemon_  `xfce4-volumed`&nbsp;:

```bash
$ sudo apt-get install xfce4-volumed
```

Ensuite, il faut veiller à ce que `xfce4-volumed` soit bien lancé au démarrage&nbsp;; pour ce faire, il suffit d'ouvrir le fichier `/etc/xdg/autostart/xfce4-volumed.desktop` et de commenter (en ajoutant un `#`) la dernière ligne&nbsp;:

```plain
OnlyShowIn=XFCE;
```

Il convient alors de commenter les raccourcis clavier liés au volume dans le fichier `~/.config/openbox/lubuntu-rc.xml`.