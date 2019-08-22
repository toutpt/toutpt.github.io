---
layout: post
title: Comment tester un module Plone en 10 minutes
categories: fr
tags: 
---

# Introduction

Quand on travaille avec un CMS, il est courant de vouloir tester un module
et de ne pas le faire par faute de temps. Ce que vous ne savez peut être pas
c'est qu'un module peut être testé en 10 minutes.

## Prérequis

Il faut pour suivre ce tutorial disposer d'un python version 2.6 avec
le nécessaire en dépendances (support SSL, tar, zip, libjpeg, ...)

Un autre prérequis est de connaitre l'adresse URL du dépot de code utilisé
pour ce module. Avec Plone vous avez souvent:

* [svn collective](https://svn.plone.org/collective/)
* [github collective](https://github.com/collective)
* un github privé

Sinon c'est pas bon signe, votre module ne fait pas partie de la communauté Plone.

# Tutorial

## Initialisation (2 minutes)


    svn co https://svn.plone.org/collective/collective.lemodule/trunk collective.lemodule
    /my/python/2.6/bin/python bootstrap.py
    bin/buildout
    bin/instance fg

Si le module ne dispose pas des fichiers buildout.cfg et bootstrap.py c'est assez facile de les ajouter:

    wget http://svn.zope.org/*checkout*/zc.buildout/trunk/bootstrap/bootstrap.py
    wget https://github.com/collective/collective.googledocsviewer/blob/master/buildout.cfg

Il vous reste à modifier le fichier buildout.cfg pour remplacer le contenu
de la variable package-name par le nom du module que vous souhaitez tester
(dans notre cas il faudra mettre collective.lemodule)

## Lecture du code (3 minutes)

Il faut jeter un oeil pour savoir:

* setup.py il y a des dépendances ?
* [src/]collective/package/profiles/default/* Qu'est ce que le module installe, un tool ? un type de contenu ? des vues ? ...)
* regarder rapidement le reste du code pour en voir la qualité
* regarder changelog ([docs]/CHANGES.txt, [docs]/HISTORY.txt) pour voir si le module est récent et s'il le développement est actif

## Tester le module

* ajouter un site plone
* installer le module
* tester les fonctionnalités que vous avez compris du default profile du module

Si Il vous reste un peu de temps tu peux faire la traduction directement et faire un mail sur la liste de diffusion plone-fr.

