---
layout: post
title: "Dealing with Carriage Return Detected with Chef and VSCode"
author: steven.murawski@gmail.com
comments: true
categories: []
tags: []
---

Rubocop, starting around 48.1 changed its behavior with how line endings are expected on Windows.

I've been in favor of defaulting to Linux line endings since Windows doesn't care and Linux (and a number of OSS tools) tend to break pretty hard with `CRLF`.

[I've previously blogged my Git and VSCode settings to help with that.](http://stevenmurawski.com/powershell/2017/02/vscode-settings/index.html)

If you are seeing an error from Rubocop in VSCode in your ruby files with `LF` line endings, you can add a setting to the `rubocop.yml` in the root of your project.

In that file set:

```
Style/EndOfLine:
  EnforcedStyle: lf
```
