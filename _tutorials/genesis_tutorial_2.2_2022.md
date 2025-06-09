---
title: "GENESIS Tutorial 2.2 (2022)"
excerpt: ""
last_modified_at: 2025-06-03T00:00:56+09:00
layout: single
toc: true
toc_sticky: true
sidebar:
  nav: sidebar-basic
---

# Force field parameters of biological molecules

## What is needed to run simulations? 

In the MD simulation, potential energy of the system is calculated by

\\[
\begin{aligned}
U &= \sum_{\textrm{bonds}} k_b (r - r_0)^2 + \sum_{\textrm{angles}}k_a(\theta - \theta_0)^2 \\\\ 
&+ \sum_{\textrm{dihedrals}}\frac{V_n}{2}\left[ 1 + \cos(n\phi - \delta) \right] \\\\ 
&+ \sum_{i > j} 4\varepsilon \left[ \left(\frac{\sigma_{ij}}{r_{ij}}\right)^{12} - \left(\frac{\sigma_{ij}}{r_{ij}}\right)^{6} \right] + 
\sum_{i>j} \frac{q_i q_j}{r_{ij}}
\end{aligned}
\\]


This equation is commonly referred to as the force field. It includes many
empirical parameters, such as force constants (e.g. \\(k_b\\) and \\(k_a,\\) for
bonds and angles), equilibrium bond lengths (\\(r_0\\)), the depth of the
dihedral potential (\\(V_n\\)), atomic charges (\\(q\\)), and more. These
physical parameters are not stored in PDB files, nor are they typically bundled
with most MD simulation programs. Therefore, to perform molecular dynamics
simulations, the appropriate **force field parameters** must be prepared and
provided as input.

In addition, we need the topology of the target molecular system, which
describes how atoms are connected—i.e., which atoms are bonded covalently. This
information is essential for evaluating each term in the force field equation.
However, due to the complexity of molecular structures, most MD programs cannot
automatically infer topology from PDB coordinates alone. As a result, the
topology file must also be prepared before starting the simulation.


## Download the force field parameters 

One of the most widely used force fields for biomolecular simulations is CHARMM
[^1], originally developed by the Karplus group at Harvard University. Over the
years, the Mackerell group at the University of Maryland has played a leading
role in extending and maintaining the CHARMM force field parameters. You can
visit [the Mackerell group's website](http://mackerell.umaryland.edu/charmm_ff.shtml) 
to find and download the latest versions of these parameters.

![](/assets/images/2021_12_charmm_toppar_website.png)

As you can see, the CHARMM force field has been updated annually in July for the
past several years. Since this tutorial was written in December 2021, we will
use the `toppar_c36_jul21.tgz` archive, which contains the CHARMM C36m parameters
[^2] — the most recent version available at that time. We recommend *checking this
page regularly* in your research, as future updates may significantly improve the
accuracy and reliability of your MD simulations.


Let’s create a new `Data` directory, and within it, a `Parameters` subdirectory
to store the downloaded file. 

This directory structure will be used consistently throughout the remaining
sections of the tutorial.  You may choose a different location if you prefer,
but please remember to update the corresponding paths accordingly in later
steps.
{: .notice--info}

```bash
# Move to the directory where save the parameter files
$ cd ~/GENESIS_Tutorials-2022
$ mkdir -p Data/Parameters
$ cd Data/Parameters

$ mv ~/Downloads/toppar_c36_jul21.tgz ./
$ tar -zxvf toppar_c36_jul21.tgz
$ ls
toppar  toppar_c36_jul21  toppar_c36_jul21.tgz
```

The `toppar_c36_jul21` directory contains many files. In this section, we will
introduce some of the most important and frequently used ones.

Files with the `.prm` extension are parameter files, while those with `.rtf` are
residue topology files.

The prefixes in the filenames indicate their target molecule types:
- `prot` – protein (amino acid residues)
- `na` – nucleic acids
- `lipid` – lipids
- `all` – all-atom models

For example:
- `par_all36m_prot.prm` contains the CHARMM C36m force field parameters for proteins in the all-atom model.
- `top_all36_prot.rtf` provides the topology information for proteins, used in both CHARMM C36 and C36m.
Note that `top_all36_prot.rtf` is shared by both CHARMM C36 and C36m versions.


```bash
# Check the contents 
$ cd toppar_c36_jul21
$ ls
00toppar_file_format.txt  par_all36_cgenff.prm       top_all35_ethers.rtf
ace                       par_all36_lipid.prm        top_all36_carb.rtf
cheq                      par_all36_lipid_ljpme.prm  top_all36_cgenff.rtf
drude                     par_all36_na.prm           top_all36_lipid.rtf
gbsw                      par_all36m_prot.prm        top_all36_lipid_ljpme.rtf
larmord                   par_hbond.inp              top_all36_na.rtf
metals                    param19.inp                top_all36_prot.rtf
non_charmm                rush                       toph19.inp
openmm_gbsaobc2           silicates                  toppar_all.history
par_all22_prot.prm        stream                     toppar_water_ions.str
par_all35_ethers.prm      tamdfff
par_all36_carb.prm        top_all22_prot.rtf
```

## What is contained in the parameter and topology files?

Let’s take a look at the contents of `par_all36m_prot.prm`.  The example below
shows a portion of the parameters for the bond energy term.  In this section,
you can find the bond force constants (3rd column) and equilibrium bond lengths
(4th column) for each atom type or atom pair in amino acids.  

Try browsing the file further to locate parameters for other energy terms, such as:
angle terms, dihedral angles, and Van der Waals interactions, etc.

```bash
# Take a look at the parameter file for proteins
$ less par_all36m_prot.prm 

:
BONDS
!
!V(bond) = Kb(b - b0)**2
!
!Kb: kcal/mole/A**2
!b0: A
:
CA   CAI   305.000     1.3750 ! from CA CA
CAI  CAI   305.000     1.3750 ! atm, methylindole, fit CCDSS
CPT  CA    300.000     1.3600 ! atm, methylindole, fit CCDSS
CPT  CAI   300.000     1.3600 ! atm, methylindole, fit CCDSS
CPT  CPT   360.000     1.3850 ! atm, methylindole, fit CCDSS
:
```

For detailed description of the parameter files, refer to the [CHARMM manual](https://www.charmm.org/archive/charmm/documentation/)
([parmfile](https://www.charmm.org/archive/charmm/documentation/by-version/c42b1/params/doc/parmfile/)).

Next, let’s examine the contents of `top_all36_prot.rtf`.
This file primarily defines the topology, mass, and partial charges of amino acids.

Below is an example of how alanine (ALA) is defined:
- Lines beginning with `MASS` specify the atomic mass.
- Lines starting with `BOND` describe the covalent bond connectivity between atoms—each adjacent atom pair indicates a covalent bond.
- The partial charge of each atom is provided in the fourth column. For instance, the hydrogen atom has a partial charge of `+0.09 e`.

For a more detailed explanation of the topology file format, refer to the
[CHARMM manual](https://www.charmm.org/archive/charmm/documentation/)
and the [rtop](https://www.charmm.org/archive/charmm/documentation/by-version/c42b1/params/doc/rtop/) documentation.


```bash
# Take a look at the topology file for proteins
$ less top_all36_prot.rtf

:
MASS  -1  CE1       12.01100 ! for alkene; RHC=CR
MASS  -1  CE2       12.01100 ! for alkene; H2C=CR
MASS  -1  CAI       12.01100 ! aromatic C next to CPT in trp
MASS  -1  C3        12.01100 ! cyclopropane carbon
MASS  -1  N         14.00700 ! proline N
MASS  -1  NR1       14.00700 ! neutral his protonated ring nitrogen
:
:
RESI ALA          0.00
GROUP   
ATOM N    NH1    -0.47  !     |
ATOM HN   H       0.31  !  HN-N
ATOM CA   CT1     0.07  !     |     HB1
ATOM HA   HB1     0.09  !     |    /
GROUP                   !  HA-CA--CB-HB2
ATOM CB   CT3    -0.27  !     |    \
ATOM HB1  HA3     0.09  !     |     HB3
ATOM HB2  HA3     0.09  !   O=C
ATOM HB3  HA3     0.09  !     |
GROUP                   !
ATOM C    C       0.51
ATOM O    O      -0.51
BOND CB CA  N  HN  N  CA  
BOND C  CA  C  +N  CA HA  CB HB1  CB HB2  CB HB3 
DOUBLE O  C
:
```

We can also find several `.str` files in the directory. These are called stream
files, which combine both topology and parameter definitions. For example,
`toppar_water_ions.str` contains the necessary topology and parameter information
for water molecules and ions.

```bash
# Take a look at the stream file for water and ions
$ less toppar_water_ions.str
```

In our tutorials, we will primarily use the following three files:
`par_all36m_prot.prm`, `top_all36_prot.rtf`, and `toppar_water_ions.str`. These
are essential for performing MD simulations of proteins in water using the
CHARMM C36m force field. For simulations involving membrane proteins, two
additional files—`par_all36_lipid.prm` and `top_all36_lipid.rtf`—are required,
as they provide the necessary parameters and topology for lipid molecules.


## Let's clean up the directory 

Now, let’s do a short exercise to organize the files and directories. In the
`Parameters` directory, you may find three items: `toppar`, `toppar_c36_jul21`,
and `toppar_c36_jul21.tgz`.

- `toppar_c36_jul21.tgz` is the original archive downloaded from the CHARMM website. While important as a backup, it is no longer needed after extraction.
- `toppar` is likely an extra file or directory mistakenly included during compression and can be safely ignored.

Rather than deleting them outright with the `rm` command (which should always be
used with caution), we recommend creating a directory called `TRASH` and moving
these unnecessary files there.

This approach helps keep the working directory tidy while preserving files just
in case you need them later. Of course, if disk space is a concern and you are
confident they are no longer needed, feel free to delete them.

```bash
# Find unnecessary files
$ cd ../
$ ls
toppar  toppar_c36_jul21  toppar_c36_jul21.tgz

# Clean up the directory
$ mkdir TRASH
$ mv toppar toppar_c36_jul21.tgz ./TRASH
$ ls
toppar_c36_jul21  TRASH
```

Now that the CHARMM force field parameter files are ready, we have the following directory structure prepared:

```bash
/home/user
       + GENESIS_Tutorials-2022      # Project name
            |
            + Programs               # Main software for this project
            |  + Source
            |  + genesis-2.0.0
            |     + src              # Source code of GENESIS 2.0.0
            |     + bin              # Binary code of GENESIS 2.0.0
            |
            + Data                   # External data of this project
            |  + PDB
            |  + Parameters
            |     + toppar_c36_jul21
            |     + TRASH
            |
            + Works                  # All simulations will be done here
               + TRASH
```

In the next Tutorial, we will learn how to setup the initial structure of the
target system using the topology file.

---

*Written by Takaharu Mori@RIKEN Theoretical molecular science
laboratory, Dec. 17, 2021*
{: .notice}


##  References

[^1]: A. D. MacKerell, Jr. *et al.*, *J. Phys. Chem. B*, **102**, 3586 (1998). [<i class="fas fa-link"></i>](https://pubs.acs.org/doi/10.1021/jp973084f)
[^2]: J. Huang *et al.*, *Nat. Methods*, **14**, 71-73 (2017). [<i class="fas fa-link"></i>](https://www.nature.com/articles/nmeth.4067)


