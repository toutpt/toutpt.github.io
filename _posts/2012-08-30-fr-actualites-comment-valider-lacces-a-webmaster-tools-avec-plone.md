---
layout: post
title:  "Comment valider l'accès à webmaster tools avec Plone"
categories: fr
tags: réalisation plone
---


Lors d'une migration d'un site Plone vers une version plus récente avec entre
autre un thème Diazo, nous avons perdu la validation du site par Google webmaster tools.

En effet le contenu renvoyé par Plone est modifié par Diazo et devient:

    <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> <html xmlns="http://www.w3.org/1999/xhtml">

Google estime alors que le contenu n'est pas valide et soulève l'erreur suivante: "Vérification Failed"

Pour fixer ce problème vous devez aller en ZMI et remplacer votre fichier html
par un 'Script Python' ayant exactement le nom du fichier HTML.

Voici le contenu à mettre dans votre script python:

    res = "CONTENU DU FICHIER"
    context.REQUEST.response.setHeader('X-Theme-Disabled', 'True')
    return res

De cette manière vous indiqué à Dizao de ne pas thémé ce contenu. Le tour est joué !