+++
date = "2017-03-30T23:24:02+01:00"
description = "New blogpost for today"
draft = false
title = "Squadron - Next week of work"
weight = 20
aliases = [
    "/squadron-next-week-of-work"
]
+++

Operational_planning_for_the_2011_DRC_elections
Plan for next week

This post shares with you with insights from developing of  getsquadron.io

I managed to outline main feature that I want to have. Let's kick off with something easy, those are main tasks to finish by the end of next week.

    Read simple Json configuration file that describe testing scenario
    Do a N requests against remote server, or you can specify frequency of requests
    Get time of all requests and dump into simple CSV file

My assumption for this week is that I should be able to perform very limited load test against one server. In future I am considering to add more servers. Of course if your application is load balanced you don't need this feature.
Big believer of TDD

I am big believer of TDD especially in a projects that you start from scratch. It couldn't be different in this project. If you are shadowing work and repository you will find more code in test solution than in normal project ;)
Abstract

As you might read in previous post I already outlined my requirements for that project, but finally I made clear abstract, what I want and that abstract will be always source of truth for my direction of work.
Test Web Application

Writing unit test is cool, but I want to have live organism to test against, that's why I decided to write just simple web application that will help to emulate some of scenarios. You might already seen this project because I started series of blog posts about ASP.NET Core application.

    Setting up .NET Core project
    Make and .NET Core