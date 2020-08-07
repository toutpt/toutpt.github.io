---
layout: post
title: JMeter, improving performance of a Plone web site
categories: en
tags: plone

---

Last week I have made a rush to improve performance of a Plone based web site. For performance testing I have used JMeter.

JMeter is really nice to use. Just launch it’s proxy, plug your browser on it, and do your test. Next you save it as xml and you can edit the test. So you can login (it support cookies) you can create content (with an once logic controller) consult content, and stress your server.

What I have learned:

* Do not use `brains` or any object in templates, or you will not beeing able to cache your logic code in ramcache. Use dict that contains every strings ready to be displayed in the templates.
* How to use the ram cache
* I can store acl_users in ramcache, and I have been surprised to see the difference. On 5 tabs hitted, I have hit the cache 278 times …
* Archetypes is damly slow (about one second to set some attributes of an object in a btree and reindexIt)
* CMFPlone.utils.createObjectByType do a reindexObject
* Do not add any index to the portal_catalog, use the buinding done by archetype_tool to be able to use other index. I m adding about one catalog tool per custom content type.
* A query on the portal_catalog can take one second if you have for example a list of 100 paths (query[‘path’] = [‘/first/path’, ‘/second/path’] and more than 100 000 entries.

I have learned many other things during the last week, but now i m using stress tests during the dev