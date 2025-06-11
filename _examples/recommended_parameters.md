---
title: "GENESIS Recommended Parameters"
excerpt: ""
last_modified_at: 2025-06-11T22:49:38+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---

## Recommended control parameters in ver. 1.4 

The followings are recommended control parameters in GENESIS ver. 1.4 to carry
out normal all-atom MD simulations in explicit solvent with the CHARMM and AMBER
force fields. In this version, we recommend the users to employ VRES integrator
with 2.5 fs for production runs, since it is more efficient than the traditional
protocol (LEAP or VVER with 2.0 fs).  In addition, BUSSI thermostat and barostat
are recommended, since the method can reproduce transport properties and
kinetics in the MD simulations more accurately than the others (see [J. E.
Basconi and M.  R. Shirts, *J. Chem. Theory Comput.*,
2013](https://pubs.acs.org/doi/abs/10.1021/ct400109a)

### MD simulation in explicit solvent with CHARMM FF (NVT)

```toml
[ENERGY]
forcefield        = CHARMM   # CHARMM force field
electrostatic     = PME      # use Particle mesh Ewald method
switchdist        = 10.0     # switch distance
cutoffdist        = 12.0     # cutoff distance
pairlistdist      = 13.5     # pair-list distance
pme_nspline       = 4        # order of the spline interpolation
vdw_force_switch  = YES      # turn on vdw force switch
pme_max_spacing   = 1.2      # Max grid spacing allowed 

[DYNAMICS]
integrator        = VRES     # [LEAP,VVER,VRES]
timestep          = 0.0025   # timestep (ps) 
nbupdate_period   = 10       # pairlist update period
elec_long_period  = 2        # period of reciprocal space calculation
thermostat_period = 10       # period of thermostat update
barostat_period   = 10       # period of barostat update

[CONSTRAINTS]
rigid_bond        = YES      # use SHAKE/RATTLE
fast_water        = YES      # use SETTLE

[ENSEMBLE]
ensemble          = NVT      # [NVE,NVT,NPT]
tpcontrol         = BUSSI    # thermostat and barostat
temperature       = 300      # K
```

### MD simulation in explicit solvent with CHARMM FF (NPT)

```toml
[ENERGY]
forcefield        = CHARMM   # CHARMM force field
electrostatic     = PME      # use Particle mesh Ewald method
switchdist        = 10.0     # switch distance
cutoffdist        = 12.0     # cutoff distance
pairlistdist      = 13.5     # pair-list distance
pme_nspline       = 4        # order of the spline interpolation
vdw_force_switch  = YES      # turn on vdw force switch
pme_max_spacing   = 1.2      # Max grid spacing allowed 

[DYNAMICS]
integrator        = VRES     # [LEAP,VVER,VRES]
timestep          = 0.0025   # timestep (ps) 
nbupdate_period   = 10       # pairlist update period
elec_long_period  = 2        # period of reciprocal space calculation
thermostat_period = 10       # period of thermostat update
barostat_period   = 10       # period of barostat update

[CONSTRAINTS]
rigid_bond        = YES      # use SHAKE/RATTLE
fast_water        = YES      # use SETTLE

[ENSEMBLE]
ensemble          = NPT      # [NVE,NVT,NPT]
tpcontrol         = BUSSI    # thermostat and barostat
pressure          = 1        # atm 
temperature       = 300      # K 
```

### MD simulation in explicit solvent with AMBER FF (NVT)

```toml
[ENERGY]
forcefield        = AMBER    # AMBER force field
electrostatic     = PME      # use Particle mesh Ewald method
switchdist        = 8.0      # switch distance
cutoffdist        = 8.0      # cutoff distance
pairlistdist      = 9.5      # pair-list distance
pme_nspline       = 4        # order of the spline interpolation
pme_max_spacing   = 1.2      # Max grid spacing allowed 

[DYNAMICS]
integrator        = VRES     # [LEAP,VVER,VRES]
timestep          = 0.0025   # timestep (ps) 
nbupdate_period   = 10       # pairlist update period
elec_long_period  = 2        # period of reciprocal space calculation
thermostat_period = 10       # period of thermostat update
barostat_period   = 10       # period of barostat update

[CONSTRAINTS]
rigid_bond        = YES      # use SHAKE/RATTLE
fast_water        = YES      # use SETTLE
water_model       = WAT      # define TIP3P residue name

[ENSEMBLE]
ensemble          = NVT      # [NVE,NVT,NPT]
tpcontrol         = BUSSI    # thermostat and barostat
temperature       = 300      # K
```

### MD simulation in explicit solvent with AMBER FF (NPT)

```toml
[ENERGY]
forcefield        = AMBER    # AMBER force field
electrostatic     = PME      # use Particle mesh Ewald method
switchdist        = 8.0      # switch distance
cutoffdist        = 8.0      # cutoff distance
pairlistdist      = 9.5      # pair-list distance
pme_nspline       = 4        # order of the spline interpolation
pme_max_spacing   = 1.2      # Max grid spacing allowed 

[DYNAMICS]
integrator        = VRES     # [LEAP,VVER,VRES]
timestep          = 0.0025   # timestep (ps) 
nbupdate_period   = 10       # pairlist update period
elec_long_period  = 2        # period of reciprocal space calculation
thermostat_period = 10       # period of thermostat update
barostat_period   = 10       # period of barostat update

[CONSTRAINTS]
rigid_bond        = YES      # use SHAKE/RATTLE
fast_water        = YES      # use SETTLE
water_model       = WAT      # define TIP3P residue name

[ENSEMBLE]
ensemble          = NPT      # [NVE,NVT,NPT]
tpcontrol         = BUSSI    # thermostat and barostat
pressure          = 1        # atm
temperature       = 300      # K 
```

