My first patch ! Improve performance of Plone.

I m proud of it, so I want to blog about it. Let me tell you the story of my first patch on Plone.

Two students from ‘[Ecole polytechnique de l université de Nantes](http://www.polytech.univ-nantes.fr/)‘ has worked for me during 6 month. They have discover so much technologies:

* Python
* Zope
* Plone
*JMeter
* …

Their subject was : “Just improve authentication performance of Plone”. They do not have succeed in doing this, but they has showed me some graphics where I have found a performance issue: the user properties. So i have kept Nasreddine BERCHIDA for the summer to continue on this. Thuesday 7 July, he has found that enumerateUsers from PlonePas.plugins.property was the source code that matters.

We have done a [report with funkload](http://jm406.free.fr/travail/plone/diff_01_connect_p_atch_bis_N4_vs_o_cache_no_admin-N2/) with lots of users and got it: lots of green means performance improved.

The patch is about to be merged (I hope).

References:

* [mailing list discussion](http://plone.org/support/forums/general#nabble-td3230154|a3230154)
* [bug tracker ticket](https://dev.plone.org/plone/ticket/9361) (need a plone.org account)