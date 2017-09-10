+++
date = "2013-12-22T23:24:02+01:00"
description = "New blogpost for today"
draft = false
title = "401 (Unauthorized) while reset password in Ghost"
weight = 20
aliases = [
    "/401-unauthorized-while-reset-password-in-ghost"
]
+++

Today evening I decided to configure with my new blog engine (Ghost), but unfortunately I forgot my password to admin (yea very clever....dumb ass!). I just thought easy as pie! it's just few minutes smtp configure, after 2 hours I realized that I was changing development configure not a production config, so my smtp config wasn't affect to production environment. Lesson for me : make sure you are working in right place! ;)

Development section in config.js


development



Production section in config.js

production