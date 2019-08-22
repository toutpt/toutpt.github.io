---
layout: post
title:  "Comment patcher CKEditor dans le CMS Plone"
categories: fr
tags: ckeditor howto
---

Cet article montre comment patcher le module Plone [collective.ckeditor](http://pypi.python.org/pypi/collective.ckeditor)
et utiliser vos changements en production.

Le cas d'utilisation ici est de changer la valeur par défaut de la largeur du tableau

Il y a des fois ou les modifications que l'on réalise ne peuvent pas être des contributions
(souvent parcequ'elles sont spécifiques au projet) et ou l'on ne peut pas simplement surcharger.
Je prend ici le cas d'utilisation suivant: Modifier la valeur par défaut du plugin tableau de l'éditeur CKEditor.
Il s'agit ici de modifier un fichier JS au milieu des autres et de permettre un déploiement toujours à partir des modules, bref on ne fait pas de fork !

Par défaut le formulaire d'ajout d'un tableau dans CKEditor spécifie une largeur de 500 pixels.
La bonne raison de changer ce comportement ?
Mon client change cette valeur manuellement à chaque ajout de tableau et met "100%".

Donc en premier lieu, en tant que développeur on récupère le code source de collective.ckeditor:

    git clone git://github.com/collective/collective.ckeditor.git

Ensuite il s'agit de trouver les fichiers à modifier. Dans notre cas

    collective/ckeditor/browser/ckeditor/_source/plugins/table/dialogs/table.js
    collective/ckeditor/browser/ckeditor/plugins/table/dialogs/table.js

Vous pouvez donc maintenant effectuer les modification puis executer la commande suivante à partir de la racine du module:

    git diff --no-prefix > table-default.diff

N'oubliez pas l'option --no-prefix ou votre patch ne sera pas utilisable.
Maintenant vous avez votre patch.
Vous avez donc besoin de l'utiliser dans le déploiement en production de votre projet.
Nous utilisons buildout pour déployer nos projet, donc voici un exemple simple de fichier buildout: ckeditor-patch.cfg

    [buildout]
    extends=http://dist.plone.org/release/4.1-latest/versions.cfg
    parts =
        instance
        patch-ckeditor
    
    [instance]
    recipe = plone.recipe.zope2instance
    user = admin:admin
    eggs=
        Plone
        collective.ckeditor
    zcml =
        collective.ckeditor
    
    [patch-ckeditor]
    recipe = collective.recipe.patch
    egg = collective.ckeditor
    patches = table-default.diff

Ce buildout installera Plone avec le module collective.ckeditor et appliquera le patch dessus. Voici le résultat dans la console:

    $ bin/buildout -c test-patch.cfg
    Installing instance.
    Getting distribution for 'collective.ckeditor'.
    warning: no previously-included files matching '*pyc' found anywhere in distribution
    Got collective.ckeditor 3.6.2.
    Generated script '/Users/toutpt/myproject/bin/copy_ckeditor_code'.
    Installing patch-ckeditor.
    patch: reading patch /Users/toutpt/myproject/table-default.diff
    patch: total files: 2  total hunks: 2
    patch: in /Users/toutpt/.buildout/installed_eggs/collective.ckeditor-3.6.2-py2.6.egg...
    patch: processing 1/2:      /Users/toutpt/.buildout/installed_eggs/collective.ckeditor-3.6.2-py2.6.egg/collective/ckeditor/browser/ckeditor/_source/plugins/table/dialogs/table.js
    patch: successfully patched /Users/toutpt/.buildout/installed_eggs/collective.ckeditor-3.6.2-py2.6.egg/collective/ckeditor/browser/ckeditor/_source/plugins/table/dialogs/table.js
    patch: processing 2/2:      /Users/toutpt/.buildout/installed_eggs/collective.ckeditor-3.6.2-py2.6.egg/collective/ckeditor/browser/ckeditor/plugins/table/dialogs/table.js
    patch: successfully patched /Users/toutpt/.buildout/installed_eggs/collective.ckeditor-3.6.2-py2.6.egg/collective/ckeditor/browser/ckeditor/plugins/table/dialogs/table.js

Parce que c'est un déploiement, on essaye toujours deux fois:

    $ bin/buildout -c ckeditor-patch.cfg
    Installing instance.
    Installing patch-ckeditor.
    patch: reading patch /Users/toutpt/myproject/table-default.diff
    patch: total files: 2  total hunks: 2
    patch: in /Users/toutpt/.buildout/installed_eggs/collective.ckeditor-3.6.2-py2.6.egg...
    patch: processing 1/2:   /Users/toutpt/.buildout/installed_eggs/collective.ckeditor-3.6.2-py2.6.egg/collective/ckeditor/browser/ckeditor/_source/plugins/table/dialogs/table.js
    patch: already patched   /Users/toutpt/.buildout/installed_eggs/collective.ckeditor-3.6.2-py2.6.egg/collective/ckeditor/browser/ckeditor/_source/plugins/table/dialogs/table.js
    patch: processing 2/2:   /Users/toutpt/.buildout/installed_eggs/collective.ckeditor-3.6.2-py2.6.egg/collective/ckeditor/browser/ckeditor/plugins/table/dialogs/table.js
    patch: already patched   /Users/toutpt/.buildout/installed_eggs/collective.ckeditor-3.6.2-py2.6.egg/collective/ckeditor/browser/ckeditor/plugins/table/dialogs/table.js

Donc maintenant vous avez une instance Zope avec Plone et un CKEditor patché. Tout le monde doit être content!

Attention 1: Comme vous pouvez le constater si vous utilisez un dossier partagés pour vos eggs, le patch y sera appliqué, donc également pour tous les projets utilisant le même egg.

Attention 2: Les plugins sont récupéré à la demande (ajax) et un cache navigateur de 24H est indiqué. Ceci signifie que vous devez vider votre cache navigateur à chaque changement effectué sur le javascript des plugins ou attendre 24 heures. Voici les en-tête HTTP de la réponse pour le fichier table.js qui indique ce cache:

    Cache-Control:public,max-age=86400
    Content-Length:8733
    Content-Type:application/javascript
    Date:Tue, 12 Jun 2012 09:07:18 GMT
    Expires:Wed, 13 Jun 2012 09:07:18 GMT
    Last-Modified:Tue, 12 Jun 2012 08:47:06 GMT
