---
title: "GENESIS Tutorial 15.5 (2022)"
excerpt: ""
last_modified_at: 2025-06-03T00:00:57+09:00
layout: single
toc: true
toc_sticky: true
sidebar:
  nav: sidebar-basic
---

# The enzyme reaction 1: Reaction path search 

## 1. Introduction 

In this section, QM/MM calculation is carried out using the string
method [^1] to search for the minimum energy path (MEP) and
[QSimulate-QM](https://qsimulate.com/academic) for the QM program. In the string method, the
reaction path is discretely represented by a number of images, which
propagates along the gradient component perpendicular to the path
tangent. Starting from an initial path connecting the reactant and
product, the images develop according to the gradient component, and
converge to the MEP after iterations. In GENESIS, each image is treated
with different MPI processes. Furthermore, QSimulate-QM features
excellent scaling with respect to the number of computing nodes. The
combination of the string method and QSimulate-QM is highly
parallelizable, and thus suitable for supercomputers. For further
details, see Ref. [^2].

Here, we simulate a proton transfer reaction of dihyroxyacetone
phosphate (DHAP) catalyzed by an enzyme, triosephosphate isomerase
(TIM), which is one of the key steps in the glycolytic pathway. The
reaction has been extensively studied since the early 2000 \[3-8\], and
is a well known benchmark system \[2,9\]. We calculate the MEP of the
following reaction:

![](/assets/images/2022_04_tim_scheme.png){: width="800" .center-image }
<figcaption style="font-size: 1.0em;">
Fig. 1. Schematic illustration of the proton transfer
reaction from DHAP to Glu165 of TIM. r<sub>1</sub> – r<sub>7</sub>
indicate relevant atomic distances.  
</figcaption>

One of the proton (H31) of DHAP is transferred to Glu165 of TIM. Note
that the proton transfer accompanies a charge transfer of the electron
from Glu165 to O2 of DHAP (red in Fig. 1), where His95 donates a
hydrogen bond. Therefore, the reaction coordinate involves not only r<sub>1</sub>
/ r<sub>2</sub> but also several intermolecular degrees of freedom (e.g., r<sub>3</sub>, r<sub>4</sub>, r<sub>6</sub>). In fact, HE2 of His95 is transferred to O2 in a later
stage, though the process is beyond the scope of this tutorial.

## 2. System preparation 

Download the tutorial file
([tutorial22-15.5b.tar.gz](fundamental/2024_07_tutorial22-15.5b.tar.gz "tutorial-15.5.zip") or [github](https://github.com/yagikiyoshi/QMMMtutorial "github")), unzip
it, and proceed to tutorial-15.5/1.tim. This directory contains five
sub-directories.

```bash
$ unzip tutorial22-15.5.zip 
$ cd tutorial-15.5/1.tim
$ ls  
0.build/ 1.pre_equil/ 2.equil/ 3.min/ 4.mep/
```

`0.build`, `1.pre_equil`, and `2.equil` are the setup and equilibration
of the system prior to QM/MM calculations. Since relevant files (pdb, psf, and rst) are retained, you can skip these steps and go directly to
QM/MM calculations in `3.min`. Nonetheless, let us give a brief
description of these steps.

The protein structure was based on a X-ray crystal structure,
[7TIM](https://www.rcsb.org/structure/7TIM), obtained from the protein data
bank.[CHARMM-GUI](https://www.charmm-gui.org/ "CHARMM-GUI") was used to setup the system, namely, add
hydrogen atoms, set the protonation states, add water molecules and
ions, and so on. `0.build` contains the resulting pdb and psf files.

``` bash
$ ls 0.build
step2_solvator.pdb  step2_solvator.psf
```

The PDB file is visualized in Fig. 2. Note that TIM is a homo-dimer
(segment name TIMA and TIMB), and that the reaction site of both
proteins contains the ligand.

![](/assets/images/2022_04_tim_overall.png){: width="400" .center-image }
<figcaption style="font-size: 1.0em;text-align: center;">
Fig. 2. The overall structure of the system.  
</figcaption>

In `1.pre_equil`, the system is equilibrated in the following four MD
steps.

<figure class="wp-block-table">
<table>
<tbody>
<tr class="odd">
<td>Step</td>
<td>Ensemble</td>
<td>Integrator</td>
<td>Time / ps</td>
<td>dt / fs</td>
</tr>
<tr class="even">
<td>3.1</td>
<td>Min</td>
<td>–</td>
<td>1,000 step</td>
<td>–</td>
</tr>
<tr class="odd">
<td>3.2</td>
<td>NVT(0.1 -&gt; 300 K)</td>
<td>LEAP, Langevin</td>
<td>100</td>
<td>2</td>
</tr>
<tr class="even">
<td>3.3</td>
<td>NPT (1 atm, 300K)</td>
<td>VVER, Bussi</td>
<td>100</td>
<td>2</td>
</tr>
<tr class="odd">
<td>3.4</td>
<td>NVT (300 K)</td>
<td>VVER, Bussi</td>
<td>100</td>
<td>2</td>
</tr>
</tbody>
</table>
</figure>

The positional restraints are added to the backbone (with k = 10 kcal/mol/Å<sup>2</sup>) and the sidechain, ligand, and crystal water molecules
(with k = 2 kcal/mol/Å<sup>2</sup>).

```bash
$ cd 1.pre_equil
$ ls
crd_convert.inp       step3.1_minimization.inp 
step3.2_heating.inp   step3.3_npt.inp     step3.4_nvt.inp
run.sh*               toppar
```

`step3.x_*.inp` are the input files of GENESIS and `run.sh` is a script
to run the job. `crd_convert.inp` wraps the final trajectory,
`step3.4_nvt.dcd`, in the simulation box. `toppar` contains the force
field parameters. Note that the parameters of DHAP,
`toppar/toppar_dhap.3.str`, were generated with the force field toolkit
(ffTK) utility [^10] of VMD.

In `2.equil`, the final snapshot structure of step3.4 is first cut out
to a non-periodic system using qmmm_generator. 15 Å around the TIM dimer
is extracted.

![](/assets/images/2022_04_tim_cut.png){: width="800" .center-image }
<figcaption style="font-size: 1.0em;">
Fig. 3. Cut 15 Å around the TIM dimer from a periodic system
(left) to create a non-periodic system (right)
</figcaption>

Then, the system is equilibrated by NVT-MD at 300K. The positional
restraints of the backbone (bb), sidechain (sc) and crystal water (cw)
are gradually reduced in 8 steps, while retaining those of DHAP. In the
last three steps (9 -- 11), DHAP is relaxed with QM/MM-MD using DFTB3
for the QM calculations.

<figure class="wp-block-table">
<table>
<tbody>
<tr class="odd">
<td>Step</td>
<td>Potential</td>
<td>Time / ps</td>
<td>k [bb]</td>
<td>k [sc/cw]</td>
<td>k [dhap]</td>
</tr>
<tr class="even">
<td>4.1</td>
<td>MM</td>
<td>100</td>
<td>10</td>
<td>2</td>
<td>2</td>
</tr>
<tr class="odd">
<td>4.2</td>
<td>MM</td>
<td>100</td>
<td>5</td>
<td>1</td>
<td>2</td>
</tr>
<tr class="even">
<td>4.3</td>
<td>MM</td>
<td>100</td>
<td>2.5</td>
<td>0.5</td>
<td>2</td>
</tr>
<tr class="odd">
<td>4.4</td>
<td>MM</td>
<td>100</td>
<td>1</td>
<td>0</td>
<td>2</td>
</tr>
<tr class="even">
<td>4.5</td>
<td>MM</td>
<td>100</td>
<td>0.5</td>
<td>0</td>
<td>2</td>
</tr>
<tr class="odd">
<td>4.6</td>
<td>MM</td>
<td>100</td>
<td>0.1</td>
<td>0</td>
<td>2</td>
</tr>
<tr class="even">
<td>4.7</td>
<td>MM</td>
<td>1000</td>
<td>0</td>
<td>0</td>
<td>2</td>
</tr>
<tr class="odd">
<td>4.8</td>
<td>MM</td>
<td>1000</td>
<td>0</td>
<td>0</td>
<td>2</td>
</tr>
<tr class="even">
<td>4.9</td>
<td>DFTB3/MM</td>
<td>20</td>
<td>0</td>
<td>0</td>
<td>1</td>
</tr>
<tr class="odd">
<td>4.10</td>
<td>DFTB3/MM</td>
<td>20</td>
<td>0</td>
<td>0</td>
<td>0.5</td>
</tr>
<tr class="even">
<td>4.11</td>
<td>DFTB3/MM</td>
<td>100</td>
<td>0</td>
<td>0</td>
<td>0</td>
</tr>
</tbody>
</table>
</figure>

The files in `2.equil` are as follows:

``` bash
$ cd 2.equil
$ ls
qmmm_generator.inp          qsimulate.json
run.sh*                     step4.1_nvt.inp
step4.2_nvt.inp             step4.3_nvt.inp
step4.4_nvt.inp             step4.5_nvt.inp
step4.6_nvt.inp             step4.7_nvt.inp
step4.8_nvt.inp             step4.9_qmmm_nvt.inp
step4.10_qmmm_nvt.inp       step4.11_qmmm_nvt.inp
step4.11_qmmm_nvt.rst       step4_nvt_100.crd
step4_nvt_100.pdb           step4_nvt_100.psf   
toppar
```

`qmmm_generator.inp` is an input file of qmmm_generator, `step4.x_*.inp`
are the input files of GENESIS, `qsimulate.json` is a control file of
QSimulate-QM (details are given below), and `run.sh` is a script to run
all jobs. As a result, we obtain the files shown in red,
`step4_nvt_100.* `(by qmmm_generator) and `step4.11_qmmm_nvt.rst` (by MD). These files are used in the subsequent QM/MM calculations

## 3. Locate the reactant and product 

We first obtain the reactant and product. Proceed to 3.min,

```bash
$ cd 3.min
$ ls
min.vmd          qmmm_min2a.inp   qsimulate.json   run.sh*
qmmm_min1.inp    qmmm_min2b.inp   rst_convert.inp  toppar
```

`qmmm_min1.inp` is an input file to find the reactant,

```bash
[INPUT]
topfile = toppar/top_all36_prot.rtf, toppar/top_all36_cgenff.rtf
parfile = toppar/par_all36_prot.prm, toppar/par_all36_cgenff.prm
strfile = toppar/toppar_water_ions.str, toppar/toppar_dhap.3.str
psffile = ../2.equil/step4_nvt_100.psf     # protein structure file
pdbfile = ../2.equil/step4_nvt_100.pdb     # PDB file
reffile = ../2.equil/step4_nvt_100.pdb     # reference file 
rstfile = ../2.equil/step4.11_qmmm_nvt.rst # restart file

[OUTPUT]
rstfile = qmmm_min1.rst
dcdfile = qmmm_min1.dcd

[ENERGY]
forcefield       = CHARMM
electrostatic    = CUTOFF
switchdist       = 16.0          # switch distance
cutoffdist       = 18.0          # cutoff distance
pairlistdist     = 19.5          # pair-list distance
vdw_force_switch = YES

[MINIMIZE]
method              = LBFGS      # BFGS optimizer
nsteps              = 500
eneout_period       = 1
crdout_period       = 1
rstout_period       = 1
nbupdate_period     = 1
fixatm_select_index = 2
macro               = yes   # use macro/micro-iteration scheme
nsteps_micro        = 20

[BOUNDARY]
type            = NOBC

[QMMM]
qmtyp              = qsimulate        # QSimulate-QM
qmcnt              = qsimulate.json   # control file of QSimulate-QM
workdir            = qmmm_min1
basename           = job
qmmaxtrial         = 1
qmsave_period      = 10
qmatm_select_index = 1
exclude_charge     = group

[SELECTION]
group1 = sid:DHA or (sid:TIMA and (rno:95 or rno:165) \          and not (an:CA |an:C |an:O |an:N |an:HN |an:HA))  # QM region
group2 = not (sid:DHA or sid:DHA around_res:6.0) # fixed atoms during minimization
```

