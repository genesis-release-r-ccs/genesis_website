---
title: "GENESIS Tutorial 2.3 (2022)"
excerpt: ""
last_modified_at: 2025-06-03T00:00:56+09:00
layout: single
toc: true
toc_sticky: true
sidebar:
  nav: sidebar-basic
---

# Building the initial structure for MD simulation 

aaa
This tutorial describes how to set up a system for all-atom MD
simulations using the CHARMM force field. The figure below outlines a
general scheme for building a system for soluble proteins. The scheme
consists of five steps: 1) prepare the PDB file, 2) remove unnecessary
molecules from the PDB and move the center of mass (COM) of the protein
to the origin, 3) add hydrogen atoms to heavy atoms, 4) place water
molecules around the protein, and 5) randomly place ions in the water.
Here, we select [Protein G](https://www.rcsb.org/structure/2QMT) as an
example, and solvate the protein in 150 mM NaCl solution. Since GENESIS
is not providing a structure setup tool, we will use VMD for this
purpose.[^1] We will also use [psfgen-plugin](https://www.ks.uiuc.edu/Research/vmd/plugins/psfgen/),
[solvate-plugin](https://www.ks.uiuc.edu/Research/vmd/plugins/solvate/), and
[autoionize-plugin](https://www.ks.uiuc.edu/Research/vmd/plugins/autoionize/) in VMD.

![](/assets/images/2019_07_scheme-1.jpg)

##  Preparation

All the files required for this tutorial are hosted in the
[GENESIS tutorials repository on GitHub](https://github.com/genesis-release-r-ccs/genesis_tutorial_materials).
If you haven't downloaded the files yet, open your terminal and run
 the following command (see more in
[Tutorial 1.1](/tutorials/genesis_tutorial_1.1_2022/)):

```bash
$ cd ~/GENESIS_Tutorials-2022
# if not yet
$ git clone https://github.com/genesis-release-r-ccs/genesis_tutorial_materials
```

If you already have the tutorial materials, let's go to our working directory:
```bash
$ cd genesis_tutorial_materials/tutorial-2.3
```

We have prepared these five directories to
carry out each step. In fact, we will obtain multiple output files at
each step, and if we do all the work in one directory, it will be
difficult to recognize which file was output in which step. Such
situation can lead to accidental mistakes. Therefore, we create a
directory for each step and work "sequentially" in each directory. This
is one of the effective techniques to keep the directories organized in
MD simulation studies.

In Step 3, we need CHARMM topology files. Here, we create a symbolic
link to the directory where the CHARMM force field files are stored (see [Tutorial 2.2](/tutorials/genesis_tutorial_2.2_2022/))
using the `ln -s` command. Symbolic link is a kind of "shortcut" to
the target. It is useful for referring to a directory or file that is
far from the current working directory.

```bash
# Make a symbolic link to the CHARMM toppar directory
$ ln -s ../../Data/Parameters/toppar_c36_jul21 ./toppar
$ ls
1_oripdb  2_modpdb  3_psfgen  4_solvate  5_ionize  toppar

# Check the contents in the symbolic link
$ ls ./toppar/
00toppar_file_format.txt  par_all36_cgenff.prm       top_all35_ethers.rtf
ace                       par_all36_lipid.prm        top_all36_carb.rtf
cheq                      par_all36_lipid_ljpme.prm  top_all36_cgenff.rtf
drude                     par_all36_na.prm           top_all36_lipid.rtf
:
```

##  Step1. Prepare the PDB file of the target protein 

Here, we use the PDB ID: [2QMT](https://www.rcsb.org/structure/2QMT), which was solved by X-ray crystallography at 1.05 Å resolution. 
We have already downloaded the PDB file in [Tutorial 2.1](/tutorials/genesis_tutorial_2.1_2022/). Let's go to the `1_oripdb` directory, make a symbolic link to the PDB file, and look at the structure in VMD. 
We can find not only protein atoms but also oxygen of water and other compounds such as phosphate ion, isopropyl alcohol, and methylpentane diol. 
In fact, these additional compounds were used for crystallization of the protein in the experiment, and are not essential for this MD simulation. 
Now, we would like to also check the center of mass (COM) of the protein by using the `measure center` command in VMD. 
We can see that it has deviated significantly from the origin (0, 0, 0). This displacement may be inconvenient when the protein is solvated in a water box (Step 4). 
In the next step, we are going to remove unnecessary compounds from the PDB file and move the COM to the origin as well.

```bash
# Change the working directory
$ cd 1_oripdb

# Download the PDB file of protein G
$ ln -s ../../../Data/PDB/2QMT.pdb ./
$ ls
2QMT.pdb

# View the PDB structure by using VMD
# Measure the center of mass of the protein
$ vmd 2QMT.pdb
vmd > set sel [atomselect top "protein"]
vmd > measure center $sel weight mass
2.618640661239624 15.951170921325684 17.753435134887695
```

![](/assets/images/2019_07_setup_figure1.jpg)

##  Step2. Modify the original PDB file 

In Step 2, we modify the PDB file in order to set up the system correctly. Let's go to the `2_modpdb` directory. We can find `build.tcl` in this directory. 
Let's take a look at this file with the `less` command. This is a tcl script, which contains the VMD commands we want to execute. 
By executing this script through VMD, the following procedures can be performed sequentially and automatically: 
1) load the PDB file, 
2) change the residue name "HIS" to "HSD", 
3) change the atom name "CD1" to "CD" in ILE, and 
4) change the C-terminal atom names "O" and "OXT" to "OT1" and "OT2", respectively, 
5) select all protein atoms, 
6) measure the COM of the protein, and 
7) move COM to the origin. 
Finally, write the coordinates of the selected atoms (i.e., protein) to `proa.pdb`. 
The reason for changing the atom and residue names is that some of the atom and residue names are defined differently in the CHARMM force field than in the general PDB rules, and these names need to be changed to follow the CHARMM rules. 
The definitions of the names in CHARMM are found in the topology file.


```bash
# Change directory
$ cd ../2_modpdb
$ ls
build.tcl

# View the script
$ less build.tcl

# Load the original PDB
mol load pdb ../1_oripdb/2QMT.pdb

# Rename "PDB general atom name" to "CHARMM-specific atom name"
#   HIS => HSD (but not included in this protein)
#   CD1 atom of ILE => CD
#   C-terminal carboxyl oxygen O and OXT => OT1 and OT2
[atomselect top "resname HIS"                      ] set resname HSD
[atomselect top "resname ILE and name CD1"         ] set name CD
[atomselect top "chain A and resid 56 and name O"  ] set name OT1
[atomselect top "chain A and resid 56 and name OXT"] set name OT2

# Measure the center of mass (com) of the selected atoms (protein)
# Shift the com of the protein to the origin
set sel [atomselect top "protein"]
set com [measure center $sel weight mass]
$sel moveby [vecscale -1.0 $com]

# Write the modified PDB of the selected atoms
$sel writepdb proa.pdb
exit
```

Let's run this script in VMD with the following command. 
The option `-dispdev text` means to start VMD in text mode, i.e. not to open the viewer window. 
Once the above script is loaded with `-e build.tcl`, all the commands in the script will be executed automatically by VMD. 
In the `log` file, we can see what has been executed by VMD. 
Look at the obtained PDB file (`proa.pdb`), and make sure that the COM has indeed been moved to the origin.

```bash
# Run the script using VMD
$ vmd -dispdev text -e build.tcl > log
$ ls
build.tcl  log  proa.pdb

# View the structure by using VMD, and check the center of mass
$ vmd proa.pdb
vmd > set sel [atomselect top "all"]
vmd > measure center $sel weight mass
-0.0003528242523316294 0.00021110136003699154 0.0004838312161155045
```

![](/assets/images/2019_07_setup_figure2.jpg)

Also, check that the names of the atoms and residues you specified have been changed correctly.

```bash
# Check the renamed atoms 
$ less proa.pdb

:
ATOM     53  CG2 ILE A   6      -0.309   1.530  -8.307  1.00 11.81           C
ATOM     54  CD  ILE A   6       0.907   4.322  -7.613  1.00 22.44           C
ATOM     55  N   LEU A   7      -1.907  -0.854  -6.370  1.00  9.47           N
:
ATOM    431  C   GLU A  56       4.078  -9.678  -9.367  1.00 19.58           C
ATOM    432  OT1 GLU A  56       3.967  -8.911 -10.297  1.00 18.77           O
ATOM    433  CB  GLU A  56       2.136 -10.127  -7.893  1.00 14.63           C
:
ATOM    437  OE2 GLU A  56       0.400 -11.802  -5.347  1.00 16.91           O
ATOM    438  OT2 GLU A  56       4.660 -10.856  -9.352  1.00 23.62           O
```

##  Step3. Make PDB and PSF files of the protein

In Step 3, the missing atoms in the protein are constructed, and then,
"PSF file" as well as "complete PDB file" are created using the
`psfgen-plugin` on VMD. PSF is a "protein structure file" that contains
information about the entire system, including bond connectivity, atomic
charge, mass, etc., excluding atomic coordinates.

Go to the `3_psfgen` directory. Again, a script for VMD is provided
here. This script first turns on the plugin and loads the CHARMM C36
topology file. Then, a segment name for the protein is defined. In
CHARMM, a segment is usually defined as a molecule that has a complete
single chain. In this example, we named the protein `PROA`.  The
coordinates of each atom in PROA are read from the PDB file
(`../2_modpdb/proa.pdb`), and missing atoms are automatically built with
the `guesscoord` command. Finally, the PSF and PDB files are output.

```bash
# Change directory
$ cd ../3_psfgen
$ ls
build.tcl

# Load psfgen-plugin and CHARMM topology file
package require psfgen
resetpsf
topology ../toppar/top_all36_prot.rtf

# Define the segment name as "PROA"
segment PROA {pdb ../2_modpdb/proa.pdb} 

# Read the coordinates from the PDB file
coordpdb ../2_modpdb/proa.pdb PROA

# Guess the coordinates of missing atoms
guesscoord 

# Generate PDB and PSF files
writepdb protein.pdb
writepsf protein.psf
exit
```

Let's run this script in VMD, and then take a look at the obtained PDB file (`protein.pdb`). 
Here, we load the PSF file (`protein.psf`) along with the PDB file using the `-psf` option. 
The PSF file contains information about the bond connectivity of atoms in the system, so if the PDB and PSF files are not created correctly, you will see strange covalent bonds in VMD. 
If there are no problems with the structure, both PDB and PSF files should be fine.

```bash
# Run the script using VMD 
$ vmd -dispdev text -e build.tcl > log 
$ ls
build.tcl  log  protein.pdb  protein.psf

# View the structure while reading PSF file
$ vmd protein.pdb -psf protein.psf
```

![](/assets/images/2019_07_setup_figure3.jpg)

Let's take a look at the obtained PDB file with the `less` command. In the system, there are 858 atoms including hydrogen. The segment name "PROA" is written in the 12th column.

```bash
# View the processed PDB file
$ less protein.pdb

REMARK original generated coordinate pdb file
ATOM      1  N   MET A   1      -2.908  11.971   7.927  1.00  0.00      PROA N
ATOM      2  HT1 MET A   1      -2.554  12.700   8.513  0.00  0.00      PROA  
ATOM      3  HT2 MET A   1      -2.550  11.087   8.229  0.00  0.00      PROA  
ATOM      4  HT3 MET A   1      -3.907  11.963   7.959  0.00  0.00      PROA  
ATOM      5  CA  MET A   1      -2.483  12.211   6.569  1.00  0.00      PROA C
:
```

Check the PSF file as well. 
In the header section, you will find `first NTER` and `last CTER`. 
Here, `first NTER` indicates that the N-terminus of PROA has been capped with NH<sub>3</sub><sup>+</sup>. 
If you look at the CHARMM topology file, you will see that `NTER` is defined as a patch residue (PRES) that creates an NH<sub>3</sub> group at the N-terminus. 
Similarly, the C-terminus has been capped with COO<sup>-</sup> using the patch residue `CTER`. In the section right after that, you can also see detailed information about all the atoms, including residue name, atom name, type, charge, and mass for each atom. 
In the middle section, the bond connectivity is defined. 
There are 864 covalent bonds in the system, and the bonded atoms are defined by the indices of the neighboring atoms (e.g., [1-5], [2-1], and [3-1] pairs are corresponding to "N-CA", "HT1-N", and "HT2-N" bonds, respectively). 
Lists of angles and dihedral angles are also defined in the same manner. These lists are used for Σ<sub>bonds</sub>, Σ<sub>angles</sub>, and Σ<sub>dihedrals</sub> in the equation in [Tutorial 2.2](/tutorials/genesis_tutorial_2.2_2022/).

```bash
# View the PSF file 
$ less protein.psf

:
 REMARKS segment PROA { first NTER; last CTER; auto angles dihedrals }
 REMARKS defaultpatch CTER PROA:56  
 REMARKS defaultpatch NTER PROA:1
:
     858 !NATOM
       1 PROA 1    MET  N    NH3   -0.300000       14.0070           0
       2 PROA 1    MET  HT1  HC     0.330000        1.0080           0
       3 PROA 1    MET  HT2  HC     0.330000        1.0080           0
       4 PROA 1    MET  HT3  HC     0.330000        1.0080           0
       5 PROA 1    MET  CA   CT1    0.210000       12.0110           0
:

     864 !NBOND: bonds
       1       5       2       1       3       1       4       1
       5       6       7       5       7       8       7       9
      10       7      10      11      10      12      13      10
:

    1556 !NTHETA: angles
       1       5       6       1       5      18       2       1       5
       2       1       4       2       1       3       3       1       5
       3       1       4       4       1       5       5      18      19
:
```

##  Step4. Add water 

In Step 4, we solvate the protein in water. Let's go to the `4_solvate` directory. Again, a script for VMD is provided here. 
In the script, we turn on the solvate-plugin, set the input PDB and PSF files of the protein, and add water molecules around the protein with a box size of 64 Å × 64 Å × 64 Å. The `minmax` option specifies the minimum and maximum coordinates of the water box. 
The output file name is defined with the `-o` option.


```bash
# Change directory
$ cd ../4_solvate
$ ls
build.tcl

# Solvate the protein in 64x64x64 A^3 water box
package require solvate
set psffile "../3_psfgen/protein.psf"
set pdbfile "../3_psfgen/protein.pdb"
solvate $psffile $pdbfile -minmax \{\{-32 -32 -32\} \{32 32 32\}\} -o wbox 
exit
```

Let's run the script in VMD. As a result, we get `wbox.pdb` and `wbox.psf`. In the PDB file, we see that the water residue name is "TIP3", indicating that the [TIP3P water model](https://en.wikipedia.org/wiki/Water_model) is used.

```bash
# Run the script in VMD 
$ vmd -dispdev text -e build.tcl > log 
$ ls
build.tcl  log  wbox.log  wbox.pdb  wbox.psf

# Check the water molecules
$ less wbox.pdb

# Look at the structure in VMD
$ vmd wbox.pdb -psf wbox.psf
```

![](/assets/images/2019_07_setup_figure4.jpg)

------------------------------------------------------------------------

The optimal box size depends on the system and should be determined carefully. 
Basically, for adequate solvation, the margin between the protein surface and the edge of the water box needs to be wide. 
One choice is to set the margin size to a distance that is longer than the cutoff distance for non-bonded interactions. 
However, for instance, if the protein undergoes unfolding during the simulation, such a choice may not be sufficient. This is because a small box allows the protein to interact directly with itself in the image cells under the periodic boundary condition, which can stabilize the unfolded states (see Figure (a)). 
A similar problem occurs when elongated proteins are solvated in a rectangular water box. 
During the simulation, the protein may rotate, and then self-interactions may occur (see Fig. (b)). 
Those artifacts will affect the reliability of the results obtained from the MD simulation. **This kind of mistake has been frequently made by many people, so please be careful.**

![](/assets/images/2019_08_bad_example2.jpg)

------------------------------------------------------------------------

##  Step5. Add ions 

In most simulation studies, proteins are placed in water in which ions
are dissolved, i.e., in a solution with a certain ionic concentration.
Since the concentration of K<sup>+</sup> in the cell is \~150 mM, to simulate
proteins functioning inside the cell, we usually solvate the protein in
a 150 mM KCl solution \[mM = mmol/L (millimoles per liter)\]. On the
other hand, for proteins that reside outside the cell, a 150 mM NaCl
solution is usually used.

To add ions to the system, we use the autoionize-plugin in VMD. Let's
move to the `5_ionize` directory. In this directory, you will find a
script to randomly add sodium ions (Na<sup>+</sup>: SOD) and chloride ions
(Cl<sup>--</sup>: CLA) in the system. The total number of each ion is
automatically adjusted to reproduce the specified ion concentration
(0.15M). If you want to add potassium ions (K<sup>+</sup>), specify
`-cation POT` in the script. For other cases, please refer to the user
manual of autoionize-plugin.


```bash
# Change directory
$ cd ../5_ionize
$ ls
build.tcl

# Add ions in the system (Salt concentration: 150 mM NaCl)
package require autoionize
set psffile "../4_solvate/wbox.psf"
set pdbfile "../4_solvate/wbox.pdb"
autoionize -psf $psffile -pdb $pdbfile -sc 0.15 -cation SOD -anion CLA
exit
```

Eventually, we get `ionized.pdb` and `ionized.psf`.


```bash
# Run the script using VMD 
$ vmd -dispdev text -e build.tcl > log 
$ ls
build.tcl  ionized.pdb  ionized.psf  log

# View the obtained structure by using VMD 
$ vmd ionized.pdb -psf ionized.psf
```

![](/assets/images/2019_07_setup_figure5.jpg)

Now let's count the total number of sodium ions, chloride ions, and water molecules in `ionized.pdb` to see if the ion concentration is actually 150 mM. To count the sodium ions, we use the `grep` command to pick up the lines containing "SOD". You will find 26 sodium ions.


```bash
# Count sodium ions (Na+) by using "grep"
$ grep "SOD" ionized.pdb
:
ATOM  24530  SOD SOD I  26     -18.942 -14.615  18.877  1.00  0.00      ION
```

Here, the `wc` command is useful to count the lines. In the following commands, we first pick up the lines including `SOD` with the `grep` command, write them in a new file `sod.dat`, and then execute the `wc` command to this file.


```bash
# Count sodium ions (Na+) by using "grep" followed by "wc"
$ grep "SOD" ionized.pdb > sod.dat
$ wc -l sod.dat
26  sod.dat
```

These two commands can be combined via the pipeline `|`. Piping is very useful in many cases, and it will be frequently used in our Tutorials. Please, master the piping.


```bash
# Count chloride ions (Cl-) by using "grep" and "wc" commands with pipeline "|"
$ grep "SOD" ionized.pdb | wc -l
      26
```

Similarly, let's count the chloride ions and water molecules in the system.


```bash
# Count chloride ions (Cl-)
$ grep "CLA" ionized.pdb | wc -l
      22

# Count water molecules
$ grep "OH2 TIP3" ionized.pdb | wc -l
    7882
```

Now, remember basic chemistry. 
Since we specified 150 mM NaCl solution, the number of moles of solute in 1 litter (L) solution should be 150 mmol. 
The Avogadro number is 6.02 × 10<sup>23</sup>. 
Accordingly, there are 150 × 10<sup>-3</sup> × 6.02 × 10<sup>23</sup> = 9.03 × 10<sup>22</sup> NaCl molecules in the 1 L NaCl solution. 
Here, we assume that the density of NaCl solution is almost same with that of solvent. 
The density of water is \~0.997 g/cm<sup>3</sup> at the room temperature, and thus, the weight of 1 L water is \~997 g. 
Since the weight of 1 mol H<sub>2</sub>O is \~18.02 g, the 1 L water is composed of \~3.33 × 10<sup>25</sup> H<sub>2</sub>O molecules (55.3 mol). 
According to these relationships, when there are *N* water molecules in the system, we should add (9.03 × 10<sup>22</sup>)/(3.33 × 10<sup>25</sup>) × *N* = **0.002712 × *N*** NaCl molecules in the system to make 150 mM NaCl solution. 
In the case of *N* = 7,882, this equation gives \~21.4, which is close to the number of Cl<sup>−</sup> in the constructed system. 
Actually, 4 more Na<sup>+</sup> were added to the system than Cl<sup>−</sup>. 
This is because Protein G has a total net charge of −4, and the corresponding positive charge was needed to neutralize the system, which is required to use the particle mesh Ewald method (PME) in the MD simulations.

##  Let's take a note
Now, we have completed Tutorial 2.3. Let's move on to the `Works`
directory and take a look inside the directory again. At the moment, we
can see that the directory related to the tutorial is only
`tutorial-2.3`. You may expect that many directories will be added
here. Because the directory name `tutorial-x.x` alone does not tell us
what was done inside, we would like to create a `README` file and
write a brief description of each directory in this file as a note. In
the following example, the text "tutorial-2.3: Setup Protein G in NaCl"
is written directly to the `README` file using the `echo` command. Of
course, you can create the `README` file using `vi` or any other editor.


```bash
# Take a look inside the Works directory
$ cd ../../
$ ls
TRASH  tutorial-2.3

# Write down a brief description of tutorial-2.3
$ echo "tutorial-2.3: Setup Protein G in NaCl" > README
$ ls
README  TRASH  tutorial-2.3

# Check the file contents
$ less README
```

##  What is next?

In the previous tutorial, we downloaded the CHARMM parameter and topology files, and here, we learned how to build the initial structure of the target system by using VMD/PSFGEN to create the PDB and PSF files. We note that other tools such as CHARMM [^2] or CHARMM-GUI [^3] are also useful to create PDB and PSF files. We are now ready to start the MD simulation. These four files (PDB, PSF, parameter, and topology) are used as the inputs of GENESIS. In Tutorial 3, we will learn how to execute GENESIS. The system constructed here will be simulated in [Tutorial 3.3](/tutorials/genesis_tutorial_3.3_2022/).

![](/assets/images/2019_06_scheme_charmm.jpg)

------------------------------------------------------------------------

*Written by Takaharu Mori@RIKEN Theoretical molecular science laboratory\
Aug. 10, 2019*
{: .notice}

##  References

[^1]: [W. Humphrey *et al.*, *J. Mol. Graph.*, **14**, 33-38 (1996)](https://www.sciencedirect.com/science/article/pii/0263785596000185)

[^2]: [B. R. Brooks *et al.*, *J. Comput. Chem.*, **30**, 1545-1614 (2009)](https://onlinelibrary.wiley.com/doi/abs/10.1002/jcc.21287)

[^3]: [S. Jo *et al.*, *J. Comput. Chem.*, **29**, 1859-1865 (2008)](https://onlinelibrary.wiley.com/doi/10.1002/jcc.20945)
