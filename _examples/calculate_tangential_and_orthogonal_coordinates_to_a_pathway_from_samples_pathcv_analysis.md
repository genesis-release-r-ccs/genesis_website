---
title: "GENESIS Input Example: Calculate tangential and orthogonal coordinates to a pathway from samples (`pathcv_analysis`)"
excerpt: ""
last_modified_at: 2025-06-11T22:49:37+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---


`pmf_analysis` calculates tangential and orthogonal coordinates to a pathway
from samples. This utility is expected to be used in the context of string
method calculation.


```toml
[INPUT]
pathfile     = ../../trajectories/path/path.dat  # pathway coordinates
cvfile       = ../../trajectories/path/{}.dat    # collective coordinates
 
[OUTPUT]
pathcvfile   = {}.pathcv     # path coordinates (tangential and orthogonal)
 
[OPTION]
nreplica     = 4             # number of replicas
```
