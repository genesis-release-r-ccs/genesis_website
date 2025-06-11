---
title: "GENESIS Input Example: Generate inputs for string method calculation from Targeted MD data (`rpath_generator`)"
excerpt: ""
last_modified_at: 2025-06-11T22:49:38+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---


`pmf_analysis` generate inputs for string method calculation from Targeted MD
data. This utility is expected to be used after targetd MD simulation for
generating an initial pathway for subsequent string method calculation.


```toml
[INPUT]
cvfile       = cv.cv          # collective variable file
dcdfile      = run.dcd        # trajectory file of coordinates
dcdvelfile   = run.dvl        # trajectory file of velocities
prmtopfile   = 1ake.top       # amber parmtop file
pdbfile      = 1ake.pdb       # pdb file
 
[OUTPUT]
pdbfile      = {}.pdb       # pdb files used as reffile for string method
rstfile      = {}.rst       # restart files used for string method
 
[OPTION]
nreplica     = 4             # number of replicas
```
