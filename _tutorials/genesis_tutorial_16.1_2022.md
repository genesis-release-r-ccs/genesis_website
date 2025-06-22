---
title: "GENESIS Tutorial 16.1 (2022)"
gpos: 016.001
excerpt: ""
last_modified_at: 2025-06-03T00:00:57+09:00
layout: single
toc: true
toc_sticky: true
sidebar:
  nav: sidebar-basic
---

# Cryo-EM flexible fitting for TPC2 channel 

Cryo-electron microscopy (cryo-EM) is a powerful tool for determining
the three-dimensional structure of biomolecules with near-atomic
resolution. Flexible fitting is widely used to model atomic structures
from experimentally obtained density maps.[^1] One of the most
commonly used methods is MD-based flexible fitting,[^2] which is
available in ATDYN and SPDYN in the GENESIS program package.[^3]<sup>,</sup> [^4] 
Here we describe the basic scheme for MD-based flexible fitting using
the GB/SA implicit solvent model (see also [Tutorial 8.1](/tutorials/genesis_tutorial_8.1_2022/)), using
cryo-EM density maps of the TPC2 channel. Fitting from a closed-form
structure to an open-form density map will be demonstrated.

![](/assets/images/2022_03_Figure16.1.1.png){: width="800" .align-center}

## 0. Preparation 

### 0.1. Experimental data 

First, let's download the PDB file of the closed form of the TPC2
channel ([PDBID: 6NQ1](https://www.rcsb.org/structure/6nq1)), and put it to the `~/GENESIS_Tutorials-2022/Data/PDB`
directory. We will use this structure as the initial structure of the
flexible fitting. In the following example commands, we use the `wget`
command to download the file.

![](/assets/images/2022_03_Figure16.1.2.png){: width="800" .align-center}

```bash
# Download the PDB file of the closed form
$ cd ~/GENESIS_Tutorials-2022/Data
$ wget https://files.rcsb.org/download/6NQ1.pdb
$ mv 6NQ1.pdb ./PDB
```

Second, we download the experimental density map of the open form of the
TPC2 channel. Let's access to the website of
[EMDataResource](https://www.emdataresource.org/), in which we can find experimental data deposited by
cryo-EM experimentalists. The EM density map data of the open form of
the TPC2 channel was determined by J. She et al (eLife, 2019), and it is
registered as the `EMD-0477` in this data base. Let's search for `0477`
in the search box on the top of the page.
![](/assets/images/2022_03_Figure16.1.3.png){: width="800" .align-center}

In the page for `EMD-0477`, we can see various information such as
summary of the sample preparation, experimental condition, and so on.
The resolution of the density map is 3.7 &#8491;. To download the density map,
click the `Download` Tab, and then `emd_0477.map.gp`. Alternatively, in
the below example commands, we use the `wget` command for the download.
Put the density map file to the `~/GENESIS_Tutorials-2022/Data/EMmap`
directory. We will use this density map as the target map of the
flexible fitting.
![](/assets/images/2022_03_Figure16.1.4.png){: width="800" .align-center}

```bash
# Download the EM density map file of the open form
$ wget https://ftp.wwpdb.org/pub/emdb/structures/EMD-0477/map/emd_0477.map.gz
$ gzip -d emd_0477.map.gz

$ mkdir EMmap
$ mv emd_0477.map ./EMmap
```

Let's view the PDB structure and cryo-EM density map using VMD.  
After loading the files in VMD, we have to move the seek-bar of the `Isovalue`
in the Graphical Representation window to take a look at the density map.  
(Select `[Graphics]` tab in the VMD Main window \> Select `[Representations]`   
(1) Select `[Isosurface]` in the Graphical Representation window   
(2) Select `[Solid Surface]` in the Draw option  
(3) Move `Isovalue` seek-bar).   
We can see that the structure is already close to the density map, but not perfectly fitted to the map.  
In the VMD Main window, we can see that the total number of heavy atoms
( \\(N_\\mathrm{atom}\\) ) in the system is about 10,000. Please keep this number in
mind, which will be used later.

```bash
# Check the PDB structure and EM density map
$ vmd -ccp4 ./EMmap/emd_0477.map -pdb ./PDB/6NQ1.pdb
```

![](/assets/images/2022_03_Figure16.1.5.png){: width="800" .align-center}

### 0.2. Tutorial file

All the files required for this tutorial are hosted in the 
[GENESIS tutorials repository on GitHub](https://github.com/genesis-release-r-ccs/genesis_tutorial_materials).
If you haven't downloaded the files yet, open your terminal and 
run the follwing command (see more in [Tutorial 1.1](/tutorials/genesis_tutorial_1.1_2022/))

In this tutorial, we use the CHARMM36m force field
parameters, and we make a symbolic link to the CHARMM toppar directory
(for details, see [Tutorial 2.2](/tutorials/genesis_tutorial_2.2_2022/)).

```bash
# Download the tutorial file
$ cd ~/GENESIS_Tutorials-2022

# if not yet
$ git clone https://github.com/genesis-release-r-ccs/genesis_tutorial_materials

# Check the contents of Tutorial 16.1
$ cd genesis_tutorial_materials/tutorial-16.1
$ ln -s ../../Data/Parameters/toppar_c36_jul21 ./toppar
$ ln -s ../../Programs/genesis-2.0.0/bin ./bin
$ ls
1_build  2_minimize  3_fitting  4_analysis  bin  toppar
```

## 1. Build the input files for GENESIS 

First, input PDB and PSF files are generated using VMD (see [Tutorial 2.3](/tutorials/genesis_tutorial_2.3_2022/) or [Tutorial 5.1](/tutorials/genesis_tutorial_5.1_2022/) for details). For convenience, we also create a symbolic link to the EM
density map in the same directory.

```bash
# Change directory to setup
$ cd 1_build
$ ls
build.tcl

# Make a symbolic link to the PDB and density map files
$ ln -s ../../../Data/PDB/6NQ1.pdb ./
$ ln -s ../../../Data/EMmap/emd_0477.map ./

# Generate PDB and PSF files
$ vmd -dispdev text -e build.tcl > build.log

$ ls
6NQ1.pdb   build.tcl     initial.pdb  proa.pdb
build.log  emd_0477.map  initial.psf  prob.pdb
```

## 2. Energy minimization

Before running the flexible fitting, energy minimization is carried out
to remove clashes between atoms in the initial structure. The control
file is already contained in the directory. Here, we simply perform the
energy minimization without the EM biasing. We execute `ATDYN` for
`INP`. After the calculation, `min.dcd` and `min.rst` are obtained.

```bash
# Perform energy minimization using 16 CPU cores
$ cd ../2_minimize
$ export OMP_NUM_THREADS=4
$ mpirun -np 4 ../bin/atdyn INP > log

$ ls
INP  log  min.dcd  min.rst
```

## 3. Flexible fitting

Then, we carry out the flexible fitting. The control file is already
included in the directory.

```bash
# Change directory to run the simulation
$ cd ../3_fitting
$ ls
INP

# Take a look at the control file
$ less INP
```

The following shows the most important parts in the control file. You
can understand that the flexible fitting is a kind of "restrained MD
simulation", and most parameters are common with the conventional MD
simulations. We apply the EM biasing potential on the protein heavy
atoms, where the force constant of the bias is set to \\(3N_\\mathrm{atom} =  30,000~\\mathrm{kcal/mol}\\) [^5]. 
`emfit_sigma` is a resolution parameter, which is
usually set to the half of the resolution of the target density map
(sigma = 3.7 &#8491;/ 2 = 1.85 &#8491;).

```toml
[INPUT]
topfile = ../toppar/top_all36_prot.rtf
parfile = ../toppar/par_all36m_prot.prm
pdbfile = ../1_build/initial.pdb
psffile = ../1_build/initial.psf
rstfile = ../2_minimize/min.rst

[SELECTION]
group1          = all and not hydrogen

[RESTRAINTS]
nfunctions      = 1
function1       = EM               # apply restraints from EM density map
constant1       = 30000            # force constant in Ebias = k*(1 - c.c.)
select_index1   = 1                # apply restraint force on protein heavy atoms

[EXPERIMENTS]
emfit           = YES                      # perform EM flexible fitting
emfit_target    = ../1_build/emd_0477.map  # target EM density map
emfit_sigma     = 1.85                     # half of the map resolution (3.7 A)
```

Now, let's run `ATDYN` for `INP`. The following is an example to execute
`mpirun` using 16 CPU cores:

```bash
# Perform flexible fitting
$ export OMP_NUM_THREADS=4
$ mpirun -np 4 ../bin/atdyn INP > log

$ ls
INP  log  run.dcd  run.pdb  run.rst
```

After the calculation, we obtain `run.dcd`, `run.pdb`, and `run.rst`,
where the PDB file is the structure at the last step. The `log` file
contains time courses of the energy and c.c. (Column 19: `RESTR_CVS001`). Let's view the trajectory by using VMD. You can see
that the structure is well fitted to the target density.

```bash
$ vmd -ccp4 ../1_build/emd_0477.map -pdb ../1_build/initial.pdb -psf ../1_build/initial.psf -dcd run.dcd
```

![](/assets/images/2022_03_Figure16.1.6.png){: width="600" .align-center}

<span style="color: #ff6600"><i class="fas fa-exclamation-triangle"></i></span> 
In this tutorial, we have done 500 steps of
energy minimization and 5 ps of flexible fitting. However, in practice
this should not be enough to obtain a well-fitted structure. Keep in
mind that this tutorial is only a demonstration. If you try to use the
same conditions as in this tutorial in your actual research, your
calculations may become unstable or the fitting may not work. In such
cases, please extend the minimization or simulation steps. In general,
you may need more than 1,000 steps of minimization or 100 ps of flexible
fitting. 
{: .notice--warning}

## 4. Analysis

### 4.1. Cross-correlation coefficient 

We analyze the time courses of c.c., which can be easily obtained by
using the following command. We select the value in the Column 19
(`RESTR_CVS001`) in the `log` file. You can see that the c.c. is
successfully increased during the fitting.

```bash
# Change directory for analysis
$ cd ../4_analysis

# Analyze time courses of c.c.
$ grep "INFO:" ../3_fitting/log | awk '{print $3, $19}' > cc.log
$ less cc.log
```

Let us plot the c.c. using gnuplot.

![](/assets/images/2022_06_figure-16-1-7.png){: width="400" .align-center}

### 4.2. MolProbity score

Finally, we evaluate the quality of the obtained structure. One of the
commonly used criteria is the MolProbity score,[^6] which represents
how good the structure is as a protein. It can be computed with the GUI
server. Let's access to the [MolProbity Server](http://molprobity.biochem.duke.edu/), 
and upload `run.pdb` obtained from the flexible fitting.

![](/assets/images/2022_03_Figure16.1.8.png){: width="800" .align-center}

After the process, the original hydrogen atoms are removed, 
but the original PDB file `run.pdb` are used in the next process.  
We select "Currently working on \[run.pdb File (modified) uploaded by user\]",  
and then click "analyze all-atom contacts and geometry".  
In the next page, we just click the \[Run programs to perform these analyses\] at the bottom of the page.

![](/assets/images/2022_03_Figure16.1.9.png){: width="800" .align-center}

Finally, we can obtain the MolProbity score of `run.pdb`. 
The score 1.94 seems to be good.

![](/assets/images/2022_03_Figure16.1.10.png){: width="800" .align-center}

There is still a room to improve the structural quality. If we use a
simulated annealing protocol in the flexible fitting, or if we further
carry out an energy minimization after the flexible fitting, the score
should be improved.

---

*Written by Takaharu Mori@RIKEN Theoretical molecular science
laboratory\
March 26, 2022*
{: .notice}

##  References

[^1]: F. Tama *et al.*, *J. Mol. Biol.*, **337**, 985--999 (2004). [<i class="fas fa-link"></i>](https://www.sciencedirect.com/science/article/pii/S0022283604001184)

[^2]: M. Orzechowski and F. Tama, *Biophys. J*., **95**, 5692--5705 (2008). [<i class="fas fa-link"></i>](https://www.cell.com/biophysj/fulltext/S0006-3495(08)81986-8)

[^3]: O. Miyashita *et al.*, *J. Comput. Chem.*, **38**, 1447--1461 (2017). [<i class="fas fa-link"></i>](https://onlinelibrary.wiley.com/doi/abs/10.1002/jcc.24785)

[^4]: T. Mori *et al.*, *Structure,* **27**, 161--174.e3 (2019). [<i class="fas fa-link"></i>](https://www.sciencedirect.com/science/article/pii/S0969212618303307)

[^5]: D. Matsuoka, Y. Sugita, and T. Mori (in preparation).

[^6]: V. B. Chen *et al.*, *Acta Cryst.*, D**66**, 12--21 (2010). [<i class="fas fa-link"></i>](http://scripts.iucr.org/cgi-bin/paper?S0907444909042073)

