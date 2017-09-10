+++
date = "2013-01-16T23:24:02+01:00"
description = "New blogpost for today"
draft = false
title = "IIS mod_rewrite – Wordpress fix"
weight = 20
aliases = [
    "/iis-mod_rewrite-wordpress-fix"
]
+++

tl;dr : How to fix IIS with lack of mod_rewriter. Used to Friendly URLs

My blog is powered by Oktawave. I decided to keep here my personal projects and blog, when I installed my blog, I wanted to configure friendly url, for SEO purpose, but unfortunately was a problem. Basic IIS wasn’t configured.

First of all you have to download ISAPI_Rewrite 3 make sure that it’s lite version next step is is add ISAPI to IIS filter site, navigate your IIS Manager to ISAPI Filters

isapifilters

Then click add, and fill path to your executable file, named “ISAPI_Rewriter3.dll”.

Last but not least is fill your httpd.conf file, if you have lite version it’s places in C:Program FilesHeliconISAPI_Rewrite3 folder, otherwise find this at your web folders. Edit file with next context

RewriteBase /
 RewriteCond %{REQUEST_FILENAME} !-f
 RewriteCond %{REQUEST_FILENAME} !-d
 RewriteRule ^(.*)$ index.php?p=$1 [NC,L]

Finally save & restart IIS Server.