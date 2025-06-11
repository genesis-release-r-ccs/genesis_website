---
title: "GENESIS Input Example: WHAM: weighted histogram analysis method (`wham_analysis`)"
excerpt: ""
last_modified_at: 2025-06-11T22:49:38+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---


`wham_analysis` can compute a free energy curve/surface along certain reaction
coordinate(s). This utility supports two types of input; (1) trajectory
(`dcdfile`) or[ (2) collective variable (`cvfile`)]. Basically, we recommend you
to use the latter style, since that overall computational cost is usually lower
than the former one. You should prepare `cvfile` from `trj_analysis` or
something equivalent to it at first. We here show sample input files of
`wham_analysis`. (One-dimensional sample is almost the same as the one used in
[Tutorial-2.2](/tutrials/genesis_tutorial_2.2_2022/).)  Note that this
`wham_analysis` supports OpenMP parallelization. If you can use a multicore CPU
in this analysis, setting `OMP_NUM_THREADS` environmental variable may improve
the efficiency of wham calculations.


```toml
# Sample of one-dimensional wham
[INPUT]
cvfile         = input{}.cv   # Collective variable file
                              # For REUS runs,
                              # list of values for each replica parameter ID
                              # (you may need to run remd_convert or something                                   #  equivalent to prepare input data)

[OUTPUT]
pmffile        = output.pmf   # potential of mean force file

[WHAM]
dimension      = 1            # WHAM dimension
nblocks        = 1            # number of data blocks (>0)
                              # input data will be evenly divided into nblocks
                              # blocks solved independently and then merged
                              # if you have too many amounts of data,
                              # you may consider to increase this value
temperature    = 300.0        # temperature used for pmf calculation (K)
tolerance      = 10E-08       # convergence criterion
rest_function1 = 1            # restraint function
grids1         = 0.0 15.0 301 # grid definition
                              # (min value) (max value) (number of grids)
                              # in this case, grid size is 0.05.
                              # (number of grids) might be determined as
                              # ((max value) - (min_value) / (gridsize)) + 1

[RESTRAINTS]
# function type should not be specified if cvfiles are used
constant1      = 1.2 1.2 1.2 1.2 1.2 1.2 1.2 \
                 1.2 1.2 1.2 1.2 1.2 1.2 1.2
reference1     = 1.80 2.72 3.64 4.56 5.48 6.40 7.32 \
                 8.24 9.16 10.08 11.00 11.92 12.84 13.76
is_periodic1   = NO           # (whether first and last ones are adjacent)

# Sample of two-dimensional wham
[INPUT]
cvfile         = cv{}.dat   # 1D series. if there are 2*4 replicas,
                            # cv1.dat .. cv8.dat will be used.

[OUTPUT]
pmffile        = pmf.dat

[WHAM]
dimension      = 2          # 3 or more higher dimensions are not yet supported
nblocks        = 1          # nblocks > 1 is not supported for 2d
temperature    = 300
tolerance      = 1E-08
rest_function1 = 1
rest_function2 = 2
grids2         = 4.0 11.0 71
grids1         = 7.0 15.0 81

[RESTRAINTS]
constant2      = 3.0 3.0 3.0 3.0 3.0 3.0 3.0
reference2     = 5.5 6.5 7.2 7.9 8.7 9.5 10.3
is_periodic2   = NO

constant1      = 2.0 2.0 2.0 2.0 2.0 2.0
reference1     = 7.5 8.75 9.5 10.5 11.5 12.5
is_periodic1   = NO
```

`cvfile` format:
```
(step)  (value1) (value2) ....
```

Number of values must be the same as "dimension" parameter value in
`[WHAM]` section. Step numbers (1st column) are required but not used in
the actual calculations.

Example cvfile (intermediate data of [Tutorial-2.2](/tutorials/genesis_tutorial_2.2_2022/)):
steps and distances of replica parameter 1 (calculated by [trj_analysis](/examples/distance__angle__dihedral_angle__com_distance__com_angle__com_dihedral_angle_trj_analysis/))
```
  1   2.088
  2   2.300
  3   1.810
  4   2.324
  5   2.522
  6   2.585
  7   2.117
  8   2.356
  9   2.185
 10   2.048
...
```

Example cvfile (2D):
```
 100  7.6504  4.9960 
 200  7.7205  4.9158 
 300  7.7520  5.0611 
 400  7.7510  4.9667 
 500  7.7095  5.1865 
 600  7.6802  4.9187 
 700  7.6914  5.0818 
 800  7.7069  5.3156 
 900  7.6633  5.4473 
1000  7.6438  4.9710 
...
```
