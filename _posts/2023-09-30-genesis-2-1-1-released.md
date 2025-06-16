---
title: GENESIS 2.1.1 released
excerpt_separator: "<!--more-->"
categories:
 - News
sidebar:
  nav: sidebar-basic
---

This version has the following updates.

-   Add energy_analysis for QM/MM calculation
-   Enable prediodic CV in pmf_analysis
<!--more-->
-   (ATDYN/SPDYN) Update simd directive for Intel compiler version \>
    23.1
-   (ATDYN) Increase output frequency
-   (SPDYN) Update configure file for CUDA \> 12.0
-   (ATDYN) Added QM interface with ORCA
-   (SPDYN) Bugfix: fix timing of transfer charge values to GPU card in
    gREST
-   (SPDYN) Bugfix: Ryckaert-Bellemans dihedral energy sum
-   (SPDYN) Bugfix: Virial term in amber bond correction was not written
    correctly and corrected.
-   (SPDYN) Bugfix: error in water number count is fixed in TIP4P case.
-   (ATDYN/SPDYN) BugFix: fix generation of random seed in replica
    simulations (REMD/RPATH)
-   (ATDYN/SPDYN)  BugFix: fix wrong file name in large number of
    replicas

Get GENESIS 2.1.1 now from our [download
page](https://github.com/genesis-release-r-ccs/genesis/releases/tag/v2.1.1)!
