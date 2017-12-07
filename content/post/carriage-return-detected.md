---
author: steven.murawski@gmail.com
categories: []
comments: true
date: 2017-11-17T00:00:00Z
tags: []
title: Dealing with Carriage Return Detected with Chef and VSCode

---

Rubocop, starting around 48.1 changed its behavior with how line endings are expected on Windows.

I've been in favor of defaulting to Linux line endings since Windows doesn't care and Linux (and a number of OSS tools) tend to break pretty hard with `CRLF`.

[I've previously blogged my Git and VSCode settings to help with that.]({{< relref "vscode-settings.md" >}})

If you are seeing an error from Rubocop in VSCode in your ruby files with `LF` line endings, you can add a setting to the `rubocop.yml` in the root of your project.

In that file set:

```
Style/EndOfLine:
  EnforcedStyle: lf
```

The other area where I've seen this error reported is not having an empty comment line at the top and/or if there is a comment bloc at the top, not having an empty comment at the end.

```
#
# Cookbook:: testing
# Spec:: default
#
# Copyright:: 2017, The Authors, All Rights Reserved.
#

```
