---
title: "GENESIS Tutorial Appendix 1 (2022)"
gpos: 999.001
excerpt: ""
last_modified_at: 2025-06-03T00:00:57+09:00
layout: single
toc: true
toc_sticky: true
sidebar:
  nav: sidebar-basic
---

# How to analyze multiple DCD files in the trajectory analysis tools?

In this page, we explain how to handle single and multiple DCD files in
the GENESIS trajectory analysis tool sets. We assume that we have five
dcd files: `run1.dcd`, `run2.dcd`, `run3.dcd`, `run4.dcd`, and
`run5.dcd`, each of which contains 1000, 1000, 5000, 2500, and 2500
snapshots, respectively. For example, `run1.dcd` was obtained from the
following the MD simulation:


```toml
[OUTPUT]
dcdfile = run1.dcd         # DCD file

[DYNAMICS]
integrator       = LEAP    # [LEAP,VVER]
nsteps           = 50000   # number of MD steps
timestep         = 0.002   # timestep (ps)
eneout_period    = 50      # energy output period
crdout_period    = 50      # coordinates output period
nbupdate_period  = 10      # nonbond update period
```

We show some examples of the `[TRAJECTORY]` section in the control file
of the analysis tools.

### Case 1: Analyze run1.dcd

In this case, we set `md_step1 `and `mdout_period1 `in the
`[TRAJECTORY]` section to be `nsteps` and `crdout_period` in the
`[DYNAMICS]` section of the MD control file. Because
`mdout_perid1 = ana_period1`, all 1,000 snapshots are analyzed. If we
specify `ana_period1 = 100`, snapshots are analyzed every 2 steps,
namely, 500 snapshots are analyzed.


```toml
[TRAJECTORY]
trjfile1      = run1.dcd
md_step1      = 50000
mdout_period1 = 50        
ana_period1   = 50
repeat1       = 1
trj_format    = DCD
trj_type      = COOR+BOX     
```

### Case 2: Analyze run1.dcd (same with Case 1)

This is same with Case 1. All 1,000 snapshots in the DCD file are
analyzed. If we specify `ana_period1 = 2`, snapshots are analyzed every
2 steps, namely, 500 snapshots are analyzed.


```toml
[TRAJECTORY]
trjfile1      = run1.dcd
md_step1      = 1000
mdout_period1 = 1        
ana_period1   = 1
repeat1       = 1
trj_format    = DCD
trj_type      = COOR+BOX         
```

### Case 3: Analyze run1.dcd and run2.dcd sequentially

In this control file, we analyze `run1.dcd` and `run2.dcd` sequentially:


```toml
[TRAJECTORY]
trjfile1      = run1.dcd
trjfile2      = run2.dcd
md_step1      = 1000
mdout_period1 = 1        
ana_period1   = 1      
md_step2      = 1000
mdout_period2 = 1        
ana_period2   = 1      
repeat1       = 1
repeat2       = 1
trj_format    = DCD 
trj_type      = COOR+BOX 
```

Because `run1.dcd` and `run2.dcd` contain same number of snapshots, we
can set `repeat1 = 2`, and omit `md_srtep2`, `mdout_period2`,
`ana_period2`, and `repeat2`:


```toml
[TRAJECTORY]
trjfile1      = run1.dcd
trjfile2      = run2.dcd
md_step1      = 1000
mdout_period1 = 1        
ana_period1   = 1      
repeat1       = 2           # for trjfile1 and trjfile2
trj_format    = DCD
trj_type      = COOR+BOX 
```

### Case 4: Analyze run1.dcd to run5.dcd sequentially

If we analyze multiple DCD files with different number of snapshots, we
should combine the above rules. Please, note that the index of the
parameters (`md_step`, `mdout_period`, `ana_period`, and `repeat`)
should be sequential from **1** to **n** (Do not skip any number).


```toml
[TRAJECTORY]
trjfile1      = run1.dcd
trjfile2      = run2.dcd
trjfile3      = run3.dcd
trjfile4      = run4.dcd
trjfile5      = run5.dcd
md_step1      = 1000
mdout_period1 = 1        
ana_period1   = 1      
md_step2      = 5000
mdout_period2 = 1        
ana_period2   = 1      
md_step3      = 2500
mdout_period3 = 1        
ana_period3   = 1      
repeat1       = 2          # for trjfile1 and trjfile2
repeat2       = 1          # for trjfile3
repeat3       = 2          # for trjfile4 and trjfile5
trj_format    = DCD
trj_type      = COOR+BOX
```

------------------------------------------------------------------------

*Written by Takaharu Mori@RIKEN Theoretical molecular science
laboratory\
March 27, 2022*
{: .notice}

