---
title: "GENESIS Input Example: Diffusion constant (diffusion_analysis)"
excerpt: ""
last_modified_at: 2025-06-11T22:49:37+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---


`diffusion_analysis` calculates the translational diffusion coefficients from
either simulation trajectory files or MSD data files produced by `msd_analysis`.
It is necessary to understand how msd_analysis works before using
diffusion_analysis, so please study the
[msd_analysis](/examples/mean-square_displacement_msd_analysis/) page if you
haven't done so already.

The following text is the help message that is output with `./diffusion_analysis
-h ctrl`:

A sample input file to calculate diffusion coefficients when MSD data is
already provided:
```toml
# This program calculates translational diffusion coefficient.

# The translational diffusion coefficient is
# computed from MSD data by a simple least
# squares fitting method.

# This program uses MSD output produced by
# msd_analysis. The "msdfile" file in the
# [INPUT] section specifies this file. 
# Please see msd_analysis help for a more 
# detailed description.

# The "ndofs" option in [OPTION] section specifies
# the degrees of freedom for the MSD data.
# When the value is a single number, the specified
# degrees of freedom are applied for all MSD sets.
# When the value is a space separated number
# sequence, the degrees of freedom are set
# separately for each MSD data set, but it
# has to match the amount of MSD data sets
# exactly.
[INPUT]
msdfile = BPTI.msd   # msd data obtained by msd_analysis

[OUTPUT]
outfile = output.msd # msd data with fitted data

[OPTION]
time_step = 2     # time interval of one step in MSD data is 2 ps
start = 20 %      # skip initial 19 % of MSD data when fitting
```
