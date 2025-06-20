---
title: "GENESIS Tutorial 5.2 (2022)"
gpos: 005.002
excerpt: ""
last_modified_at: 2025-06-03T00:00:56+09:00
layout: single
toc: true
toc_sticky: true
sidebar:
  nav: sidebar-basic
---

# Creating initial files for the AMBER force field

In this tutorial, we provide a guide for preparing parameter files,
topology files (`prmtop` file), and coordinate files (`crd` file) for
running an MD simulation using the AMBER force field. Throughout this
tutorial, we use the LEaP program, which is included in AMBER Tools,
therefore before beginning this tutorial the user should install the
latest version of AMBER Tools and set the environment for executing the
LEaP program. There are two tools in LEaP, tleap and xleap. The former
is a command-based tool to be used in a terminal, while the latter is an
X Windows system tool for performing modeling in GUI. In this tutorial,
we use tleap, but the same procedures can also be performed using xleap.
The prmtop and crd files produced in this tutorial can be used as input
for GENESIS.

##  Preparations

In this tutorial, we use
[AmberTools18](http://ambermd.org/doc12/Amber18.pdf) as an example, which is available free of charge. Of course, you can use the latest version of AmberTools.
First, download the program from the webpage.
We assume that you install the AMBER tools in `$HOME/GENESIS_Tutorials-2022/Programs`.
Then, add the following commands in the `.bashrc` or the `.bash_profile` file with respect to your installation path.
Or alternatively, execute the commands directly in the command line. 

```bash
source ~/GENESIS_Tutorials-2022/Programs/amber18/amber.sh
export AMBERHOME="~/GENESIS_Tutorials-2022/Programs/amber18"
```

This tutorial consists of 8 sections, create a directory for each
section.


```bash
# Preparation of Tutorial 5.2
$ cd ~/GENESIS_Tutorials-2022/Works

# Let's take a note
$ echo "tutorial-5.2: Creating input files for AMBER" >> README

$ mkdir tutorial-5.2
$ cd tutorial-5.2
$ mkdir Sec1 Sec2 Sec3 Sec4.1 Sec4.2 Sec5 Sec6 Sec7 Sec8
```

A large number of files will be created during the execution of the
following tutorial. In order to keep an uncluttered environment, execute
the commands for each section inside the sections's directory.

##  1. Single-chain protein

Using [PDB ID: 3MP9](https://www.rcsb.org/structure/3MP9) as an example, we show how to create `prmtop`
and `crd` files for a protein composed of a single chain. In the
commands presented below, we use VMD to select chain A of the protein
and write it out as `proa.pdb`. The phrase `and not altloc B` refers to
cases such as Thr25 or Glu50, in which the side-chain has two possible
orientations. In such case not B (namely A) should be chosen. In the
3MP9 structure, which was obtained using X-ray crystallography, hydrogen
atoms are not resolved. LEaP will automatically add hydrogen atoms. The
obtained `proa.pdb` file is read by tleap, and prmtop and crd files are
created using the ff14SB the force field.


```bash
# Download the PDB file
$ cd Sec1
$ wget https://files.rcsb.org/download/3MP9.pdb

# Use VMD to create a PDB file containing only chain A of the protein
$ vmd -dispdev text 3MP9.pdb
vmd > set sel [atomselect top "(chain A and protein) and not altloc B"]
vmd > $sel writepdb proa.pdb
vmd > exit

# Use tleap in AMBER tools to create the prmtop and crd files
$ tleap
> source leaprc.protein.ff14SB
> mol = loadpdb proa.pdb
> saveamberparm mol test.prmtop test.crd
> savepdb mol test.pdb
> quit
```

This protein contains several histidine residues. Histidines can exist
as three different types (HID, HIE, HIP) depending on the position of
the hydrogen atom on the sidechain. In AMBER, HIE is the default
histidine type, thus all HIS residues in the PDB file are automatically
converted to HIE and hydrogen atoms are added in the corresponding
locations. Open the `test.prmtop` and `test.crd` output files in VMD and
confirm that the structure is correctly displayed.


```bash
$ vmd -parm7 test.prmtop -rst7 test.crd
```

Also look at the `test.pdb` file and verify its content. In the original
PDB file, residue numbers started with 4, but here they were shifted to
start with 1. This feature is specific to LEaP, and the user should be
careful upon referencing residue numbers when performing trajectory
analysis.

Note: In this section, we used a single-chain
protein with no missing amino acid residues. Within the structures
deposited in the PDB, there are some in which parts such as loop regions
are missing. In such cases, residue numbers are not consecutive and care
should be taken. If tleap is executed as is, without considering the
number skip, a covalent bond will be generated, bypassing the missing
part. In case of missing regions, programs such as
[Modeller](https://salilab.org/modeller/) may be used to model them, or, if the missing
region does not have an important functional role, modeling may be
skipped, and the protein can be treated as a multi-chain. tleap will
automatically treat the protein as multi-chain if in the input PDB file
the TER line is inserted in the missing region.

##  2. Multi-chain protein

Here we show how to create `prmtop` and `crd` files for proteins made up
of several chains. We use the NMR-resolved [PDB ID: 1AFO](https://www.rcsb.org/structure/1afo) structure. This protein is a homo-dimer
consisting of chains A and B. The PDB file contains several models of
the protein structure, here we use Model 1, which corresponds to frame 0
when opening the file in VMD. In the original PDB file, all histidine
residues are seen to adopt the HID form. Thus, before executing tleap,
we use the "sed" command to change residue names appropriately. In the
previous section, we mentioned that inserting the TER line will
automatically separate the main chain before and after that line.
However here, we prepare a separate PDB file for each chain and use the
"combine" command in tleap for combining them.


```bash
# Download the PDB file
$ cd Sec2
$ wget https://files.rcsb.org/download/1AFO.pdb

# Use VMD to create separate PDB files for each chain, A and B
$ vmd -dispdev text 1AFO.pdb
vmd > set sel1 [atomselect top "chain A" frame 0]
vmd > set sel2 [atomselect top "chain B" frame 0]
vmd > $sel1 writepdb proa.pdb
vmd > $sel2 writepdb prob.pdb
vmd > exit

# Change all HIS residue names in the PDB to HID
$ sed -e "s/HIS/HID/g" proa.pdb > proa_mod.pdb
$ sed -e "s/HIS/HID/g" prob.pdb > prob_mod.pdb

# Create prmtop and crd files using tleap
$ tleap
> source leaprc.protein.ff14SB
> mol1 = loadpdb proa_mod.pdb
> mol2 = loadpdb prob_mod.pdb
> dimer = combine {mol1 mol2}
> saveamberparm dimer test.prmtop test.crd
> savepdb dimer test.pdb
> quit
```

As in the previous section, verify that the content of the `test.pdb`
file is displayed correctly. The residues in this protein were
originally numbered 62-101 (including both A and B chains), but upon
executing LEaP, the numbering changed into 1-40 (chain A) and 41-80
(chain B). Namely, upon executing the `savepdb` command, the residues of
chain A of a multi-chain protein are numbered from 1, and from the
second chain onward they receive consecutive numbering.

The protein used in the current example was rather small. However, for
huge protein complexes, instead of using TER and let chain splitting be
automatically executed, it is better to use the "combine" command for
creating prmtop and crd files while carefully inspecting the structure
of each chain. This may reduce the chances of errors in modeling.

##  3. Changing the protonation states of side-chains 

The side-chains of aspartatic acid (ASP) and glutamatic acid (GLU) are
usually deprotonatd under a neutral pH of 7. However, when such charged
residues are buried inside the hydrophobic core of a protein, the
charged state is unstable, and the side-chains often become protonated.
Protonation states can be predicted by estimating the pKa values using
well-known software such as
[PROPKA](https://github.com/jensengroup/propka-3.1),
[MCCE](https://gunnerlab.github.io/Stable-MCCE/), or
[MEAD](https://rtullmann.de/index.php?name=extended-mead).

As explained in sections 1 and 2, the positions of the added hydrogen
atoms are determined by tleap upon reading the residue names from the
PDB file. In case the pKa of a charged amino acid was predicted to be
high and we wish to protonate it, all we need to do is change its
residue name to represent a charged amino acid. For example, if we wish
to protonate an ASP residue, we change its residue name to ASH. The
following table summarizes protonated amino acid residue names. The
detailed command sequence will not be demonstrated here, but the user
may independently try to execute the procedure. Use the protein from
section 1, and change the residue name of one of the ASP or GLU residues
and verify that the residue has indeed been protonated.

  |Amino acid          |  Residue name in AMBER |
  |----|----|
  |Aspartic acid (ASP) |  ASP (deprotonated), ASH (protonated) |
  |Glutamic acid (Glu) |  GLU (deprotonated), GLH (protonated) |
  |Histidine (His)     |  HID (H in the δ-position N), HIE (H in the ε-position N), HIP (both sites are protonated) |
  |Cysteine (Cys)      |  CYS, CYX (in SS bonds), CYM (coordinated with a metal) |

Note: In zinc containing metalloproteins, The SH
group in Cys often appear in the S<sup>−</sup> form and coordinated with Zn<sup>2+</sup>.
In such cases, make sure to use the CYM form.

##  4. Adding disulfide bonds

**4.1 Single-chain protein**

We show how to create disulfide bonds between proximate cysteine
residues in a protein. Here we use the single-chain protein [PDB ID: 3EAC](https://www.rcsb.org/structure/3EAC) as an example. In the original PDB file,
cysteine residues are named CYS. In order to generate a disulfide bond
between two cyteine residues, we first change their names to CYX. CYX is
a cysteine residue in which the H atom is deleted from the S-H group in
order to form a disulfide bond. In 3EAC, there is a disulfide bond
between residues 122 and 164. Here, we use the "sed" command to change
the residue names, and then execute the "bond" command in tleap to
create a covalent bond between the SG atoms of CYX122 and CYX164.


```bash
# Download the PDB file
$ cd Sec4.1
$ wget https://files.rcsb.org/download/3EAC.pdb

# Use VMD to create a PDB file for chain A
$ vmd -dispdev text 3EAC.pdb
vmd > set sel [atomselect top "(chain A and protein) and not altloc B"]
vmd > $sel writepdb proa.pdb
vmd > exit

# Change the residue names of the cysteine residues
# for which we wish to create the disulfide bond to CYX
$ sed -e "s/CYS A 122/CYX A 122/g" proa.pdb | sed -e "s/CYS A 164/CYX A 164/g" > proa_mod.pdb

# Create prmtop and crd files using tleap
$ tleap
> source leaprc.protein.ff14SB
> mol = loadpdb proa_mod.pdb
> bond mol.122.SG mol.164.SG
> saveamberparm mol test.prmtop test.crd
> savepdb mol test.pdb
> quit
```

Open the obtained `test.prmtop` and `test.crd` files in VMD and verify
that the disulfide bonds were properly created. Just in case, check the
`test.pdb` as well. The original residue numbers for this protein start
with 73. However, they were now changed to start with 1. When executing
the `bond mol.122.SG mol.164.SG` command for single-chain proteins, the
original PDB file residue numbering can be used for creating the
disulfide bonds.

**4.2 Multi-chain protein**

Next, we show how to create `prmtop` and `crd` files for a multi-chain
protein containing a disulfide bond between chains. We use Insulin ([PDB ID: 3I40](https://www.rcsb.org/structure/3I40)) as an example. Insulin contains two chains,
A and B, with one intra-chain disulfide bond (A: Cys6-A: Cys11), and two
inter-chain (A: Cys7-B: Cys7 and A: Cys20-B: Cys19) disulfide bonds.
Similarly to the previous section, we use a patch to create the bonds.
As done in previous sections, we create a PDB file for each chain,
change the appropriate residue names from CYS to CYX, and combine the
two PDB files using tleap. Here, we execute tleap in two steps. In the
first step, we combine the two PDB files and use the `savepdb` command
to generate the combined the PDB file. In the resulting PDB file,
residue numbering has changed, and we write down the residue numbers for
which we wish to form the disulfide bonds. In the second step, we use
the `bond` command in tleap to create a covalent bond between the SG
atoms of the CYX residues whose numbers we obtained in the first step.


```bash
# Download the PDB file
$ cd Sec4.2
$ wget https://files.rcsb.org/download/3I40.pdb

# Use VMD to create a separate PDB file for each chain, A and B
$ vmd -dispdev text 3I40.pdb
vmd > set sel1 [atomselect top "(chain A and protein) and not altloc B"]
vmd > set sel2 [atomselect top "chain B and protein"]
vmd > $sel1 writepdb proa.pdb
vmd > $sel2 writepdb prob.pdb
vmd > exit

# Change the residue names of cysteine residues
# for which we wish to create a disulfide bond to CYX
$ sed -e "s/CYS/CYX/g" proa.pdb > proa_mod.pdb
$ sed -e "s/CYS/CYX/g" prob.pdb > prob_mod.pdb

# Using tleap, create PDB files with residues numbered from 1
$ tleap
> source leaprc.protein.ff14SB
> mol1 = loadpdb proa_mod.pdb
> mol2 = loadpdb prob_mod.pdb
> complex = combine {mol1 mol2}
> savepdb complex tmp.pdb
> quit

# Write down the residue numbers of the cysteine residue
# for which we wish to form a disulfide bond from the temp.pdb file,
# create the bond using the "bond" command
$ tleap
> source leaprc.protein.ff14SB
> complex = loadpdb tmp.pdb
> bond complex.6.SG complex.11.SG
> bond complex.7.SG complex.28.SG
> bond complex.20.SG complex.40.SG
> saveamberparm complex test.prmtop test.crd
> savepdb complex test.pdb
> quit
```

The reason we execute tleap in two stages is that residue numbers of the
second chain (renumbered in stage 1), is unknown to the user beforehand.
Therefore, it is difficult to specify residue numbers upon creating the
covalent bond in the `bond` command.

##  5. Protein-DNA complex

Up to now, we illustrated how to create `prmtop` and `crd` files for
systems containing only a protein. Here, we show how to deal with cases
in which a protein is bound to DNA. We use [PDB ID: 3LNQ](https://www.rcsb.org/structure/3LNQ) as an example. We follow the same procedure
as before, by creating separate PDB files for each chain and using tleap
to combine them. The `leaprc.protein.ff14SB` file does not contain
parameters for DNA, thus we additionally read either `leaprc.DNA.bsc1`[^1] or `leaprc.DNA.OL15`[^2]. Recently, Galindo-Murillo et al
provided useful information regarding which of the two parameter sets to
be chosen[^3]. Here, we use the latter parameter set.


```bash
# Download the PDB file
$ cd Sec5
$ wget https://files.rcsb.org/download/3LNQ.pdb

# Use VMD to create a PDB file containing only chain A
$ vmd -dispdev text 3LNQ.pdb
vmd > set sel1 [atomselect top "chain A and protein"]
vmd > set sel2 [atomselect top "chain B and nucleic"]
vmd > set sel3 [atomselect top "chain C and nucleic"]
vmd > $sel1 writepdb proa.pdb
vmd > $sel2 writepdb dna1.pdb
vmd > $sel3 writepdb dna2.pdb
vmd > exit

# Create prmtop and crd files using tleap
$ tleap
> source leaprc.protein.ff14SB
> source leaprc.DNA.OL15
> mol1 = loadpdb proa.pdb
> mol2 = loadpdb dna1.pdb
> mol3 = loadpdb dna2.pdb
> complex = combine {mol1 mol2 mol3}
> saveamberparm complex test.prmtop test.crd
> savepdb complex test.pdb
> quit
```

##  6. Including crystal structure water and ions

Here we show how to create `prmtop` and `crd` files for systems
containing water molecules which were resolved in the X-ray crystal
structure, internal water molecules predicted using programs such as
[DOWSER](https://www.ks.uiuc.edu/Research/vmd/plugins/dowser/), or ions coordinated with the protein. Here,
we present a simple example for creating a PSF file for [PDB: 2I5M](https://www.rcsb.org/structure/2I5M), which contains water and a magnesium ion.
Recently it is being increasingly recommended that the TIP4P water model
should be used when using the AMBER force field. Here, assuming that
electrostatic interactions are calculated using the Ewald method, tleap
reads the `leaprc.water.tip4pew` file ("ew" stands for Ewald). Files for
ions are read as follows. For monovalent ions, Joung and Chetham
parameter is used, whereas for 2-4 valent ions, the 12-6 Lennard-Jones
potential form is used. These parameters were optimized to be used
together with TIP4Pew.


```bash
# Download the PDB file
$ cd Sec6
$ wget https://files.rcsb.org/download/2I5M.pdb

# Create separate PDB files for the protein, water, and ions
$ vmd -dispdev text 2I5M.pdb
vmd > set sel1 [atomselect top "protein and not altloc B"]
vmd > set sel2 [atomselect top "water"]
vmd > set sel3 [atomselect top "resname MG"]
vmd > $sel1 writepdb proa.pdb
vmd > $sel2 writepdb water.pdb
vmd > $sel3 writepdb mg.pdb
vmd > exit

$ tleap
> source leaprc.protein.ff14SB
> source leaprc.water.tip4pew
> mol1 = loadpdb proa.pdb
> mol2 = loadpdb water.pdb
> mol3 = loadpdb mg.pdb
> complex = combine {mol1 mol2 mol3}
> saveamberparm complex test.prmtop test.crd
> savepdb complex test.pdb
> quit
```

When we open the resulting `test.prmtop` and `test.crd` in VMD, we
notice that there is a covalent bond between the two hydrogen atoms of
each water molecule. This is due to the fact that water molecules are
treated as rigid. When using the SHAKE method, the H-H distance is
restrained (as well as the O-H bond length), thus this covalent bond is
required, and there is no need to be concerned if such H-H bond appears.
One can also notice that hydrogen atoms are randomly oriented.
Therefore, in order to optimize the structure of the crystal water,
either energy minimization, or an MD simulation constraining them to
oxygen atoms is required. Let's verify the content of the `test.pdb`
file. Residue names for water molecules were changed from HOH to WAT,
and because TIP4P is used, a dummy atom named EPW was added.

##  7. Ligand-containing system

For systems containing small molecules such as drugs that bind to
proteins (ligands), the Generalized Amber Force Field (GAFF) is widely
used. The accuracy of this force field is low comparing to those for
amino acids, RNA, or DNA, for which parameters were carefully optimized.
However, this is a simple parameter set, useful for applications such as
drug design in which numerous calculations on various ligand types are
performed. Here, we use [PDB: 1OV5](https://www.rcsb.org/structure/1OV5) as an example for a ligand-bound
protein system. First, similarly to previous sections, we create PDB
files separately for the protein and for the ligand (residue name 2LP).


```bash
# Download the PDB file
$ cd Sec7
$ wget https://files.rcsb.org/download/1OV5.pdb

# Use VMD to create separate PDB files for chain A
# of the protein and for the ligand
$ vmd -dispdev text 1OV5.pdb
vmd > set sel1 [atomselect top "chain A and protein"]
vmd > set sel2 [atomselect top "chain A and resname 2LP"]
vmd > $sel1 writepdb proa.pdb
vmd > $sel2 writepdb 2LP.pdb
vmd > exit
```

The resulting `2LP.pdb` file does not contain hydrogen atoms, thus they
need to be added. One simple option, which we use here, is using the
[MolProbity](http://molprobity.biochem.duke.edu/) web server to add the hydrogen atoms. We
upload the `2LP.pdb` file to the server and use the "Add hydrogen"
function to add the atoms. Download the output file (`2LPFH.pdb`) and
verify using VMD that hydrogen atoms were added correctly. As an
alternative MolProbity, the molecule editing function in molecular
visualization software can be used.

GAFF does not support all ligand types. For those ligands, we need to
perform quantum chemical calculations in order to obtain the parameters
which are missing from GAFF. This procedure can be easily performed
using the antechamber and parmchk2 tools, which are included in Amber
Tools. Upon providing the PDB file of the ligand (after adding hydrogen atoms) and executing antechamber, a quantum chemical calculation is
performed, resulting in a mol2 output file. Upon further providing the
mol2 as an input for the parmchk2 tool, parameters missing from GAFF are
identified and obtained in the form of a frcmod file. Here, we show the
procedure for GAFF2, which is the latest version of GAFF.


```bash
# Create a directory for the antechamber calculation
# and copy the hydrogen-included ligand file
$ mkdir Antechamber
$ cd Antechamber
$ cp ~/Download/2LPFH.pdb ./

# Execute antechamber and obtain the parameters
# missing from gaff using the prmchk2 command
$ antechamber -i 2LPFH.pdb -fi pdb -o ligand.mol2 -fo mol2 -c bcc -at gaff2
$ parmchk2 -i ligand.mol2 -f mol2 -o ligand.frcmod -s gaff2
```

The desired calculation method for charges can be specified by the -c
option in antechamber. Here, we used the AM1-BCC method. In case we wish
to use RESP charges, we need to obtain parameters from an external
Gaussian calculation. Let's have a look at the content of the mol2 file
and focus on the part which is tagged with `@<TRIPOS>ATOM`. Atom types
are displayed in the 6th column whereas charges are in the 9th column.
Atom types are written in lower case letters, differentiating GAFF from
other amino acids. Let's have a look at the `ligand.frcmod` file, which
was obtained from parmchk2. Several lists were added to IMPROPER. These
are the parameters which are missing in GAFF. Finally, we read all
output files obtained so far to tleap and generate the prmtop and crd
files.


```bash
# Create prmtop and crd files using tleap
$ cd ../
$ tleap
> source leaprc.protein.ff14SB
> source leaprc.gaff2
> mol1 = loadpdb proa.pdb
> mol2 = loadmol2 ./Antechamber/ligand.mol2
> loadamberparams ./Antechamber/ligand.frcmod
> complex = combine{mol1 mol2}
> saveamberparm complex test.prmtop test.crd
> savepdb complex test.pdb
> quit
```

##  8. Solvating the system 

In the above sections, we have learned how to create input files for
proteins in the absence of bulk water. However, in most cases, we
simulate proteins in solution. In typical, 150 mM KCl solution is
employed to mimic the cellular environment. In order to build such
environment, we have to add water and ions in the system, and also
neutralize the net charge of the system to utilize the Ewald method. In
this section, we first explain the basic scheme to add water, and then
add ions to make 150 mM KCl solution.

### Add water

In the following example, we solvate the protein used in Section 1 with
the TIP4P-EW water model. Here, we shift the center of mass of the
protein to the origin using VMD, and add water with a given box size
using tLeap.

```bash
# Download the PDB file
$ cd Sec8
$ wget https://files.rcsb.org/download/3MP9.pdb

# Create PDB file that contains the protein only
# At the same time, center of mass is shifted
$ vmd -dispdev text 3MP9.pdb
vmd > set sel [atomselect top "(chain A and protein) and not altloc B"]
vmd > set com [measure center $sel weight mass]
vmd > $sel moveby [vecscale -1.0 $com]
vmd > $sel writepdb proa.pdb
vmd > exit

# Add water by specifying the box size
$ tleap 
> source leaprc.protein.ff14SB 
> source leaprc.water.tip4pew 
> mol = loadpdb proa.pdb
> set mol box {80 80 80}
> solvatebox mol TIP4PEWBOX 0
> saveamberparm mol test.prmtop test.crd 
> savepdb mol test.pdb 
> quit
```

### Add water and ions

Now, we explain how to add ions in the system to solvate the protein in
150 mM KCl solution. Remember basic chemistry. In 150 mM solution, the
number of moles of solute in 1 litter (L) solution should be 150 mmol.
The Avogadro number is 6.02 × 10<sup>23</sup>. Accordingly, there are 150 ×
10<sup>-3</sup> × 6.02 × 10<sup>23</sup> = 9.03 × 10<sup>22</sup> KCl molecules in the 1 L KCl
solution. Here, we assume that the density of KCl solution is almost
same with that of solvent. The density of water is \~0.997 g/cm<sup>3</sup> at
the room temperature, and thus, the weight of 1 L water is \~997 g.
Since the weight of 1 mol H<sub>2</sub>O is \~18.02 g, the 1 L water is composed
of \~3.33 × 10<sup>25</sup> H<sub>2</sub>O molecules (55.3 mol). According to these
relationships, when there are *N* water molecules in the system, we
should add (9.03 × 10<sup>22</sup>)/(3.33 ×10<sup>25</sup>) × *N* = **0.002712 × *N*** KCl
molecules in the system to make 150 mM KCl solution. For instance, in
the case of *N* = 10,000, we add 27 K<sup>+</sup> and 27 Cl<sup>-</sup>.

Since we usually use the Ewald method in the MD simulations, the total
charge of the system must be zero. In tLeap, we specify the number of
ions manually so as to neutralize the system. In the following example,
we add K<sup>+</sup> and Cl<sup>-</sup> to make 150 mM solution with charge neutrality.
Since the total charge of the protein is -5, and there are 15,519 water
molecules in the system, we add 42 Cl<sup>-</sup> and (42 + 5) K<sup>+</sup>. These ions
are randomly placed in the system. Total charge of the system can be
check with the `charge` command.


```bash
$ tleap 
> source leaprc.protein.ff14SB 
> source leaprc.water.tip4pew 
> mol = loadpdb proa.pdb
> charge mol
Total unperturbed charge: -5.000000
Total perturbed charge: -5.000000
> set mol box {80 80 80}
> solvatebox mol TIP4PEWBOX 0
  Added 15519 residues.
> addionsrand mol Cl- 42
> addionsrand mol K+ 47
> charge mol
Total unperturbed charge: 0.000000
Total perturbed charge: 0.000000
> saveamberparm mol test.prmtop test.crd 
> savepdb mol test.pdb 
> quit
```

We can see the box size of the system in the 1st line of the obtained
PDB file (`test.pdb`). This is an input information to be specified in
the `[BOUNDARY]` section of the GENESIS control file.

##  Appendix. Executing tleap using a script

In this tutorial, we illustrated how to set up a system by executing the
commands one by one. It will be convenient to collect all the commands
into a single script file and execute it using VMD or tleap. For
example, here we create VMD and tleap script files (`vmd.tcl` and `tleap.inp`, respectively) for the command sequence introduced in
section 1.

**vmd.tcl:**


```bash
mol load pdb 3MP9.pdb
set sel [atomselect top "(chain A and protein) and not altloc B"] 
$sel writepdb proa.pdb
exit
```

**tleap.inp:**


```bash
source leaprc.protein.ff14SB 
mol = loadpdb proa.pdb 
saveamberparm mol test.prmtop test.crd 
savepdb mol test.pdb 
quit
```

Next, we execute the two script files using the following command.


```bash
$ vmd -dispdev text -e vmd.tcl > vmd.log
$ tleap -f tleap.inp > tleap.log
```

When done this way, an execution log file can be written, making error
tracking easy.

*Written by Takaharu Mori@RIKEN Theoretical molecular science laboratory
(Original Japanese document)\
Translated by Ai Shinobu@RIKEN Computational Biophysics Research Team
March 23, 2022*\
*Updated links by Chigusa Kobayashi@RIKEN Computational Biophysics
Research Team May 30, 2024*
{: .notice}

##  References 

[^1]: [I. Ivani *et al.*, *Nat. Methods*, **13**, 55-58 (2016).](https://www.nature.com/articles/nmeth.3658)
[^2]: [M. Zgarbova *et al.*, *J. Chem. Theory Comput.,* **11**, 5723-5736 (2015).](https://pubs.acs.org/doi/abs/10.1021/acs.jctc.5b00716)
[^3]: [R. Galindo-Murillo *et al.*, *J. Chem. Theory Comput.,* **12**, 4114-4127 (2016).](https://pubs.acs.org/doi/full/10.1021/acs.jctc.6b00186)
[^4]: [S. Joung and T. E. Chetham, *J. Phys. Chem. B*, **112**, 9020-9041 (2008).](https://pubs.acs.org/doi/10.1021/jp8001614)
