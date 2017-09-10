+++
date = "2017-03-29T23:24:02+01:00"
description = "New blogpost for today"
draft = false
title = "Makefile and .NET Core"
weight = 20
aliases = [
    "/makefile-and-net-core"
]
+++

building-blocks-456616_1280
Overview

In this post you will learn what is build script or build tool, what products are out there and why it's helpful during whole life cycle of you application.

It it a part of "Get Noticed!" series, check out how to create your first .NET Core application. If you already know, check out this repository
What is build script ?

Build script tools automate certain task that you have to do repeatedly with your
code, data store, application and other tools.
For example if you would like to get a clean run of our application (.NET Core).
You have to invoke four commands - dotnet clean, dotnet restore, dotnet build, dotnet run that is pretty irritating if you have to type that N times, over and over again.

What if you would like to get clean database each run ? Or let's say that you would like to patch some meta data file with version every time when you publish new version of application or library, again very problematic.

You don't really want to do it every time by hand, to be honest, sometimes you cant!
Like if your publishing or deploying process is handled by external machine, then it's impossible to do such a step manually. That's why we want to script it!

You will see later that you can automate various things, not only building your application but further steps in life cycle of your project
What is make ?


    GNU Make is a tool which controls the generation of executables
    and other non-source files of a program from the program's source files.

    (source)

In simple way, GNU Make allows you to describe to create executable file, but you will see that not only!
Where that can be useful ?
Project building

We mentioned above that getting a clean run, required from us a lot of commands typing, OK but hold on! Let's create Makefile.

run-clean: cleanup restore build run 

all : cleanup restore build

cleanup:
	dotnet clean src/Squadron.TestApplication/

restore:
	dotnet restore src/Squadron.TestApplication/

build:
	dotnet build src/Squadron.TestApplication/

run:
	dotnet run -p src/Squadron.TestApplication/Squadron.TestApplication.csproj

That way you can describe how to build all your projects. As you can see we just simply wrap up our well known .NET building tool commands with Make clean, restore, build, run .

Moreover we chained up those with one command run-clean, so now if you run make run-clean Make should run all other commands in order, at the end you should get running application, isn't simpler ?
Make and Docker

I assume that you know what Docker is. If not there will be post coming in next few days, so stay tuned!

Let's say that your application depend on PostgresSQL, for development you want to have clean database each time you run your application.

run-with-db: database-prep run-clean

run-clean: cleanup restore build run 

all : cleanup restore build

cleanup:
	dotnet clean src/Squadron.TestApplication/

restore:
	dotnet restore src/Squadron.TestApplication/

build:
	dotnet build src/Squadron.TestApplication/

run:
	dotnet run -p src/Squadron.TestApplication/Squadron.TestApplication.csproj

database-cleanup:
	docker stop squadron-test-application
	docker rm squadron-test-application

database-prep:
	docker pull kiasaki/alpine-postgres
	docker run --name squadron-test-application -e POSTGRES_PASSWORD=mysecretpassword -d kiasaki/alpine-postgres

As you can see we added three new steps in our Makefile. run-with-db ,database-cleanup and  database-prep , again we just wrap up some well known docker commands, like stop and remove container or pull image and run container. At the end we chained up to run-with-db so that clean database will be prepared and then our application  will be run.
It's not end!

Those two cases are just few where you can use Makefile to make your life easier. Stay tuned there will be more scenarios where you can use build tools.

Make is not just one tool out there, check out also FAKE, CAKE.