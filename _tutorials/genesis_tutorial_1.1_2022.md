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


First, let’s create a working directory for the GENESIS tutorials under your
home directory (`/home/username/`) by executing the following commands. We will
set up a main directory named `GENESIS_Tutorials-2022`, and inside it, create two
subdirectories: `Programs` and `Data`.
- `Programs` will store the main software used in this project, such as the GENESIS executable.
- `Data` will store external resources that are commonly used throughout the tutorials, including PDB files (see [Tutorial 2.1](/tutorials/genesis_tutorial_2.1_2022/)) and force field parameter files (see [Tutorials 2.2](/tutorials/genesis_tutorial_2.2_2022/)).

Therefore, within the `Data` directory, we will also create two subdirectories:
`PDB` and `Parameters`.  

For downloading the actual tutorial materials, we will use Git to clone the
repository into a subdirectory named `genesis_tutorial_materials` (starting from
Tutorial 3 onward).

```bash
# Move to the home directory 
$ cd
$ pwd
/home/username

# Create a directory for this project
$ mkdir GENESIS_Tutorials-2022
$ cd GENESIS_Tutorials-2022

# Create main directories
$ mkdir Programs Data
$ git clone https://github.com/genesis-release-r-ccs/genesis_tutorial_materials
$ ls
Data  Programs  genesis_tutorial_materials

# Create sub-directories
$ mkdir Data/PDB
$ mkdir Data/Parameters
```

## Download GENESIS

In this tutorial, we use [GENESIS version 2.0.0](/docs/download/).
We put the GENESIS source code in `Programs`.

```bash
# Put the source code of GENESIS in the "Programs" directory 
$ cd Programs
$ git clone https://github.com/genesis-release-r-ccs/genesis.git
$ cd genesis
$ git checkout v2.0.0
```


## Install GENESIS


Now, let’s run the `configure` command. This script automatically checks your
computing environment and generates a `Makefile` with appropriate compilation
options for your system.

If you encounter any issues during this process, please refer to the GENESIS
user manual or visit the [Installation](/docs/installation/) guide for
troubleshooting tips.

Once configure completes successfully, compile and install the program using
`make` and `make install`.

This will create a `bin` directory in your current location and install the
compiled programs there. After installation, you will find several executables
in the `bin` directory. Among them, `atdyn` and `spdyn` are the main MD
programs. The rest are auxiliary tools for trajectory analysis.

Starting from Tutorial 3, we will use many of these tools throughout our
simulations and analyses.

```bash
# Install GENESIS
$ ./configure
$ make
$ make install
```

Here, we did not add any options to the `configure` command. This simple command
will install GENESIS running on a CPU. In fact, most tutorials are designed to
use the CPU version. If you want to run the tutorials using CPU+GPU, please add
the [options for GPU](/docs/installation/). However, note that in this case only
`spdyn` will be installed, and some tutorials cannot be run with `spdyn` alone.


In the example above, we did not specify any options with the `configure` command.
This default setup installs the CPU version of GENESIS, which is sufficient for
most of the tutorials in this series.

If you wish to run GENESIS using both CPU and GPU, please refer to the 
[GPU installation options](/docs/installation/) and add the appropriate flags 
during configuration.

**Note**: When using the GPU option, only the `spdyn` program will be installed.
Some tutorials in this series require the `atdyn` program and therefore cannot
be completed with `spdyn` alone. Be sure to choose the installation method that
matches your needs.
{: .notice--warning}


## Let's check the directory

Now, we have constructed the following directory structure:

```bash
/home/user
+ GENESIS_Tutorials-2022  # Project name
    |
    + Programs           # Main software for this project
    |  + genesis
    |     + src          # Source code of GENESIS 2.0.0
    |     + bin          # Binary code of GENESIS 2.0.0
    |     + doc          # User manual of GENESIS 2.0.0
    |     + tests        # Regression tests
    |     + ...
    |
    + Data               # External data of this project
    |  + PDB
    |  + Parameters
    |
    + genesis_tutorial_materials  # Simulations
    |  + tutorial_2.3
    |  + tutorial_3.1
    |  + tutorial_3.2
    |  + ...
```


### A Note on Installation and Directory Organization

Some users may wish to install programs in system-wide directories such as
`/usr/local/bin`. However, this approach can make it difficult to trace where
the original source code resides, especially in large collaborative projects.
To avoid such confusion, we recommend installing all software locally, within
the project directory.

In our tutorial, we created a `Data` directory to store all external input
files. This includes files such as PDB structures, force field parameters, and
other reference data. Keeping these files in a dedicated `Data` directory helps
to clearly distinguish input data from the results and ensures that you can
easily track what was used during simulations. In real research scenarios, this
could include unpublished PDB files provided by collaborators — and it’s
especially important to preserve their integrity. Storing them in a dedicated
location helps avoid accidental overwrites or modifications.


### Why Good Directory Hygiene Matters

Throughout this tutorial series, we will also introduce simple techniques to
keep your project directory clean and well-organized. In computational research,
large numbers of files are often generated automatically. Without a clear and
consistent directory structure, it's easy for things to get messy — leading to:

- Confusion and mistakes in simulation or analysis steps
- Difficulty in writing reports or publications
- Poor reproducibility and data traceability
- Frustration when handing the project off to other team members

A cluttered directory is not just inconvenient — it can also hinder your ability
to publish results effectively.

By following the practices shown in this tutorial, we hope you will develop good
habits for managing your research data and code, helping to maintain both
scientific rigor and collaborative efficiency.

---

*Written by Takaharu Mori@RIKEN Theoretical Molecular Science
Laboratory, Dec. 16, 2021* \\
*Edited by Cheng Tan@RIKEN Center for Computational Science, Jun. 05, 2025*
{: .notice}

