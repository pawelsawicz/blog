+++
date = "2017-03-15T23:24:02+01:00"
description = "New blogpost for today"
draft = false
title = "Setting up .NET Core project"
weight = 20
aliases = [
    "/setting-up-net-core-project"
]
+++

Overview

This tutorial will work on most of OS, but I encourage you to get a Ubuntu, to go out of your comfort zone. It's very easy to start with Linux. Working with console is very important to me, so that all work is done via command line.

Article is part of "Get Noticed!", project squadron, so we will try to recreate what I have already done to Squadron.TestApplication repository
Install .NET Core

Installation of .NET Core is super easy, and I don't want to copy what is already written so check out dotnet core website, and follow their steps. If you have any problems let me know in a comment below, I will try to help you.
Validating of installation

Once you installed framework, open command line and invoke

dotnet --version

you should get at least 1.0.1 (depend what time you are reading this article)

dotnetversionresult
Create folder structure

Main difference between standard .NET framework tooling and .NET Core. Previously you didn't have to have folder structure, your projects could live in different places in your system. That information was stored in solution file.

.NET Core moved towards folder structure where you are not forced to have solution file that keep all information about your projects, so that you can open it in different IDEs or editors.
Let's kick off with creating a directory structure, you can download full script here
cd ~/
mkdir Squadron.TestApplication
cd Squadron.TestApplication
mkdir src
cd src

Convention says that you should have two folders src and tests placed at the top folder of your project. For a now we just stick with src folder, because we are not going to create any test project yet
Initializing project

Make sure that you are inside src folder, then invoke in order.

mkdir Squadron.TestApplication
cd Squadron.TestApplication
dotnet new console

That will initialize your first project.
Base commands

There are 4 base commands that you have to know, for more commands just invoke dotnet --help in console.

    dotnet new, initialize new project and create base files
    dotnet restore, restore all packages
    dotnet build, compile your project
    dotnet run, run your project

After you initialized your new project run those commands in a order

    dotnet restore
    dotnet build
    dotnet run

After last command you should get your first "Hello world"
What is next ?

Stay tuned in the next post will carry on this project and implement API based on ASP.NET Core