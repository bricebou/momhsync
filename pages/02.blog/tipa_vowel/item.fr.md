---
title: 'tipa & vowel'
media_order: 'tipa.jpg,vowel.jpg,tipa.tex,tipaman.pdf'
date: '14-08-2011 00:00'
taxonomy:
    category:
        - LaTeX
    tag:
        - linguistique
        - package
        - phonétique
        - phonologie
---

Le package _tipa_ vous permet d'insérer au sein d'un document les symboles de l'alphabet phonétique international (API). Je vous en propose une présentation générale, peu poussée.

Le package _vowel_ permet de créer des triangles et trapèzes vocaliques.

===

## tipa

Une fois installé le package (disponible [ici](ftp://ftp.tex.ac.uk/tex-archive/fonts/tipa/)), il vous suffit de l'appeler ainsi dans votre préambule&nbsp;:

```latex
\usepackage{tipa}
```

Puis pour pouvoir intégrer les symboles de l'API au sein de votre document, il vous suffit d'utiliser la commande _\textipa{X}_ ou X est le caractère souhaité ou d'autres commandes comme _\textschwa_&nbsp;:
<table border="0">
<tbody>
<tr>
<td width="400">
<pre class="language-latex">
    <code class="language-latex">
\textipa{a} 
 
\textipa{E}
 
\textturna 
 
\textschwa
</code>
</pre>
</td>
<td width="50"></td>
<td width="250"><img src="/user/pages/02.blog/tipa_vowel/tipa.jpg" alt="tipa" style="vertical-align: middle;" height="147" width="146" border="0" /></td>
</tr>
</tbody>
</table>

Si la manipulation du package_ tipa_ est assez intuitive pour une utilisation basique, certains symboles de l'API sont plus délicats à obtenir ; heureusement la [documentation](tipaman.pdf) est exhaustive et liste l'ensemble des symboles (à partir de la page 36).

## vowel

L'environnement _vowel_ est fourni par le package _vowel_ proposé avec _tipa_. Il permet de créer des triangles et trapèzes vocaliques hautement « configurables ». Voici un exemple tiré de la documentation du package&nbsp;:
<table border="0">
<tbody>
<tr>
<td width="400">
<pre class="language-latex">
    <code class="language-latex">
\begin{vowel}
\putcvowel[l]{i}{1}
\putcvowel[r]{y}{1}
\putcvowel[l]{e}{2}
\putcvowel[r]{\o}{2}
\putcvowel[l]{\textepsilon}{3}
\putcvowel[r]{\oe}{3}
\putcvowel[l]{a}{4}
\putcvowel[r]{\textscoelig}{4}
\putcvowel[l]{\textscripta}{5}
\putcvowel[r]{\textturnscripta}{5}
\putcvowel[l]{\textturnv}{6}
\putcvowel[r]{\textopeno}{6}
\putcvowel[l]{\textramshorns}{7}
\putcvowel[r]{o}{7}
\putcvowel[l]{\textturnm}{8}
\putcvowel[r]{u}{8}
\putcvowel[l]{\textbari}{9}
\putcvowel[r]{\textbaru}{9}
\putcvowel[l]{\textreve}{10}
\putcvowel[r]{\textbaro}{10}
\putcvowel{\textschwa}{11}
\putcvowel[l]{\textrevepsilon}{12}
\putcvowel[r]{\textcloserevepsilon}{12}
\putcvowel{\textsci\ \textscy}{13}
\putcvowel{\textupsilon}{14}
\putcvowel{\textturna}{15}
\putcvowel{\ae}{16}
\end{vowel}
    </code>
</pre>
</td>
<td width="50"></td>
<td width="250"><img src="/user/pages/02.blog/tipa_vowel/vowel.jpg" alt="vowel" height="164" width="246" border="0" /></td>
</tr>
</tbody>
</table>

Vous trouverez [ici](tipa.tex) un ECM (nécessitant cependant les packages longtable et array) présentant un corpus de transcription de mots en alphabet phonétique international ; les exemples d'utilisation de _vowel_ viennent de la documentation du package.