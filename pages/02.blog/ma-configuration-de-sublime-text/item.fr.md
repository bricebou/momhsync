---
title: 'Ma configuration de Sublime Text '
date: '01-11-2020 11:34'
taxonomy:
    category:
        - Linux
        - Windows
    tag:
        - 'Sublime Text'
twitterenable: true
twittercardoptions: summary
facebookenable: true
---

Il s'agit ici pour moi de conserver une trace de mes usages restreints mais variés de l'éditeur [Sublime Text](https://www.sublimetext.com), notamment les raccourcis claviers modifiés, les packages utilisés, les snippets créés...

## Synchronisation via Dropbox

La solution retenue pour synchroniser notamment la configuration, les packages installés et leurs paramètres ainsi que les snippets passe par Dropbox, comme présenté [dans la documentation](https://packagecontrol.io/docs/syncing).

On commence par récupérer le `.deb` qui va bien depuis cette page et on lance ensuite l'installation&nbsp;:

```shell
sudo apt install python3-gpg && sudo apt install ./Téléchargements/dropbox_2020.03.04_amd64.deb
```

On redémarre et on lance ensuite l'application Dropbox depuis le menu `Applications > Internet > Dropbox`.

Ensuite, on n'a plus qu'à suivre les instructions fournies&nbsp;:

- sur la première machine, où Sublime Text est configuré aux petits oignons&nbsp;:     
```shell
cd ~/.config/sublime-text-3/Packages/
mkdir ~/Dropbox/Sublime
mv User ~/Dropbox/Sublime/
ln -s ~/Dropbox/Sublime/User
```
- sur les autres ordinateurs&nbsp;:     
```shell
cd ~/.config/sublime-text-3/Packages/
rm -r User
ln -s ~/Dropbox/Sublime/User
```

## Apparence


## Langages

- [LaTeX](/blog/sublime-text-and-latex)&nbsp;;
- [Markdown](/blog/sublime-text-and-markdown).



