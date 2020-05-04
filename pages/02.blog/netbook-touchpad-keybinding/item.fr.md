---
title: 'Activer/Désactiver le touchpad avec un raccourci clavier sous Xubuntu sur netbook ASUS F200M'
date: '02-03-2015 00:00'
taxonomy:
    category:
        - Linux
    tag:
        - keybinding
        - touchpad
        - Xfce
        - 'ASUS F200M'
twitterenable: true
twittercardoptions: summary
facebookenable: true
---

Dans un [article précédent](/blog/netbook-keybindings), j'avais présenté un court script permettant d'activer et de désactiver le touchpad sur mon netbook ASUS F200M sous Xubuntu.

Après la mise à jour vers la dernière version de Xubuntu (14.10), ce raccourci clavier ne fonctionne plus, le script n'étant plus opérant. Toutes les tentatives que j'ai faites avec des scripts utilisant _synclient_ ont échoué.

Du coup, la lecture de [ce thread sur le forum Ubuntu anglophone](http://ubuntuforums.org/showthread.php?t=2141992) m'a permis de découvrir _xinput_ et un script me permettant de pouvoir activer ou désactiver mon touchpad facilement.

On crée donc le script "trackpad-toggle.sh"&nbsp;:

```bash
#!/bin/bash
id=$(xinput --list --id-only 'ETPS/2 Elantech Touchpad')
devEnabled=$(xinput --list-props $id | awk '/Device Enabled/{print&nbsp;!$NF}')
xinput --set-prop $id 'Device Enabled' $devEnabled
notify-send --icon computer 'Synaptics TouchPad' &quot;Device Enabled = $devEnabled&quot;
```

Une fois enregistré (chez moi dans `bin/trackpad-toggle.sh`), il faut le rendre exécutable&nbsp;:
```bash
$ chmod +x /home/$USER/bin/trackpad-toggle.sh
```

Ensuite, ouvrez le panneau de configuration du clavier de Xfce depuis le menu ou directement avec la commande&nbsp;:
```bash
$ xfce4-keyboard-settings
```
et rendez-vous dans l'onglet &laquo;&nbsp;Raccourcis d'applications&nbsp;&raquo; et ajouter un raccourci avec pour commande&nbsp;:
```
/home/$USER/bin/trackpad-toggle.sh
```
puis le raccourci souhaité. Chez moi, le raccourci `Fn+F9` est bien reconnu dans l'utilitaire de Xfce.