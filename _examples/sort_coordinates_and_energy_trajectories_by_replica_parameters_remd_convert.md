---
title: "GENESIS Input Example: Sort coordinates and energy trajectories by replica parameters (`remd_convert`)"
excerpt: ""
last_modified_at: 2025-06-11T22:49:38+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---


This tool can manupulate REMD trajectory files as in the `crd_convert` tool.
Since REMD trajectories generated from GENESIS are the "mixture" of trajectories
in different temperatures, the user has to sort trajectories by
temperatures. Here, we consider that the user specified the following
information in the control file of REMD:

```toml
[OUTPUT]
logfile         = run_rep{}.log
rstfile         = run_rep{}.rst
dcdfile         = run_rep{}.dcd
remfile         = run_rep{}.rem
 
[REMD]
exchange_period = 1000
type1           = TEMPERATURE
nreplica1       = 8
parameters1     = 300 302 304 306 308 310 312 314
 
[DYNAMICS]
integrator      = LEAP        # [LEAP,VVER]
nsteps          = 100000      # number of MD steps
timestep        = 0.002       # timestep (ps)
eneout_period   = 100         # energy output period
crdout_period   = 100
```

An example of `remd_convert` for this REMD simulation is as follows,
where the trajectory at T = 300 is generated. The color means one-to-one
correspondence between REMD and `remd_convert`.

```toml
[INPUT]
psffile  = 2ech.psf          # protein structure file
reffile  = 2ech.pdb          # PDB file
remfile  = run_rep{}.rem     # REMD parameter ID file
dcdfile  = run_rep{}.dcd     # DCD file
logfile  = run_rep{}.log     # REMD energy log file

[OUTPUT]
trjfile  = run_par{}.dcd     # coordinates sorted by temperature
logfile  = run_par{}.log     # energy log sorted by temperature

[SELECTION]
group1          = all        # selection group 1

[FITTING]
fitting_method  = NO         # [NO,TR,TR+ROT,TR+ZROT,XYTR,XYTR+ZROT]
mass_weight     = NO         # mass-weight is not applied

[OPTION]
check_only      = NO
convert_type    = PARAMETER  # (REPLICA/PARAMETER)
num_replicas    = 8          # total number of replicas used in the simulation
convert_ids     = 1          # selected index (empty = all)(example: 1 2 5-10)
nsteps          = 100000     # nsteps in [DYNAMICS]
exchange_period = 1000       # exchange_period in [REMD]
crdout_period   = 100        # crdout_period in [DYNAMICS]
eneout_period   = 100        # eneout_period in [DYNAMICS]
trjout_format   = DCD        # (PDB/DCD)
trjout_type     = COOR+BOX   # (COOR/COOR+BOX)
trjout_atom     = 1          # atom group
centering       = NO
pbc_correct     = NO
```
