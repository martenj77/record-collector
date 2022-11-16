---
title: "Being lazy with git"
date: 2022-11-16
description: ""
categories:
  - Windows
images:
  - /files/git01.jpg
authorname: "Mårten Johannesson"
authorimage: "/files/mj.jpg"
robots: "noindex"
---
I run the combo "git add . + git commit -m "something" + git push" so many times...
<!--more-->

In Powershell there is a settings file that run everytime. To edit it in [VS Code](https://code.visualstudio.com/), in a powershell terminal - run `code $profile`.

Once there, just do a little something like this and save.

```ps
function quickgit() {
    Param ([string]$1)    
    git add .
    git commit -a -m "$1"
    git push
}
```
And reload your powershell terminal.

Now you can do `quickgit "nice message"` instead of..

```ps
git add .
git commit -m "nice message"
git push
```

Nice.