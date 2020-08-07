---
layout: post
title: My point of BrowserView
categories: en
tags: plone

---

Since Plone2.5 has been released, there is a good way of seperate logic and presentation from templates, but the use of BrowserView is not used in the same way by developpers. i m trying here to explain my point of view about that component.

A very good presentation of BrowserView is already done by Optilude;

According to my point of view and the MVC pattern, a BrowserView is just a controller. its role is to prepare data to be displayed, or to trigger a process. Most of the time i m querying the portal_catalog, redirect the user, add status messages, ….

I like the way portlets are done under plone2.5. for me it s the best example of how to use BrowserView.

The other use case of BrowserView is to render the attached template and insert a “view” instance in it. This is a kind of “implicit” behaviour that i hate in zope2. So you can call it directly by the url. I don’t understand that choice, but Plone3 use it in that way. And that do not let you reuse the logic code inside the BrowserView in an other template. Controller is known to be reusable throw the entire software.So please, use the BrowserView component like a controller.