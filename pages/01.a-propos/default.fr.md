---
title: 'À propos'
process:
    markdown: true
    twig: true
twig_first: true
visible: false
---

_My Own Memory Hole_ se veut une sorte de «&nbsp;vitrine&nbsp;» de mon parcours et de mon expérience mais aussi de mes centres d'intérêt, à travers la mise à disposition de l'ensemble des dossiers et des mémoires que j'ai réalisés au cours de [mes études](/parcours) ainsi que par le biais d'articles consacrés à diverses applications des distributions [GNU/Linux](/blog/category:Linux) que j'utilise, (Debian ou Debian based essentiellement), à [LaTeX](/blog/category:LaTeX) &ndas;&nbsp;et plus particulièrement aux packages utiles lorsque l'on suit un cursus de Sciences du langage&nbsp;&ndash; ainsi qu'à des outils web, entre autres choses...

_My Own Memory Hole_ constitue donc en quelque sorte ma mémoire en ligne, mémoire virtuelle délocalisée mais ordonnée et indexée…

_My Own Memory Hole_, bien que moins exigeant et essentiel, est une référence au site _The Memory Hole_, aujourd'hui disparu (mais [archivé sur archive-it.org](http://wayback.archive-it.org/924/20100423160550/http://www.thememoryhole.org/ "Archives de The Memory Hole sur archive-it.org")), qu'animait Russ Kick et qui «&nbsp;avait pour but de préserver et publier des documents risquant d'être perdus, difficiles à trouver ou peu connus&nbsp;» ( [The Memory Hole (web site)](http://en.wikipedia.org/wiki/The_Memory_Hole_%28web_site%29) sur le Wikipedia anglophone [traduction personnelle]), reprenant le concept de «&nbsp;_memory hole_&nbsp;» popularisé par George Orwell dans _1984_ (cf. [Wikipedia](http://en.wikipedia.org/wiki/Memory_hole)).

## Aspects techniques

_My Own Memory Hole_ repose sur le CMS [Grav](https://getgrav.org/) sous [licence MIT](http://fr.wikipedia.org/wiki/Licence_MIT) et dont les sources sont disponible sur [GitHub](https://github.com/getgrav/grav).


### Plugins

_My Own Memory Hole_ utilise un certain nombre de plugins, outre les «&nbsp;classiques&nbsp;» Admin Panel, Email, Error, Form, Login, Markdown Notices, Problems...&nbsp;:

- _Admin Media Actions_ qui permet l'édition des médias (titre, alt, caption)&nbsp;;
- _Featherlight_ fournit la lightbox&nbsp;;
- _Pagination_&nbsp;;
- _Sitemap_&nbsp;;
- _TNT Search_&nbsp;;
- _Twig Extensions_ qui donne notamment accès au filtre _localizeddate_.

### Thème

J'ai développé le thème `momh` pour _My Own Memory Hole_; disponible sur [GitHub](https://github.com/bricebou/pico_momh), il repose sur les éléments suivants&nbsp;:

- le framework CSS [Spectre](https://picturepan2.github.io/spectre/index.html) (utilisé par la thème par défaut de Grav, Quark)&nbsp;
- la police [Route 159](http://dotcolon.net/font/route159/) par Sora Sagano&nbsp;;
- [non jQuery FitText.js](https://github.com/adactio/FitText.js)&nbsp;;
- ...

## Mentions légales

### Droits d'auteur

L'ensemble des articles proposés sur ce site sont rédigés et mis en ligne par Brice Boucard et sont mis à disposition sous [Licence Creative Commons Attribution -  Partage dans les Mêmes Conditions 4.0 International](http://creativecommons.org/licenses/by-sa/4.0/)

Cependant, certains contenus de ce site peuvent ne pas dépendre de cette licence, comme éventuellement les différents logos présentés à titre d'illustration, les œuvres photographiques ou tout autre élément dont je ne suis pas l'auteur. Si vous souhaitez reproduire certains contenus ou éléments de ce site et que vous n'êtes pas certains de la licence à appliquer, n'hésitez pas à me contacter ({{ "bricebou@momh.fr"|safe_email }}).

### Réalisation / Responsable de la publication&nbsp;:

Brice Boucard  

### Accès au site &amp; Hébergement&nbsp;:

[momh.fr](https://momh.fr)

OVH  
2, rue Kellermann  
BP 80157  
59053 Roubaix cedex 1  
France  
[www.ovh.com](http://www.ovh.com "Site d'OVH")

### Cookies
Pour tout cookie, _My Own Memory Hole_ ne fait appel qu'au tag javascript de suivi de fréquentation de notre propre instance de [Matomo](https://matomo.org/) (ex-Piwik), «&nbsp;_un logiciel libre et open source de mesure de statistiques web_&nbsp;» ([Wikipédia](https://fr.wikipedia.org/wiki/Matomo_(logiciel))).