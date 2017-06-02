---
layout: post
title: "Quick Tip: Restart all the VMs via the Azure Cloud Shell"
author: steven.murawski@gmail.com
comments: true
categories: []
tags: []
---

The [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview) is a handy new addition to the Azure Portal.

Currently, you have access to a bash shell with the Azure CLI tooling (and some other stuff), and a PowerShell option is in limited preview.

I've needed this a couple of times (with some infrastructure testing) and using the Cloud Shell, I could restart all the VMs in my subscription with

```
az vm restart --ids $(az vm list --query "[].id" -o tsv)
```

If you want to limit it to just one resource group, you could use

```
az vm restart --ids $(az vm list --resource-group YourResourceGroupNameHere --query "[].id" -o tsv)
```
