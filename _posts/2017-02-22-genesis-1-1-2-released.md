---
title: GENESIS 1.1.2 released!
excerpt_separator: "<!--more-->"
categories:
 - News
---

This version contains a new feature and some fixes as follows.

New feature:

<!--more-->
-   (spdyn) Cartesian coordinates can be employed as CV in the string
    method

Fixes:

-   (rpath_generator) improved functionality
-   (atdyn/spdyn) more smart handling of restart files
-   (spdyn) show GPU hardware information in log file
-   (spdyn) multiple GPUs in a single node can be used effectively in
    non-OpenMPI environment (such as Intel MPI)
-   (spdyn) `structure_check` is implemented in spdyn (runtime structure
    check upon pairlist generation)
-   (atdyn) support CHARMM19

Please also check [the updated usage page]({{ "/docs/usage_on_gpu_workstations" | relative_url }}) about GPUs.
