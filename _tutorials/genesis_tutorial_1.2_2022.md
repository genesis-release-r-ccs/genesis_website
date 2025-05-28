---
title: "GENESIS Tutorial 1.2 (2022)"
excerpt: ""
last_modified_at: 2025-05-13T11:59:26+09:00
layout: single
toc: true
toc_sticky: true
sidebar:
  nav: sidebar-basic
---

# Let's take a quick look at the source code of GENESIS

## Take a look at the source directory

In this tutorial, we briefly explain the source code of GENESIS. We expect that
you have already learned basic theories of MD simulations [^1] [^2] [^3]. Even if you
are an experimental scientist, we recommend you to take a look at the source
code of GENESIS for a better understanding of the MD algorithm. GENESIS is
written mainly in Fortran90, which is a traditional language but still widely
used in the computational science due to its simplicity. Let's go to the
directory where the source code of the MD program `atdyn` is contained. In this
directory, you will find many files with the filename extensions `.fpp`, `.f90`,
`.o`, and `.mod`. Of these, `fpp` file is the original source code, while the
other files were generated after the compilation. Therefore, we will mainly
focus on the `fpp` files and refer to each `fpp` file as a "module".

```bash
# Change directory to see the source codes of GENESIS
$ cd ~/GENESIS_Tutorials-2022
$ cd Programs
$ cd genesis-2.0.0/src/atdyn

# Check the contents
$ ls *.fpp
at_boundary.fpp             at_energy_table_cubic.fpp
at_boundary_str.fpp         at_energy_table_linear.fpp
at_constraints.fpp          at_energy_table_linear_bondcorr.fpp
at_constraints_str.fpp      at_ensemble.fpp
at_control.fpp              at_ensemble_str.fpp
at_dynamics.fpp             at_experiments.fpp
at_dynamics_str.fpp         at_experiments_str.fpp
at_dynvars.fpp              at_gamd.fpp
at_dynvars_str.fpp          at_input.fpp
at_enefunc.fpp              at_md_leapfrog.fpp
at_enefunc_amber.fpp        at_md_vverlet.fpp
at_enefunc_charmm.fpp       at_md_vverlet_cg.fpp
at_enefunc_gamd.fpp         at_minimize.fpp
at_enefunc_gbsa.fpp         at_minimize_str.fpp
at_enefunc_go.fpp           at_morph.fpp
at_enefunc_gromacs.fpp      at_morph_str.fpp
at_enefunc_pme.fpp          at_output.fpp
at_enefunc_restraints.fpp   at_output_str.fpp
:
```

Before taking a look at the source code, let's check the total number of lines
in the program using the `wc` command. You can see that `atdyn` consists of more
than 100,000 lines. You would be surprised how big the MD program is. In
addition to `atdyn`, there are other directories in `src`, such as `spdyn`,
`analysis`, and `lib`. Here, `lib` stands for "library", which contains modules
commonly used by `atdyn`, `spdyn`, and `analysis`. You can see that these
programs also consist of many lines.  Since GENESIS is a huge program package,
it may be difficult for you to know where to start reading the program when
trying to understand it.  But, please don't worry. If you read the source code
in the order described below, you will be able to grasp the whole picture or the
core of the MD program.

```bash
# Check the total number of lines in the program
$ wc -l *.fpp
:
    588 at_rpath_fep.fpp
    3849 at_rpath_mep.fpp
    569 at_rpath_str.fpp
    1216 at_setup_atdyn.fpp
    989 at_setup_mpi.fpp
    1819 at_vibration.fpp
    55 at_vibration_str.fpp
    514 atdyn.fpp
106744 Total

$ cd ../spdyn
$ wc -l *.fpp
:
140056 Total

$ cd ../lib
$ wc -l *.fpp
:
63691 Total
```

## In which module are the energy and forces calculated? 

One of the main processes in the MD simulation is the calculation of the
potential energy and force. In general, the potential energy of a system is
given by

\\[ 
\begin{aligned} 
U &= \sum_{\textrm{bonds}} k_b (r-r_0)^2 + \sum_{\textrm{angles}} k_a (\theta-\theta_0)^2 \\\\ 
&+\sum_{\textrm{dihedrals}} \frac{V_n}{2} [1+\cos(n\phi - \delta)] \\\\ 
&+\sum_{i > j} 4\varepsilon \left[ \left(\frac{\sigma_{ij}}{r_{ij}}\right)^{12} - \left(\frac{\sigma_{ij}}{r_{ij}}\right)^{6} \right] + \sum_{i > j} \frac{q_i q_j}{r_{ij}} 
\end{aligned} 
\\]

where the first through fifth terms on the right-hand side are the energies for
bond stretching, angle bending, dihedral angle rotation, van der Waals
interaction, and electrostatic interaction, respectively.  The first three terms
are also called bonded interactions, and the last two terms are called
non-bonded interactions.

In the `atdyn` directory, you can see several files whose names start with
`at_energy`. You can easily imagine that `at_energy_bonds.fpp`,
`at_energy_angles.fpp`, `at_energy_dihedrals.fpp`, and `at_energy_nonbonds.fpp`
are related to the energy calculation for the bond stretching, angle bending,
dihedral angle rotation, and non-bonded interactions, respectively. As for the
other files, we will not discuss them in detail here, since they are more
advanced.

```bash
# List up the files related to the energy calculation
$ cd ../atdyn
$ ls at_energy*.fpp
at_energy.fpp               at_energy_morph.fpp
at_energy_angles.fpp        at_energy_nonbonds.fpp
at_energy_bonds.fpp         at_energy_pme.fpp
at_energy_cg_nonlocal.fpp   at_energy_restraints.fpp
at_energy_dihedrals.fpp     at_energy_soft.fpp
at_energy_eef1.fpp          at_energy_str.fpp
at_energy_gamd.fpp          at_energy_table_cubic.fpp
at_energy_gbsa.fpp          at_energy_table_linear.fpp
at_energy_go.fpp            at_energy_table_linear_bondcorr.fpp
```

Now, let us focus on the bond stretching energy, as it is the simplest term in
the above equation. The energy and force are given by

\\[ U_\textrm{bond} = k_b({r_{ij}} - {r_0})^2 \\]

\\[ \boldsymbol{F} _j = - 2k_b(r _{ij} - r_0) \frac{ \boldsymbol{r} _{ij} }{ r _{ij} } \\]

\\[ \boldsymbol{F} _i = - \boldsymbol{F} _j \\]

where \\(k_b\\) is the force constant, \\(r_{ij}\\) is the distance between
\\(i\\)-th and \\(j\\)-th atoms, and \\(r_0\\) is the equilibrium distance
between the atoms. With these equations in mind, let's take a look at
`at_energy_bond.fpp` using the `less` command. According to the variable names
and comment lines, you can indeed see that the distance between the atoms
(`r12`), bond stretching energy (`ebond`), and force acting on each atom
(`force`) are calculated from the atomic coordinates (`coord`), force constant
(`fc`), and equilibrium distance (`r0`). For now you can just skim through the
source code, since the main purpose of this tutorial is not to understand the
source code in detail, but to get a quick idea of how the energy and force are
calculated in GENESIS. To quit the `less` command, type "q" in the terminal
window.

```bash
# Check the source code for the bond energy term (type "q" to quit)
$ less at_energy_bonds.fpp

    subroutine compute_energy_bond(enefunc, coord, force, virial, ebond)
:
    do i = istart, iend
    
        ! bond energy: E=K[b-b0]^2
        !
        d12(1:3) = coord(1:3,list(1,i)) - coord(1:3,list(2,i))
        r12   = sqrt( d12(1)*d12(1) + d12(2)*d12(2) + d12(3)*d12(3) )
        r_dif = r12 - r0(i)
        ebond = ebond + fc(i) * r_dif * r_dif

        ! gradient: dE/dX
        !
        cc_frc    = (2.0_wp * fc(i) * r_dif) / r12
        work(1:3,i) = cc_frc * d12(1:3)
:
    end do

    ! store force: F=-dE/dX
    !
    do i = istart, iend
        force(1:3,list(1,i)) = force(1:3,list(1,i)) - work(1:3,i)
        force(1:3,list(2,i)) = force(1:3,list(2,i)) + work(1:3,i)
    end do
```

 If you are more interested in the source
code for the energy and force calculations, please take a look at the
contents of `at_energy_angles.fpp`, `at_energy_dihedrals.fpp`, and so
on, as well as their parent module, `at_energy.fpp`. In fact, the parent
subroutine in `at_energy.fpp` is called from within the do loop of the
integrator, as explained next.

## In which module are the atomic coordinates and velocities updated?

In MD simulation, the atomic coordinates and velocities are updated at each time
step ( \\(\Delta t\\) ), which is repeated many times to realize the time
evolution of the system. Such a protocol is called "time integration".  Typical
integrators include the leap-frog algorithm and the velocity Verlet algorithm,
both of which are available in `atdyn`. If you look in the `atdyn` directory,
you can find `at_md_leapfrog.fpp` and `at_md_vverlet.fpp`. These modules are
exactly related to the time integration. Here, we focus on the leap-frog
algorithm, where the following two equations are solved iteratively.

\\[ \boldsymbol{v} _i (t + \frac{\Delta t}{2}) =
\boldsymbol{v} _i (t - \frac{\Delta t}{2}) + \frac{\Delta
t}{m_i} \boldsymbol{F} _i (t) \\]

\\[ \boldsymbol{r} _i (t + \Delta t) = \boldsymbol{r} _i (t) +
\Delta t \ \boldsymbol{v} _i (t + \frac{\Delta t}{2}) \\]

These equations give an NVE (microcanonical) ensemble to the system.
Let's take a look at `at_md_leapfrog.fpp` with the `less` command, and
try to find the equations. You can see that there is a do loop with the
comment "Main MD loop", in which the two equations are solved
iteratively. The following is an extract of the most important parts in
the main MD loop.

```bash
# View the source code of the leapfrog integrator
$ less at_md_leapfrog.fpp

    ! Main MD loop 
    !   coord is at 0 +  dt and vel is at 0 + 1/2dt, if restart off
    !   coord is at t + 2dt and vel is at t + 3/2dt, if restart on
    !
    do i = istart, iend
:
        ! Compute energy(t + dt), force(t + dt), and internal virial(t + dt)
        !
        call compute_energy(molecule, enefunc, pairlist, boundary, &
:
        ! Newtonian dynamics
        !   v(t+3/2dt) = v(t+1/2dt) + dt*F(t+dt)/m
        !   r(t+2dt)   = r(t+dt) + dt*v(t+3/2dt)
        !
        do j = 1, natom
        vel(1,j) = vel(1,j) + dt*force(1,j)*inv_mass(j)
        vel(2,j) = vel(2,j) + dt*force(2,j)*inv_mass(j)
        vel(3,j) = vel(3,j) + dt*force(3,j)*inv_mass(j)

        coord(1,j) = coord(1,j) + dt*vel(1,j)
        coord(2,j) = coord(2,j) + dt*vel(2,j)
        coord(3,j) = coord(3,j) + dt*vel(3,j)
        end do
:
        ! Output energy(t + dt) and dynamical variables(t + dt)
        !   coord     is at t +   2dt, coord_ref    is at t +    dt
        !   vel       is at t + 3/2dt, vel_ref      is at t + 1/2dt
        !   box_size  is at t +   2dt, box_size_ref is at t +    dt
        !
        call output_md(output, molecule, enefunc, dynamics, boundary, &
                        ensemble, dynvars)

    end do
```

Here, `coord` and `vel` are the atomic coordinates and velocities, respectively,
`dt` is the time step, and `inv_mass` is the inverse of the atomic mass. As
described above, the atomic forces are calculated in the subroutine
`compute_energy`. The trajectories of the coordinates and energy are output in
the subroutine `output_md`, which is contained in `at_output.fpp`.

In the case of the NVT or NPT ensemble, temperature and pressure are kept
constant, where the velocities and coordinates are re-scaled according to the
instantaneous temperature and pressure. In the leap-frog integrator of `atdyn`,
Langevin thermostat/barostat [^4] [^5] and Berendsen thermostat/barostat [^6]
are available. Let's search for `langevin` or `berendsen` in
`at_md_leapfrog.fpp` to see how the velocities and coordinates are re-scaled.
In addition, let's search for `constraints` to understand the SHAKE algorithm
[^7], which further updates the coordinates to fix the length of the covalent
bond involving hydrogen.

## Overview of the core of MD simulation 

The following figure summarizes the flowchart of the time integration, energy
and force calculations, and trajectory output. Since GENESIS is a huge program
package, it is not effective to read the source code from the beginning of the
program. Rather, it is recommended to first understand the source code for
energy and force calculations or time integration, which are the core of MD
simulation. Then, you can expand your understanding of the source code based on
your own knowledge of MD simulation such as the SHAKE algorithm, temperature and
pressure controls. The MD program is indeed encoded with the theories you have
learned in textbooks and literature.

![](/assets/images/2019_08_source.jpg)


---

*Written by Takaharu Mori@RIKEN Theoretical molecular science
laboratory\
Jan. 3, 2022*
{: .notice}


## References

[^1]: M. Tuckerman, "Statistical Mechanics: Theory and Molecular Simulation", Oxford University Press (2010). [<i class="fas fa-link"></i>](https://global.oup.com/academic/product/statistical-mechanics-theory-and-molecular-simulation-9780198525264?q=Tuckerman&lang=en&cc=jp)
[^2]: M. P. Allen and D. J. Tildesley, "Computer Simulation of Liquids", Oxford University Press (2017). [<i class="fas fa-link"></i>](https://global.oup.com/academic/product/computer-simulation-of-liquids-9780198803201?q=Computer%20Simulation%20of%20Liquids&lang=en&cc=jp)
[^3]: D. Frenkel and B. Smit, "Understanding Molecular Simulation 2nd Edition", Academic Press (2001). [<i class="fas fa-link"></i>](https://www.elsevier.com/books/understanding-molecular-simulation/frenkel/978-0-12-267351-1)
[^4]: A. Brünger *et al.*, *Chem. Phys. Lett.*, **105**, 495-500 (1984). [<i class="fas fa-link"></i>](https://www.sciencedirect.com/science/article/abs/pii/0009261484800986)
[^5]: S. E. Feller *et al.*, *J. Chem. Phys.*, **103**, 4613-4621 (1995). [<i class="fas fa-link"></i>](https://aip.scitation.org/doi/10.1063/1.470648)
[^6]: H. J. C. Berendsen *et al.*, *J. Chem. Phys.*, **81**, 3684-3690 (1984). [<i class="fas fa-link"></i>](https://aip.scitation.org/doi/10.1063/1.448118)
[^7]: J. P. Ryckaert *et al.,* *J. Comput. Phys.*, **23**, 327-341 (1977). [<i class="fas fa-link"></i>](https://www.sciencedirect.com/science/article/pii/0021999177900985)

