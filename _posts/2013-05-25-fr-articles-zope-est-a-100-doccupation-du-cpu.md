---
layout: post
title:  "Que faire quand Zope reste à 100% de CPU"
categories: fr
tags: debug zope system
---

Cet article donne une méthode de travail sous Linux pour trouver un problème
d'occupation CPU à 100% qui peut vite rendre votre site inaccessible (503).

Il y a des matins comme ça ou on a pas de chance: Erreur 503 sur un portail
réalisé avec le CMS Plone.

Au bout d'un temps aléatoire le processus monte à 100% du CPU et il y reste.
Puis c'est au tour des autres processus de votre cluster jusqu'à ce que toutes les instances soient à 100%.

Le premier reflexe, on relance tout :

    supervisorctl restart all

Si le problème persiste:

* On coupe tout
* On lance un backup full
* On télécharge tous les logs
* On prend un café pour réfléchir

Les questions à poser durant le café (les réponses à avoir):

* (Aux administrateurs système) Depuis quelle heure le problème a lieu ?
* (Aux administrateurs système) Des livraisons ont elle eut lieu ? (mise à jour système, d'applications, de webservices, ...)
* (Au client) Quoi que ce soit d' inhabituel dans les manipulations réalisées sur le site ?
 
Le but de ces questions est d'obtenir un maximum d'information sur les dernières modifications du contenu et de l'environnement, pour pouvoir réduire l'étendue de la recherche.

Cas d'une mise à jour
---------------------

Il devient simple de résoudre le problème: Une dicotomie du code, correction puis livraison du patch ou retour arrière sur la mise à jour (pas toujours possible)

Autres
------

Dans ce cas, site planter pour site planter, vous pouvez travailler
directement sur le serveur quand c'est possible sinon il faut reproduire
le problème ailleur et donc synchronier votre base de données avec celle de prod.

### Regarder les logs et corriger les erreurs 500 et les 404 les plus fréquentes

Il faut analyser les logs (instance-X-Z2.log et instance-X.log) et tenter de corriger les erreurs dans un premier temps.

Une page qui fait une erreur est couteux en CPU pour le serveur. Moins vous avez d'erreur plus votre site sera rapide à répondre et plus vos utilisateurs seront content. Il est important de temps en temps de regarder le fichier instance.log pour ça.

Pour corriger une 404 rien de plus simple, il suffit de créer une série de dossier en ZMI avec comme dernier élément un PythonScript avec l'instructuion suivante:
return ''
 
### Rechercher le contenu qui pose problème

Votre client n'a pas pu vous indiquer d'ou venait le problème (ou peut être n'avez vous pas lu vos emails ...) Il faut donc partir à la recherche du contenu.

Dans un premier temps, vous aller augmenter le niveau de log d'une seule instance que vous allez démarrer (seule) puis vous aller regarder les log passer avec la commande tail -f var/log/instance1.log et dans une autre console tail -f var/log/instance1-Z2.log)

Récupérez également la liste des derniers contenus publiées dans votre site et essayer de les afficher.

Regarder dans le fichier access log (Z2.log) vous trouverez les URLs visitée. Pour plus d'information sur les logs Zope, vous pouvez lire la documentation.

Un exemple: La newsletter a été envoyée 12H avant le début des problèmes et il y a eu pas mal d'activité sur Twitter. Dans ce cas afficher l'ensemble des contenus un par un. Il faut trouver ce contenu !

C'est lui !

Vous avez trouvez le contenu qui pose problème ? Chaque fois qu'on essaye de l'afficher, l'instance Zope reste à 100% ! Alors un dernier restart de votre instance et faite /edit pour pouvoir analyser le problème.

Pour ma part le problème était simple, un rédacteur a réaliser un copier/coller d'une image sur le bureau dans l'éditeur WYSIWYG et sous MSWindows cette manipulation insert le contenu de l'image au format base 64. du coup le contenu censé être du texte devient tout de suite plus volumineux. Il suffit d'afficher le code source (bouton HTML dans l'éditeur) pour s'en rendre compte. Supprimez le vitre !

Vous pouvez maintenant revenir en arrière:

* repasser le niveau de log en erreur
* démarrer les autres instances
* appeler votre client

Je ne suis pas certain de la raison mais je pense que le problème vient à l'affichage de la page. En effet le code base64 est valide au sens HTML du terme, donc le CMS le conserve. En revanche la transformation qui a lieu au moment de l'affichage (post filtre) doit être la responsable de l'occupation CPU. Plus de recherche serait nécessaire pour corriger le vrai problème mais ici je me suis contenté de supprimer ce contenu et de faire tourner une note comme quoi il ne faut pas copier/coller une image dans un wysiwyg (tout du moins sous Windows).

Dans mon cas j'ai appris ensuite que le client m'avait envoyé par email cet manipulation réalisée la veille, cet article était bien dans la newsletter mais je l'ai 'manqué' durant ma recherche et j'étais trop occupé pour lire mes emails ... Bref ce fut une mauvaise journée !

L'utilisateur n'est pas le problème ici c'est bien la technologies qui doit permettre de filtrer les images copier/coller dans le wysiwyg au moment de la sauvegarde.
