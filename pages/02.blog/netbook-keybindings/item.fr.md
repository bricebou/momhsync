---
title: 'Raccourcis clavier de réglages système avec Xubuntu sur netbook ASUS F200M'
date: '17-08-2014 00:00'
taxonomy:
    category:
        - Linux
    tag:
        - keybinding
        - touchpad
        - Xfce
        - 'ASUS F200M'
        - wifi
twitterenable: true
twittercardoptions: summary
facebookenable: true
---

Sur mon netbook ASUS F200M, je n'ai rencontré aucun souci à l'installation de la dernière version de [Xubuntu](http://xubuntu.org/), la 14.04, et tout semble fonctionner parfaitement _out of the box_.

Hormis quelques touches de fonction du clavier telles celles concernant le réglage de la luminosité, le touchpad, le mode avion...

## Activer / désactiver le touchpad

Si le touchpad est d'emblée fonctionnel et configurable à travers le panneau de configuration de Xubuntu, la combinaison `Fn+F9` ne donne aucun résultat.

Après de nombreuses recherches et d'essais je suis finalement tombé sur la documentation anglophone d'ArchLinux et sur son article [Touchpad Synaptics](https://wiki.archlinux.org/index.php/Touchpad_Synaptics) qui donne une solution, que j'ai mixée avec deux ou trois détails venant d'ailleurs...

Commencer par créer un répertoire `bin` dans votre `/home/$USER`&nbsp;:

```bash
$ mkdir /home/$USER/bin
```

Créez un fichier&nbsp;:
```bash
$ nano /home/$USER/bin/trackpad-toggle.sh
```
dans lequel vous copiez ce script&nbsp;:
```bash
#!/bin/bash
synclient TouchpadOff=$(synclient -l | grep -c 'TouchpadOff.*=.*0')
```

Rendez-le exécutable&nbsp;:
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

__MAJ__&nbsp;: sous Xubuntu 14.10, ce script ne fonctionnait plus&nbsp;; pour une nouvelle version, rendez-vous sur [Activer/Désactiver le touchpad avec un raccourcis clavier sous Xubuntu sur netbook ASUS F200M](/blog/netbook-touchpad-keybinding).


## Activer / désactiver le wifi

Là encore aucun problème de wifi après l'installation à l'exception du bouton &laquo;&nbsp;mode avion&nbsp;&raquo; (`Fn+F2`) qui ne fonctionne pas, que ce soit pour le Bluetooth ou le wifi.

Du coup, comme pour le touchpad je souhaitais pouvoir désactiver le wifi très rapidement sans avoir à utiliser une souris ou le touchpad.

Cette fois, les remerciements doivent aller à Gerhard Burger on [askubuntu.com](http://askubuntu.com/questions/240515/how-to-toggle-wireless-using-a-script).

Créez le fichier&nbsp;:
```bash
$ nano /home/$USER/bin/wifi-toggle.sh
```
et collez-y ce script&nbsp;:
```bash
#!/bin/bash
status=$(nmcli -t -f WIFI nm)
if [ $status = "enabled" ]&nbsp;; then
    notify-send -i network-wireless-disconnected "Wireless" "Wireless disabled"
    nmcli nm wifi off
else
    notify-send -i network-wireless-none "Wireless" "Wireless enabled"
    nmcli nm wifi on
fi
exit 0
```

__ATTENTION__&nbsp;: en fonction de la langue de votre système, vous devrez modifier la ligne 3 (`enabled`) en fonction de la réponse à la commande&nbsp;:
```bash
$ nmcli -t -f WIFI nm
```

Si vous ne souhaitez pas voir apparaître de notifications lors de l'activation ou la désactivation du wifi, il faut commenter les lignes 4 et 7.

Enregistrez et rendez-le exécutable&nbsp;:
```bash
$ chmod +x /home/$USER/bin/wifi-toggle.sh
```

Puis créer un nouveau raccourci clavier en utilisant la commande&nbsp;:

```
/home/$USER/bin/wifi-toggle.sh
```

avec le raccourci de votre choix. Chez moi le raccourci `Fn+F2` n'est pas reconnu et du coup j'ai choisi `Super+F2`.

__MAJ__&nbsp;: sous Xubuntu 15.04, ce script ne fonctionnait plus&nbsp;; pour une nouvelle version, rendez-vous sur [Activer/Désactiver le Wifi avec un raccourci clavier sous Xubuntu 15.04 sur netbook ASUS F200M](/blog/netbook_wifi_keybinding).

## Régler la luminosité

Après l'installation, impossible de régler la luminosité de l'écran avec les raccourcis clavier `Fn+F5` et `Fn+F6`.

J'ai commencé par installer les derniers pilotes grâce à l'application fournie par Intel comme expliqué dans la [documentation Ubuntu francophone](http://doc.ubuntu-fr.org/intel_graphics#installer_les_derniers_pilotes_intel).

Au redémarrage toujours rien... Quelques recherches plus tard, j'installe le paquet `xfce4-power-manager-plugins` qui fournit l'applet de tableau de bord &laquo;&nbsp;Régler la luminosité&nbsp;&raquo;&nbsp;:

```bash
$ sudo apt-get install xfce4-power-manager-plugins
```
Mais même avec l'applet, rien à faire.

De nouveau, quelques heures de recherche en tout genre pour finalement découvrir l'article [Backlight de wiki.ubuntu.com](https://wiki.ubuntu.com/Kernel/Debugging/Backlight).

On commence par vérifier que le répertoire `/sys/class/backlight/intel_backlight/` existe bien. Si non, je ne sais que dire&nbsp;; si oui, c'est tout bon.    
On ouvre le fichier `/etc/default/grub`
```bash
$ sudo nano /etc/default/grub
```
et on modifie la ligne

```
GRUB_CMDLINE_LINUX_DEFAULT=&quot;quiet splash&quot;
```
en
```
GRUB_CMDLINE_LINUX_DEFAULT=&quot;quiet splash video.use_native_backlight=1&quot;
```
Puis on lance la commande&nbsp;:
```shell
$ sudo update-grub
```
On redémarre et on doit pouvoir régler la luminosité grâce à l'applet du tableau de bord.

On commence par installer l'utilitaire `xbacklight`&nbsp;:
```bash
$ sudo apt-get install xbacklight
```
On lance la commande 
```bash
$ xbacklight
```
et l'on doit avoir comme retour `100.000000` ou peut-être une autre valeur.

Ensuite essayez de lancer les commandes suivantes&nbsp;:
```bash
$ xbacklight -dec 20
$ xbacklight -inc 20
```
La luminosité de votre écran devrait varier. Pour plus d'information sur le fonctionnement de `xbacklight`&nbsp;:
```bash
$ xbacklight -h
usage: xbacklight [options]
  where options are:
  -display <display> or -d <display>
  -help
  -set <percentage> or = <percentage>
  -inc <percentage> or + <percentage>
  -dec <percentage> or - <percentage>
  -get
  -time <fade time in milliseconds>
  -steps <number of steps in fade>
```

Les combinaisons `Fn+F5` et `Fn+F6` n'étant pas reconnues, on peut créer deux nouveaux raccourcis clavier, le premier attribué à la combinaison `Super+F5` avec la commande
```bash
$ xbacklight -dec 20
```
le second associé à la combinaison `Super+F6` avec la commande
```bash
$ xbacklight -inc 20
```

Vous devriez pouvoir vérifier la luminosité de l'écran de deux façon&nbsp;:

- soit avec la commande 
```bash
$ xbacklight
```
qui retourne un pourcentage&nbsp;;

- soit avec la commande 
```bash
$ cat /sys/class/backlight/intel_backlight/brightness
```
qui renvoie la valeur absolue de la luminosité, la valeur maximale étant accessible avec la commande 
```bash
$ cat /sys/class/backlight/intel_backlight/max_brightness
```
  
À noter que l'on peut également jouer sur le contraste en utilisant `xgamma`&nbsp;:
```bash
$ xgamma -gamma 0.7
```

## La mise en veille ne fonctionne plus&nbsp;?

Après ces manipulations et la modification du fichier `/etc/default/grub` (ou peut-être dès le départ -- je ne sais plus trop), la mise en veille ne fonctionnait plus. Une lecture rapide de la page [_BootOptions_ sur help.ubuntu.com](https://help.ubuntu.com/community/BootOptions#Changing_boot_options_Permanently_for_an_Existing_Installation) permet de repérer l'option `acpi_osi=Linux`.  
Du coup, on modifie le fichier `/etc/default/grub`
```bash
$ sudo nano /etc/default/grub
```
et on modifie la ligne
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash video.use_native_backlight=1";
```
en
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash video.use_native_backlight=1 acpi_osi=Linux"
```
Puis on lance la commande&nbsp;:

```bash
$ sudo update-grub
```
et on redémarre. Vous devriez maintenant bénéficier de la mise en veille.