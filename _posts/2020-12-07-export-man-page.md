---
title: "How to Export man page "
last_modified_at: 2020-12-08T11:59:02+03:00
categories:
  - Blog
tags:
  - Unix
  - Man Page
  - Terminal
---

# Export man page 

Sometimes when use of CLI tool it is very helpful to use man page . But in default mac terminal it is very useful when it comes to search specific terms. 

Currently i found it useful to export as a separate file and use GUI to search terms.

```bash
man -t yourcommand | open -fa "Preview"
```

```bash
$ man -t youtube-dl | open -fa "Preview"
```