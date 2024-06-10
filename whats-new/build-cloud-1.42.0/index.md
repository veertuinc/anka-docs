---
date: 2024-06-10T01:00:00-00:00
title: "Anka Build Cloud Controller & Registry Version 1.42.0"
---

### New Sub-Menu for Templates

This change splits out the Template List and Distribution UIs into two separate pages.

{{< imgwithlink src="images/whatsnew/1.42.0/newtemplatesui.png" width="300" >}}

### Ability to Search through Templates

You can now search through Templates by name.

{{< imgwithlink src="images/whatsnew/1.42.0/searchtemplates.png" >}}

### Ability to start an Intel VM with a specific VNC Password

You can now set a VNC password for Intel VMs. This is similar to using `ANKA_VNC_PASSWORD` on the CLI.

Your POST request for the API must include a string key/value `vnc_password` in the POST data. The agent will then log something like this:

```text
. . .instance "86ce07b9-c614-4484-78c7-4b738f5c4df2" setting vm VNC password, SHA256 sum for the password is ecd7187. . .
```

### New dialogue showing table/list of templates being deleted

When deleting templates, a new dialogue will show a table/list of templates being deleted.

{{< imgwithlink src="images/whatsnew/1.42.0/templatedeletelist.png" >}}