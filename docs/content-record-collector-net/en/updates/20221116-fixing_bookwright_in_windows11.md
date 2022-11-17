---
title: "Fixing Blurb Bookwright in Windows 11"
date: 2022-11-16
description: ""
categories:
  - Windows
  - Photography
images:
  - /files/photography.jpg
authorname: "Mårten Johannesson"
authorimage: "/files/mj.jpg"
robots: "noindex"
---
Fixing a problem in Blurb Bookwright.
<!--more-->
The company [Blurb](https://www.blurb.com) which makes nice photo books which I've used for some years have a program called Bookwright for designing the books. Out of the blue it stopped working on my Windows 11 and said all my projects had corrupt files. The version of Bookwright was 2.4.9 and it was the same version if I uninstalled the program and downloaded it from Blurb.

I got stuck, looking at Windows logs not finding anything, configuring the application to run in Vista mode etc. Finally I tried tried running it as an administrator and now I got a hint that a file had been blocked while running. Go to *Windows Security* in the start menu and *Protection history* in the menu to the left.

In the history event list you should see *Protected folder access blocked*. If you clikc the event you are able to override the rule and now the Bookwright application gets updated from 2.4.9 to 2.5.0 to 2.5.1 and the old projects work as normal again.