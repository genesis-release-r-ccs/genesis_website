---
title: "GENESIS Input Example: Structure clustering using the k-means algorithm (`kmeans_clustering`)"
excerpt: ""
last_modified_at: 2025-06-11T22:49:38+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---


"`kmeans_clustering`" is a tool to cluster the output conformations from MD
trajectory. The tool uses k means algorithm where the user needs to specify the
number of clusters as well as the selection criteria. The results can be further
refined by performing a subsequent calculation upon reading the previous
clustering "`indexfile`". So in principle the user can start the calculation
using large number of clusters and then re-run the calculation to reduce the
number of clusters upon checking the cluster radius in the output file. The tool
generates separate dcd file for each cluster which can be used for further
analysis with other tools.


```toml
[INPUT]
psffile        = input.psf       # protein structure file
reffile        = input.pdb       # PDB file
indexfile      = input.idx       # Index file
 
[OUTPUT]
indexfile      = output.idx      # Index file
pdbfile        = output_{}.pdb   # PDB files
trjfile        = output{}.dcd    # trajectory file
 
[TRAJECTORY]
trjfile1       = sample.dcd      # trajectory file
md_step1       = 500000          # number of MD steps
mdout_period1  = 500             # MD output period
ana_period1    = 500             # analysis period
repeat1        = 1
trj_format     = DCD             # (PDB/DCD)
trj_type       = COOR+BOX        # (COOR/COOR+BOX)
trj_natom      = 0               # (0:uses reference PDB atom count)
 
[SELECTION]
group1         = segid:PROA      # selection group 1
group2         = an:CA           # selection group 2

 
[FITTING]
fitting_method = TR+ROT          # NO/TR+ROT/TR/TR+ZROT/XYTR/XYTR+ZROT
fitting_atom   = 2               # atom group
mass_weight    = yes              # mass-weight is not applied
 
[OPTION]
check_only      = NO             # only checking input files (YES/NO)
analysis_atom   = 1              # target atoms for the cluster analysis
num_clusters    = 10             # number of clusters
max_iteration   = 100            # max number of iterations for k-means algorithm
stop_threshold  = 98.0           # stop threshold of convergence (%) for k-means algorithm
num_iterations  = 10             # number of iterations for calculating averaged coordinates
trjout_atom     = 1              # atom selection for pdbfile and trjfile
trjout_format   = DCD            # (PDB/DCD)
trjout_type     = COOR           # (COOR/COOR+BOX)
iseed           = 3141592        # random number seed
```
