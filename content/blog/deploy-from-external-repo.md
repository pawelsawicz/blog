+++
date = "2013-05-02T23:24:02+01:00"
description = "New blogpost for today"
draft = false
title = "Deploy from external repo"
weight = 20
popular = 1
aliases = [
    "/azure-external-deploy"
]
+++

    tl;tr; Deploy from external repo on MS Azure

Few days ago I was on a Global Windows Azure Bootcamp,We had few sessions, one of those session was lectured by @mpraglowski, he showed us this wonderful feature. What if we want to deploy our website on each git push without any extra action on our server ? With help come Azure Web Sites.

First, create new web site by choosing New -> Compute -> Web site-> Custom createcreatewebsite_0

Next step will be, fill out these boxes. Important is that to check box "Publish from source control".
createwebsite_1

Third step is to choose which service we want to use (Bitbucket, TFS, Github etc.)

screatewebsite_2

In final step of creating web site, we choose repository that will be use.

createwebsite_3

If there is no error we should see deploy with a green header like in this picture. Azure has a nice feature, we can freely manage which git push we want on our website any time. Let's imagine that you push commits but something don't work exactly how you assumed. You can change to early version of deploy just clicking on it and clicking "redeploy" on bottom of website

createwebsite_4