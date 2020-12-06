---
title: 'Auto-hébergement, DynHost OVH et certificat Let''s Encrypt sous Raspbian'
date: '06-12-2020 08:06'
twitterenable: true
twittercardoptions: summary
facebookenable: true
---

Ayant «&nbsp;associé&nbsp;» ma station météo Netatmo à mon Raspberry Pi grâce à WeeWX ([lire ce post](/blog/station-meteo-netatmo-and-weewx)), je souhaite désormais rendre accessible sur Internet le site web ainsi généré. Je considère ainsi que vous avez déjà un serveur Apache fonctionnel et configuré (même si nous aborderons la création d'un VirtualHost par la suite).

## DynHost et mise à jour de l'IP

Étant chez OVH, je profite de leur service `DynHOST` qui «&nbsp;_permet de faire pointer un sous-domaine vers une adresse IP dynamique qui sera mise à jour dans votre zone DNS à chaque changement de celle-ci._&nbsp;».

On crée notre DynHost puis on en gère les accès, en créant un nouvel identifiant. Puis, sur notre Raspberry Pi, on installe `ddclient`&nbsp;:

```shell
sudo apt install ddclient
```

Lors de l'installation, des écrans successifs vont nous permettre de le configurer&nbsp;:
1. _Fournisseur de service de DNS dynamique_&nbsp;: `Autre`
2. _Protocole de mise à jour du DNS dynamique_&nbsp;: `dyndns2`
3. _Serveur de DNS dynamique_&nbsp;: `www.ovh.com`
4. _Mandataire HTTP_&nbsp;: néant
5. _Identifiant_&nbsp;: l'identifiant saisi précédemment (comprenant le nom de domaine en préfixe)
6. _Mot de passe_&nbsp;: le mot de passe correspondant
7. _Méthode de découverte d'adresse IP_&nbsp;: `Service de découverte d'IP basée sur le web`
8. _Hôtes à mettre à jour_&nbsp;: le DynHost créé précédemment

La configuration peut se faire sinon à la main, en éditant le fichier `/etc/ddclient.conf` qui doit ressembler à cela&nbsp;:

```
protocol=dyndns2
server=www.ovh.com
login=DOMAINE-USER
password='PASSWORD'
DYNHOST
```

## Redirection des ports

Ensuite, il faut se rendre dans l'interface de gestion de votre Box Internet et configurer, dans la section NAT, la redirection des port 80 et 443 vers ceux de votre Raspberry Pi (pour en connaître l'IP sur votre réseau local, utilisez la commande `hostname -I`).

Désormais, en saisissant l'URL de votre DynHost, vous devriez accéder à la même page que lorsque vous accédez à l'adresse `localhost`.

## Mise en place du HTTPS

Pour ce faire, nous allons générer un certificat Let's Encrypt pour notre DynHost.

On commence par copier, dans `/etc/apache2/sites-available/`, le fichier `000-default.conf` vers `DYNHOST.conf`&nbsp;; par exemple&nbsp;:

```shell
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/meteo.momh.fr.conf
```

puis par le configurer pour pointer vers notre répertoire `/var/www/html/weewx` ou autre répertoire en fonction du ou des skin(s) que vous utilisez. Par exemple, minimalement&nbsp;:

```
<VirtualHost *:80>
        ServerName meteo.momh.fr

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html/weewx

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>
```

Pour générer le certificat, nous allons utiliser `certbot`, qu'il convient d'installer&nbsp;:

```shell
sudo apt install certbot python3-certbot-apache
```

puis d'activer le module ssl d'Apache&nbsp;:

```shell
sudo a2enmode ssl
sudo systemctl restart apache2
```

On peut alors générer notre certificat avec la commande suivante&nbsp;:

```shell
sudo certbot --apache -d DynHOST
```

Il vous est proposé de rediriger l'éventuel traffic HTTP vers HTTPS&nbsp;:

```
Please choose whether or not to redirect HTTP traffic to HTTPS, removing HTTP access.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
1: No redirect - Make no further changes to the webserver configuration.
2: Redirect - Make all requests redirect to secure HTTPS access. Choose this for new sites, or if you're confident your site works on HTTPS. You can undo this change by editing your web server's configuration.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Select the appropriate number [1-2] then [enter] (press 'c' to cancel): 2
```

Un certificat est alors généré et un nouvel hôte virtuel est configuré dans Apache, à l'emplacement `/etc/apache2/sites-available/DYNHOST-le-ssl.conf`. Si vous avez choisi l'option 2, le VirtualHost présenté ci-dessus se voit modifié avec les lignes suivantes&nbsp;:

```
RewriteEngine on
RewriteCond %{SERVER_NAME} =DYNHOST
RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]
```

Pour voir que tout fonctionne, on peut utiliser [l'outil de test proposé par ssllabs.com](https://www.ssllabs.com/ssltest/analyze.html).