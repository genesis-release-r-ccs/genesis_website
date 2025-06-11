---
title: "GENESIS Input Example: MD simulation in the NPT ensemble with Langevin, Berendsen, and Bussi thermostat and barostat"
excerpt: ""
last_modified_at: 2025-06-11T22:49:38+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---


In GENESIS, three types of thermostat and barostat are available: Langevin,
Berendsen, and Bussi (Stochastic velocity rescaling) methods.  Available
combinations between thermostats/barostats and integrators are as follows:

| Thermostat/barostat | Integrator |
|:---:|:---:|
| LANGEVIN  | LEAP, VVER |
| BERENDSEN |    LEAP |
|   BUSSI   |    VVER |


Thermostat/barostat is specified by `tpcontrol` in the `[ENSEMBLE]` section, and
integrator is specified by `integrator` in the `[DYNAMICS]` section. To control
the temperature and pressure, `gamma_t` and `gamma_p` are used in `LANGEVIN`,
and `tau_t` and `tau_p` are used in `BERENDSEN` and `BUSSI`.

In GENESIS, different types of thermostat and barostat cannot be combined. For
example, Langevin thermostat with Berendsen barostat is not available.

## Langevin thermostat and barostat

```toml
[INPUT]
topfile  = top_all36_prot.rtf     # topology file
parfile  = par_all36_prot.prm     # parameter file
strfile  = toppar_water_ions.str  # stream file
psffile  = ionized.psf            # protein structure file
pdbfile  = ionized.pdb            # input PDB file
rstfile  = minimized.rst          # restart file
 
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
integrator       = LEAP      # [LEAP,VVER]
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
tpcontrol        = LANGEVIN  # thermostat/barostat
temperature      = 300.0     # target temperature (K)
pressure         = 1.0       # target pressure (atm)
isotropy         = ISO       # isotropic pressure coupling
gamma_t          = 1.0       # thermostat friction coefficient (ps-1)
gamma_p          = 0.1       # barostat friction coefficient (ps-1)

[BOUNDARY]
type             = PBC       # periodic boundary condition
```

## Berendsen thermostat and barostat

```toml
[DYNAMICS]
integrator       = LEAP      # [LEAP]
nsteps           = 10000     # number of MD steps
timestep         = 0.002     # timestep (ps)
eneout_period    = 100       # energy output period
crdout_period    = 100       # coordinates output period
rstout_period    = 10000     # restart output period
nbupdate_period  = 10        # nonbond update period

[ENSEMBLE]
ensemble         = NPT       # NPT ensemble
tpcontrol        = BERENDSEN # thermostat/barostat
temperature      = 300.0     # target temperature (K)
pressure         = 1.0       # target pressure (atm)
isotropy         = ISO       # isotropic pressure coupling
tau_t            = 5.0       # temperature coupling time (ps)
tau_p            = 5.0       # pressure coupling time (ps)
compressibility  = 0.0000463 # compressibility (atm-1)
```

## Bussi thermostat and barostat (Stochastic velocity rescaling method)

```toml
[DYNAMICS]
integrator       = VVER      # [VVER]
nsteps           = 10000     # number of MD steps
timestep         = 0.002     # timestep (ps)
eneout_period    = 100       # energy output period
crdout_period    = 100       # coordinates output period
rstout_period    = 10000     # restart output period
nbupdate_period  = 10        # nonbond update period

[ENSEMBLE]
ensemble         = NPT       # NPT ensemble
tpcontrol        = BUSSI     # thermostat/barostat
temperature      = 300.0     # target temperature (K)
pressure         = 1.0       # target pressure (atm)
isotropy         = ISO       # isotropic pressure coupling
tau_t            = 5.0       # temperature coupling time (ps)
tau_p            = 5.0       # pressure coupling time (ps)
```
