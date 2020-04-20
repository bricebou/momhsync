---
title: 'Birthday : ne manquez plus d''anniversaires !'
date: '27-08-2011 00:00'
taxonomy:
    category:
        - Linux
    tag:
        - console
        - personnalisation
---

Vous êtes du genre à oublier de souhaiter un bon anniversaire à vos amis ou parents&nbsp;? Ce ne sera plus le cas désormais grâce à un petit logiciel permettant d'afficher dans un terminal virtuel les anniversaires à venir&nbsp;: _birthday_.

Commençons par installer _birthday_&nbsp;:

```bash
$ sudo apt install birthday
```

Inutile de lancer le programme maintenant, il nous faut tout d'abord créer un fichier de données, avec les dates que l'on souhaite se voir rappeler.

```bash
$ nano ~/.birthdays
```

Les anniversaires («&nbsp;_birthdays_&nbsp;») doivent être entrées sous cette forme&nbsp;:

```
Joe Blow=25/04/1974
```

Vous pouvez également préciser des «&nbsp;anniversaires&nbsp;» («&nbsp;_anniversaries_&nbsp;») ou des événements («&nbsp;_events _») grâce aux options _ann_ et _ev_ respectivement&nbsp;:

```
Attentats du World Trade Center=11/09/2001 ann
Printemps=21/03 ev
```

Une fois votre fichier créé et complété, vous pouvez lancer _birthday_&nbsp;:

```bash
$ birthday
```

Cette commande vous permet d'afficher, par défaut, les anniversaires, événements… à venir dans les 21 prochains jours (comportement par défaut). Vous pouvez spécifier le nombre de jours grâce à l'option _-W nombre\_de\_jour_&nbsp;:

```bash
$ birthday -W 7
```

ne vous affichera que les dates à ne pas oublier pour les 7 prochains jours.

Vous pouvez également afficher les dates dans un calendrier grâce à l'option _-c_ et limiter le nombre de jours à afficher avec l'option _-d_&nbsp;:

```bash
$ birthday -c -d 3
```

C'est bien beau tout cela, mais encore faut-il se souvenir de lancer birthday de temps en temps tout de même… C'est pas gagné&nbsp;? Pas de panique&nbsp;: nous allons faire en sorte de lancer _birthday_ automatiquement à chaque ouverture d'un terminal virtuel&nbsp;: pour cela nous éditons le fichier _~/.bashrc_&nbsp;:

```bash
$ nano ~/.bashrc
```

et on y ajoute tout simplement la ligne suivante&nbsp;:

```bash
birthday
```

et le tour est joué&nbsp;!

Pour plus d'options et en savoir plus&nbsp;:

```bash
$ man birthday
```