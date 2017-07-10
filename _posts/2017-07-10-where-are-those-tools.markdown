---
layout: post
title: "Where Are Those Tools?"
author: steven.murawski@gmail.com
comments: true
categories: []
tags: []
---

I was looking to try out [`yo team`](http://donovanbrown.com/post/yo-Team) to look at some possible extensions.  I found that `node` and `npm` were already installed on my machine (though I didn't remember installing it - the chocolaty package was there.).

After googling around a bit, I found

```
npm install --global yo
```

But, I still couldn't launch `yo`.

```
C:\Users\stmuraws>yo
'yo' is not recognized as an internal or external command,
operable program or batch file.
```

Turns out that `npm` installs the command stubs to `%USERPROFILE%\AppData\Roaming\npm`, which was not on my path.  Adding that to my path made everything happy.
