+++
date = "2016-06-13T23:24:02+01:00"
description = "New blogpost for today"
draft = false
title = "Refactoring legacy GUI application to CLI"
weight = 20
aliases = [
    "/refactoring-legacy-gui-application-to-cli"
]
+++

If you ever wonder how it is to work with 15-year-old legacy C++ code, and how to make refactoring, this blog is perfect for you :) 

As ou may remember, I promised you to show you work that I do for Helbreath. When I decided to work on that, the first decision I made was to try to get rid of this horrible GUI, that was aperitif before I do more serious work.

Before we dive into C++ code, let have a discussion why GUI is evil in your backend server applications, shall we ?
If backend service, CLI only!
Be clean

The first argument is that your code is much cleaner because a program doesn't have an unnecessary code, which is responsible for drawing and behaviour of your GUI, additionally you don't mix context of GUI and context of your service. Which means that you don't have noise in your code.
Fewer resources and dependencies

Another very important argument, your server will need fewer resources to run your application. Even you can run your OS without GUI in headless mode.

But, wait what with fewer dependencies ? If you don't have GUI your code immediately has fewer dependencies to external libraries, pure profit! That way you don't have to manage additional packages and worry that something won't work on "very special" environment or OS settings. Moreover, developers who want to work on that project are less likely to get problems with the project set up.
Automatisation and ops work

The ultimate argument that you have to read and it applies to any software in production for more than 1 people.

Having you service as a CLI will help a lot with ops work, with CLI service you can automate everything from deployment, templating, to a startup of your application. Whereas with GUI application you can't do that very easily, due to involved manual steps.

Next important argument - remote access.

Ideally, you don't have to have access to the whole server/VM/machine to maintain your application, instead, you can easily connect to this application remotely and manage from your computer. This approach is more secure, we are avoiding direct access to a server and we also can whitelist IP address with specific port.
Refactoring time
Old Way

Let's move on to our services that make Helbreath server running.

old_way_hb_services

Above you can see how two services looked like before refactorization, it was horrible GUI, lot of manual steps, such as providing username/password to the database. Each time you restart application you have to manually put credentials to the database. Imagine now that I do 20-30+ releases a day, it means that I would need to waste my keystrokes each time.
New Way

new_way_hb_services

In the other hand, this image above shows the current state of both services. It is much more beautiful, isn't it ?!

Pure console, with some output information and nothing else!

No dialogue boxes, no fucking buttons, no weird messages. Just pure console.
But HOLA! wait! How can you see what is going on with your services ?

Easy answer!

Logs, Luke log everything!

In this case, I log everything to file and then use nxlog to send to papertrail.

papertrail_log
Now let's check some code!

At the beginning I created a story on Github, just to have some place where I can track my work, and then pull request.

This is very important, every refactorization in legacy code (this is almost 15 yo) is big and difficult. At this example, I will try to share some of the mine strategies.
Make a research!

Spend a good time to analyse code and dependencies. For this specific problem, it took me like 1-2 days to understand a problem and come up with a solution. This is very important for young junior developers! You are a problem solver, not a code monkey, research is part of your job, don't worry if you send one or two days on researching something.

Since I wanted to get rid of GUI, I had to check which part of code has the dependency to GUI the code or libs. So as you can see here and here I listed out all main places where GUI sits.
Cheat, Wrap, Hide!

My next advice is, cheat if you can, don't refactor everything at once!

Do small bits until your old code will be so granular that you can understand the domain, and rewrite it. In this commit, I wrapped all the things into a new class.

I cheated because it still has GUI dependency (to the HWND class) but it's hidden. But at least it doesn't have code for dialog boxes, buttons etc.
Remove, Remove, Remove!

Most enjoyable part is, removing unused code and here, as you can see I removed a lot of graphics drawing specific stuff, which is not in use anymore.

At the end of this refactoring, I still have some dependencies to GUI, mostly to HWND class, but it is necessary to run a service because old windows messaging use this library to create async calls via TCP/UDP. Yeah, you read that correctly messaging require GUI dependency, total madness. It ended up with fairly ok refactored code, I don't need any manual step apriori to run a server, everything is automated. I am ok with that for a now.