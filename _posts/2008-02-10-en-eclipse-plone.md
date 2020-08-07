---
layout: post
title: Plone3 & Acceleo, the first step
categories: en
tags: eclipse plone

---


I have work some hours on a simple plone2.5 code generator with Acceleo. It is available on the acceleo svn:

svn checkout svn://svn.forge.objectweb.org/svnroot/acceleo/trunk/modules/community/uml21/zope/plone/25/org.acceleo.module.pim.uml21.plone25/trunk

This code generator is not finished, but the way of doing is good enought to start the plone3 one. I would like here to explain the purpose of the Acceleo Generator for Plone3 i want to make.I will first explain the problems i have with ArchgenXML.

What i don’t like in AGX:

The license in each source file (i prefer just a license.txt file)
The billion tagged values (i have lost hours here)
The generated code itself doesn’t look like with the code i would have produced.
The command line
ArgoUML
All the hacks done every where to make the code compatible with two versions of Plone
You can’t modify a line of code generated without lost it if you re-generate your code
What i like in AGX:

The way you use UML (copy the model, and then do a simple class diagram, it s up)
The i18nized schema generated with po files
The generated tests
It works on all well known OS (linux, macos, windows)
Lots of documentation
The user code slots well thought.
For sure i want to keep all that good point for the project. So the overview of what i want:

Easy to install and to use
Running on most OS known
Code template easy to customize (making multiple branches of my own templates)
Do not generate 100% of the code by working hours in your UML diagrams
Be able to get an existing UML and generate only what you want
An other point: generate something only if it save your time. The best example i have is tagged values from AGX, like Searchable = 1. One tagged value for one line of code !A first advice from Cédric Brun (obeo) is to don’t fall in the modelisation of the code itself. For example doing an UML component to generate a zope component (BrowserView, adapter, …). In that case you will lost a lot of time in doing you UML diagram, and be obliged to add stereotypes (adapter, …). So to follow this advice, i have think about the idea of using Component diagram from UML, and i finally don’t want to use it, cause for me an UML component is not equal to a zope component. A UML component can be more seen as an egg. I need to think a bit more about that point, but that could be a great aspect to zope code generation.

Would we need to ‘model’ workflow and generate them according to a state diagram ? Here the point is a bit more complex. In fact you know that you need to make them to explain to your customers the need of specify workflows by UML. But the permission system in zope is specific to it, and the state diagram is not suppose to support this (in AGX we use tagged value one more time). And since we use GenericSetup to specify workflow now, the time saved by doing the state diagram for your workflow is negative. So i think we will just generate the state, but not the permissions associated, that are often explain with the diagram in a documentation. But i would like to generate the test associated with workflows. There were a good conference at Naple on that point.

Next, do we force the use of stereotypes to generate stuff or do we do as with AGX, and so force the use of ‘stub’ stereotype to indicate the generator that this class is not a content type to generate. I personally prefer the first option. In that way you can take an existing UML diagram, load the plone3 profile, and said this package is an egg, this class is a ATContentType.

Well, a good demo package to do is the case of the Martin Aspeli ‘s book.

Next time i will publish the UML from what i want the martin’s code to be generated.