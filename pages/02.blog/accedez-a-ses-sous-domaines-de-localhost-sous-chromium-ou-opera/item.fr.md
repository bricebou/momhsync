---
title: 'Accédez à ses sous-domaines de localhost sous Chromium ou Opera'
date: '27-07-2015 00:00'
taxonomy:
    category:
        - Linux
        - Windows
    tag:
        - dev
        - réseau
twitterenable: true
twittercardoptions: summary
facebookenable: true
---

Sous Chromium ou Opera, sous Xubuntu 15.04, il m'était impossible d'accéder à mes sous-domaines de localhost.

Opera était assez peu bavard et se contentait du message&nbsp;:

> Cette page web n'est pas disponible
> 
> Impossible de se connecter à avec2.localhost.

Chromium était un peu plus bavard&nbsp;:

> Page Web inaccessible
> 
> DNS_PROBE_FINISHED_NXDOMAIN

Pourtant, mes sous-domaines de localhost sont bien définis dans mon `/etc/hosts` et je ne rencontre aucun souci pour y accéder avec Firefox (ce n'est plus le cas en 2020... voir [cet article](/blog/accedez-a-ses-sous-domaines-de-localhost-sous-firefox)).

Après quelques recherches et quelques tentatives infructueuses, j'ai découvert une page de Chromium permettant de &laquo;&nbsp;suivre&nbsp;&raquo; le cache DNS de Chromium&nbsp;: `chrome://net-internals/#dns`.

On commence par purger le cache puis on essaie d'accéder à l'un de ses sous-domaines.    
On peut alors voir apparaître une ligne de ce type&nbsp;:

```
localhost.	IPV4	error: -105 (ERR_NAME_NOT_RESOLVED)	2015-07-26 23:07:56.563 [Expired]
```

Suivent d'autres recherches qui aboutissent à [un commentaire](https://code.google.com/p/chromium/issues/detail?id=489973#c22) d'un des rapports de ce &laquo;&nbsp;bug&nbsp;&raquo; (qui est en fait une _feature_...)

On modifie donc son fichier `/etc/hosts` en y ajoutant simplement cette ligne&nbsp;:

```
	127.0.0.1 	localhost.
```

Et tout rentre dans l'ordre, aussi bien sous Chromium que sous Opera.