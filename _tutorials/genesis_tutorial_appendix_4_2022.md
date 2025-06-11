---
title: "GENESIS Tutorial Appendix 4 (2022)"
excerpt: ""
last_modified_at: 2025-06-03T00:00:57+09:00
layout: single
toc: true
toc_sticky: true
sidebar:
  nav: sidebar-basic
---

# Appendix 4. REMD of protein G with implicit solvent or CG models

Although REMD is a powerful tool to overcome energy barriers in MD
sampling, sometimes people want to accelerate further, by decreasing the
degree of freedom of the target system. One way is to keep the atomistic
resolution but treating all the solvent molecules implicitly. GENESIS
can perform simulations with the GB/SA implicit solvent model, please
see [Tutorial 8.1](/tutorials/genesis_tutorial_8.1_2022/). An alternative way is to
use a coarse-grained (CG) representation of all the biomolecules,
including water and ions. GENESIS also supports simulations with such CG
models, including MARTINI and SPICA (tutorials in preparation).
Residue-level CG models embody both two features (lower resolution and implicit solvent). Please refer to Tutorials
[11.1](/tutorials/genesis_tutorial_11.1_2022/), [11.2](/tutorials/genesis_tutorial_11.2_2022/),
[11.3](/tutorials/genesis_tutorial_11.3_2022/), and [11.4](/tutorials/genesis_tutorial_11.4_2022/) to
learn how to run simulations with these CG models in GENESIS.

Here, we show a simple example of running REMD with the residue-level CG
models. Generally speaking, there is no big difference between CG REMD
and all-atom REMD simulations. Therefore, we will briefly describe the
main steps and focus on the distinctions between CG and all-atom
simulations.

This example is to simulate the folding/unfolding dynamics of protein G.
In principle, for such a simple CG system, a constant temperature run is
enough to capture the folding/unfolding events, as shown in [Tutorial 11.1](/tutorials/genesis_tutorial_11.1_2022/).
Nevertheless, we think this system is good to illustrate how to carry
out REMD simulations in couple with the residue-level CG models. You can
also compare the results of this section with those from [Tutorial 11.1](/tutorials/genesis_tutorial_11.1_2022/).

## 0. Preparations 

### 0.1 Install necessary software 

As introduced in [Tutorial 11.1](/tutorials/genesis_tutorial_11.1_2022/), we use
the [GENESIS-CG-tool](https://github.com/noinil/genesis_cg_tool) to generate CG
MD files. Please install dependencies (programming language Julia and some packages) to run this tool.

### 0.2 Download the files for this tutorial 

All the files required for this tutorial are hosted in the
[GENESIS tutorials repository on Github](https://github.com/genesis-release-r-ccs/genesis_tutorial_materials)

If you haven't downloaded the files yet, open your terminal and run the following command:

```bash
$ git clone https://github.com/genesis-release-r-ccs/genesis_tutorial_materials
```

This tutorial consists of three parts: 1) system setup, 2)REMD simulations, and 3) trajectory analysis.

```bash
$ cd genesis_tutorial_materials/tutorial-A.4
$ ls
01_build 02_REMD 03_analysis
```
## 1. Setup

You can go through section 0 and 1 of [Tutorial 11.1](/tutorials/genesis_tutorial_11.1_2022/) 
to prepare the topology and coordinate files for the following simulations.

Basically, we execute the following commands to create the necessary
files (`1PGB_cg.top`, `1PGB_cg.itp`, and `1PGB_cg.gro`):

```bash
# download the PDB file (PDB code 1PGB)
$ cd 01_build
$ wget https://files.rcsb.org/download/1PGB.pdb
$ /home/user/genesis_cg_tool/src/aa_2_cg.jl 1PGB.pdb  
$ ls  
1PGB.pdb 1PGB_cg.gro 1PGB_cg.itp 1PGB_cg.top
```
## 2. REMD Simulations

Since we are going to start from the native structure (global energy minimum), we don't perform energy minimization and equilibration runs in
this tutorial. However, generally speaking, it is highly recommended to
go through those steps.

Now let's change to the REMD directory and take a look at the files:

```bash
$ cd ../02_REMD
$ ls
remd.inp  param/
```

In the `param/` directory there are standard parameter files for the
CG models. The file `remd.inp` is the control file that contains the
information for GENESIS to perform the REMD simulations:

```toml
[INPUT] 
grotopfile = 1PGB_cg.top
grocrdfile = 1PGB_cg.gro

[OUTPUT] 
pdbfile    = gb1_remd_run_rep{}.pdb
dcdfile    = gb1_remd_run_rep{}.dcd
rstfile    = gb1_remd_run_rep{}.rst
remfile    = gb1_remd_run_rep{}.rem
logfile    = gb1_remd_run_rep{}.log

[ENERGY] 
forcefield    = RESIDCG
electrostatic = CUTOFF

[DYNAMICS] 
integrator      = VVER_CG
nsteps          = 100000000
timestep        = 0.010
rstout_period   = 100000
crdout_period   = 1000
eneout_period   = 1000
nbupdate_period = 20

[CONSTRAINTS] 
rigid_bond = NO

[ENSEMBLE] 
ensemble    = NVT
tpcontrol   = LANGEVIN
temperature = 320
gamma_t     = 0.01

[BOUNDARY] 
type = PBC
box_size_x = 300.0
box_size_y = 300.0
box_size_z = 300.0

[REMD] 
dimension       = 1
exchange_period = 5000
type1           = temperature
nreplica1       = 16
parameters1     = 320.00 \
                  330.00 \
                  340.00 \
                  350.00 \
                  360.00 \
                  370.00 \
                  380.00 \
                  390.00 \
                  400.00 \
                  410.00 \
                  420.00 \
                  430.00 \
                  440.00 \
                  450.00 \
                  460.00 \
                  470.00
```

Please refer to [Tutorial 12.1](/tutorials/genesis_tutorial_12.1_2022/) for the
detailed explanations of the directives, especially those in the
`[REMD]` block. The most important thing is to choose the temperature
range and the number of replicas, which is dependent on the a priori
knowledge. Based on the results of [Tutorial 11.1](/tutorials/genesis_tutorial_11.1_2022/), we have a
good guess of the "folding temperature" (\\(T\_f\\), defined as the temperature where folded and unfolded states have equal probabilities), which is around 400 K. Therefore, here we want
to use a range of temperatures to cover 400 K
(\\(\lbrack 0.8T\_{f},1.2T\_{f}\rbrack\\)).
To determine the number of replicas, one way is to first run simulations
at constant temperatures and estimate the overlap of energy
distributions at the neighboring temperatures. Here we choose an
interval of 10 K. As will be shown later in this tutorial, this choice
produce a good energy overlap.

```bash
# copy necessary files
$ cp ../01_build/1PGB_cg.* .

# Run REMD production 
$ export OMP_NUM_THREADS=2
$ mpirun -np 16 /home/user/GENESIS/bin/atdyn remd.inp | tee remd_master.log
```

On a workstation with 32 CPU cores, it takes around 1.5 hours to finish
the simulation.

## 3. Analysis

In this tutorial, we aim at calculating the potential-of-mean-force
(PMF) of the Q-value (nativeness) at 400 K. Same as [Tutorial 12.1](/tutorials/genesis_tutorial_12.1_2022/), 
we are going to utilize the MBAR re-weighting method.

```bash
$ cd ../03_analysis
$ ls
1_calc_ratio 2_plot_index 3_sort_dcd 4_plot_potential 5_q_val 6_MBAR 7_PMF
```

### 3.1 Calculate the acceptance ratio 

Let's first calculate the acceptance ratio to get an estimation of the
REMD efficiency:

```bash
# change directory
$ cd 1_calc_ratio
$ ls 
$ calc_ratio.sh
# make the file executable and use it
$ chmod u+x calc_ratio.sh
$ ./calc_ratio.sh
1 > 2 0.7788
3 > 4 0.7764
5 > 6 0.7709
7 > 8 0.688
9 > 10 0.4993
11 > 12 0.7869
13 > 14 0.8171
15 > 16 0.831
```

As can be seen, almost all the acceptance ratio are above 0.5, showing
quite high-efficient exchange between replicas.

### 3.2 Plot time courses of replica indices and temperatures 

We next examine the "random-walk" of each replica in the temperature
space by plotting the time courses of replica indices (for each temperature) and temperatures (for each replica).

```bash
# change directory
$ cd ../2_plot_index
$ ls
01_extract_index_temperature_info.sh 02.1_plot_rep_exchange.py 02.2_plot_temperature_exchange.py
```

The first file `01_extract_index_temperature_info.sh` gets replica IDs
and temperatures from the "master" log file. Its content is like this:

```bash
#!/bin/bash -f

# get replica IDs in each snapshot
grep "ParmIDtoRepID:" ../../02_REMD/remd_master.log | sed 's/ParmIDtoRepID://' > T-REMD_parmID-repID.dat

# get replica temperatures in each snapshot
grep "Parameter :" ../../02_REMD/remd_master.log | sed 's/Parameter ://' > T-REMD_repID-Temperature.dat

# get step number and combine to replica index
grep "REMD> Step:" ../../02_REMD/remd_master.log | cut -c 12-25 > step.log

# adding simulation steps in the beginning
paste step.log T-REMD_parmID-repID.dat > T-REMD_parmID-repID.log
paste step.log T-REMD_repID-Temperature.dat > T-REMD_repID-Temperature.log

# remove tmp files
rm -f T-REMD_parmID-repID.dat T-REMD_repID-Temperature.dat step.log
```

We then use two python scripts to plot the time series of repclia IDs
and temperatures. If you are not familiar with python and `matplotlib`,
we provide a simple introduction to data analysis with python
in [tutorial 4.3](/tutorials/genesis_tutorial_4.3_2022/).

Content of `02.1_plot_rep_exchange.py`:

```python
#!/usr/bin/env python3

import numpy as np
import matplotlib.pyplot as plt

parm_rep_data = np.loadtxt("./T-REMD_parmID-repID.log")

N_REP = 16 # total number of replicas
n_row = 4 # number of subplot rows
n_col = 4 # number of subplot columns

fig, axes = plt.subplots(n_row, n_col, figsize=(n_col * 4, n_row * 3), constrained_layout=True, sharex=False, sharey=False)
X = parm_rep_data[:, 0] # common X-axis data: time step
for i_ax in range(N_REP):
    m, n = i_ax // n_col, i_ax % n_col
    Y = parm_rep_data[:, i_ax + 1] # Y-axis data: repID
    axes[m, n].plot(X, Y, ".", c="red")
    axes[m, n].set_xticks([20000000 * j for j in range(6)])
    axes[m, n].set_xticklabels([0, 2, 4, 6, 8, 10], fontsize=12)
    axes[m, n].set_xlim(0, 1e8)
    axes[m, n].set_xlabel(r"MD steps ($\times 10^7$)", fontsize=16)
    axes[m, n].set_yticks([j for j in range(1, 17)])
    axes[m, n].set_yticklabels(["", "", "", "4", "", "", "", "8", "", "", "", "12", "", "", "", "16"], fontsize=12)
    axes[m, n].set_ylim(0, 17)
    axes[m, n].set_ylabel("Replica ID", fontsize=16)
    title_text = "Temperature = {0} K".format(i_ax * 10 + 320)
    axes[m, n].set_title(title_text, fontsize=16)

plt.savefig("Replica_exchange_all.png")

```

Content of `02.2_plot_temperature_exchange.py`:

```python
#!/usr/bin/env python3

import numpy as np
import matplotlib.pyplot as plt

rep_parm_data = np.loadtxt("./T-REMD_repID-Temperature.log")

N_REP = 16 # total number of replicas
n_row = 4 # number of subplot rows
n_col = 4 # number of subplot columns

fig, axes = plt.subplots(n_row, n_col, figsize=(n_col * 4, n_row * 3), constrained_layout=True, sharex=False, sharey=False)
X = rep_parm_data[:, 0] # common X-axis data: time step
for i_ax in range(N_REP):
    m, n = i_ax // n_col, i_ax % n_col
    Y = rep_parm_data[:, i_ax + 1] # Y-axis data: repID
    color_ax = [i_ax / N_REP, 1, 1 - i_ax / N_REP]
    axes[m, n].plot(X, Y, "-", c=color_ax)
    axes[m, n].set_xticks([20000000 * j for j in range(6)])
    axes[m, n].set_xticklabels([0, 2, 4, 6, 8, 10], fontsize=12)
    axes[m, n].set_xlim(0, 1e8)
    axes[m, n].set_xlabel(r"MD steps ($\times 10^7$)", fontsize=16)
    axes[m, n].set_yticks([j * 10 + 320 for j in range(16)])
    axes[m, n].set_yticklabels(["320", "", "", "", "360", "", "", "", "400", "", "", "", "440", "", "", ""], fontsize=12)
    axes[m, n].set_ylim(315, 475)
    axes[m, n].set_ylabel("Temperature (K)", fontsize=16)
    title_text = "Replica = {0}".format(i_ax + 1)
    axes[m, n].set_title(title_text, fontsize=16)

plt.savefig("Temperature_exchange_all.png")
```

Now, let's execute these scripts:

```bash
$ ./01_extract_index_temperature_info.sh
$ ./02.1_plot_rep_exchange.py
$ ./02.2_plot_temperature_exchange.py
```

These commands will create two text files
(`T-REMD_parmID-repID.log` and `T-REMD_repID-Temperature.log`) and two
figures (`Replica_exchange_all.png` and `Temperature_exchange_all.png`).

![](/assets/images/2022_06_tutorial_A4_Replica_exchange_all.png){: style="width:100%;" .align-center}
<!-- {: style="width:100%; display:block; margin:0 auto;"} -->

This figure shows that each temperature visits every replica randomly.

![](/assets/images/2022_06_tutorial_A4_Temperature_exchange_all.png){: style="width:100%;" .align-center}
<!-- {: style="width:100%; display:block; margin:0 auto;"} -->

This figure shows the random walk of each replica in the temperature
space.

### 3.3 Sort coordinates in DCD trajectory files by parameters 

We next use GENESIS analysis tool `remd_convert` to sort the frames in
the trajectories based on their temperature.

```bash
# change directory
$ cd ../3_sort
$ ls
log_convert.inp remd_convert.inp

# Sort frames by parameters
$ /home/user/GENESIS/bin/remd_convert log_convert.inp | tee log_convert.log
$ /home/user/GENESIS/bin/remd_convert remd_convert.inp | tee remd_convert.log
```

### 3.4 Plot potential energy distribution for each temperature 

Now we have got all the log files sorted and will next plot potential
energy distributions to make sure that there are sufficient overlaps in
energy between simulations at different temperatures.

```bash
# change directory
$ cd ../4_plot_potential
$ ls
01_extract_potential_energy.sh 02_plot_potential_energy_temperature.py
```

The script `01_extract_potential_energy.sh` get potential energies from
simulations at each temperature:

```bash
#!/bin/bash

# get potential energy of each replica
for i in {1..16}
do
    grep "INFO:" ../3_sort_dcd/remd_paramID$i.log | tail -n +2 | awk '{print $5}' > potential_energy_rep$i.dat
done

# insert step number to the beginning of each line
grep "INFO:" ../3_sort_dcd/remd_paramID1.log | tail -n +2 | awk '{print $2}' > step.log
for i in {1..16}
do
    paste step.log potential_energy_rep$i.dat > potential_energy_rep$i.pot
done

# cleaning
rm -f potential*.dat step.log
```

The python script `02_plot_potential_energy_temperature.py` plot the
energy distributions at each temperature:

```python
#!/usr/bin/env python3

import numpy as np
import matplotlib.pyplot as plt

N_REP = 16 # total number of replicas

fig, axes = plt.subplots(1, 1, figsize=(6, 4), constrained_layout=True, sharex=False, sharey=False)
for i_rep in range(N_REP):
    potential_data_fname = "./potential_energy_rep{0}.pot".format(i_rep + 1)
    pot_energy = np.loadtxt(potential_data_fname, usecols=(1))

    color_tmp = [i_rep / N_REP, 1 - i_rep / N_REP, 0]
    hist, edge = np.histogram(pot_energy, bins=50, density=True)
    X = 0.5 * (edge[:-1] + edge[1:])
    axes.plot(X, hist, "-", color=color_tmp)

    axes.set_xticks([-300 + 50 * j for j in range(6)])
    axes.set_xticklabels([-300 + 50 * j for j in range(6)], fontsize=12)
    axes.set_xlim(-310, -40)
    axes.set_xlabel(r"Potential Energy ($kcal/mol$)", fontsize=16)
    axes.set_yticks([0.02 * j for j in range(4)])
    axes.set_yticklabels(["0", "0.02", "0.04", "0.06"], fontsize=12)
    axes.set_ylim(0, 0.07)
    axes.set_ylabel("Probability", fontsize=16)

plt.savefig("potential_energy_temperature.png")
```

![](/assets/images/2022_06_tutorial_A4_potential_energy_temperature.png){: style="width:60%;" .align-center}
<!-- style="width:60%; display:block; margin:0 auto;"} -->

This figure shows the potential energy distribution at each temperature
(indicated by the colors).

### 3.5 Calculate the Q values 

We use the GENESIS analysis tool `qval_residcg_analysis` to calculate
the nativeness (Q-value) of each simulated protein structure.

```bash
# change directory
$ cd ../5_q_val
$ ls
05_qval_residcg_analysis.inp 05_calculate_qval.sh
```

Please refer to [Tutorial 11.1](/tutorials/genesis_tutorial_11.1_2022/) for the
explanation of the control file `05_qval_residcg_analysis.inp`.

Don't forget to change the path of GENESIS in line 6 of the bash
script `05_calculate_qval.sh`:

```bash
#!/usr/bin/bash

for irep in {1..16}; do
    cp 05_qval_residcg_analysis.inp tmp.inp
    sed -e "s/RUNID/ID$irep/g" -i tmp.inp
    /home/user/GENESIS/bin/qval_residcg_analysis tmp.inp
    rm -f tmp.inp
done
```

Now, let's execute the Q-value calculations:

```bash
$ ./05_calculate_qval.sh
```

### 3.6 MBAR analysis 

Now let's carry out the MBAR analysis:

```bash
# change directory
$ cd ../6_MBAR
$ ls
mbar.inp

$ /home/user/GENESIS/bin/mbar_analysis mbar.inp | tee mbar.log
```

### 3.7 Calculate PMF of Q-value 

Finally, we use the GENESIS analysis tool `pmf_analysis` to get the PMF
of Q-value:

```bash
# change directory
$ cd ../7_PMF
$ ls
pmf.inp 01_remove_first_line_of_weight_files.sh 02_plot_PMF_Q.py
```

We use the script `01_remove_first_line_of_weight_files.sh` to remove
the first line of the "weight" files, to be consistent with the CV
(Q-value) files. The script has the following content:

```bash
#!/bin/bash
for j in {1..16}; do
    sed "1d" ../6_MBAR/weight$j.dat > weight$j.dat
done
```

We then calculate the PMF:

```bash
$ /home/user/GENESIS/bin/pmf_analysis pmf.inp | tee pmf.log
```

This command generates a new file, `qval.pmf`. We can now plot the PMF
with the python script `02_plot_PMF_Q.py`. The content
of `02_plot_PMF_Q.py` is:

```python
#!/usr/bin/env python3

import numpy as np
import matplotlib.pyplot as plt

pmf_data = np.loadtxt("./qval.pmf")

fig, ax = plt.subplots(1, 1, figsize=(6, 4), constrained_layout=False, sharex=False, sharey=False)
ax.plot(pmf_data[:, 0], pmf_data[:, 2], "r-")

ax.set_xticks([0.2 * i for i in range(6)])
ax.set_xticklabels(["0.0", "0.2", "0.4", "0.6", "0.8", "1.0"], fontsize=12)
ax.set_xlim(-0.02, 1.02)
ax.set_xlabel("Q value", fontsize=16)

ax.set_yticks([1 * i for i in range(6)])
ax.set_yticklabels([1 * i for i in range(6)], fontsize=12)
ax.set_ylim(-0.2, 4.2)
ax.set_ylabel("PMF (kcal/mol)", fontsize=16)

plt.savefig("qval_pmf.png", dpi=150)
```

Now let's plot the PMF:

```bash
$ ./02_plot_PMF_Q.py
```

![](/assets/images/2022_06_tutorial_A4_qval_pmf.png){: style="width:60%;" .align-center}
<!-- {: style="width:60%; display:block; margin:0 auto;"} -->

This figure shows the PMF of Q value at 400 K.

------------------------------------------------------------------------

*Written by Cheng Tan@RIKEN Center for Computational Science,
Computational Biophysics Research Team\
June, 2022*
{: .notice}

