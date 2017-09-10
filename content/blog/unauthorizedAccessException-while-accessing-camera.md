+++
date = "2014-04-17T23:24:02+01:00"
description = "New blogpost for today"
draft = false
title = "UnauthorizedAccessException while accessing camera"
weight = 20
aliases = [
    "/unauthorizedaccessexception-while-accessing-camera"
]
+++

Recently I got that kind of Exception, UnauthorizedAccessException it occurred when I wanted initialize my webcamera in Windows 8 application.

access_deny_microphone_w8

The weirdest thing is that if your application just takes a photo you have to include in requirements that it has to allow a microphone...yes to microphone. Because CaptureControl.Source is a type of MediaCapture, and this class provide a possibility of  taking a photos, recording video and sound, in my opinion this is very weird because if I am making a embedded system it force from me that I have to provide a hardware that has got microphone.

In my opinion there should be a separate classes that provides taking a photos, and recording a video/sound.

Solution to that exception: You have to check in capabilities that your application require a microphone.

microphone_capabilities_w8


P.S "Odmowa dostępu" - yep I have polish version of VS, but it means "Access deny"


