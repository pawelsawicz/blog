+++
date = "2013-01-24T23:24:02+01:00"
description = "New blogpost for today"
draft = false
title = "Entity Framework - missing feature"
weight = 20
aliases = [
    "/entity-framework-missing-feature"
]
+++

When I was working on this test
I saw big disadvantage, there is no method created to delete collection of items at our database, on the other hand other ORMs have this feature. I think it's good idea to have this feature, so I created topic with my idea. If you need this feature, please vote.

Now :

foreach (var item in collection)
{
 db.Customers.Remove(item);
}
db.SaveChanges();

After :

db.Customers.Remove(collection);
db.SaveChanges();