---
title: 'Scanner Epson WF-2865 en WiFi sous Debian/Raspbian'
date: '27-02-2021 19:53'
twitterenable: true
twittercardoptions: summary
facebookenable: true
---

Si l'imprimante de mon multifonction Epson WorkForce WF-2865 connecté en WiFi ne m'a guère posé problème, l'utilisation du scanner a été plus délicate...     

On commence par installer `sane`&nbsp;:

```shell
sudo apt install sane sane-utils
```

On édite le fichier `/etc/sane.d/dll.conf`&nbsp;:

```shell
sudo nano /etc/sane.d/dll.conf
```

et on décommente la ligne du pilote `epson2`.


Pour l'étape suivante, on va avoir besoin de connaître l'adresse de notre imprimante au sein de notre réseau&nbsp;; pour ce faire, on utilise l'utilitaire `nmap` qu'il convient d'installer&nbsp;:

```shell
sudo apt install nmap
```

On peut ensuite, pour effectuer un scan rapide, lancer la commande suivante&nbsp;:

```shell
sudo nmap -sn 192.168.1.0/24
```

et on obtient, entre autres&nbsp;:

```shell
[...]
Nmap scan report for 192.168.1.12
Host is up (-0.17s latency).
MAC Address: 38:9D:92:29:1C:FC (Seiko Epson)
[...]
```

Nous pouvons désormais éditer le ficier `/etc/sane.d/epson2.conf`&nbsp;:

```shell
sudo nano /etc/sane.d/epson2.conf
```

en modifiant la fin du fichier de la sorte&nbsp;:

```
#net autodiscovery
net 192.168.1.12
```


On se rend ensuite sur le site d'[Epson](https://support.epson.net/linux/en/imagescanv3.php) pour récupérer le logiciel ImageScan ainsi que les pilotes pour notre architecture.

Par exemple&nbsp;:

```shell
wget https://download2.ebz.epson.net/imagescanv3/common/deb/arm/imagescan-bundle-common-3.65.0.arm.deb.tar.gz
tar xvzf imagescan-bundle-common-3.65.0.arm.deb.tar.gz
cd imagescan-bundle-common-3.65.0.arm.deb/
sudo ./install.sh
```

On ouvre ensuite le fichier `/etc/imagescan/imagescan.conf`&nbsp;:

```shell
sudo nano /etc/imagescan/imagescan.conf 
```

et on le modifie pour obtenir cela&nbsp;:

```
[devices]

myscanner.udi    = esci:networkscan://192.168.1.12:1865
myscanner.vendor = Epson
myscanner.model  = WF2865
```

Désormais vous devriez voir votre scanner dans Imagescan et sane (et tous les logiciels qui s'appuient sur lui tels xsane ou Gimp) devrait trouver votre périphérique sans soucis !