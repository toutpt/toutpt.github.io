---
layout: post
title: Look at my profile
categories: en
tags: plone

---

First of all I would like to thank guys who wrote GenericSetup. I m writing this post cause i think profiles are not well used. They are used like Extensions/Install.py scripts. So what is a profile, and how to use it ?

Profile is about configuration of portals (portal_xxx inside the zmi). In that way I have started by asking myself “what means install/uninstall in Plone” ? Some answers:

“its about the Extensions/install.py scripts.”
“Like on your computer, you install a software or a lib”
Well, there is no sens for the word “install” in the world of plone. We are speaking about configuration of tools. So the CMFPlone/profiles/defaults, contains a plone default configuration. Right ?

In that way extending default plone configuration means, change it a bit, adding some directory views in portal_skins for example.

What happens if you are “installing” 20 products to you configuration ? Here it is ! You have lost the configuration of your Plone project and will not be able to understand why this new product you are trying to install break your website.

Seeing this, I have proposed to add one single profile by project. In that way you controle your entire Plone, and if you have a problem, you just have to apply this profile. But that also means you have to write it. Here is how i proceed:

* Install every product you need
* Export all steps
* Adding a new product/egg specially for your project
* Put the results of the exports in it
* Read it,
* Add all constraints in it. For example the order of layers inside skins.xml

This is done, you get it ! There is no duplication of files, but integration of products inside Plone configuration (my work). Writing XML is boring ? Writing them faster by using my eclipse templates ;)

The next point is about setup handlers. I hate product that add setup handler just to say “hey i know how to add a step”. I always ask for “what is a step for you ?” So steps are not the way to call a python script. If you are tented to add a step, just use Extension/install.py to put your script. Adding a step make sens only if you are adding a tools, and you want that tools to be configurable. So you don’t add setuphandler.py but you write import/export.py for your tool, and then you adding the step with import_step.xml.
An other problem is the configuration of your Plone project can be different for a production server and developpement local server. For example the mailhost.xml file can be different. In that way you can extend your profile with just a smaller profile that reconfigure what you need.

This is why i m laughing when i heard “uninstall profile”.

Finally, i don’t understand why the portal_quickinstaller is now “aware” of extensions profile. That doesn t help to understand profiles. People will continue to write install/uninstall profiles. If any one know why i m ready to discuss about it.