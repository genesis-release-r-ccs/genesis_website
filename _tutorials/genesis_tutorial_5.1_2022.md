---
title: "GENESIS Tutorial 5.1 (2022)"
gpos: 005.001
excerpt: ""
last_modified_at: 2025-06-03T00:00:56+09:00
layout: single
toc: true
toc_sticky: true
sidebar:
  nav: sidebar-basic
---

# Creating input files for the CHARMM force field

In this tutorial, we show how to create PSF and PDB files using VMD, for
performing an MD simulation using the CHARMM force field. Such files can
easily be created using the "Solvater" or the "membrane builder" tool in
CHARMM-GUI. However, there are cases where we wish to perform an MD
simulation of a protein for which the crystal structure has only
recently been solved and was yet to be published. In such cases, we do
not want to upload the structure to an external web server such as
CHARMM-GUI. Also, when uploading very large protein systems, the
CHRAMM-GUI server might overload, resulting in long calculation times.
Therefore, having an option to create PSF files in a local computer
without depending on CHARM-GUI is very useful. This task requires some
experience and knowledge. For example, special care should be taken in
cases where the protein is composed of several chains, or when we wish
to protonate a certain amino acid residue, or add a disulfide bond. The
PSF and PDB files created using the procedure introduced here can be
used as input files for GENESIS.

##  Preparations

All the files required for this tutorial are hosted in the 
[GENESIS tutorials repository on
GitHub](https://github.com/genesis-release-r-ccs/genesis_tutorial_materials).

If you haven't downloaded the files yet, open your terminal 
and run the following command:

```bash
# if not yet
$ git clone https://github.com/genesis-release-r-ccs/genesis_tutorial_materials
```

Then, make a symbolic link to the CHARMM `toppar` directory.
In the directory, there are a lot of files that are separated according
to molecule type. We use these files to build complex systems.

```bash
# If you already have the tutorial materials, let's go to our working directory:
$ cd genesis_tutorial_materials

# Let's take a note
$ echo "tutorial-5.1: Creating input files for CHARMM" >> README

# Check the contents in Tutorial 5.1
$ cd tutorial-5.1
$ ln -s ../../Data/Parameters/toppar_c36_jul21 ./toppar
$ ls
System1  System2  System3  System4  System5  System6  toppar
```

##  1. Single-chain protein

In many cases, an X-ray crystal structure is used as an initial
structure of the MD simulation. Hydrogen atoms are usually not resolved
under X-ray crystal structure analysis, thus when running an all-atom MD
simulation, we first need to add the missing hydrogens. Here, we use the
Human thioredoxin ([PDB ID: 1ERT](https://www.rcsb.org/structure/1ERT)) as an example. This PDB file includes the
protein and water molecules, but first, we will extract only the protein
and create a PSF file. The VMD script for the procedure is presented
below.


```bash
# Change directory and download the PDB file of the target protein
$ cd System1
$ ls 
1ERT.pdb  build1.tcl  build2.tcl

# Make PDB and PSF files of the target system 
$ vmd -dispdev text -e build1.tcl 
$ vmd -dispdev text -e build2.tcl 
$ ls 
1ERT.pdb  build1.tcl  build2.tcl  proa.pdb  protein.pdb  protein.psf
```

Looking at the content of the `../toppar/top_all36_prot.rtf` file, we
notice that in the CHARMM force field, histidine (HIS) residues can be
assigned three possible different residue names (HSD, HSE, and HSP)
according to the position of the protonatable hydrogen atom(s). Here, we
consider all HIS residues as HSD type. Also, for isoleucine (ILE) the δ
carbon atom is named CD1 under the PDB naming conventions but CD under
the CHARMM force field naming conventions, thus we need to change the
atom name. In the phrase `protein and not altloc B` only the protein is
being selected. `not altloc B` refers to cases such as Asp20 or His43,
in which the sidechain has two possible orientations. In such case not B
(namely A) is selected.

**build1.tcl:**


```bash
# 1) Load the PDB file
mol load pdb 1ERT.pdb

# 2) Rename "PDB general name" to "CHARMM-specific name"
[atomselect top "resname ILE and name CD1"] set name CD
[atomselect top "resname HIS"             ] set resname HSD

# 3) Shift the center of mass to the origin
# set sel [atomselect top "all"]
# set com [measure center $sel weight mass]
# $sel moveby [vecscale -1.0 $com]

# 4) Output the PDB file of each segment
set sel1 [atomselect top "protein and not altloc B"]
$sel1 writepdb proa.pdb
exit
```

The PSF file contains information regarding atoms' charges, masses and
bond types. For creating the PSF file, topology files, which contain
this information, are read as input. Here we want to create a PSF file
for the protein, therefore we use the `top_all36_prot.rtf` file, which
contains amino acid topology information. Next, `proa.pdb` is read and
defined as the PROA segment, and the coordinates information is
filled-in for PROA. In the `guesscoord` command, coordinates of hydrogen
atoms (or heavy atoms, if necessary) which were missing in the original
PDB file are guessed and added.

**build2.tcl:**


```bash
# 1) Load the psfgen-plugin and CHARMM topology file
package require psfgen
resetpsf
topology ../toppar/top_all36_prot.rtf

# 2) Define a segment name for each chain
segment PROA {pdb proa.pdb}

# 3) Read the coordinates of atoms in each chain
coordpdb proa.pdb PROA

# 4) Guess the coordinates of missing atoms
guesscoord

# 5) Generate PDB and PSF files
writepdb protein.pdb
writepsf protein.psf
exit
```

Upon executing the `guesscoord` command,
numerous "`psfgen) Warning: poorly guessed coordinate for atom`" warning
messages will appear. If these messages refer to hydrogen atoms or to
the c-terminus carbonyl oxygen, no special care should be taken.
However, in cases other than these, it is better to look at the original
PDB file and trace the cause of the warning.

Finally, the `protein.pdb` and `protein.psf` files are obtained. It is
better to inspect the content of `protein.pdb` and `protein.psf`, and
visualize `1ERT.pdb` and `protein.pdb` in VMD to confirm that the
structures were built as expected. It is very important to inspect the
PSF file. The PSF file contains information on bonds, and in case there
is an error during the making of the PSF file, bonds will be displayed
incorrectly.


```bash
# Check the obtained PDB and PSF files using VMD
$ vmd protein.pdb -psf protein.psf
vmd > mol load pdb 1ERT.pdb
```

In this section, we used a single-chain
protein with no missing amino acid residues. Within the structures
deposited in the PDB, there are some in which parts such as loop regions
are missing. In such cases, residue numbers are not consecutive and
special treatment is required for handling the gaps in residue
numbering. Programs such as
[Modeller](https://salilab.org/modeller/) may be used to model the missing residues,
or, if the missing region does not have an important functional role,
modeling may be skipped, and the protein can be treated as a
multiple-chain molecule (as will be explained in section 3).

##  2. Multiple-chain protein

Change directory to "`System2`". Here we show how to create a PSF file
for a protein composed of two or more chains. We use the NMR-solved
FoxM1 transcription factor ([PDB ID: 6OSW](https://www.rcsb.org/structure/6OSW)) as an example, which consists of chains A
and B. The overall procedure is similar to the above. However, one point
of caution is that a separate PDB file should be prepared for each
chain. Here, chains A and B were given the segment names PROA and PROB,
respectively. We use the first model (frame 0) out of the 10 NMR model
structures. Segment names appear on the 12<sup>th</sup> column in the
`protein.pdb` file.

**build1.tcl:**

``` bash
# 4) Output the PDB file of each segment
set sel1 [atomselect top "protein and chain A" frame 0]
set sel2 [atomselect top "protein and chain B" frame 0]
$sel1 writepdb proa.pdb
$sel2 writepdb prob.pdb
```

**build2.tcl:**


```bash
# 2) Define a segment name for each chain
segment PROA {pdb proa.pdb}
segment PROB {pdb prob.pdb}

# 3) Read the coordinates of the atoms in each chain
coordpdb proa.pdb PROA
coordpdb prob.pdb PROB
```

##  3. Including crystal structure water and ions 

There are cases in which we want the PSF file to include water molecules
which were resolved in the crystal structure (will be referred to as "crystal water") or internal water molecules which were predicted using
programs such as dowser, or ions coordinated with the protein.
Topology information for water and ions is included in the
`toppar_water_ions.str` file. Here, we show a simple example for
creating a PSF file for [PDB: 3B32](https://www.rcsb.org/structure/3B32), which contains crystal water and calcium
ions.

In the CHARMM force field, the TIP3P water model is commonly used for
simulating water molecules. Hence, the residue name for water molecules
is changed from HOH to TIP3. Also, oxygen atom names were changed from O
to OH2. Observing the crystal water molecules in the `protein.pdb` file,
we notice that hydrogen atoms are located randomly. In order to optimize
the structure of the crystal water, either energy minimization, or an MD
simulation constraining them to oxygen atoms is required.

**build1.tcl:**


```bash
# 2) Rename "PDB general name" to "CHARMM-specific name"
[atomselect top "resname ILE and name CD1"] set name CD
[atomselect top "resname HIS"             ] set resname HSD
[atomselect top "resname HOH and name O"  ] set name OH2
[atomselect top "resname HOH"             ] set resname TIP3
[atomselect top "resname CA  and name CA" ] set name CAL
[atomselect top "resname CA"              ] set resname CAL

# 4) Output the PDB file of each segment
set sel1 [atomselect top "protein"    ]
set sel2 [atomselect top "water"      ]
set sel3 [atomselect top "resname CAL"]
$sel1 writepdb proa.pdb
$sel2 writepdb water.pdb
$sel3 writepdb cal.pdb
```

**build2.tcl:**


```bash
# 1) Load psfgen-plugin and CHARMM topology file
package require psfgen
resetpsf
topology ../toppar/top_all36_prot.rtf
topology ../toppar/toppar_water_ions.str

# 2) Define a segment name for each chain
segment PROA {pdb proa.pdb }
segment SOLV {pdb water.pdb} 
segment CAL  {pdb cal.pdb  }

# 3) Read the coordinates of the atoms in each chain
coordpdb proa.pdb  PROA 
coordpdb water.pdb SOLV
coordpdb cal.pdb   CAL
```

If we look at `protein.pdb` with
`protein.psf` in VMD, there is a covalent bond between the two hydrogen
atoms of each water molecule. This is due to the fact that water
molecules are treated as rigid. When using the SHAKE method, the H-H
distance is restrained (as well as the O-H bond length), hence this
covalent bond is required, and there is no need to be concerned if such
H-H bond appears.

##  4. Changing the protonation state of side-chains

The side-chains of aspartatic acid (ASP) and glutamatic acid (GLU) are
usually deprotonatd under a neutral pH of 7. However, if such charged
residues are buried inside the hydrophobic core of a protein, the
charged state is unstable, and they often become protonated. Protonation
states can be predicted by estimating the pKa values using well-known
software such as
[PROPKA](https://github.com/jensengroup/propka-3.1),
[MCCE](https://sites.google.com/site/mccewiki/home?authuser=0), or
[MEAD](https://rtullmann.de/index.php?name=extended-mead). If the pKa of a charged amino acid
side-chain was predicted to be high, and we wish to perform an MD
simulation using the protonated state, we can perform the protonation by
applying a patch upon creating the PSF file. Here, as an example, we
show the script for protonating Asp26 in the Human thioredoxin ([PDB ID: 1ERT](https://www.rcsb.org/structure/1ERT)) from the previous section.

Let's have a look inside `top_all36_prot.rtf.` We notice that "ASPP" is
a patch (PRES) for protonating an aspartic acid residue. Similarly, for
protonating a glutamic acid residue, the "GLUP" patch is used. It is
important to execute the `regenerate angles dihedrals` command after
applying the patch. If we forget to apply it, the angle and dihedral
terms involving the added hydorgen atoms will be missing in the PSF
file, and during the simulation hydrogen atoms will twist in various
directions.

**build2.tcl:**


```bash
# 2) Define a segment name for each chain
segment PROA {pdb proa.pdb}

# 3) Apply patches to protonate high pKa residues
patch ASPP PROA:26
regenerate angles dihedrals

# 4) Read the coordinates of the atoms in each chain
coordpdb proa.pdb PROA
```

In proteins containing metal ions such as
Zn<sup>2+</sup>, the SH groups of cysteine residues will become S<sup>-</sup> and might
attach to Zn<sup>2+</sup>. In such cases, the CYM patch in `top_all36_prot.rtf`
can be used for deprotonating the cysteine residues.

##  5. Adding disulfide bonds

Here we will show how to create PSF files for proteins which contain
disulfide bonds, using Insulin ([PDB ID: 3I40](https://www.rcsb.org/structure/3I40)) as an example. Insulin contains two chains,
A and B, with one intra-chain disulfide bond in chain A, and two
inter-chain (A-B) bonds. Similarly to the previous section, we use a
patch to create the bonds.

Let's have a look inside `top_all36_prot.rtf`. `DISU` is a patch for
creating a disulfide bond. As in the previous section, we make sure to
execute the `regenerate angles dihedrals` command after applying the
patch.


```bash
# Confirm the sites of the disulfide bonds
$ grep SSBOND 3I40.pdb
SSBOND  1  CYS A   6  CYS  A  11        1555 1555 2.03 
SSBOND  2  CYS A   7  CYS  B   7        1555 1555 2.04 
SSBOND  3  CYS A  20  CYS  B  19        1555 1555 2.04
```

**build2.tcl:**


```bash
# 2) Define a segment name for each chain
segment PROA {pdb proa.pdb}
segment PROB {pdb prob.pdb}

# 3) Apply patches to create disulfide bonds
patch DISU PROA:6  PROA:11
patch DISU PROA:7  PROB:7
patch DISU PROA:20 PROB:19
regenerate angles dihedrals

# 4) Read the coordinates of the atoms in each chain
coordpdb proa.pdb PROA
coordpdb prob.pdb PROB
```

##  6. Protein-DNA complex 

The procedure for creating a PSF for a protein-DNA complex is rather
complicated. This is because in the CHARMM force field, the default
bases are those of RNA, and in order to treat DNA, the "DEOX" patch
needs to be applied for each and every base (make sure you understand the difference between DNA and RNA structures). Moreover, for the
5′-end, the "DEO5" patch needs to be applied. The following commands use
the [PDB ID: 3LNQ](https://www.rcsb.org/structure/3LNQ) as an example. Sections involving DNA are
colored. The two DNA strands contain 14 bases each, numbered 1-14. The
number of required patches is large, thus the *for* statement is used
for applying the DEOX patch for bases number 2-14.

In the original PDB file, the bases were named DA, DT, DC, and DG. Here,
base names were converted to the CHARMM force field names ADE, THY, CYT,
and GUA, respectively. In addition, the C7 carbon atom of thymine was
converted to C5M, and the OP1 and OP2 atoms of each base were converted
to O1P and O2P, respectively. The `complex.pdb` and `complex.psf` files
are obtained. Make sure to check the actual structure and confirm that
it represents a DNA structure. In case the patch was not applied
properly, the resulting structure will be of an RNA.

**build1.tcl:**


```bash
# 1) Load the PDB file
mol load pdb 3LNQ.pdb

# 2) Rename "PDB general name" to "CHARMM-specific name"
[atomselect top "resname ILE and name CD1"] set name CD
[atomselect top "resname HIS"             ] set resname HSD
[atomselect top "resname DT  and name C7" ] set name C5M
[atomselect top "nucleic and name OP1"    ] set name O1P
[atomselect top "nucleic and name OP2"    ] set name O2P
[atomselect top "resname DA"              ] set resname ADE
[atomselect top "resname DT"              ] set resname THY
[atomselect top "resname DC"              ] set resname CYT
[atomselect top "resname DG"              ] set resname GUA

# 4) Output the PDB file of each segment
set sel1 [atomselect top "chain A and protein"]
set sel2 [atomselect top "chain B and nucleic"]
set sel3 [atomselect top "chain C and nucleic"]
$sel1 writepdb proa.pdb
$sel2 writepdb dna1.pdb
$sel3 writepdb dna2.pdb
```

**build2.tcl:**


```bash
# 1) Load psfgen-plugin and CHARMM topology file
package require psfgen
resetpsf
topology ../toppar/top_all36_prot.rtf
topology ../toppar/top_all36_na.rtf

# 2) Define a segment name for each chain
segment PROA {pdb proa.pdb}
segment DNA1 {first 5TER; last 3TER; pdb dna1.pdb}
segment DNA2 {first 5TER; last 3TER; pdb dna2.pdb}

# 3) Apply patches to convert RNA to DNA
patch DEO5 DNA1:1
for {set i 2} {$i <= 14} {incr i} {patch DEOX DNA1:$i}
patch DEO5 DNA2:1
for {set i 2} {$i <= 14} {incr i} {patch DEOX DNA2:$i}
regenerate angles dihedrals

# 4) Read the coordinates of the atoms in each chain
coordpdb proa.pdb PROA
coordpdb dna1.pdb DNA1
coordpdb dna2.pdb DNA2
```

##  Appendix 

The following is a command to calculate the total net charge of the
system in the PSF file.


```bash
awk '/NATOM/,/NBOND/' system.psf | awk '{sum+=$7} END {print sum}'
```

------------------------------------------------------------------------

*Written by Takaharu Mori@RIKEN Theoretical molecular science laboratory
(Original Japanese document)\
Translated by Ai Shinobu@RIKEN Computational Biophysics Research Team\
March 23, 2022*
{: .notice}
