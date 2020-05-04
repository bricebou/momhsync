---
title: 'Quels outils pour réduire CSS et JS ?'
date: '22-07-2018 00:00'
taxonomy:
    category:
        - Linux
    tag:
        - dev
        - bash
---

## cleancss

```bash
$ sudo apt install cleancss
```

Ensuite, il suffit de lancer la commande&nbsp;:

```bash
$ cleancss -o mon_fichier.min.css mon_fichier.css
```

### Traitement par lot

Mais c'est un peu lassant de répéter cette opération lorsque l'on a de multiples fichiers à traiter. J'ai donc adapté [le script proposé par Marco G](https://solariz.de/de/minify_all_css_files_in_a_folder.htm) qui utilise `yui-compressor`.

On crée donc le script `mincss` dans un répertoire `bin` de notre `home` puis on le rend exécutable avant d'y coller le script ci-dessous&nbsp;:

```bash
$ mkdir /home/$USER/bin
$ touch /home/$USER/bin/mincss
$ chmod +x /home/$USER/bin/mincss
$ nano /home/$USER/bin/mincss
```

```bash
#!/bin/sh
echo Compressing CSS Files...
saved=0
for f in `find -name "*.css" -not -name "*.min.css"`;
do
    target=${f%.*}.min.css
    echo "\t- "$f to $target
    FILESIZE=$(stat -c%s "$f")
    cleancss -o $target $f
    FILESIZEC=$(stat -c%s "$target")
    diff=$(($FILESIZE - $FILESIZEC))
    saved=$(($saved + $diff))
    echo "\t  $diff bytes saved"
done
echo "Done&nbsp;! Total saved: $saved bytes"
```

## UglifyJS

Pour les fichiers Javascript, j'utiliser UglifyJS

```bash
$ sudo apt install node-uglify
```
ou en utilisant `npm` l'utilitaire de gestion des paquets de NodeJS&nbsp;:

```bash
$ sudo npm install -g uglify-js
```

La commande `uglifyjs` est désormais disponible&nbsp;:

```bash
$ uglifyjs mon_fichier.js -o mon_fichier.js.min.js
```

Ajouter la compression&nbsp;:

```bash
$ uglifyjs mon_fichier.js -o mon_fichier.minc.js -c
```

Ajouter en plus l'«&#8239;altération&#8239;»&nbsp;:

```bash
$ uglifyjs mon_fichier.js -o mon_fichier.minc.js -m
```

On peut ainsi lancer la commande pour n'obtenir plus qu'un fichier à partir de tous les fichiers spécifiés&nbsp;:

```bash
$ uglifyjs *.js -o output.js -c -m
```

### uglifyjs-folder

Pour traiter l'ensemble des fichiers d'un répertoire (et de ses sous-répertoire), on peut également utiliser `uglifyjs-folder`.

```bash
$ sudo npm install -g uglifyjs-folder
```

Pour compresser tous les fichiers d'un répertoire, il suffit de lancer&nbsp;:

```bash
$ uglifyjs-folder mon_dossier/ -o script.min.js
```

On peut aussi vouloir conserver des fichiers séparés, ce que permet l'option `-e`&nbsp;; dans un tel cas, l'arborescence du dossier traité est conservé dans le répertoire indiqué avec l'option `o`&nbsp;:

```bash
$ uglifyjs-folder ../test/ -o output/ -e
```