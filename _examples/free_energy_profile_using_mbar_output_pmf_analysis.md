---
title: "GENESIS Input Example: Free energy profile using MBAR output (`pmf_analysis`)"
excerpt: ""
last_modified_at: 2025-06-11T22:49:38+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---


`pmf_analysis` utility calculates the potential of mean force (or free energy
profile) along given collective variable (CV) coordinates. This utility can be
used with the outputs (weight files) of 
[`mbar_analysis`](/examples/mbar_multistate_bennett_acceptance_ratio_mbar_analysis/)
and
[`pathcv_analysis`](/examples/calculate_tangential_and_orthogonal_coordinates_to_a_pathway_from_samples_pathcv_analysis/).
An example shown below illustrates a 1-dimensional free energy profile
calculation by using CV and weight files (output of mbar_analysis).  [Tutorial
of the string method](/tutorials/genesis_tutorial_13.1_2022/) and its material
also illustrates a usage of this utility.


```toml
[INPUT]
cvfile       = {}.cv          # Collective variable files
weightfile   = {}.weight      # weight files from mbar_analysis
 
[OUTPUT]
pmffile      = output.pmf     # potential of mean force file
 
[OPTION]
nreplica     = 16             # number of replicas
dimension    = 1              # dimension of cv space
temperature  = 300            # temperature (K)
grids1       = 1.0 360.0 360    # (min max num_of_bins)
band_width1  = 0.1            # sigma of gaussian kernel
is_periodic1 = YES            # periodicity of cv1
box_size1    = 360.0          # periodic range of cv1
```

The input cvfiles can be different from those used in mbar_analysis.
This means you can "transform" the pmf onto some other cv space. For
example, you can calculate the free energy profie along a dihedral angle
from REUS using a atomic distance coordinates, by following the
procedure described below,

1.  perform REUS run using a distance as CV
2.  analyze pmf using mbar_analysis (free energy along the distance)
3.  using the weight files from 2, run pmf_analysis using a dihedral angle as CV (free energy along the dihedral angle)

Please check
[`mbar_analysis`](/examples/mbar_multistate_bennett_acceptance_ratio_mbar_analysis/)
page for weight files.

