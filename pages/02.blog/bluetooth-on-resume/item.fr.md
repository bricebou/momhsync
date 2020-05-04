---
title: 'Connexion Bluetooth au retour de veille sous Lubuntu 18.04 sur ASUS F200M'
date: '07-07-2018 00:00'
taxonomy:
    category:
        - Linux
    tag:
        - 'ASUS F200M'
        - bluetooth
---

Sous Lubuntu 18.04, il m'était impossible de me re-connecter en Bluetooth à mon ampli Hi-Fi au retour de la veille.    
Et aucune des solutions repérées sur le forum de la communauté anglophone d'Ubuntu, ni celle de [Ferux](https://ubuntuforums.org/showthread.php?t=1387211&s=3564333119c56faa1374ad23baadb419&p=8728266#post8728266), ni celle de [ lotharmat](https://ubuntuforums.org/showthread.php?t=1746771&p=10755857#post10755857) ne me permettait de re-connecter mon ampli, bien qu'il soit bien enregistré comme de confiance. J'avais toujours le même message d'erreur&nbsp;:

```bash
Connection Failed: blueman.bluez.errors.DBusFailedError: Resource temporarily unavailable
```
===

## Une solution simple et définitive &nbsp;!

La solution vient de [Halka sur askubuntu.com](https://askubuntu.com/a/1037065), bien qu'il ne faille pas se contenter d'une version `>=5.28.2` de `bluez`&nbsp;: la version disponible dans les dépôts de Lubuntu 18.04 est la `5.48-0ubuntu3` (au 7 juillet 2018)...

Par contre, l'utilisation de la version que propose la «&nbsp;Ubuntu Bluetooth team&nbsp;» via son PPA, la `5.50-0ubuntu0ppa1` (au 7 juillet 2018) permet de résoudre le problème.     
Donc, on fait tout simplement&nbsp;:

```bash
$ sudo add-apt-repository ppa:bluetooth/bluez
$ sudo apt-get update && sudo apt-get upgrade
```

## Si ça ne marche pas... comment retrouver mes connections Bluetooth&nbsp;?

J'utilisais le programme `bluetoothctl` et devais retirer l'appareil avant de l'appairer à nouveau et de le connecter...

```bash
$ bluetoothctl 
Agent registered
[bluetooth]# disconnect FC:58:FA:14:27:BC 
Attempting to disconnect from FC:58:FA:14:27:BC
Successful disconnected
[bluetooth]# connect FC:58:FA:14:27:BC
Attempting to connect to FC:58:FA:14:27:BC
[CHG] Device FC:58:FA:14:27:BC Connected: yes
Connection successful
[CHG] Device FC:58:FA:14:27:BC ServicesResolved: yes
[Tangent Ampster BT]#
```

Si la seule déconnexion ne suffit pas, il faut alors complètement retirer le périphérique&nbsp:

```bash
$ bluetoothctl
Agent registered
[Tangent Ampster BT]# remove FC:58:FA:14:27:BC
[CHG] Device FC:58:FA:14:27:BC ServicesResolved: no
Device has been removed
[CHG] Device FC:58:FA:14:27:BC Connected: no
[DEL] Device FC:58:FA:14:27:BC Tangent Ampster BT
[bluetooth]# scan on
Discovery started
[CHG] Controller 54:27:1E:C9:81:80 Discovering: yes
[NEW] Device FC:58:FA:14:27:BC Tangent Ampster BT
[bluetooth]# pair FC:58:FA:14:27:BC
Attempting to pair with FC:58:FA:14:27:BC
[CHG] Device FC:58:FA:14:27:BC Connected: yes
[CHG] Device FC:58:FA:14:27:BC UUIDs: 00001101-0000-1000-8000-00805f9b34fb
[CHG] Device FC:58:FA:14:27:BC UUIDs: 0000110b-0000-1000-8000-00805f9b34fb
[CHG] Device FC:58:FA:14:27:BC UUIDs: 0000110c-0000-1000-8000-00805f9b34fb
[CHG] Device FC:58:FA:14:27:BC UUIDs: 0000110e-0000-1000-8000-00805f9b34fb
[CHG] Device FC:58:FA:14:27:BC UUIDs: 00001200-0000-1000-8000-00805f9b34fb
[CHG] Device FC:58:FA:14:27:BC ServicesResolved: yes
[CHG] Device FC:58:FA:14:27:BC Paired: yes
Pairing successful
[CHG] Device FC:58:FA:14:27:BC ServicesResolved: no
[CHG] Device FC:58:FA:14:27:BC Connected: no
[bluetooth]# connect FC:58:FA:14:27:BC
Attempting to connect to FC:58:FA:14:27:BC
[CHG] Device FC:58:FA:14:27:BC Connected: yes
Connection successful
[CHG] Device FC:58:FA:14:27:BC ServicesResolved: yes
[Tangent Ampster BT]#
```

Quelque peu pénible... le plus simple étant de redémarrer le service Bluetooth&nbsp;:

```bash
$ sudo service bluetooth restart
```

puis de reconnecter l'ampli depuis l'applet Blueman.