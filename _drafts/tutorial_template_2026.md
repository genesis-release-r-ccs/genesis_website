---
title: "GENESIS Tutorial X.Y (2026)"
gpos: 00X.00Y
excerpt: "[One sentence description of this chapter]"
last_modified_at: 2026-XX-XXT00:00:00+09:00
layout: single
toc: true
toc_sticky: true
sidebar:
  nav: sidebar-basic
---

<!-- Hardware tier (pick one, delete others) -->
<!-- - <i class="fas fa-laptop"></i> Laptop (≤ 4 CPU cores) -->
<!-- - <i class="fas fa-computer"></i> Workstation (~ 16 CPU cores) -->
<!-- - <i class="fas fa-network-wired"></i> Supercomputer (≥ 64 CPU cores) -->

> <i class="fas fa-laptop"></i> Laptop (≤ 4 CPU cores)
> **Prerequisites:** [Tutorial X.A](link) and [Tutorial X.B](link).
> **Estimated time:** ~XX minutes on a [laptop/workstation/supercomputer].

<!-- Add the following line only if a minimum GENESIS version is required -->
> **Notice:** This tutorial requires GENESIS v2.X or later.

# [Full chapter title]

## Overview

[2–4 sentences: what system, what method, what the reader will learn.]

This tutorial consists of [N] steps:
1. [Step name]
2. [Step name]
3. [Step name]

## Preparation

All files are in the [GENESIS tutorial materials repository](https://github.com/genesis-release-r-ccs/genesis_tutorial_materials).

```bash
$ cd ~/GENESIS_Tutorials-2026
$ git clone https://github.com/genesis-release-r-ccs/genesis_tutorial_materials
# or, if already cloned:
$ cd genesis_tutorial_materials && git pull
```

```bash
$ cd tutorial-X.Y
$ ln -s ../../Data/Parameters/toppar_c36_jul21 ./toppar
$ ln -s ../../Programs/genesis-2.0.0/bin ./bin
$ ls
01_setup  02_minimization  03_production_run  04_analysis  bin  toppar
```

## 1. [Step name]

```bash
$ cd 01_setup
$ ls
```

[What are we doing here and why.]

```bash
$ [command]
```

[What files are produced and what each one is.]

## 2. [Step name]

```bash
$ cd ../02_[step]
$ less INP
```

[Explain key parameters before showing the control file.]

```toml
[INPUT]  
key = value # comment

[OUTPUT]  
key = value # comment
```

[Explain any parameters specific to this chapter or method.]

```bash
$ export OMP_NUM_THREADS=N
$ mpirun -np N ../bin/atdyn INP > log
# (~XX minutes on a workstation with N cores)
```

Output files:
- `filename`: [what it is]
- `filename`: [what it is]

## 3. [Step name]

[...]

## 4. Analysis

```bash
$ cd ../04_analysis
```

[What quantities are analyzed and why they are meaningful.]

```bash
$ [analysis command]
```

[Show expected numerical range or plot shape so the reader can
verify their own results.]

> Reference output is available in `04_analysis/reference/` for comparison.

![Caption: what the reader should observe.](path/to/figure.png)

## Summary

[2–3 sentences recapping what was demonstrated. Point to related chapters.]

## References

[^1]: Author A et al., *Journal*, **vol**, pages (year). [DOI](link)

---

*Written by [Name] @ [Institution], [Month Year]*
<!-- Updated by [Name] @ [Institution], [Month Year] -->
{: .notice}

