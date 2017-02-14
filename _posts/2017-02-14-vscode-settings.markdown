---
layout: post
title: "My Git and VS Code Settings"
author: steven.murawski@gmail.com
comments: true
categories: []
tags: []
---

I work regularly with several different project types and languages across Windows, Mac, and Linux.

I find some basic settings for Git and VS Code smooth out some headaches.

For VS Code, I have (at a minimum) several base plugins:

* C#
* Chef Extension for Visual Studio Code
* Git Blame
* Git History (git log)
* PowerShell
* Rust
* XML Tools

And my user settings consists of:

```
"files.eol": "\n", // Windows usually doesn't care, but Linux/Mac tooling is way more sensitive
"editor.trimAutoWhitespace": true,
"editor.fontSize": 16,
"rust.cargoHomePath": "C:\\Users\\Steven\\.cargo", // Path to Cargo home directory, mostly needed for racer. Needed only if using custom rust installation.
"editor.formatOnPaste": true,
"editor.formatOnSave": true,
"window.zoomLevel": 0
```

My git config consists of:

```
[credential]
	helper = wincred
[user]
	email = <my email here>
	name = Steven Murawski
	signingkey = <my gpg signing key here>
[diff]
	tool = default-difftool
[difftool "default-difftool"]
	cmd = code --wait --diff $LOCAL $REMOTE
[alias]
	commit = commit -S -s
[core]
	editor = code --wait
	eol = lf
	autocrlf = input
	excludesfile = ~/.gitignore
[difftool "winmerge"]
	cmd = /c/Program\\ Files\\ \\(x86\\)/WinMerge/WinMergeU.exe
[gpg]
	program = c:/Program Files (x86)/GNU/GnuPG/gpg2.exe
```

Since I work on a number of projects that require a [DCO signoff](https://developercertificate.org/), I just default to signing off my commits (`alias.commit`) and I GPG sign them because I like how they look in the GitHub UI. ;)

I've set git to only use Linux line feeds for new clones/fetches (`core.eol`) and change anything with CRLFs on upload (`core.autocrlf`).
