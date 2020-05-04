---
title: 'Utiliser lftp sans le mode interactif'
date: '17-09-2011 00:00'
taxonomy:
    category:
        - Linux
    tag:
        - ftp
        - lftp
        - console
---

Dans un [précédent article](/blog/lftp "lftp&nbsp;: un client FTP en ligne de commande"), nous avons présenté le client FTP en ligne de commande [lftp](http://lftp.yar.ru/ "Le site de lftp") et son utilisation en nous limitant au mode interactif, qui permet de naviguer au sein de l'arborescence distante et des dossiers locaux, de transférer des fichiers... le tout de manière «&nbsp;visuelle&nbsp;». lftp permet cependant de se passer de ce mode interactif grâce à l'option _-e_ qui prend pour argument une ou plusieurs commandes, ce qui permet de lancer en une seule commande de multiples tâches.

## Uploader des fichiers

Comme nous l'avons vu précédemment, il va donc falloir nous connecter à notre serveur FTP en utilisant la commande&nbsp;:

```bash
$ lftp LOGIN:PASSWORD@SERVER
```

mais en ajoutant à cette commande l'option _-e_ avec pour argument entre guillemets la commande ou les commandes à exécuter. Par exemple, pour envoyer un fichier de votre ordinateur vers la racine de votre serveur, vous utiliserez la commande&nbsp;:

```bash
$ lftp LOGIN:PASSWORD@SERVER -e "put MON_FICHIER_LOCAL;"
```

Il est à noter que vous pouvez, pour des raisons de sécurité, ne pas préciser votre mot de passe en clair dans la commande, celui-ci vous étant alors demandé par un prompt&nbsp;:

```bash
$ lftp LOGIN@SERVER -e "put MON_FICHIER_LOCAL;"
```

Vous pouvez également, si vous êtes pressé et avez créé des signets, vous connecter ainsi&nbsp;:

```bash
$ lftp SIGNET -e "put MON_FICHIER_LOCAL;"
```

Si vous souhaitez quitter lftp suite à votre transfert, il s'agit de rajouter la commande _quit_ dans l'argument&nbsp;:
```bash
$ lftp LOGIN:PASSWORD@SERVER -e "put MON_FICHIER_LOCAL; quit;"
```

Si vous désirez, uploader votre fichier vers un répertoire distant autre que la racine, deux options s'offrent à vous :

- soit vous utilisez la commande _cd _afin de vous déplacer dans le répertoire souhaité&nbsp;:
```bash
$ lftp LOGIN:PASSWORD@SERVER -e "cd REP_DISTANT; put MON_FICHIER_LOCAL; quit;"
```
- soit vous utilisez l'option _-O_ de la commande _put _qui vous permet de spécifier le répertoire distant où uploader le fichier&nbsp;:  
```bash
$ lftp LOGIN:PASSWORD@SERVER -e "put -O REP_DISTANT/ MON_FICHIER_LOCAL; quit;"
```

Si vous désirez que le fichier uploadé porte un nom différent que le nom du fichier local, il vous faut utiliser l'option _-o_ de la commande _put_&nbsp;:
```bash
$ lftp LOGIN:PASSWORD@SERVER -e "put -O REP_DISTANT/ MON_FICHIER_LOCAL -o NOUVEAU_NOM; quit;"
```

Vous pouvez bien sûr en une seule commande uploader plusieurs fichiers&nbsp;:
```bash
$ lftp LOGIN:PASSWORD@SERVER -e "put -O REP_DISTANT/ MON_FICHIER_LOCAL -o NOUVEAU_NOM MON_FICHIER_LOCAL_2 -o NOUVEAU_NOM_2; quit;"
```

## Télécharger des fichiers

Pour récupérer des fichiers de votre serveur, l'utilisation est similaire si ce n'est qu'il faut utiliser la commande _get_ à la place de _put_. Ainsi pour télécharger un fichier vers le répertoire local courant, il vous faut utiliser la commande suivante&nbsp;:
```bash
$ lftp LOGIN@SERVER -e "get MON_FICHIER_DISTANT; quit;"
```

Pour télécharger un fichier vers un répertoire local autre que le répertoire courant, il vous faut utiliser l'option _-O_ de la commande _get_ (notez qu'il faut que le répertoire existe)&nbsp;:
```bash
$ lftp LOGIN@SERVER -e "get -O REPERTOIRE_LOCAL MON_FICHIER_DISTANT; quit;"
```

Vous pouvez également renommer localement le fichier téléchargé grâce à l'option _-o_&nbsp;:
```bash
$ lftp LOGIN@SERVER -e "get MON_FICHIER_DISTANT -o NOM_FICHIER_LOCAL; quit;"
```

Vous pouvez bien évidemment là encore téléchargé plusieurs fichiers en une seule commande&nbsp;:
```bash
$ lftp LOGIN@SERVER -e "get MON_FICHIER_DISTANT -o NOM_FICHIER_LOCAL MON_FICHIER_DISTANT_2 -o NOM_FICHIER_LOCAL_2; quit;"
```

## Dossiers, sauvegarde et mise à jour

Nous allons maintenant nous intéresser à la commande _mirror_ de lftp qui permet de copier des dossiers de votre ordinateur vers votre serveur et inversement et donc de faire des sauvegardes aisément de votre site ou de mettre ce dernier à jour en ne prenant en compte que les nouveaux éléments.

Ainsi pour copier de votre serveur vers votre ordinateur un dossier vous utiliserez la commande suivante&nbsp;:
```bash
$ lftp LOGIN:PASSWORD@SERVER -e "mirror REPERTOIRE_DISTANT; quit;"
```

Vous noterez que le répertoire local est créé automatiquement. Vous pouvez spécifier un répertoire local (qui sera également créé automatiquement) où l'ensemble des dossiers et fichiers du dossier distant sera copié&nbsp;:
```bash
$ lftp LOGIN:PASSWORD@SERVER -e "mirror REPERTOIRE_DISTANT REPERTOIRE_LOCAL; quit;"
```

Pour copier un dossier de votre ordinateur vers la racine de votre serveur, il vous faudra alors utiliser l'option _-R_ de la commande _mirror_ comme ceci&nbsp;:
```bash
$ lftp LOGIN:PASSWORD@SERVER -e "mirror -R REPERTOIRE_LOCAL; quit;"
```

Si le répertoire n'existe pas sur votre serveur, il sera créé automatiquement. Vous pouvez spécifier un répertoire de destination sur votre serveur qui sera là aussi créé automatiquement s'il n'existe pas&nbsp;:
```bash
$ lftp LOGIN:PASSWORD@SERVER -e "mirror -R REPERTOIRE_LOCAL REPERTOIRE_DISTANT; quit;"
```

Notez bien que si vous ajoutez un slash (/) à la fin du nom du répertoire distant, le répertoire local sera créé à l'intérieur du répertoire cible sur le serveur. Comparez&nbsp;:

```bash
$ lftp racine -e "mirror -R test testdistant;"
```

qui donne sur le serveur :   

```bash
cd testdistant/ && cls
test.html  test2.html
```

et

```bash
$ lftp racine -e "mirror -R test testdistant/;"
```

qui donne sur le serveur :  

```bash
cd testdistant/ && cls
test/
cd test/ && cls
test.html  test2.html
```

## Sauvegarde de site

Si vous souhaitez réaliser une sauvegarde intégrale de votre site depuis la racine, vous pouvez utiliser cette commande&nbsp;:
```bash
$ lftp LOGIN:PASSWORD@SERVER -e "mirror; quit;"
```

Vous pouvez spécifier les répertoires à ignorer lors de la copie en utilisant l'option _-x_&nbsp;:
```bash
$ lftp LOGIN:PASSWORD@SERVER -e "mirror -x DOSSIER_IGNORE -x DOSSIER_IGNORE_2&nbsp;; quit;"
```

Vous pouvez par la suite limiter la sauvegarde aux seuls fichiers nouveaux (attention : si vous avez ignoré des dossiers, il vous faudra les ignorer à nouveau) grâce à l'option _-n_ (ou _--only-newer_)&nbsp;:
```bash
$ lftp LOGIN:PASSWORD@SERVER -e "mirror -n; quit;"
```

## Mettre à jour son site

La commande _mirror_ associée à son option _-n_ est particulièrement utile lorsque l'on souhaite uploader de manière régulière le contenu d'un dossier local vers notre serveur afin de mettre à jour notre site. Ainsi la première fois, il me suffit de saisir la commande&nbsp;:
```bash
$ lftp racine -e "mirror -R test testdistant;"
```

Ensuite, pour n'ajoute que les contenus nouveaux, il me suffit de faire&nbsp;:
```bash
$ lftp racine -e "mirror -Rn test testdistant;"
```

### Exemple

Un répertoire test/ ne contenant que deux fichiers&nbsp;:
```bash
$ ls test
test2.html  test.html
```

Je copie ce répertoire vers mon serveur&nbsp;:
```bash
$ lftp racine -e "mirror -R test testdistant; quit;"
cd ok, cwd=/www                                                            
Total&nbsp;: 1 répertoire, 2 fichiers, 0 liens symboliques   
Nouveau&nbsp;: 2 fichiers, 0 liens symboliques
```

Je crée dans mon répertoire local un nouveau fichier&nbsp;:
```bash
$ touch test/test_new.html
```

Je relance une copie du répertoire local vers le répertoire distant en utilisant l'option _-n_&nbsp;:
```bash
$ lftp racine -e "mirror -Rn test testdistant; quit;"
cd ok, cwd=/www                                              
Total&nbsp;: 1 répertoire, 3 fichiers, 0 liens symboliques
Nouveau&nbsp;: 1 fichier, 0 liens symboliques
```

Nous verrons dans un [prochain article](/blog/lftp_auto "lftp&nbsp;: automatiser les transferts") que l'on peut automatiser les transferts soit en passant par des alias soit par le biais de scripts propres à lftp.