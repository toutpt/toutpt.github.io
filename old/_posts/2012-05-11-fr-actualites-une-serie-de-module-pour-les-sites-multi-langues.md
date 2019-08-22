---
layout: post
title:  "Une série de module pour les sites multi-langues"
categories: fr
tags: plone module multilangues
---

Je viens de mettre à disposition une série de module pour le CMS Plone,
réalisé en collaboration avec le [CIRB](http://www.cirb.irisnet.be/):

* [collective.linguaanalytics](http://pypi.python.org/pypi/collective.linguaanalytics)
* [collective.linguadomains](http://pypi.python.org/pypi/collective.linguadomains)
* [collective.linguasitemap](http://pypi.python.org/pypi/collective.linguasitemap)
 

## collective.linguaanalytics

Ce module surcharge le module très utilisés collective.googleanalytics
pour permettre la mise à disposition d'un code de tracking différent par langue.
Il peut tout de même être utilisé pour un site mono langue si vous souhaitez
ne pas utiliser la fonction d'authentification à google via OAuth mais juste y mettre un code de tracking.

## collective.linguadomains

Ce module permet de traduire vos URL et forcer la redirection d'une URL
à l'autre si vous changez de langue. Ce module est utile si vous n'utilisez pas
les navigationroot du CMS (dossier /fr, /en, ...).
Ceci vous évitera de voir des contenus de différentes langues pour chaque domaine.
Il va de paire avec le module suivant.

##collective.linguasitemap

Ce module met à disposition un sitemap par langue.
Par définition un sitemap est multi-langues et doit contenir tous les contenus.
Mais si vous traduisez vos noms de domaines, vous allez avoir besoin de prendre en compte la langue et de faire un sitemap par langue.
