---
title: 'Introduction à lftp, un client FTP en ligne de commande'
date: '16-09-2011 00:00'
taxonomy:
    category:
        - Linux
    tag:
        - console
        - ftp
        - lftp
---

[lftp](http://lftp.yar.ru/ "Site “officiel” de lftp") est un «&#8239;_programme en ligne de commande de transfert de fichiers pour Unix et systèmes d'exploitation apparentés […] écrit par Alexander Lukyanov et […] distribué sous Licence publique générale GNU._&#8239;» [[1]](http://fr.wikipedia.org/wiki/Lftp "lftp sur Wikipedia") Parmi ses très nombreuses fonctionnalités, lftp permet de «&#8239;_répliquer récursivement une arborescence entière de répertoires, l'enregistrement de signets d'adresses et la capacité à reprendre un téléchargement arrêté_&#8239;». De plus, lftp «&#8239;_peut entièrement s'insérer dans un script au lieu d'être utilisé de manière interactive par l'utilisateur._&#8239;»

Je viens de découvrir ce programme et m'en sers uniquement (pour le moment du moins) comme client FTP et de manière restreinte au vu de ses très nombreuses fonctionnalités&nbsp;; cet article se contentera donc de vous aider dans vos premiers pas.

===

Avant de commencer, il faut l'intaller&nbsp;:

```bash
$ sudo apt install lftp
```

Vous pouvez maintenant lancer lftp&nbsp;:
```bash
$ lftp
```

Mais que faire maintenant…

## Première connexion

Une fois que vous avez ouvert lftp, vous vous trouvez face à ce prompt&nbsp;:
```bash
lftp&nbsp;:~>
```

Pour vous connecter à votre serveur, il vous faut utiliser la commande suivante [dans le reste de cet article, avant chaque commande, j'ajouterai la «&nbsp;flèche&nbsp;» (>) du prompt]&nbsp;:
```bash
> open -u LOGIN SERVER
```

Une fois le mot de passe saisi, vous devriez obtenir un prompt correspondant à ceci&nbsp;:
```bash
lftp LOGIN@SERVER:~>
```

Vous êtes maintenant connecté à votre serveur.

Il est à noter que vous pouvez lancer lftp tout en vous connectant à votre serveur, en une seule commande, d'au moins deux façons&nbsp;:

- la commande suivante vous demandera votre mot de passe que vous aurez alors à taper sans qu'il n'apparaisse à l'écran&nbsp;:  
```bash
$ lftp -u LOGIN SERVER
```
- cette commande par contre, vous permet de tout faire d'un seul coup&nbsp;:  
```bash
$ lftp LOGIN:PASSWORD@SERVER
```

Il est encore possible de faire plus simple. Une fois connecté à votre serveur, vous pouvez créer un signet [voir ci-dessous] qui pointe vers votre racine (le répertoire de base)&nbsp;:
```bash
> bookmark add NOM
```

Bien évidemment vous remplacez NOM par le nom que vous souhaitez. Ainsi, pour lancer lftp et vous connecter vous n'aurez plus qu'à saisir la commande suivante&nbsp;:
```bash
$ lftp NOM
```
 

Pour vous déconnecter, utilisez une des trois commandes suivantes&nbsp;:
```bash
> quit
> exit
> bye
```

## Navigation

### Bases

La navigation au sein de votre serveur avec lftp fonctionne exactement comme celle au sein de vos dossiers sur votre poste de travail avec le terminal et en reprend en partie les commandes. Notez que vous bénéficiez de l'autocomplétion tant au niveau des commandes que des noms de fichiers avec la touche tab.

Avant d'aller plus loin, appuyer deux fois sur la touche tab permet de voir l'ensemble des commandes disponibles&nbsp;; parmi elles, la commande _help_ qui lancée permet de voir l'ensemble des fonctions avec leur utilisation&nbsp;:

```bash
> help
    !<commande-shell>                    (commandes)                          alias [<nom> [<valeur>]]             attach [PID]
    bookmark [SOUS-COMMANDE]             cache [SOUS-COMMANDE]                cat [-b] <fichiers>                  cd <rép-dis>
    chmod [OPTS] mode fichier...         close [-a]                           [re]cls [opts] [chemin/][motif]      debug [OPTS] [<level>|off]
    du [options] <répertoires>           edit [OPTS] <file>                   exit [<code>|bg]                     get [OPTS] <fichier-distant> -o <fichier-local>]
    glob [OPTS] <cmd> <args>             help [<cmd>]                         historique : -w fichier|-r fichier|-c|-l [num]
    jobs [-v] [<job_no...>]              kill all|<num>                       lcd <répertoire-local>               lftp [OPTS] <site>
    ln [-s] <file1> <file2>              ls [<args>]                          mget [OPTS] <fichiers>               mirror [OPTS] [distant [local]]
    mkdir [OPTS] <dirs>                  module nom [args]                    more <fichiers>                      mput [OPTS] <fichiers>               mrm <fichiers>
    mv <fichier1> <fichier2>             mmv [OPTS] <files> <target-dir>      [re]nlist [<args>]                   open [OPTS] <site>
    pget [OPTS] <fichier-distant> [-o <fichier-local>]                        put [OPTS] <fichier-local> [-o <fichier-distant>]                         pwd [-p]
    queue [OPTS] [<cmd>]                 quote <cmd>                          repeat [OPTS] [délai] [commande]     rm [-r] [-f] <fichiers>
    rmdir [-f] <répertoires>             scache [<num_session>]               set [OPT] [<var> [<val>]]            site <site-cmd>
    source <fichier>                     torrent [OPTS] <file|URL>...         user <user|URL> [<pass>]             wait [<num_travail>]                 zcat <fichiers>
    zmore <fichiers>
```

La commande _help_ peut prendre pour argument le nom d'une autre commande ce qui permet d'obtenir un descriptif sur cette dernière&nbsp;:
```bash
> help cls
```

De plus, les commandes semblables à celles que vous utilisez localement, sont «&nbsp;inversables&nbsp;» en les faisant précédé du caractère _!_. Ainsi, si pour lister les éléments présents sur votre serveur, vous utilisez cette commande&nbsp;:
```bash
> ls
```

pour lister les fichiers sur votre ordinateur vous utiliserez&nbsp;:
```bash
> !ls
```

Bien sûr, il est possible d'utiliser des options et des arguments&nbsp;; ainsi&nbsp;:
```bash
> !ls -al REP/
```

Vous noterez que par défaut les options _a_ et _l_ sont inutiles côté serveur, la commande _ls_ les appelant automatiquement. Si vous souhaitez lister les fichiers de votre serveur et obtenir un simple listing, utilisez la commande _cls_ qui elle aussi peut prendre un chemin comme argument&nbsp;:
```bash
> cls REP/
```

Pour afficher récursivement le contenu du répertoire courant ou spécifié en argument, vous pouvez également utiliser la commande _find_.

Pour naviguer dans les répertoires distants ou locaux&nbsp;:
```bash
> cd REP/
> !cd REP
```

Par contre avec cette dernière commande (_!cd REP/_) le changement de dossier local n'est que temporaire&nbsp;; pour que le changement de dossier local soit «&nbsp;retenu&nbsp;», il faut utiliser la commande _lcd_&nbsp;:
```bash
> lcd REP/
```

Si vous ne risquez pas de vous perdre dans les répertoires de votre serveur du fait du prompt, cela peut arriver sur votre poste&nbsp;; pour connaître le répertoire local où vous vous trouvez&nbsp;:
```bash
> !pwd
/home/bbrice
```

Il est possible de combiner plusieurs commandes&nbsp;:
```bash
> cd REP/ && ls
```

Par contre, pour faire de même localement, il ne faut pas «&nbsp;échapper&nbsp;» (ajout du caractère _!_) la seconde commande :
```bash
> !cd REP/ && ls
```

### Signets

Comme nous l'avons déjà évoqué, lftp propose un système de signets (_bookmarks_). Pour créer un signet sur le répertoire courant, il faut utilise la commande&nbsp;:
```bash
> bookmark add NOM
```

Pour ensuite vous rendre à ce signet&nbsp;:
```bash
> open NOM
```

Pour lister l'ensemble des signets disponibles, vous pouvez utiliser la sous-commande _list_ mais vous pouvez vous en passer&nbsp;:
```bash
> bookmark
```

Pour effacer un signet&nbsp;:
```bash
> bookmark delete NOM
```

Vous pouvez également éditer directement la liste des signets, lftp ouvrant alors soit vi soit emacs (selon votre configuration)&nbsp;:
```bash
> bookmark edit
```

## Envoi et récupération de données

Pour copier un fichier situé sur votre serveur vers le dossier local courant, il suffit de lancer la commande&nbsp;:
```bash
> get FILE
```

Par défaut, lftp attribue au fichier local le même nom que le fichier distant que l'on a copié&nbsp;; pour remédier à cela, il faut alors utiliser l'option _-o nom\_local_. De plus, il est possible de télécharger plusieurs fichiers à la fois (tout en leur attribuant un nom local différent de leur nom distant) et de spécifier le répertoire local où télécharger les fichier (option _-O base_)&nbsp;:
```bash
> get -O REP_LOCAL/ FICHIER_DISTANT1 -o NOM_LOCAL1 FICHIER_DISTANT2 -o NOM_LOCAL2
```

Pour envoyer un fichier local vers le répertoire courant de votre serveur, il faut utiliser la commande _put_&nbsp;:
```bash
> put FILE
```

Comme pour la commande _get_, il est possible d'uploader plusieurs fichiers tout en les renommant vers un répertoire spécifique en une seule commande&nbsp;:
```bash
> put -O REP_DISTANT/ FICHIER_LOCAL1 -o NOM_DISTANT1 FICHIER_LOCAL2 -o NOM_DISTANT2
```

Si vous avez besoin de créer un dossier [ou plusieurs], il vous faudra utiliser la commande _mkdir_ (elle aussi «&nbsp;inversable&nbsp;» avec _!_)&nbsp;:
```bash
> mkdir REP [REP2 REP3]
```

Pour effacer un fichier [ou plusieurs], il faudra alors utiliser la commande _rm_ (« inversable » aussi bien évidemment) ; pour effacer un dossier il faudra alors utiliser l'option _-r_ (mais faîtes attention…) :
```bash
> rm FILE [FILE2 FILE3]
```
 

Il est également possible de dupliquer un répertoire distant vers le répertoire local et inversement grâce à la commande _mirror_. Pour dupliquer un répertoire distant dans le répertoire local courant&nbsp;:
```bash
> mirror REP_DISTANT
```

Notez que le répertoire local est créé automatiquement. Vous pouvez cependant spécifier un répertoire local (sans l'avoir créé précédemment) dans lequel le répertoire distant sera copié&nbsp;:
```bash
> mirror REP_DISTANT REP_LOCAL
```

L'option _-R_ permet de dupliquer un répertoire local vers un répertoire distant&nbsp;:
```bash
> mirror -R REP_LOCAL/ REP_DISTANT
```

Attention : si vous ajoutez un slash à la fin de _REP\_DISTANT_ le répertoire local sera créé dans ce répertoire distant ; un exemple valant parfois mieux qu'un long discours, constatez la différence entre :
```bash
> mirror -R TEST/memoire/ TEMPtest
[...]
> cd
> cd TEMPtest/ && cls
css/ images/ index.php javascript/ maps/ menu.php pages/ stats.php
```
et&nbsp;:
```bash
> mirror -R TEST/memoire/ TEMPtest/
[...]
> cd TEMPtest/ && cls
memoire/
> cd TEMPtest/memoire && cls
css/ images/ index.php javascript/ maps/ menu.php pages/ stats.php
```

Parmi les nombreuses options de la commande _mirror_, nous retenons celle permettant de limiter le transfert aux seuls nouveaux fichiers, l'option _-n_ (ou _--only-newer_)&nbsp;:
```bash
> mirror -R TEST/memoire/ TEMPtest
Total&nbsp;: 21 répertoires, 122 fichiers, 0 liens symboliques 
Nouveau&nbsp;: 122 fichiers, 0 liens symboliques
[...]
[//On créé un fichier]
> !touch TEST/memoire/TEST.php
[//On relance la duplication avec l'option -n]
> mirror -Rn TEST/memoire/ TEMPtest
Total&nbsp;: 21 répertoires, 123 fichiers, 0 liens symboliques 
Nouveau&nbsp;: 1 fichier, 0 liens symboliques
```

Cet article s'achève ici, dans la mesure où je suis presque à bout de ce que je connais de lftp et où cet article est déjà bien assez long… La suite [au prochain épisode](/blog/lftp_non_interactive).