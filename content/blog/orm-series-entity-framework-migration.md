+++
date = "2013-01-10T23:24:02+01:00"
description = "New blogpost for today"
draft = false
title = "ORM Series : Entity Framework Migration"
weight = 20

+++

Entity Framework, has given with last update, migrations at code first, undoubtedly it's the best feature in this update. This tool is very helpful if we change something at our model, and then we can simply upgrade our database. Moreover it can fulfill role as a database versioning, because we can easily to manage our updates and freely manage between them.

Whole system is based on Package Manager Console. Basic commands

```
    Enable-Migrations
    Add-Migration {name of commit}
    Update-Database
```

Enable-Migration we are using only once. It’s command which create basic files need to start work with migrations. Add-Migration is command we will use if we want to add some changes to database, and it generate file with . Update-Database - execute migrations files

First we have to create our model, I created simple music database:

```csharp
public class Artist
 { 
public int ArtistId { get; set; } 
public string Name { get; set; } 
public virtual List<Album> Albums { get; set; }
 }


public class Album 
 { 
public int AlbumId { get; set; } 
public string Name { get; set; } 
public int ArtistId { get; set; } 
public virtual Artist Artist { get; set; }
public virtual List<Song> Songs { get; set; }
 }


public class Song
 { 
public int SongId { get; set; } 
public string Name { get; set; } 
public int AlbumId { get; set; } 
public virtual Album Album { get; set; }
 }
```

Now we will run our Packer Manager Console with Enable-Migrations command.

If executed command was properly , we will see ‘Migrations’ folder.

image

Now we can execute next command, Add-Migration BaseOfDatabase. ‘BaseOfDatabase’ is name of commit so choose wisely because it should describe in one - two words what you have done here. Moreover there is a new generated file.

image1

That file include code that will be executed at final step, code sample one from few methods.

```
CreateTable("dbo.Artists", c => new { 
ArtistId = c.Int(nullable: false, identity: true), 
Name = c.String(),}).PrimaryKey(t => t.ArtistId);
```

Now we can execute all of updates. By using Update-Database command. If everything goes properly, let’s change our model to show you, power of Migrations.

Album.cs
```csharp
public DateTime ReleaseDate { get; set; }
```

Song.cs

```csharp
public int Rating { get; set; }
```

Let’s now commit our changes, execute Add-Migration command, result of execution

```csharp
public partial class ReleaseDateAndRating : DbMigration 
{ 
public override void Up() 
 { 
AddColumn("dbo.Albums", "ReleaseDate", c => c.DateTime(nullable: false)); AddColumn("dbo.Songs", "Rating", c => c.Int(nullable: false)); 
 } 

public override void Down()
 { 
DropColumn("dbo.Songs", "Rating"); DropColumn("dbo.Albums", "ReleaseDate");
 } 

}
```

Update database, our database has given these two columns. Now we can freely downgrade or upgrade our version of database. :) so if we decided after thousands of commit to delete these columns

```
Update-Database -TargetMigration:"201301022136026_BaseOfDatabase"
```

Score : For this feature I will give EF two stars in category “Uniqueness”
summary_sec