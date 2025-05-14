---
title: GENESIS 1.1.5 released!
excerpt_separator: "<!--more-->"
categories:
 - News
---

This version contains a new feature and some fixes as follows.

New feature:

<!--more-->
-   (atdyn/spdyn) Fitting method (used in targeted MD, steered MD,
    string method etc.) renewal; \[FITTING\] section is introduced to
    extend the capability of structure fitting.
-   (spdyn) improve GPU detection mechanism (automatically ignore GPUs
    with CC \< 3.5)

Fixes:

-   (atdyn/spdyn) Fix of R-path (string method)
-   (atdyn/spdyn) fix random generator used in BUSSI thermostat/barostat
-   (spdyn) fix for communication tag
-   (spdyn/tests) update of BUSSI NVT (regression test is also updated)
-   (atdyn/spdyn) fix of gromacs input for SMOG2
-   (atdyn/spdyn) fix of restart file reader (K-computer and FX100 only;
    there could be large delay when reading input files (not always))
-   (tests) test.py script now accepts relative path for the atdyn/spdyn
    executables
