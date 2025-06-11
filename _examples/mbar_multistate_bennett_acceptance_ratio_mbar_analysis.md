---
title: "GENESIS Input Example: MBAR: multistate Bennett acceptance ratio (`mbar_analysis`)"
excerpt: ""
last_modified_at: 2025-06-11T22:49:38+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---

[Multistate Bennett acceptance ratio (MBAR; JCP 129, 124105 (2008))
](http://dx.doi.org/10.1063/1.2978177) method estimates relative free energy
between states. Using this method, `mbar_analysis` utility calculates relative
free energies from given data sets. MBAR is a kind of general purpose method and
the method is applicable to various kinds of calculations (such as free energy
surface, binding free energy, etc.). Currently, `mbar_analysis` of GENESIS is
able to treat the data sets of umbrella sampling, temperature-REMD, REST, FEP,
and REST.

As like
[wham_analysis](/examples/wham_weighted_histogram_analysis_method_wham_analysis/),
`mbar_analysis` supports two types of input files, (1) trajectory files
(`dcdfile`) and (2) collective variable files (`cvfile`).  `cvfile` file, is
recommended as in the case of `wham_analysis`. The sample below show an example
of reading a `cvfile`. The file format of `cvfile` is the same as the case of
[wham_analysis](/examples/wham_weighted_histogram_analysis_method_wham_analysis/).

Below is a sample input (ctrl) file for `mbar_analysis`. This input calculates
the relative free energy between states (the "state" here means a umbrella
window) and the free energy along the given restraint coordinate. If you would
like to get the potential of mean force along a certain pathway,
[pathcv_analysis](/examples/calculate_tangential_and_orthogonal_coordinates_to_a_pathway_from_samples_pathcv_analysis/)
and
[pmf_analysis](/examples/free_energy_profile_using_mbar_output_pmf_analysis/)
must be executed before using `mbar_analysis`. Please check samples of these
utilities and [string method tutorial](/tutorials/genesis_tutorial_13.1_2022/).

`mbar_analysis` also supports OpenMP parallelization when the environmental
variable `OMP_NUM_THREADS` is correctly set.

```toml
# mbar_analysis sample, keywords are almost same as that of wham_analysis
# inputs of 2D-MBAR is similar to that of 2D-wham_analysis
[INPUT]
cvfile = ../5_calc_dist/parameter/{}.dis # input cv file

[OUTPUT]
fenefile       = fene.dat     # relative free energy between windows
pmffile        = pmf.dat  # potential of mean force
weightfile     = {}.weight # sample weights used by pmf_analysis

[MBAR]
nreplica       = 16
input_stype    = US # US REMD FEP REST MBGO
dimension      = 1
nblocks        = 1
temperature    = 300.0
target_temperature = 300.0
tolerance      = 10E-08
rest_function1 = 1
grids1         = 0.0 15.0 301

[RESTRAINTS]
constant1      = 1.2 1.2 1.2 1.2 1.2 1.2 1.2 \
                 1.2 1.2 1.2 1.2 1.2 1.2 1.2
reference1     = 1.80 2.72 3.64 4.56 5.48 6.40 7.3 \
                 8.24 9.16 10.08 11.00 11.92 12.84 13.76
is_periodic1   = NO
```
