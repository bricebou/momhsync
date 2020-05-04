---
title: '(Dés)Activer le Wifi avec un raccourci clavier sous Xubuntu 15.04 sur ASUS F200M'
date: '03-05-2015 00:00'
taxonomy:
    category:
        - Linux
    tag:
        - keybinding
        - wifi
        - 'ASUS F200M'
        - Xfce
---

Dans un [article précédent](/blog/netbook-keybindings), j'avais présenté un court script permettant d'activer et de désactiver le Wifi sur mon netbook ASUS F200M sous Xubuntu avec les touches "fonctions".

Après la mise à jour vers la dernière version de Xubuntu (15.04), ce raccourci clavier ne fonctionne plus, le script n'étant plus opérant. Après quelques rapides tests, il semblerait que la syntaxe de `nmcli` ait changé.

Voici donc la version revue du script "wifi-toggle.sh"&nbsp;:

```bash
#!/bin/bash
status=$(nmcli -t -f WIFI g)
if [ $status = "activé" ]&nbsp;; then
    notify-send -i network-wireless-disconnected "Wireless" "Wireless disabled"
    nmcli r wifi off
else
    notify-send -i network-wireless-none "Wireless" "Wireless enabled"
    nmcli r wifi on
fi
exit 0
```

Une fois enregistré (chez moi dans `~/bin/wifi-toggle.sh`), il faut le rendre exécutable&nbsp;:

```bash
$ chmod +x /home/$USER/bin/wifi-toggle.sh
```

Ensuite, ouvrez le panneau de configuration du clavier de Xfce depuis le menu ou directement avec la commande&nbsp;:

```bash
$ xfce4-keyboard-settings
```
et rendez-vous dans l'onglet &laquo;&nbsp;Raccourcis d'applications&nbsp;&raquo; et ajouter un raccourci avec pour commande&nbsp;:

```
/home/$USER/bin/wifi-toggle.sh
```

puis le raccourci souhaité. Chez moi, le raccourci `Fn+F9` est bien reconnu dans l'utilitaire de Xfce.