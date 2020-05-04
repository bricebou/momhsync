---
title: 'Accédez à ses sous-domaines de localhost sous Firefox'
date: '10-04-2020 16:43'
taxonomy:
    category:
        - Linux
        - Windows
    tag:
        - dev
        - réseau
        - Firefox
recaptchacontact:
    enabled: false
---

Quelle surprise de découvrir &ndash;&nbsp;sans doute tardivement&nbsp;&ndash; sur ma Debian Buster que Firefox ne me permettait plus d'accéder à mes sous-domaines de localhost, [comme Chromium ou Opera quelques années plus tôt](/blog/accedez-a-ses-sous-domaines-de-localhost-sous-chromium-ou-opera)...

Il faut désormais se rendre dans l'onglet `about:congif` et lister ses sous domaines pour le paramètre `network.dns.localDomains`.