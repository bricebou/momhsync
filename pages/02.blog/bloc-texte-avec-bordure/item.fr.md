---
title: 'Bloc texte avec bordure'
date: '13-08-2011 00:00'
taxonomy:
    category:
        - LaTeX
    tag:
        - TeX
        - macro
twitterenable: true
twittercardoptions: summary
facebookenable: true
---

Un environnement permettant de créer un bloc de texte dont on peut faire varier la largeur avec une barre verticale à gauche à l'épaisseur modififable bien évidemment.

===

Code proposé par François Pétiard et Jean-Côme Charpentier sur [fr.comp.text.tex](http://groups.google.fr/group/fr.comp.text.tex/browse_thread/thread/31de7edc68f7db2a/8c20a9f44439f9ad?lnk=gst&q=barre+gauche#8c20a9f44439f9ad).

À noter que cet environnement nécessite le package _framed_ (ainsi que, pour l'exemple seulement, _lipsum_ pour le faux texte).

Voici un ECM :
```latex
\documentclass{article}
% \usepackage[T1]{fontenc}
% \usepackage[latin1]{inputenc}
% \usepackage[a4paper]{geometry}
% \usepackage{lmodern}
\usepackage{framed}
\usepackage{lipsum}
% \usepackage[frenchb]{babel}
 
\newenvironment{barregauche}{%
% ajout pour une marge droite augmentée
\par
\addtolength{\hsize}{-1cm}%
% fin de l'ajout
\renewcommand\FrameCommand{%
\hspace*{1cm}%
\vrule width 1pt
\hspace{10pt}%
}%
\MakeFramed{%
\addtolength{\hsize}{-\width}%
\FrameRestore
}
}%
{\endMakeFramed}
 
\begin{document}
\lipsum[1]
\begin{barregauche}
\lipsum[1-4]
\end{barregauche}
 
\lipsum[1]
\end{document}
```