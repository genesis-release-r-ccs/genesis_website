### 15.6 The enzyme reaction 2: Free-energy calculation {#the-enzyme-reaction-2-free-energy-calculation .wp-block-heading}

Contents

-   [1. Introduction](genesis_tutorial_15.6_2022.md#1_Introduction)
-   [2. Setup of window](genesis_tutorial_15.6_2022.md#2_Setup_of_window)
-   [3. Equilibration 1](genesis_tutorial_15.6_2022.md#3_Equilibration_1)
-   [4. Equilibration 2](genesis_tutorial_15.6_2022.md#4_Equilibration_2)
-   [5. Production run](genesis_tutorial_15.6_2022.md#5_Production_run)
-   [6. The potential of mean-force
    (PMF)](genesis_tutorial_15.6_2022.md#6_The_potential_of_mean-force_PMF)
    -   [6.1. Sort DCD](genesis_tutorial_15.6_2022.md#61_Sort_DCD)
    -   [6.2. Calculate
        distance](genesis_tutorial_15.6_2022.md#62_Calculate_distance)
    -   [6.3 MBAR](genesis_tutorial_15.6_2022.md#63_MBAR)
    -   [6.4 1D-PMF along
        pathCV](genesis_tutorial_15.6_2022.md#64_1D-PMF_along_pathCV)
    -   [6.5. 2D-PMF in
        r1/r2](genesis_tutorial_15.6_2022.md#65_2D-PMF_in_r1r2)
-   [7. Concluding
    remarks](genesis_tutorial_15.6_2022.md#7_Concluding_remarks)
-   [References](genesis_tutorial_15.6_2022.md#References)

#### [1. Introduction]{#1_Introduction} {#introduction .wp-block-heading}

In this section, we demonstrate the replica-exchange umbrella sampling
(REUS) simulation based on a QM/MM potential and calculate the
free-energy profile of an enzyme reaction. REUS \[1\] is one of the
enhanced sampling methods, in which multiple MD simulations (replicas)
are carried out with different restraint potentials, exchanging them
stochastically. In [tutorial
11.1](genesis_tutorial_11.1.md){target="_blank"
rel="noreferrer noopener"}, REUS has been performed with classical
MM-MD. This tutorial is similar to it but differs in:

1.  The reaction coordinate is set to a pre-determined minimum energy
    path (MEP).
2.  QM/MM-MD simulations are carried out using
    [QSimulate-QM](https://qsimulate.com/academic){target="_blank"
    rel="noreferrer noopener"}.

Following [tutorial 15.5](genesis_tutorial_15.5_2022.md), the method is
applied to a proton transfer reaction of dihyroxyacetone phosphate
(DHAP) catalyzed by triosephosphate isomerase (TIM) \[2\]. Although the
MEP and restart files are provided in the tutorial file, it is
recommended to first work on [tutorial 15.5](genesis_tutorial_15.5_2022.md),
where the MEP is obtained by the string method.

<figure class="aligncenter is-resized">
<img src="wp-content/uploads/2022/04/tim_scheme_r_img-1024x369.png"
data-fetchpriority="high" decoding="async" width="768" height="277"
alt="This image has an empty alt attribute; its file name is tim_scheme_r_img-1024x369.png" />
<figcaption>Fig. 1. (a) Schematic illustration of the proton transfer
reaction (H31) from DHAP to Glu165 of TIM, and the atomic distances,
r<sub>1</sub> – r<sub>7</sub>. (b) The variation of r<sub>1</sub> –
r<sub>7</sub> along the MEP.</figcaption>
</figure>

The target proton transfer reaction and selected atomic distances (r~1~
-- r~7~) are schematically shown in Fig. 1 (a). The variation of r~1~ --
r~7~ along the MEP is shown in Fig. 1 (b). The figure indicates that
r~1~ -- r~4~ strongly vary along the path, whereas r~5~ and r~7~ are
rather insensitive. r~6~ is relatively flat compared to r~1~ -- r~4~,
yet it does change 0.1 Å before and after the reaction. Therefore, the
five atomic distances, r~1~ -- r~4~ and r~6~ , well represent the
reaction coordinate. Here, we carry out the off-lattice REUS using these
atomic distances as a collective variable (CV).

#### [2. Setup of window]{#2_Setup_of_window} {#block-2f415db2-4990-4e15-a169-50cc0993c11b .wp-block-heading}

Download the tutorial file
([tutorial22-15.6.tar.gz](assets/tutorial_files/2022_05_tutorial22-15.6.tar.gz){.mtli_attachment
.mtli_zip}, or [github](https://github.com/yagikiyoshi/QMMMtutorial)),
unzip it, and proceed to tutorial-15.6/1.tim. The directory contains
three sub-directories.

``` {#block-3378ce85-dea1-41de-b770-af1cfa0a5f56 .wp-block-preformatted}
$ unzip tutorial22-15.6.zip 
$ cd tutorial-15.6/1.tim
$ ls  
2.equil/ 4.mep/  5.reus/
```

`2.equil` contains psf and pdb files, and a restart file,

``` {#block-2fa50415-f17c-4353-8a0a-a5e6855204b8 .wp-block-preformatted}
$ ls 2.equil
step4.11_qmmm_nvt.rst   step4_nvt_100.pdb   step4_nvt_100.psf
```

and `4.mep` contains the information of the MEP,

``` {#block-e1115ecd-4b72-4fab-8909-77f4ade87e5b .wp-block-preformatted}
$ ls 4.mep/2.analysis
rpath_93.dat
```

You are welcome to overwrite these files with your results in tutorial
15.5 (or add `5.reus` to your `tutorial15.5/1.tim`). Proceed to
`5.reus`, and you will find seven sub-directories.

``` {#block-63450ba0-a27a-4746-8f8f-ef58c6c2d09c .wp-block-preformatted}
$ cd 5.reus
$ ls
0.window/   2.equil2/          3.prod3/           4.pmf/
1.equil1/   2.equil2_analysis/ 3.prod3_analysis/
```

Now, let us generate the windows of US,

``` {#block-33eda96c-23c5-4824-bffb-14a59f9bb24e .wp-block-preformatted}
$ cd 0.window
$ ls
make_window.f90  make_window.sh  win_rr.gpi
```

`make_window.f90` is a fortran program to generate the windows, and
`make_window.sh` is a script to run the program.

``` {#block-79e7f173-db79-4f5f-87f1-5ea2c6a95949 .wp-block-preformatted}
$ cat make_window.sh
#!/bin/bash

gfortran make_window.f90 -o make_window
./make_window -dat ../../4.mep/2.analysis/rpath_93.dat \
              -ds 0.1 -ndim 5 -idx 1,2,3,4,6 > win_rr.log
```

The first line compiles the program. "gfortran" can be replaced with
other fortran compilers. The second line executes the program. The
options are:

-   -dat : the information of the MEP
-   -ds : the interval of window (in Å)
-   -ndim : the number of dimensions
-   -idx : optionally specifies which distances are used. r~1~-r~x~ are
    selected with "-ndim x" by default. "-idx 1,2,3,4,6" specifies
    r~1~-r~4~ and r~6~.

Now run the script,

``` {#block-b614f1c9-6870-41f8-9905-ed9ad33021d5 .wp-block-preformatted}
$ ./make_window.sh
$ ls
make_window      make_window.f90  make_window.sh 
win_rr.dat       win_rr.gpi       win_rr.log
```

The values of r~1~-r~4~ and r~6~ are written for each window in
win_rr.dat,

``` {#block-60d260f5-71fb-46f5-8076-114b76949e5e .wp-block-preformatted}
$ cat win_rr.dat
cat win_rr.dat
   1  2.5300  1.0980  1.7620  1.8690  2.7770
   2  2.4319  1.0968  1.7876  1.8607  2.7627
...
  21  1.0160  1.8780  2.2570  1.6640  2.6780
```

The variation along the MEP can be visualized by gnuplot,

``` {#block-44983bcd-d93c-41aa-9273-d139f076f423 .wp-block-preformatted}
$ gnuplot win_rr.gpi
$ ls
make_window      make_window.sh   win_rr.gpi       win_rr.pdf
make_window.f90  win_rr.dat       win_rr.log
```

<figure class="aligncenter is-resized">
<img src="wp-content/uploads/2022/04/tim_window.png" decoding="async"
width="452" height="293"
alt="This image has an empty alt attribute; its file name is tim_window.png" />
<figcaption>Fig. 2. Variation of r<sub>1</sub> – r<sub>4</sub> and
r<sub>6</sub> along the MEP.</figcaption>
</figure>

Note that 21 windows are set by the program. The number of window
depends on the window interval, "-ds". A large "ds" value reduces the
number of window and the computational cost, yet with a higher risk that
the neighboring windows have less or insufficient overlap of the
probability distribution. The window interval of 0.1 Å with a force
constant of 100 kcal/mol/Å^2^ is often used for chemical reactions.
Nonetheless, it is always good to check the overlap in the initial
equilibration steps, as we shall see in the next sub-section.

#### [3. Equilibration 1]{#3_Equilibration_1} {#block-cae9b95e-84d6-4195-9073-2d59c6be98de .wp-block-heading}

The equilibration MDs are done in two steps (Fig. 3). First, the MD is
carried out sequentially starting from the first window to the last one.
Each window is propagated for 500 fs at the level of DFTB3/MM. Then, in
the next step, MDs of all windows (replicas) are carried out in parallel
for 1 ps at the level of B3LYP-D3/MM.

<figure class="aligncenter is-resized">
<img src="wp-content/uploads/2022/04/tim_reus_equil.png"
decoding="async" width="430" height="237"
alt="This image has an empty alt attribute; its file name is tim_reus_equil.png" />
<figcaption>Fig. 3. The procedure of equilibration.</figcaption>
</figure>

Proceed to `1.equil`,

``` {#block-7fa6ae15-434e-4a5f-9034-63da4d4a6664 .wp-block-preformatted}
$ cd 1.equil
$ ls
geninp1.sh   qsimulate.json   run.sh   template.inp   toppar
```

`geninp1.sh` is a script to generate input files of GENESIS for each
window based on a template file, `template.inp`. The template file is
shown below:

``` {#block-d2ea83b9-7dee-498d-ba07-1d0959736f56 .wp-block-preformatted}
[INPUT]
topfile = toppar/top_all36_prot.rtf, toppar/top_all36_cgenff.rtf
parfile = toppar/par_all36_prot.prm, toppar/par_all36_cgenff.prm
strfile = toppar/toppar_water_ions.str, toppar/toppar_dhap.3.str
psffile = ../../2.equil/step4_nvt_100.psf     # protein structure file
pdbfile = ../../2.equil/step4_nvt_100.pdb     # PDB file
reffile = ../../2.equil/step4_nvt_100.pdb     # reference file
rstfile = ../../2.equil/step4.11_qmmm_nvt.rst # restart file

[OUTPUT]
dcdfile = equil1_ID.dcd
rstfile = equil1_ID.rst

[ENERGY]
forcefield       = CHARMM
electrostatic    = CUTOFF
switchdist       = 16.0      # switch distance
cutoffdist       = 18.0      # cutoff distance
pairlistdist     = 19.5      # pair-list distance
water_model      = NONE
vdw_force_switch = YES

[DYNAMICS]
integrator      = VVER
timestep        = 0.0005     # timestep (ps)
nsteps          = 1000       # number of MD steps
crdout_period   = 500
eneout_period   = 500
rstout_period   = 500
nbupdate_period = 10
stoptr_period   = 10
iseed           = 20190319

[CONSTRAINTS]
rigid_bond      = YES        # constraints all bonds involving hydrogen
shake_tolerance = 1.0D-10
hydrogen_type   = BOTH
fast_water      = YES
noshake_index   = 4 9 10 11  # don't constraint these hydrogen atoms

[ENSEMBLE]
ensemble        = NVT
tpcontrol       = BUSSI     # thermostat
temperature     = 300.0     # temperature (K)

[BOUNDARY]
type          = NOBC
spherical_pot = yes         # spherical potential

[QMMM]
qmtyp          = qsimulate       # QSimulate-QM
qmcnt          = qsimulate.json  # control file of QSimulate-QM
workdir        = equil1_ID
basename       = job
qmsave_period  = 500
qmmaxtrial     = 1
qmatm_select_index = 1
exclude_charge = group

[SELECTION]
group1  = sid:DHA or (sid:TIMA and (rno:95 or rno:165) and \
          not (an:CA  | an:C   | an:O   | an:N | an:HN | an:HA))
group2  = atomno:1900 or atomno:5687 # COM of TIMA/TIMB
group3  = atomno:1442   # NE2 of HSE95
group4  = atomno:1443   # HE2 of HSE95
group5  = atomno:2559   # OE1 of GLU165
group6  = atomno:2560   # OE2 of GLU165
group7  = atomno:7584   # O2  of DHAP
group8  = atomno:7585   # C3  of DHAP
group9  = atomno:7587   # HO3 of DHAP
group10 = atomno:7588   # H31 of DHAP
group11 = atomno:7589   # H32 of DHAP


[RESTRAINTS]
nfunctions    = 6
function1     = POSI
constant1     = 10.0
select_index1 = 2

function2     = DIST    # r1(OE2-H31)
constant2     = 300.0
reference2    = R1
select_index2 = 6  10

function3     = DIST    # r2(C3-H31)
constant3     = 300.0
reference3    = R2
select_index3 = 8  10

function4     = DIST    # r3(OE1-HO3)
constant4     = 300.0
reference4    = R3
select_index4 = 5  9

function5     = DIST    # r4(O2-HE2)
constant5     = 300.0
reference5    = R4
select_index5 = 7   4

function6     = DIST    # r6(NE2-HO3)
constant6     = 300.0
reference6    = R5
select_index6 = 3  9
```

