---
title: "GENESIS Tutorial 16.3 (2022)"
excerpt: ""
last_modified_at: 2025-06-03T00:00:57+09:00
layout: single
toc: true
toc_sticky: true
sidebar:
  nav: sidebar-basic
---

# 16.3 Flexible fitting refinement for *de novo* models

In this tutorial, we learn how to refine the structure (decoy) obtained
from *de novo* modeling such as MAINMAST [^1] and Pathwalking [^2].
The full-atom model is first constructed from Cα model, and then the
flexible fitting refinement is carried out using GENESIS ATDYN. We also
learn how to fix local structural errors such as chirality errors, ring
penetrations, and cis peptide bonds in the full-atom model. We
additionally use external programs like
[VMD](https://www.ks.uiuc.edu/Research/vmd/),
[PULCHRA](https://www.pirx.com/pulchra/), and [MMTSB toolset](http://www.mmtsb.org) 
for the structure modeling. In addition,
parameter files for the EEF1 implicit solvent model are utilized, which
are currently available in the
[CHARMM](https://www.charmm.org) program package. Please download and install
these programs in advance (installation of CHARMM is not required). This
tutorial is related to our recent publication [^3].


## Preparation

First, we make a directory in the `Data/Parameters` directory, to which
the CHARMM C19 parameter and topology files for the EEF1 model
(`solver.inp`, `toph19_eef1.1.inp`, and `param19_eef1.1.inp`) are
compied from the CHARMM program package. Here, we assume that the users
download the CHARMM program in the `~/Downloads` directory. The
parameter files are found in the `support/aspara` directory in CHARMM. 


```
# Copy EEF1 parameter files in the Data directory
$ cd ~/GENESIS_Tutorials-2022/Data/Parameters
$ mkdir eef1
$ cd ./eef1
$ cp ~/Downloads/charmm/support/aspara/solvpar.inp ./
$ cp ~/Downloads/charmm/support/aspara/toph19_eef1.1.inp ./
$ cp ~/Downloads/charmm/support/aspara/param19_eef1.1.inp ./

```

Second, we set environmental variables: `VMDBIN`, `PULCHRABIN`, and
`CONVPDBPL`, which are the path to VMD, PULCHRA, and convpdb.pl of
MMTSB, respectively. Note that the path is depending on where the user
installed these programs. The example commands are as follows:


```
# Set environmental variables for this tutorial
$ export VMDBIN="/home/user/Software/vmd-1.9.3/LINUXAMD64/vmd_LINUXAMD64"
$ export PULCHRABIN="/home/user/Software/pulchra_306/pulchra"
$ export CONVPDBPL="/home/user/Software/mmtsb/perl/convpdb.pl"

```

Now, let's download the tutorial files:
[tutorial22-16.3.tar.gz](/assets/tutorial_files/2022_04_tutorial22-16.3.tar.gz). 


```
# Download the tutorial file
$ cd ~/GENESIS_Tutorials-2022/Works/
$ mv ~/Downloads/tutorial22-16.3.zip ./
$ unzip tutorial22-16.3.zip

# Let's clean up the directory
$ mv tutorial22-16.3.zip TRASH

# Let's take a note
$ echo "tutorial-16.3: Flexible fitting refinement for de novo models" >> README

# Check the contents of Tutorial 16.3
$ cd tutorial-16.3
$ ln -s ../../Programs/genesis-2.0.0/bin ./
$ ls
0_data     2_psfgen    4_fix_errors_1  5_build_c19  Parameters
1_pulchra  3_minimize  4_fix_errors_2  6_saua-ffr   bin

```

We make symbolic links of the CHARMM `toppar` and `eef1` directories in
the `Parameters` directory. 


```
# Make symbolic links to the parameter files
$ cd Parameters
$ ln -s ../../../Data/Parameters/toppar_c36_jul21 ./toppar
$ ln -s ../../../Data/Parameters/eef1 ./eef1

```

Let's look around the `0_data` directory, in which the target density
map and PDB files of decoys are contained. There are 50 decoys
(`model1.pdb` ... `model50.pdb`) in the directory, and they are Cα
model. `input.sit` is the target cryo-EM density map in the SITUS
format. 


```
$ cd ../0_data
$ ls
input.sit    model19.pdb  model29.pdb  model39.pdb  model49.pdb
model1.pdb   model2.pdb   model3.pdb   model4.pdb   model5.pdb
model10.pdb  model20.pdb  model30.pdb  model40.pdb  model50.pdb
model11.pdb  model21.pdb  model31.pdb  model41.pdb  model6.pdb
...

```

##  1. Full-atom modeling 

### 1.1 Generate full-atom models using PULCHRA

We generate the full-atom model from each decoy using PULCHRA. In the
`1_pulchra` directory, there is one script: `Run.csh`, which applies
PULCHRA to all decoys contained in the `../0_data` directory.


```
$ cd ../1_pulchra
$ ls
Run.csh

```

Let's execute `Run.csh`. We obtain 50 PDB files of the full-atom model
(`model1.rebuilt.pdb` ... `model50.rebuilt.pdb`):


```
$ ./Run.csh
$ ls
Run.csh              model24.rebuilt.pdb  model4.rebuilt.pdb
model1.rebuilt.pdb   model25.rebuilt.pdb  model40.rebuilt.pdb
model10.rebuilt.pdb  model26.rebuilt.pdb  model41.rebuilt.pdb
model11.rebuilt.pdb  model27.rebuilt.pdb  model42.rebuilt.pdb
model12.rebuilt.pdb  model28.rebuilt.pdb  model43.rebuilt.pdb
...

```

### 1.2 Energy minimization using GENESIS

We first carry out energy minimization for the obtained models to remove
steric clashes in the initial structure. In order to perform the
minimization with GENESIS, we make input PDB and PSF files for all
decoys. Let's move to the `2_psfgen` directory. We can find `Run.csh`
and `build.tcl`. Here, `build.tcl` is a TCL script of VMD/PSFGEN. We use
the CHARMM C36m force field parameters. 


```
$ cd ../2_psfgen
$ ls
Run.csh  build.tcl  tmp

```

Let's execute `Run.csh`. You can obtain the PSF file (`model.psf`) and
PDB files (`model1.pdb` ... `model50.pdb`) of all decoy. Here, `log1`
... `log50` are log files of VMD/PSFGEN.


```
$ ./Run.csh
$ ls
Run.csh    log21  log35  log49  model16.pdb  model3.pdb   model43.pdb
build.tcl  log22  log36  log5   model17.pdb  model30.pdb  model44.pdb
log1       log23  log37  log50  model18.pdb  model31.pdb  model45.pdb
log10      log24  log38  log6   model19.pdb  model32.pdb  model46.pdb
log11      log25  log39  log7   model2.pdb   model33.pdb  model47.pdb
...

```

We carry out the energy minimization for all decoys. Let's move to the
`3_minimize` directory. You can find three scripts: `Make.csh`,
`Submit.csh`, and `Check.csh`, and `template` directory. `Make.csh` is a
script that creates control files of GENESIS automatically for all
decoys


```
# Move to the directory for minimization
$ cd ../3_minimize
$ ls
Check.csh  Make.csh  Submit.csh  template

# Check the script file
$ less Make.csh

```

As seen in `Make.csh`, the control file (`INP`) and batch job script
file (`job.sh`) in the `template` directory are replicated to generate
50 control files (`INP1` ... `INP50`) and script files (`job1.sh` .. `job50.sh`). Before executing `Make.csh`, let's check the files
contained in `template`. 


```
# Check the template batch script file
$ less ./template/job.sh
$ less ./template/INP

```

In the template control file (`INP`) and batch script file (`job.sh`),
you can find "MODELID", which is replaced with the decoy index by the
`sed` command in `Make.csh`. Note that the users must edit the template
batch script file (`job.sh`) by yourself according to your computer
system. 


```
...

mpirun -np 8 -x OMP_NUM_THREADS=3 ../bin/atdyn INPMODELID > logMODELID

...

```

Important control parameters are highlighted below. If
`check_structure` in the `MINIMIZE` section is turned on, the
chirality errors and ring penetrations are checked in the
energy-minimized structure. If suspicious chiral centers or ring
residues are detected, the warning message will be shown in the GENESIS
log file.


```
[MINIMIZE] 
method          = SD
nsteps          = 2000
eneout_period   =  100
rstout_period   = 2000
check_structure = YES

```

Let's execute `Make.csh` to create the control files and batch script
files.


```
# Replicate the files for all decoys 
$ ./Make.csh
$ ls
Check.csh  INP20  INP32  INP44  Submit.csh  job20.sh  job32.sh  job44.sh
INP1       INP21  INP33  INP45  job1.sh     job21.sh  job33.sh  job45.sh
INP10      INP22  INP34  INP46  job10.sh    job22.sh  job34.sh  job46.sh
INP11      INP23  INP35  INP47  job11.sh    job23.sh  job35.sh  job47.sh
...

```

Then, let's submit all jobs using `Submit.csh`, which executes all jobs
one by one using the `qsub` command.


```
# Check the script
$ less ./Submit.csh

# Submit all jobs
$ ./Submit.csh

```

You obtain `model1.pdb` ... `mode50.pdb`, `model1.rst` ...
`model50.rst`, and `log1` ... `log50`, which are the energy minimized
structures, restart files, and GENESIS log files, respectively.

To check the local structural errors such as chirality errors, ring
penetrations, and cis peptide bonds, we carry out `Check.csh`. As seen
in `Check.csh`, log messages are checked to investigate whether
chirality errors and ring penetrations are detected or not. For the cis
peptide bonds, VMD [cispeptide plugin](http://www.ks.uiuc.edu/Research/vmd/plugins/cispeptide/) is carried out. 


```
# Check the script
$ less ./Check.csh

# Run the script
$ ./Check.csh

```

As displayed in the terminal window, there are many errors in the
decoys. The log of VMD cispeptide plugin is saved in `cispeptide1.log`
... `cispeptide50.log`. Let's select one decoy containing the errors,
and visualize the PDB coordinates using a molecular viewer. 

### 1.3 Energy minimization for fixing ring penetrations and chirality errors 

To fix the errors, specific treatment should be applied to the
corresponding part. Here, new algorithms developed by T. Mori et al are
used [^3] (see also [Tutorial Appendix 6](/tutorials/genesis_tutorial_appendix_6_2022/)).
Hereafter, we will execute similar scripts (`Make.csh`, `Submit.csh`, and `Check.csh`) in each directory as in the previous sections. So, we
do not explain the details of the script, but explain the important part
in the control file.

Let's move to the `4_fix_errors_1` directory, in which the chirality
errors and ring penetrations are fixed with the energy minimization
protocol.


```
# Change directory to fix the errors Step1
$ cd ../4_fix_errors_1
$ ls
Check.csh  Make.csh  Submit.csh  template

```

In the control file, `fix_ring_error` and `fix_chirality_error` in the
`[MINIMIZE]` section are turned on:


```
[MINIMIZE] 
method              = SD
nsteps              = 2000
eneout_period       =  100
rstout_period       = 2000
check_structure     = YES
fix_ring_error      = YES
fix_chirality_error = YES

```

Let's execute the three scripts one by one. 


```
$ ./Make.csh
$ ./Submit.csh
$ ./Check.csh

```

In the energy-minimized structures, there are no chirality errors and no
ring penetrations, while there are still many cis peptide bonds.

### 1.4 Restraint MD for fixing *cis* peptide bonds

For fixing the cis peptide bonds, we carry out restraint MD simulation.
The dihedral angle restraint (ω = 180°) is applied to all peptide bonds
except for those involving proline. In this step, `Make.csh`
creates `selection.dat` and `restraint.dat`, which are corresponding to
the `[SELECTION]` and `[RESTRAINTS]` sections, respectively. The
information in these two files are appended to the template control
file.


```
# Change directory to fix the errors Step2
$ cd ../4_fix_errors_2
$ ls
Check.csh  Make.csh  Submit.csh  template

# Create control files and batch script files
$ ./Make.csh
$ ls *.dat
restraint.dat  selection.dat

# Run restraint MD and check structures
$ ./Submit.csh
$ ./Check.csh

```

After the calculations, we can see that the number of cis peptide bonds
are significantly reduced. There are still few cis peptide bonds, which
mainly involve proline (see `cispeptide1.log` ... `cispeptide50.log`). 

##  2. Flexible fitting refinement using the united-atom model

Finally, we carry out flexible fitting refinement (FFR), in which the
simulated-annealing (SA) MD with the united-atom model (UA) in a
combination of the EEF1 implicit solvent model is carried out.

### 2.1 Build the input PDB and PSF files

We convert the all-atom model (CHARMM C36) to the united-atom model
(CHARMM C19) for all decoys. Let's move to `5_build_c19`. The protocol
is almost same with that in `2_psfgen`. Here, we specify
`toph19_eef1.1.inp` in VMD/PSFGEN.


```
# Change directory to build the united-atom model
$ cd ../5_build_c19
$ ls
Run.csh  build.tcl  tmp

# Run the script for build
$ ./Run.csh

```

### 2.2 Run the flexible fitting refinement (SAUA-FFR)

Let's move to `6_saua-ffr`. As in our recent paper [^3], the SAUA-FFR
is carried out iteratively. The restart files of the calculations are
taken over from Cycle1 to Cycle5.


```
# Change directory to perform SAUA-FFR
$ cd ../6_saua-ffr
$ ls
Cycle1  Cycle2  Cycle3  Cycle4  Cycle5

```

Let's move to `Cycle1`, and check the template control file.


```
# Perform SAUA-FFR with 5 cycles
$ cd Cycle1
$ ls
Make.csh  Submit.csh  template

$ less ./template/INP

```

In this calculation, 100 ps MD simulation is carried out, where the
temperature is decreased from 600 to 300 K. The Langevin thermostat is
employed for the temperature control, and the SHAKE algorithm is used
for bond constraint involving hydrogen. The force constant of the EM
biasing potential is 10,000 kcal/mol.

Let's execute the script one by one. 


```
# Perform SAUA-FFR with 5 cycles
$ ./Make.csh
$ ./Submit.csh

$ cd ../Cycle2
$ ./Make.csh
$ ./Submit.csh

$ cd ../Cycle3
$ ./Make.csh
$ ./Submit.csh

$ cd ../Cycle4
$ ./Make.csh
$ ./Submit.csh

$ cd ../Cycle5
$ ./Make.csh
$ ./Submit.csh

```

You can obtain the refined models. After getting the results, it is
important to select the best model. In our recent paper [^3], we
demonstrated a new protocol for the best model selection, where the
model is selected based on c.c., RWplus score, and MolProbity score from
multiple flexible fitting runs using different force constants. 


##  Acknowledgement 

The original Cα-model PDB and density map files used in this tutorial
were provided from Kihara laboratory at Purdue University, which are
also distributed as a part of  the publication data of Reference 3 (for details, see [here](https://github.com/RikenSugitaLab/flexible-fitting-refinement-for-de-novo-modeling)).

---

*Written by Takaharu Mori@RIKEN Theoretical molecular science laboratory\ April 10, 2022*
{: .notice}


##  References

[^1]: G. Terashi and D. Kihara, *Nat. Commun.*, **9**, 1618 (2018).
[^2]: M. Y. Chen *et al.*, *J. Struct. Biol.*, **196**, 289-298 (2016).
[^3]: Mori et al., *J. Chem. Inf. Model.*, **61**, 3516--3528 (2021)..


