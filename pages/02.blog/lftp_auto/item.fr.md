---
title: 'lftp : automatiser les transferts avec des scripts'
date: '18-09-2011 00:00'
taxonomy:
    category:
        - Linux
    tag:
        - console
        - ftp
        - lftp
---

Après avoir présenté le [mode interactif de lftp](/blog/lftp "Présentation de lftp et utilisation de son mode interactif"), client FTP en ligne de commande, et exploré son [utilisation en ligne de commande](/blog/lftp_non_interactive "Utiliser lftp en ligne de commande"), nous allons maintenant voir qu'il est possible d'automatiser les transferts, que ce soit par le biais d'alias bash ou grâce à des scripts propres à lftp. En effet, lftp permet d'appeler des scripts extrêmement simples à écrire dans la mesure où chaque ligne correspond à une instruction.

## Création d'alias

Une fois que vous avez mis au point la ligne de commande permettant de réaliser les opérations récurrentes (si vous rencontrez des problèmes, reportez-vous à notre [article précédent](lftp_non_interactive "Utiliser lftp sans le mode interactif")), vous pouvez créer un alias bash afin de ne pas avoir à la saisir ou à la rechercher dans votre historique à chaque fois que vous en avez besoin.

Pour créer un alias, il vous faut éditer le fichier _.bashrc_ :
```bash
$ nano ~/.bashrc
```

Un alias se présente sous cette forme :
```
alias nom_souhaité='commande'
```

Prenons la commande lftp permettant de faire, dans notre cas, une sauvegarde de son site, en ne prenant en compte que les nouveaux fichiers tout en ignorant le répertoire tmp/, vers notre bureau&nbsp;:

```bash
$ lftp racine -e "mirror -n -x tmp /www/ /home/bbrice/Bureau/Sauvegarde; quit;"
```

Et voici l'alias que l'on peut créer :
```
alias sitesauv='lftp racine -e "mirror -n -x tmp /www/ /home/bbrice/Bureau/Sauvegarde; quit;"'
```

Une fois votre alias ajouté à la fin de votre _.bashrc_, enregistrez (Ctrl+o) puis quittez (Ctrl+x) avant de quitter et de relancer votre console. Vous pouvez maintenant utiliser votre alias pour lancer l'opération de transfert.

## Utiliser les scripts lftp

Cette fonctionnalité de lftp est ce qui en fait mon client FTP favori, notamment lorsqu'il s'agit de mettre à jour ma collection de bandes dessinées réalisée avec [GCstar](GCstar "Présentation de GCstar, logiciel de gestion de collections") vers mon serveur pour qu'elle soit [accessible en ligne](/mylibrary/?collec=0&model=list "Accéder à ma collection de bandes dessinées") grâce à [GCweb](../GCweb "Accéder aux articles consacrés à GCweb"), un frontend web à GCstar.

Imaginons le cas où vous êtes amenés à régulièrement uploader les images de votre répertoire local Images vers le répertoire images de votre serveur distant.

Vous pouvez créer un fichier, appelé _upload\_images_ par exemple, dans lequel vous ne trouveriez que cette ligne&nbsp;:
```
mirror -R ~/Images images
```

Pour exécuter l'opération à partir de votre script, il vous faudra soit :

- une fois connecté à votre serveur FTP, lancer la commande :  
```bash
source upload_images
```
- ou bien en utilisant l'option _-e _permettant de lancer la commande directement&nbsp;:

```bash
lftp LOGIN:PASSWORD@SERVER -e "source upload_images; quit;"
```
en choisissant bien évidemment le mode d'identification qui vous convient le mieux (directement, avec prompt, à travers un signet).

Il est également possible de réaliser l'authentification au sein du script 
```
open LOGIN:PASSWORD@SERVER
mirror -R ~/Images images
```

Si vous avez un signet pointant vers la racine de votre site, vous pouvez modifier le script de la sorte :
```
open NOM_SIGNET
mirror -R ~/Images images
```

Désormais, pour lancer le transfert, il vous suffit de lancer la commande :
```bash
$ lftp -f upload_images
```

Même si cette commande est assez simple, vous pouvez en créer un alias comme vu précédemment.

Pour en savoir plus sur l'utilisation des scripts, vous pouvez vous rendre à la section «  [Scripts](http://mewbies.com/lftp/lftp.html#scripts "Script sur le tutoriel (en anglais) réalisé par Peter Matulis") » du tutoriel (en anglais) rédigé par Peter Matulis.

Enfin, vous pouvez utiliser [cron](http://doc.ubuntu-fr.org/cron "cron sur la documentation d'ubuntu-fr.org") afin de lancer périodiquement et automatiquement une commande, que celle-ci soit un simple alias ou qu'elle fasse appel à un script, les deux méthodes pouvant d'ailleurs être complémentaires.