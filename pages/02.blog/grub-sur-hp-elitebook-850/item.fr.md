---
title: 'GRUB sur HP EliteBook 850'
date: '03-04-2020 00:00'
taxonomy:
    category:
        - Linux
    tag:
        - partition
        - GRUB
        - 'HP EliteBook'
twitterenable: true
twittercardoptions: summary
facebookenable: true
---

Après quelque frayeurs au re-démarrage après l'installation en dual-boot d'une Debian Buster sur mon portable HP EliteBook 850, je me retrouvais à devoir d'abord utiliser le _bootloader_ d'HP pour sélectionner ma partition où était installé ma Lubuntu ainsi que GRUB, puis de devoir sélectionner à nouveau sur quel système je voulais booter&nbsp;;la même mésaventure m'était arrivée avec une Lubuntu 18.04. Le «&nbsp;SecureBoot&nbp;» était pourtant bien désactivé...

===

La solution m'a été fourni par [michal_za sur askubuntu.com](https://askubuntu.com/a/663388)&nbsp;:

1. on accède au _bootloader_ d'HP, on sélectionne «&nbsp;Boot from EFI File&nbsp;» puis on va chercher où se trouve GRUB&nbsp;; dans mon cas&nbsp;: `\EFI\debian\grubx64.efi` (pour Ubuntu et ses dérivés&nbsp;: `\EFI\ubuntu\grubx64.efi` ).
2. on crée une entrée «&nbsp;Custom Boot&nbsp;» dans laquelle on saisit cette adresse&nbsp;; bien évidemment, le tout en QWERTY... le backslash étant en AZERTY l'astérisque, le point étant le slash...
3. On place cette entrée «&nbsp;Custom Boot&nbsp;» en tête des options de boot.

Et voilà, le tour est joué.