---
title: "GENESIS Tutorials 2022"
excerpt: "Tutorials of using GENESIS to run MD simulations and data analysis."
last_modified_at: 2025-05-28T11:59:26+09:00
layout: single
classes: wide
toc: false
sidebar:
  nav: sidebar-basic
---

Here, we provide basic, standard, and advanced MD tutorials for **GENESIS version 2.x**.

Before starting, please ensure that tools like
[VMD](http://www.ks.uiuc.edu/Research/vmd/), [gnuplot](http://www.gnuplot.info),
or python libraries like [numpy](https://numpy.org/) and
[matplotlib](https://matplotlib.org/) are installed on your computer. These
tools are required for visualizing MD trajectories and plotting output data,
respectively.

We strongly encourage all users — especially students and early-career
researchers — to begin with the "Level 1: Basic Tutorials" and go through all
chapters without skipping. After that, you can proceed to "Level 2: Standard MD
Tutorials" or "Level 3: Advanced MD Tutorials."

**Important**: Even if you plan to skip some Level 1 chapters, please complete at least
Chapters 1.1, 2.1, and 2.2. These chapters establish the common directory
structure and input files used in many of the later tutorials. Also, note that
Chapter 12 assumes that Chapter 3.2 has been completed.
{: .notice--info}

In the chapter list below, you will find icons such as:

- <i class="fas fa-laptop"></i> for laptops (≤ 4 CPU cores),
- <i class="fas fa-computer"></i> for workstations (~ 16 CPU cores),
- <i class="fas fa-network-wired"></i> for supercomputers (≥ 64 CPU cores).

These indicate the computational requirements of each tutorial. While some
chapters can be run on a personal laptop, others require more powerful resources
such as clusters or supercomputers. Please select tutorials based on your
available resources.



## Level 1: Basic tutorials 

- \\(1.\\) Getting started
    - [1.1 Installation of GENESIS for Tutorials](/tutorials/genesis_tutorial_1.1_2022/)
    - [1.2 Let's take a quick look at the source code of GENESIS](/tutorials/genesis_tutorial_1.2_2022/)
- \\(2.\\) Preparation of the input files for GENESIS
    - [2.1 3D structure of biological molecules](/tutorials/genesis_tutorial_2.1_2022/)
    - [2.2 Force field parameters of biological molecules](/tutorials/genesis_tutorial_2.2_2022/)
    - [2.3 Building the initial structure for MD simulation](/tutorials/genesis_tutorial_2.3_2022/)
- \\(3.\\) MD simulations of peptides and proteins with the all-atom CHARMM force field
    - [3.1 Ala-dipeptide in the gas-phase](/tutorials/genesis_tutorial_3.1_2022/) <i class="fas fa-laptop"></i>
    - [3.2 (Ala)<sub>3</sub> in water](/tutorials/genesis_tutorial_3.2_2022/) <i class="fas fa-computer"></i>
    - [3.3 Protein G in NaCl solution](/tutorials/genesis_tutorial_3.3_2022/) <i class="fas fa-computer"></i>
- \\(4.\\) Analysis of the MD trajectories
    - [4.1 Analysis of DCD file by the user's Fortran programming](/tutorials/genesis_tutorial_4.1_2022/)
    - [4.2 Statistical analysis of the trajectory data by Awk](/tutorials/genesis_tutorial_4.2_2022/)
    - [4.3 Statistical analysis of the trajectory data by Python](/tutorials/genesis_tutorial_4.3_2022/)
    - [4.4 How to make a simulation movie with PyMol?](/tutorials/genesis_tutorial_4.4_2022/)

## Level 2: Standard MD tutorials

- \\(5.\\) Preparation of the input files for various systems
    - [5.1 Creating input files of MD simulations with the CHARMM force field](/tutorials/genesis_tutorial_5.1_2022/)
    - [5.2 Creating input files of MD simulations with the AMBER force field](/tutorials/genesis_tutorial_5.2_2022/)
    - [5.3 Creating input files of MD simulations using the Gromacs input files](/tutorials/genesis_tutorial_5.3_2022/)
- \\(6.\\) MD simulations of various biomolecules with all-atom models
    - [6.1 POPC lipid bilayer](/tutorials/genesis_tutorial_6.1_2022/) <i class="fas fa-computer"></i>
    - [6.2 GPCR in a lipid bilayer](/tutorials/genesis_tutorial_6.2_2022/)
    - [6.3 N-glycan in water](/tutorials/genesis_tutorial_6.3_2022/)
    - [6.4 RNA in water](/tutorials/genesis_tutorial_6.4_2022/)
- \\(7.\\) MD simulations with the coarse-grained model
    - [7.1 Karanicolas-Brooks Gō model for the folding simulations of Protein-G](/tutorials/genesis_tutorial_7.1_2022/) <i class="fas fa-laptop"></i>
    - [7.2 Domain Motion Enhanced model (DoME) for the domain closure of Ribose Binding Protein](/tutorials/genesis_tutorial_7.2_2022/) <i class="fas fa-laptop"></i>
    - [7.3 Dual-basin Gō model for the open-to-close motions of Ribose Binding Protein](/tutorials/genesis_tutorial_7.3_2022/) <i class="fas fa-laptop"></i>
    - [7.4 All-atom Gō model for the molten globule simulation of RNase-H protein](/tutorials/genesis_tutorial_7.4_2022/) <i class="fas fa-laptop"></i>
- \\(8.\\) MD simulations with the implicit solvent model
    - [8.1 Protein G in the GB/SA model](/tutorials/genesis_tutorial_8.1_2022/) <i class="fas fa-laptop"></i>
    - [8.2 Translocator protein (TSPO) in the implicit membrane model](/tutorials/genesis_tutorial_8.2_2022/) <i class="fas fa-laptop"></i>
- \\(9.\\) MD simulations with various restraints
    - [9.1 Target MD and steered MD simulations of Ribose Binding Protein in water](/tutorials/genesis_tutorial_9.1_2022/)
    - [9.2 Restraint in the NPT ensemble](/tutorials/genesis_tutorial_9.2_2022/)


## Level 3: Advanced MD tutorials 

- \\(10.\\) Atomistic MD simulations using supercomputers and GPU clusters
    - [10.1 Acceleration of MD simulations using HMR and a longertime step](/tutorials/genesis_tutorial_10.1_2022/)
- \\(11.\\) Advanced MD simulations with the coarse-grained model
    - [11.1 Coarse-grained simulation of protein with AICG2+ model](/tutorials/genesis_tutorial_11.1_2022/) <i class="fas fa-laptop"></i> 
    - [11.2 Coarse-grained simulation of double-stranded DNA with 3PSC.N model](/tutorials/genesis_tutorial_11.2_2022/) <i class="fas fa-computer"></i>
    - [11.3 Coarse-grained simulation of protein-DNA interactions with PWMcos model](/tutorials/genesis_tutorial_11.3_2022/) <i class="fas fa-computer"></i>
    - [11.4 Coarse-grained simulation of FUS condensation with HPS model](/tutorials/genesis_tutorial_11.4_2022/) <i class="fas fa-computer"></i>
- \\(12.\\) Enhanced conformational sampling simulations of (Ala)\\(_3\\) in water
    - [12.1 Replica-exchange molecular dynamics (REMD)](/tutorials/genesis_tutorial_12.1_2022/) <i class="fas fa-network-wired"></i>
    - [12.2 Replica-exchange umbrella sampling (REUS)](/tutorials/genesis_tutorial_12.2_2022/) <i class="fas fa-network-wired"></i>
    - [12.3 Replica exchange with solute tempering (gREST)](/tutorials/genesis_tutorial_12.3_2022/) <i class="fas fa-network-wired"></i>
    - [12.4 Gaussian accelerated MD (GaMD)](/tutorials/genesis_tutorial_12.4_2022/) <i class="fas fa-computer"></i>
    - [12.5 Gaussian accelerated replica-exchange umbrella sampling (GaREUS)](/tutorials/genesis_tutorial_12.5_2022/) <i class="fas fa-network-wired"></i>
- \\(13.\\) Transition path sampling of biomolecules
    - [13.1 The mean-force string method simulations of (Ala)\\(_3\\) in water](/tutorials/genesis_tutorial_13.1_2022/) <i class="fas fa-computer"></i>
    - [13.2 The closed-to-open motions of Ribose Binding Protein (RBP) with the mean-force string method](/tutorials/genesis_tutorial_13.2_2022/) <i class="fas fa-network-wired"></i>
- \\(14.\\) Free energy perturbations
    - [14.1 Relative solvation free energy](/tutorials/genesis_tutorial_14.1_2022/) <i class="fas fa-computer"></i>
    - [14.2 Absolute solvation free energy](/tutorials/genesis_tutorial_14.2_2022/) <i class="fas fa-computer"></i>
    - [14.3 Relative protein-ligand binding affinity](/tutorials/genesis_tutorial_14.3_2022/) <i class="fas fa-network-wired"></i>
- \\(15.\\) QM/MM calculations
    - [15.1 What is QM/MM?](/tutorials/genesis_tutorial_15.1_2022/)
    - [15.2 My first QM/MM job](/tutorials/genesis_tutorial_15.2_2022/) <i class="fas fa-computer"></i>
    - [15.3 How to create a cluster system](/tutorials/genesis_tutorial_15.3_2022/) <i class="fas fa-laptop"></i>
    - [15.4 Vibrational analysis of phosphate ion in solution](/tutorials/genesis_tutorial_15.4_2022/) <i class="fas fa-computer"></i>
    - [15.5 The enzyme reaction 1: Reaction path search](/tutorials/genesis_tutorial_15.5_2022/) <i class="fas fa-network-wired"></i>
    - [15.6 The enzyme reaction 2: Free-energy calculation](/tutorials/genesis_tutorial_15.6_2022/) <i class="fas fa-network-wired"></i>
- \\(16.\\) Experimental data-driven simulations
    - [16.1 Cryo-EM flexible fitting for TPC2 channel](/tutorials/genesis_tutorial_16.1_2022/) <i class="fas fa-computer"></i>
    - [16.2 Cryo-EM flexible fitting with a multi-scale protocol](/tutorials/genesis_tutorial_16.2_2022/) <i class="fas fa-network-wired"></i>
    - [16.3 Cryo-EM flexible fitting refinement for *de novo* models](/tutorials/genesis_tutorial_16.3_2022/) <i class="fas fa-network-wired"></i>

## Appendix

- [How to analyze multiple DCD files in the trajectory analysis tools?](/tutorials/genesis_tutorial_appendix_1_2022/)
- [Atomistic MD simulations of biomolecules on GPU clusters](/tutorials/genesis_tutorial_appendix_2_2022/)
- [Atomistic MD simulations of biomolecules on Fugaku](/tutorials/genesis_tutorial_appendix_3_2022/)
- [REMD of protein G with implicit solvent or CG models](/tutorials/genesis_tutorial_appendix_4_2022/)
- [Automatic parameter tuning for REMD, REUS, REST](/tutorials/genesis_tutorial_appendix_5_2022/)
- [Removing ring penetrations and chirality errors](/tutorials/genesis_tutorial_appendix_6_2022/)

<!--
## Old tutorials

- [Tutorials for ver. 1.4-1.6](/tutorials/genesis_tutorial_2019/)
- [Tutorials for ver. 1.0-1.3](tutorials.md)

-->
