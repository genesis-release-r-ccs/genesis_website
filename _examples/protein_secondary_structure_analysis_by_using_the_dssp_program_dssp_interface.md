---
title: "GENESIS Input Example: Protein secondary structure analysis by using the DSSP program (`dssp_interface`)"
excerpt: ""
last_modified_at: 2025-06-11T22:49:38+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---


Because this is the interface program, the users should install [the DSSP
program](http://www.cmbi.ru.nl/dssp.html) in advance. The path to the binary
program is specified in `[OPTION]`. In this program, each snapshot in the
trajectory is written into `temporary_pdb`, and DSSP is executed for this PDB
file one by one, and all results are output into `outfile`.


```toml
[INPUT]
psffile       = ../BPTI_ionize.psf  # protein structure file
reffile       = ../BPTI_ionize.pdb  # PDB file

[OUTPUT]
outfile       = output.out          # output file

[TRAJECTORY]
trjfile1      = ../BPTI_run.dcd     # trajectory file
md_step1      = 500000              # number of MD steps
mdout_period1 = 500                 # MD output period
ana_period1   = 500                 # analysis period
repeat1       = 1
trj_format    = DCD                 # (PDB/DCD)
trj_type      = COOR+BOX            # (COOR/COOR+BOX)
trj_natom     = 0                   # (0:uses reference PDB atom count)

[SELECTION]
group1        = segid:BPTI          # select protein

[OPTION]
check_only    = NO                  # (YES/NO)
analysis_atom = 1                   # protein atom group
dssp_exec     = /home/user/bin/dssp # dssp binary path
temporary_pdb = temporary.pdb       # temporary pdb file name for DSSP
rename_res1   = HSE HIS
rename_res2   = HSD HIS
```

To analyze the secondary structure properties such as the helicity, the
user should write a script to analyze the output file. The following is
an example of csh script to calculate the helicity, where the file name
and number of residues are specified as in red color:

```csh
#!/bin/csh

set filename = output.out
set numres   = `grep "TOTAL NUMBER OF RESIDUES" ${filename} | head -1 | awk '{print $1}'`
set nstr     = `grep SNAPSHOT ${filename} | wc -l`
echo " total number of snapshots : ${nstr}"
echo " secondary structure formation propensity of each residue"
set i = 1
while ( ${i} <= ${numres})
 set istr = `awk '{if ($4 == "H") printf("%6s \n",$1)}' ${filename} | grep " ${i} " | wc -l`
 set ss   = `echo "scale=5; ${istr} / ${nstr}" | bc -l | sed -e 's@^\.@0\.@g'`
 echo " ${i} ${ss}"
 @ i += 1
end
```
