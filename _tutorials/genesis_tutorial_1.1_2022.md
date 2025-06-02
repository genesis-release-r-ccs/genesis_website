---
title: "GENESIS Tutorial 1.1 (2022)"
excerpt: ""
last_modified_at: 2025-05-13T11:59:26+09:00
layout: single
toc: true
toc_sticky: true
sidebar:
  nav: sidebar-basic
---

# Installation of GENESIS for Tutorials

## Preparation of the directory

First of all, let's create a directory for the GENESIS tutorials under the home
directory (`/home/user`) by executing the following commands. We create a main
directory `GENESIS_Tutorials-2022`, followed by three sub-directories
`Programs`, `Data`, and `Works`.  `Programs` is where we will put the
main software used in this project, i.e. the GENESIS program. `Data` is where
we will put external data that will be commonly used throughout this project,
such as PDB files (see [Tutorial 2.1](/tutorials/genesis_tutorial_2.1_2022/) and force
field parameter files (see [Tutorials 2.2](/tutorials/genesis_tutorial_2.2_2022/).
Thus, we would like to make two sub-directories `PDB` and `Parameters` under
`Data` beforehand. `Works` is where we will run the MD simulation of the
tutorial (see Tutorial 3 and later). Here, we also make a `TRASH` directory
under `Works`. The role of `TRASH` will be explained later.

```bash
# Move to the home directory 
$ cd
$ pwd
/home/user

# Create a directory for this project
$ mkdir GENESIS_Tutorials-2022
$ cd GENESIS_Tutorials-2022

# Create main directories
$ mkdir Programs Data Works
$ ls
Data  Programs  Works

# Create sub-directories
$ mkdir Data/PDB
$ mkdir Data/Parameters
$ mkdir Works/TRASH
```

## Download GENESIS

In this tutorial, we use [GENESIS version 2.0.0](/docs/download/).
We put the GENESIS source code in `Programs`.

```bash
# Put the source code of GENESIS in the "Programs" directory 
$ cd Programs
$ mv ~/Downloads/genesis-2.0.0.tar.bz2 ./ 
$ bzip2 -d genesis-2.0.0.tar.bz2 
$ tar -xvf genesis-2.0.0.tar 
$ cd genesis-2.0.0
$ ls
AUTHORS         INSTALL      README      configure.ac  missing
COPYING         Makefile.am  aclocal.m4  depcomp       src
COPYING.LESSER  Makefile.in  compile     fortdep.py
ChangeLog       NEWS         configure   install-sh
```

## Install GENESIS

Let's run the configure command. This `configure` is a script that checks your
computing environment, and automatically create a `Makefile` that includes
suitable compile options. If you have some troubles, please check the GENESIS
user manual or [Installation](/docs/installation/) page. Next, run the `make`
and `make install` commands, which will create a `bin` directory in the
current directory, and install the program there. After the compilation, you
will find many programs in the `bin` directory. Here, `atdyn` and `spdyn` are
the MD programs, and the others are mostly trajectory analysis tools. In
Tutorial 3 and later, we will run each of these programs.

```bash
# Install GENESIS
$ ./configure
$ ls
AUTHORS         Makefile     aclocal.m4     configure.ac  src
COPYING         Makefile.am  compile        depcomp
COPYING.LESSER  Makefile.in  config.log     fortdep.py
ChangeLog       NEWS         config.status  install-sh
INSTALL         README       configure      missing

$ make
$ make install
$ ls
AUTHORS         Makefile     aclocal.m4     configure     missing
COPYING         Makefile.am  bin            configure.ac  src
COPYING.LESSER  Makefile.in  compile        depcomp
ChangeLog       NEWS         config.log     fortdep.py
INSTALL         README       config.status  install-sh

# Check the contents
$ ls ./bin
atdyn               hb_analysis          qval_residcg_analysis
avecrd_analysis     hbond_analysis       rdf_analysis
cg_convert          kmeans_clustering    remd_convert
comcrd_analysis     lipidthick_analysis  rg_analysis
contact_analysis    mbar_analysis        ring_analysis
crd_convert         meanforce_analysis   rmsd_analysis
density_analysis    morph_generator      rpath_generator
diffusion_analysis  msd_analysis         rst_convert
distmat_analysis    pathcv_analysis      rst_upgrade
drms_analysis       pcavec_drawer        sasa_analysis
dssp_interface      pcrd_convert         spdyn
eigmat_analysis     pmf_analysis         tilt_analysis
emmap_generator     prjcrd_analysis      trj_analysis
flccrd_analysis     qmmm_generator       wham_analysis
fret_analysis       qval_analysis
```

Here, we did not add any options to the `configure` command. This simple command
will install GENESIS running on a CPU. In fact, most tutorials are designed to
use the CPU version. If you want to run the tutorials using CPU+GPU, please add
the [options for GPU](/docs/installation/). However, note that in this case only
`spdyn` will be installed, and some tutorials cannot be run with `spdyn`
alone.

## Let's clean up the directory

Now, let's do a short exercise in organizing the files and
directories. Please take a look inside
the `Programs` directory. We have `genesis-2.0.0.tar` and
`genesis-2.0.0`. In fact, we don't need the `genesis-2.0.0.tar` anymore,
and we can delete it with the `rm` command. Here, instead of deleting
the file, we would like to make a `Source` directory, compress the tar
file, and keep it there. This is because we can quickly reinstall the
program without downloading it again, and also we can install other
software in the `Programs` directory later.

```bash
# Find unnecessary files
$ cd ../
$ ls
genesis-2.0.0  genesis-2.0.0.tar

# Clean up the directory
$ mkdir Source
$ gzip genesis-2.0.0.tar
$ mv genesis-2.0.0.tar.gz Source
$ ls
Source  genesis-2.0.0
```

Now, we have constructed the following directory structure for this
project.

```
/home/user
+ GENESIS_Tutorials-2022  # Project name
    |
    + Programs           # Main software for this project
    |  + Source
    |  + genesis-2.0.0
    |     + src          # Source code of GENESIS 2.0.0
    |     + bin          # Binary code of GENESIS 2.0.0
    |
    + Data               # External data of this project
    |  + PDB
    |  + Parameters
    |
    + Works              # All simulations will be done here
        + TRASH
```

Some people may want to install programs in a common directory, such as
`/usr/local/bin`. However, this can make it difficult to know where the original
source code is. To avoid such situation, we would like to install the main
programs locally, i.e., under the project's directory.

 We made the `Data` directory to put external data, which allows us to easily
 understand what were used as the input data in the project. In the actual
 research project, such data might include unpublished PDB files you received
 from an experimental group. The external data should always maintain the
 integrity. In order to avoid accidental update of the original external data,
 we would like to keep them in a specific directory, namely, `Data` directory.
 

As demonstrated above, in our tutorial we will also show you some techniques for
cleaning up directories.  In computational research, many files and directories
are generated or created. If you do not carefully organize the directory
structure in your research, the files and directories can become cluttered or
messy, causing mistakes, affecting the research efficiency, report writing,
publication, and data management. Such cluttered directories may also hinder a
smooth handover of the project to other members of the lab, which in turn leads
to the loss of your chance to increase the number of publications. We hope that
our tutorial will also inspire you to get into a good habit of cleaning up the
directories in your actual research project. 

------------------------------------------------------------------------

*Written by Takaharu Mori@RIKEN Theoretical molecular science
laboratory\
Dec. 16, 2021*

