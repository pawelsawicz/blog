+++
date = "2013-02-08T23:24:02+01:00"
description = "New blogpost for today"
draft = false
title = "Entity Framework - reset migrations"
weight = 20
aliases = [
    "/entity-framework-reseting-migrations"
]
+++

    tl;tr; How to reset migrations in Entity framework, and start with raw database

While I was working at the project for Imagine Cup. I came across to one major problem. What if I want to delete all migrations and start over with raw migration (but at same database). In my case it was due to one of my update-database, it doesn't run correctly, some sort of error I couldn't figure out, how to fix. I decited to start with raw migrations.

    First of all, you have to type this commandn "Update-Database -TargetMigration:0 -Force" (in Package Manager Console) it wipe out all your changes at db but models stays in it current state.
    Then you have to delete Migrations folder
    Delete __MIgrationHistory at your database it can be located in SystemTables
    and all others tables in traget db.
    (Please keep the order of steps, because if you delete first migrations folder, it's over make new project...)

After all these steps, now we can start do your raw migration "Enable-Migration" if there is need, use "Enable-Migration -Force". Visit here to learn how to migrate.