---
layout: post
title: Private Pypi setup
categories: en
tags: python

---

I have succeed in seting up a full private pypi. So I want to share this.

First you need to install a Plone on a server with [plone software center](http://pypi.python.org/pypi/Products.PloneSoftwareCenter). Our system administrators has setup DNS to this plone with apache + http auth + SSL (https) and has given me one login / password.

Next you need to add a new software center to the content of your site. You can also change base workflow of a project to be approved by default.

The final step for the server is to add a user at the root of Zope with the same user/password has http auth

Now you are ready to the next step: setup your environnement on your computer. You need to add http auth ability to buildout and collective.dist. For this:

* configure the pypirc file like describe on plone.org
* Add lovely.buildouthttp extension to your buildout
* Add `.buildout` folder to your home directory if not already exists
* Add `.buildout/.httpauth` with realm, https://yourdomain, user, password
I have looking for the ‘realm’ of my server during 2 hours. So what is a realm ? Thanks to Tarek who have take times to answer me on irc:

    toupt the realm is the domain the server sends back when you do a challenge
    for instance zope sends "Zope"
    trac sends "trac"
    etc

I have apache httpd as server, so is it “Apache” ? The documentation of lovely.buildouthttp says ” My domain” but it still doesn’t work. Finaly I have found the realm. It was ‘Members Only Area’. this is the default apache realm. This is a good things to know.

A contribution: add documentation to lovely.buildouthttp, collective.dist and software center. The realm and the user in Zope trick were not easy to find.

The benefits of a private pypi:

* Release private eggs for customers
* Test some eggs before release on pypi and plone.org (fix rest for example)
* Release your last commit on collective to use it now in production
* Learn release process of an egg