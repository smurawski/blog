---
title: "Hotkeys for the new Windows Terminal Alpah"
date: 2019-05-14T00:00:00-06:00
author: 'Steven Murawski'
tags: []
comments: true
draft: false
---

**Warning!**

*These keybindings are subject to change and effective as of May 14, 2019.*

I started playing with the new [Windows Terminal](https://github.com/microsoft/Terminal).  I love hotkeys and live by them. A [bit of spleunking in the code](https://github.com/microsoft/Terminal/blob/6088134832b851c553af791ee17a9b5c0d808385/src/cascadia/TerminalApp/CascadiaSettings.cpp#L167) brought out:

* `Ctrl+T`                 - New default profile tab
* `Ctrl+W`                 - Close current tab
* `Ctrl+Tab`               - Next tab
* `Ctrl+Shift+Tab`         - Previous tab
* `Ctrl+Shift+1`           - New first profile tab
* `Ctrl+Shift+2`           - New second profile tab
* `Ctrl+Shift+3`           - New third profile tab
* `Ctrl+Shift+4`           - New fourth profile tab
* `Ctrl+Shift+5`           - New fifth profile tab
* `Ctrl+Shift+6`           - New sixth profile tab
* `Ctrl+Shift+7`           - New seventh profile tab
* `Ctrl+Shift+8`           - New eight profile tab
* `Ctrl+Shift+9`           - New ninth profile tab
* `Ctrl+Shift+0`           - New tenth profile tab
* `Ctrl+Shift+Page Up`     - Scroll up
* `Ctrl+Shift+Page Down`   - Scroll down
* `Alt+1` - Switch to tab 1
* `Alt+2` - Switch to tab 2
* `Alt+3` - Switch to tab 3
* `Alt+4` - Switch to tab 4
* `Alt+5` - Switch to tab 5
* `Alt+6` - Switch to tab 6
* `Alt+7` - Switch to tab 7
* `Alt+8` - Switch to tab 8
* `Alt+9` - Switch to tab 9
* `Alt+0` - Switch to tab 10

## Building and configuring the terminal

Rather than duplicate his work, I'll point you to Donovan Brown's posts on getting the terminal project built and running.

* [Building the new Windows Terminal with Visual Studio 2019](http://donovanbrown.com/post/Building-the-new-Windows-Terminal-with-Visual-Studio-2019)
* [How to add profiles to the new Windows Terminal](http://donovanbrown.com/post/How-to-add-profiles-to-the-new-Windows-Terminal)
* [Cursor shapes for new Windows Terminal](http://donovanbrown.com/post/Cursor-shapes-for-new-Windows-Terminal)
