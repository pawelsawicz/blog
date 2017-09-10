+++
date = "2012-12-26T23:24:02+01:00"
description = "New blogpost for today"
draft = false
title = "ORM Series : Performance Part I : NHibernate, EF, OpenAccess, XPO Devexpress"
weight = 20
aliases = [
    "/orm-series-introduce-and-performance-part-i"
]
+++

In one of my companies on of the first tasks was to create appllication, that will show which ORM will be the best choice for my team ( in terms of performance, support and features). Main focus of this task was performance. My company has a online shop that is generating a huge ammout of queries. This was an important task, because we planned to redesign whole project with ORM in order to improve efficiency and get rid off all the complex stored procedures with business logic.

I selected four popular ORMs (Entity framework, OpenAccess, XPO Devexpress, NHibernate). Each test was ran ten times to reject gross error (values were averaged)

Local machine - Summary

summary

Observations

    Insert, probably you have wondering why single insert record has more time than inserting ten records. Probably its cost of first connection with database. And this problem is not only shown in insert but it's happend in Update and Delete. The best performance has NHibernate 1 minute and 20 seconds with inserting 10k records! either OpenAccess has scored 1 minute and 39 seconds, opposite site  Entity Framework with 3 minutes and 5 second and it's the worst score.
    Update, undeniably, the fastest update has got NHibernate, it has won updating all of values but in the 10k records NHibernate just crushed his competition! 5 second . The worst time has got OpenAccess 1 minute 12 second.
    Delete, it's very good example how syntax and features influence in speed of our queries just one ORM reached this horrible time - Entity Framework, there is no something like deleting collection and I was forced to using foreach statement to delete all items in collection (10k)! 3 minutes and 49 seconds. Other ORM done well their job but the best is XPO.

Conclusion

summaryFinal

Undoubtedly the best time has NHibernate, it hasn't lost any category and it won most of them.
After NHibernate, XPO Devexpress ranks in the second position. The worst was Entity Framework it lost the most time and its deleting time is just killing for our database.

EF version : 5.0.0.0
XPO version : 12.2.4.0
NHibernate version : 3.3.1.4000
OpenAccess version : 2012.3.1209.1