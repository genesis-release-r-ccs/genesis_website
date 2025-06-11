---
title: "GENESIS Input Example: Convert restart file format from those in Ver. 1.0 to Ver. 1.1 or later (`rst_upgrade`)"
excerpt: ""
last_modified_at: 2025-06-11T22:49:37+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---


`rst_upgrade` utility converts a restart file with old format (GENESIS Ver. 1.0) to one with the current format (GENESIS Ver. 1.0 or later).


```toml
[INPUT]
rstfile       = input.rst     # restart file (old format)
 
[OUTPUT]
rstfile       = output.rst    # restart file (new format)
```
