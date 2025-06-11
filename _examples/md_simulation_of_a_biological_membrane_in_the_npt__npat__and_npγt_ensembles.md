---
title: "GENESIS Input Example: MD simulation of a biological membrane in the NPT, NPAT, and NPγT ensembles"
excerpt: ""
last_modified_at: 2025-06-11T22:49:38+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---

In order to use NPT, NPAT, or NPγT, `[ENSEMBLE]` section is modified.  Note
that `isotropy = ISO`, `ANISO`, and `SEMI-ISO` are available for the NPT
and NPγT ensembles, and `XY-FIXED` is for the NPAT ensemble. In the simulations
of biological membranes and membrane proteins, semi-isotopic pressure coupling
in the NPT ensemble is usually used. In the samples below, we use the Langevin
thermostat and barostat. Other algorithms such as Berendsen and Bussi
thermostat/barostat are also available with these ensembles.

## NPT ensemble

```toml
[INPUT]
topfile  = top_all36_prot.rtf, top_all36_lipid.rtf  # topology file
parfile  = par_all36_prot.prm, par_all36_lipid.prm  # parameter file
strfile  = toppar_water_ions.str                    # stream file
psffile  = ionized.psf                              # protein structure file
pdbfile  = ionized.pdb                              # input PDB file
rstfile  = minimized.rst                            # restart file
 
[OUTPUT]
dcdfile  = md.dcd            # DCD trajectory file
rstfile  = md.rst            # restart file

[ENERGY]
forcefield       = CHARMM    # CHARMM force field
electrostatic    = PME       # Particle Mesh Ewald method
switchdist       = 10.0      # switch distance
cutoffdist       = 12.0      # cutoff distance
pairlistdist     = 13.5      # pair-list distance
vdw_force_switch = YES       # force switch option for van der Waals
pme_nspline      = 4         # order of B-spline in [PME]
pme_max_spacing  = 1.0       # max grid spacing

[DYNAMICS]
integrator       = LEAP      # Leapfrog Verlet integrator
nsteps           = 10000     # number of MD steps
timestep         = 0.002     # timestep (ps)
eneout_period    =   100     # energy output period
crdout_period    =   100     # coordinates output period
rstout_period    = 10000     # restart output period
nbupdate_period  =    10     # nonbond update period

[CONSTRAINTS]
rigid_bond       = YES       # constraints all bonds involving hydrogen

[ENSEMBLE]
ensemble         = NPT       # NPT ensemble
tpcontrol        = LANGEVIN  # thermostat and barostat
temperature      = 323.15    # initial and target temperature (K)
pressure         = 1.0       # initial and target pressure (atm)
isotropy         = SEMI-ISO  # semi-isotropic pressure coupling
gamma_t          = 1.0       # thermostat friction coefficient (ps-1)
gamma_p          = 0.1       # barostat friction coefficient (ps-1)

[BOUNDARY]
type             = PBC       # periodic boundary condition
```

## NPAT ensemble

```toml
[ENSEMBLE]
ensemble         = NPAT      # NPAT ensemble
tpcontrol        = LANGEVIN  # thermostat and barostat
temperature      = 323.15    # temperature (K)
pressure         = 1.0       # normal pressure (atm)
isotropy         = XY-FIXED  # XY dimensions are fixed
gamma_t          = 1.0       # thermostat friction coefficient (ps-1)
gamma_p          = 0.1       # barostat friction coefficient (ps-1)
```

## NPγT ensemble

```toml
[ENSEMBLE]
ensemble         = NPgT      # NPgammaT ensemble
tpcontrol        = LANGEVIN  # thermostat and barostat
temperature      = 323.15    # temperature (K)
pressure         = 1.0       # normal pressure (atm)
gamma            = 20.0      # surface tension (dyn/cm)
isotropy         = SEMI-ISO  # semi-isotropic pressure coupling
gamma_t          = 1.0       # thermostat friction coefficient (ps-1)
gamma_p          = 0.1       # barostat friction coefficient (ps-1)
```
