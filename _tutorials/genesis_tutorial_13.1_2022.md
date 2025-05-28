### 13.1 The mean-force string method simulations of (Ala)~3~ in water

Contents

-   [1. System setup](genesis_tutorial_13.1_2022.md#1_System_setup)
-   [2. Energy minimization and
    pre-equilibration](genesis_tutorial_13.1_2022.md#2_Energy_minimization_and_pre-equilibration)
-   [3. Generating an initial pathway by steered
    MD](genesis_tutorial_13.1_2022.md#3_Generating_an_initial_pathway_by_steered_MD)
-   [4. Preparing inputs for string
    simulation](genesis_tutorial_13.1_2022.md#4_Preparing_inputs_for_string_simulation)
-   [5. Equilibration along the initial
    pathway](genesis_tutorial_13.1_2022.md#5_Equilibration_along_the_initial_pathway)
-   [6. String simulation](genesis_tutorial_13.1_2022.md#6_String_simulation)
-   [7. Umbrella sampling along the minimum free energy
    path](genesis_tutorial_13.1_2022.md#7_Umbrella_sampling_along_the_minimum_free_energy_path)
-   [8. MBAR analysis of the free energy profile along with the
    images](genesis_tutorial_13.1_2022.md#8_MBAR_analysis_of_the_free_energy_profile_along_with_the_images)
-   [9. Generate pathcv along the minimum free energy
    pathway](genesis_tutorial_13.1_2022.md#9_Generate_pathcv_along_the_minimum_free_energy_pathway)
-   [10. Evaluate the free energy profile along the converged
    pathway](genesis_tutorial_13.1_2022.md#10_Evaluate_the_free_energy_profile_along_the_converged_pathway)
-   [References](genesis_tutorial_13.1_2022.md#References)

[ ]{style="color: #ff6600"}GENESIS 1.7 or later is required for this
tutorial.

Understanding conformation changes of biomolecules is a challenging
problem in computational chemistry. The time scale of conventional
all-atom MD simulation is usually limited to several microseconds, which
is too short to observe the large conformational changes of
biomolecules. The string method (also known as
[the]{style="color: #000000"} replica path sampling method) is a
powerful approach to study the large conformational transition between
two different states [of biomolecules\[1-3\].
The]{style="color: #000000"} method [finds ]{style="color: #000000"}the
physically most probable pathway connecting two structures very
efficiently, [thus enabling ]{style="color: #000000"}us to examine the
conformational transition of biomolecular at a feasible computational
cost. In the string method, a pathway connecting the two user-defined
states [is represented]{style="color: #000000"} by discrete points
(called images or replicas) in the collective variables space. Then,
conventional MD simulations are performed around each image to find
lower free energy regions [while updating]{style="color: #000000"}
images repeatedly to converge to the minimum free energy path.
Currently, there are three major algorithms in the string method; (i)
the string method with mean forces\[1\], (ii) the on-the-fly string
method\[2\], and (iii) the string method with
swarms-of-trajectories\[3\]. GENESIS supports (i) the string method with
mean forces.

In this tutorial, we explain how to use the string method to investigate
the conformational change of a small peptide (alanine tripeptide or
(Ala)~3~) in water. [(Ala)~3~]{style="color: #000000"} undergoes a
conformational change from an alpha-helix state to a beta-sheet state.
Since this process is well characterized by two dihedral angles, Φ and
Ψ, we will search for the most probable pathway (called the [minimum
free energy path]{style="color: #ff0000"}) in the subspace spanned by
these two collective variables. Please note that it may take around one
day or even longer to finish all simulations in this tutorial with a
single-node machine. We recommend using multiple computation nodes, GPU
cluster is the best choice.

![](assets/images/2022_03_fig1.png){.alignnone
.wp-image-20919 .size-medium decoding="async" width="300" height="100"
srcset="wp-content/uploads/2022/03/fig1-300x100.png 300w, wp-content/uploads/2022/03/fig1-1024x341.png 1024w, wp-content/uploads/2022/03/fig1-768x256.png 768w, wp-content/uploads/2022/03/fig1-20x7.png 20w, wp-content/uploads/2022/03/fig1-30x10.png 30w, wp-content/uploads/2022/03/fig1-40x13.png 40w, wp-content/uploads/2022/03/fig1.png 1218w"
sizes="(max-width: 300px) 100vw, 300px"}

All the necessary files [for]{style="color: #000000"} this tutorial [can
be found]{style="color: #000000"} in the compressed zip file
([tutorial22-13.1](assets/tutorial_files/2022_04_tutorial22-13.1.tar.gz){.mtli_attachment
.mtli_zip}). Unpacking [the ]{style="color: #000000"}file creates
multiple directories in[side]{style="color: #000000"} tutorial22-13.1.
We also need to make a symbolic link to the CHARMM toppar directory (see
[Tutorial-3.1](genesis_tutorial_3.1_2022.md)):

    # Download the tutorial file
    $ cd ~/GENESIS_Tutorials-2022/Works
    $ mv ~/Download/tutorial22-13.1.zip ./
    $ unzip tutorial22-13.1.zip

    # Let's clean up the directory
    $ mv tutorial22-13.1.zip TRASH

    # Let's take a note
    $ echo "tutorial-13.1: The mean-force string method simulations of Ala3 in water" >> README

    # Make a symbolic link to the CHARMM toppar directory
    $ cd tutorial22-13.1
    $ ln -s ../../Data/Parameters/toppar_c36_jul21 ./toppar
    $ ln -s ../../Programs/genesis-2.0.0/bin ./
    $ ls
    10_pmf 2_minim_equil 4_rpath_generator 6_rpath_prod 8_mbar bin
    1_setup 3_steered_md 5_rpath_equib 7_umbrella 9_pathcv_analysis toppar

Or you can copy the CHARMM toppar directory to the current tutorial
directly. Please note that we provide an executable script file for
every step in this tutorial (`run.sh` in each directory). You can finish
each step just by executing the script file ("./run.sh") in the
corresponding directory.  

#### [1. System setup]{#1_System_setup}

In this step, we will prepare alanine tripeptide solvated in a water
box. We make PDB and PSF files of (Ala)~3~ solvated in a water box (50.2
Å × 50.2 Å × 50.2 Å) by using VMD/PSFGEN (see also
[Tutorial-2.2](genesis_tutorial_2.2_2022.md) and
[Tutorial-3.2](genesis_tutorial_3.2_2022.md)). Here, we skip this part and
provide the final PDB and PSF files directly. 

    # Change directory 
    $ cd 1_setup
    $ ls
    end.pdb start.pdb wbox.psf

In the "1_setup" directory, we provide two PDB files: `start.pdb` and
`end.pdb`, which correspond to the alpha-helix structure and beta-sheet
structure of a[lanine tripeptide ]{style="color: #000000"}respectively.
The file `start.pdb`, as the name suggests, is the initial structure of
the simulation. While the file `end.pdb` represents the target states
and is used as the reference file for the steered MD.

#### [2. Energy minimization and pre-equilibration]{#2_Energy_minimization_and_pre-equilibration}

Next, we will perform minimization and equilibration steps. The general
scheme is similar to that shown in
[Tutorial-3.2](tutorials2019/tutorial-3.2/index.html). Here we only show
the sequence of commands for performing these steps.

    # Change directory
    $ cd 2_minim_equil/

    $ export OMP_NUM_THREADS=1

    # Run minimization
    $ mpirun -np 8 ../bin/spdyn INP1 > log1

    # Run first NVT equilibration
    $ mpirun -np 8 ../bin/spdyn INP2 > log2

    # Run second NPT equilibration
    $ mpirun -np 8 ../bin/spdyn INP3 > log3

#### [3. Generat[ing]{style="color: #000000"} an initial pathway by steered MD]{#3_Generating_an_initial_pathway_by_steered_MD}

To run string simulation, firstly, we need to generate an initial
pathway connecting the start (alpha-helix) and end (beta-sheet)
structures. Several methods can be used to [obtain
]{style="color: #000000"}this initial pathway, such as steered/targeted
MD simulation, morphing[, and others]{style="color: #000000"}. Here,
starting from the alpha-helix [structure]{style="color: #000000"}, we
use steered MD simulation to [artificially]{style="color: #000000"} pull
the structure toward the beta-sheet structure by imposing a restraint on
the RMSD (Root Mean Squared Deviation) variable. A more detailed
explanation of how to perform steered MD can be found in the
Tutorial-9.1.

**** It is noted that, in the case of alanine
tripeptide, the result of [the]{style="color: #000000"} steered MD
[simulation]{style="color: #000000"} is very sensitive to [factors such
as]{style="color: #000000"} the random seed, compiler,
[and]{style="color: #000000"} hardware,
[therefore]{style="color: #000000"} it is
[difficult]{style="color: #000000"} to reproduce the result below
(t[[his is]{style="color: #000000"}]{style="color: #ff0000"} due to
subtle fluctuations of atoms, periodicity in the dihedrals space, low
free energy barriers, and so on).

The following command executes the simulation:

    # Change directory
    $ cd ../3_steered_md/

    $ export OMP_NUM_THREADS=1

    # Run steered MD
    $ mpirun -np 8 ../bin/spdyn INP1 > log

The content of `INP1` is shown below. Important
[`keywords=values`]{style="font-family: terminal, monaco, monospace;background-color: #f0f0f0"}
are indicated by [red]{style="color: #ff0000"}. In the steered MD
[simulation]{style="color: #000000"},
[`reffile`]{style="font-family: terminal, monaco, monospace;background-color: #f0f0f0"}
is required to [provide]{style="color: #000000"} the structural
information of the target state. In [this]{style="color: #000000"} case,
we specify
[`reffile=../1_setup/end.pdb`]{style="font-family: terminal, monaco, monospace;background-color: #f0f0f0"}
which [represents]{style="color: #000000"} the structure of the
beta[-sheet]{style="color: #000000"} state. The steered MD [scheme
]{style="color: #000000"}is turned on by
[`steered_MD=YES`]{style="font-family: terminal, monaco, monospace;background-color: #f0f0f0"}.
[`initial_rmsd`]{style="font-family: terminal, monaco, monospace;background-color: #f0f0f0"}
is the initial RMSD value for restraint (usually RMSD value of the
initial structure from the target state is specified).
[`final_rmsd`]{style="font-family: terminal, monaco, monospace;background-color: #f0f0f0"}
should be close to zero (exact zero might cause a crash in some cases).
Fitting of [the]{style="color: #000000"} structure (to
[`reffile`]{style="background-color: #f0f0f0;font-family: terminal, monaco, monospace"})
is [required for RMSD calculation in order]{style="color: #000000"} to
remove the contributions from global translation and rotations. Thus,
[`fitting_method=TR+ROT`]{style="background-color: #f0f0f0;font-family: terminal, monaco, monospace"}
(removing TRanslation and ROTation) is specified here. Note that
outputting a velocity file in steered MD is recommended since it allows
GENESIS to generate better restart files.

    [INPUT] 
    topfile          = ../toppar/top_all36_prot.rtf
    parfile          = ../toppar/par_all36m_prot.prm
    strfile          = ../toppar/toppar_water_ions.str
    psffile          = ../1_setup/wbox.psf
    pdbfile          = ../1_setup/start.pdb
    reffile          = ../1_setup/end.pdb    # structure of target state
    rstfile          = ../2_minim_equil/eq2.rst

    [OUTPUT] 
    rstfile          = smd.rst
    dcdfile          = smd.dcd
    dcdvelfile       = smd.dvl  # trajectory file of velocity 
     
    [ENERGY]
    forcefield       = CHARMM
    electrostatic    = PME
    switchdist       = 10.0
    cutoffdist       = 12.0
    pairlistdist     = 13.5
    vdw_force_switch = YES
    pme_nspline      = 4
    pme_max_spacing  = 1.2
     
    [DYNAMICS]
    integrator       = VVER
    nsteps           = 600
    timestep         = 0.001
    eneout_period    = 2
    crdout_period    = 2
    velout_period    = 2
    rstout_period    = 100
    nbupdate_period  = 2
    steered_md       = YES    # turn on steered MD
    initial_rmsd     = 5.0    # initial rmsd from target structure
    final_rmsd       = 0.01   # final rmsd from target structure
     
    [CONSTRAINTS] 
    rigid_bond       = YES
     
    [ENSEMBLE] 
    ensemble         = NVT
    tpcontrol        = BUSSI
    temperature      = 300.00
     
    [BOUNDARY] 
    type             = PBC
     
    [SELECTION] 
    group1           = rno:1-3 & heavy

    [RESTRAINTS]
    nfunctions       = 1
    function1        = RMSD   # RMSD restraint for steered MD
    reference1       = 5.0
    constant1        = 100.0  # Force constant of RMSD restraint
    select_index1    = 1      # Heavy atoms of (Ala)3 are used for RMSD calculation

    [FITTING]
    fitting_method   = TR+ROT
    fitting_atom     = 1         

In the folder, we also provide a control file for the calculation of the
collective variables (Φ and Ψ) with the
[`trj_analysis `]{style="font-family: terminal, monaco, monospace;color: #000000"}tool
in GENESIS. The following commands execute
[`trj_analysis`]{style="font-family: terminal, monaco, monospace;color: #000000"}
and calculate the collective variables:

    # Calculate the colletive variable phi and psi from steered MD trajectory
    $ ../bin/trj_analysis INP2

In the figure below, the trajectory of the steered MD
[simulation]{style="color: #000000"} (black dash line) is projected
along with the two collective variables (Φ and Ψ). The free energy
surface, evaluated from very long brute-force simulations, is drawn for
reference. Although the peptide successfully
[shifts]{style="color: #000000"} from the initial
alpha-[[helix]{style="color: #000000"}]{style="color: #ff0000"}
structure ([red star]{style="color: #ff0000"}) to the final beta[-sheet
]{style="color: #000000"}structure ([black
star]{style="color: #000000"}), there are large fluctuations in the
trajectory.

![](assets/images/2022_03_smd.png){.alignnone
.wp-image-20920 .size-medium fetchpriority="high" decoding="async"
width="300" height="273"
srcset="wp-content/uploads/2022/03/smd-300x273.png 300w, wp-content/uploads/2022/03/smd-768x698.png 768w, wp-content/uploads/2022/03/smd-20x18.png 20w, wp-content/uploads/2022/03/smd-30x27.png 30w, wp-content/uploads/2022/03/smd-40x36.png 40w, wp-content/uploads/2022/03/smd.png 990w"
sizes="(max-width: 300px) 100vw, 300px"}

#### [4. Prepar[ing]{style="color: #000000"} inputs for string simulation]{#4_Preparing_inputs_for_string_simulation}

In this step, we generate the necessary input files for string
simulation from [the]{style="color: #000000"} steered MD trajectory. As
we explained at the very beginning of this tutorial, in the string
simulation, we need to represent a pathway of the conformational
transition by discretized points (called images) in the collective
variable space ([Φ and Ψ]{style="color: #000000"} in this tutorial).
`rpath_generator` tool in GENESIS can prepare the images automatically
by discretizing the user-provided initial pathway into [a
predetermined]{style="color: #000000"} number of images. This tool works
as follows: it first reads the steered MD trajectory projected onto the
collective variable space. Then, trajectory snapshots [are
clustered]{style="color: #000000"}, [and]{style="color: #000000"}
coordinates of images [are set]{style="color: #000000"} in [an
equidistant]{style="color: #000000"} manner so that the distances
between adjacent images are [the]{style="color: #000000"} same. [Then,
the snapshot most similar (with the collective variable) to the image is
extracted for each image. ]{style="color: #000000"}Finally, restart
files contain[ing]{style="color: #000000"} the
[all-atom]{style="color: #000000"} coordinates, as well as the
corresponding image coordinates [are written]{style="color: #000000"}.

The following commands execute [`rpath_generator`[.
]{style="font-family: helvetica, arial, sans-serif"}]{style="font-family: terminal, monaco, monospace"}

    # Change directory
    $ cd ../4_rpath_generator/

    # Generate discreted images for string simulation
    $ ../bin/rpath_generator INP > log

[The content of
[`INP`]{style="font-family: terminal, monaco, monospace"} is shown
below.
[`cvfile`]{style="font-family: terminal, monaco, monospace;background-color: #f0f0f0"}
is a file containing the trajectory of steered MD projected on the
collective variables (generated by
[`trj_analysis`]{style="font-family: terminal, monaco, monospace"}).
[`dcdfile`]{style="font-family: terminal, monaco, monospace;background-color: #f0f0f0"}
and
[`dcdvelfile`]{style="font-family: terminal, monaco, monospace;background-color: #f0f0f0"}
are trajectory files of coordinates and velocities of the steered MD
[simulation]{style="color: #000000"}, respectively. Including the
trajectory file of velocities allows `rpath_generator` making better
restart files. The number of images [[is specified
by]{style="color: #000000"}
[`nreplica=16`]{style="font-family: terminal, monaco, monospace;background-color: #f0f0f0;color: #000000"}]{style="color: #ff0000"}.
Curly brackets
[`{}`]{style="font-family: terminal, monaco, monospace;background-color: #f0f0f0"}
in [the]{style="color: #000000"}
[`[OUTPUT]`]{style="background-color: #f0f0f0;font-family: terminal, monaco, monospace"}
section will be replaced with image indices (from 1 to 16) by GENESIS
automatically.
[`iseed`]{style="font-family: terminal, monaco, monospace;background-color: #f0f0f0"}
is used for embedding different seeds for restart files.
[`iter_reparam`]{style="font-family: terminal, monaco, monospace;background-color: #f0f0f0"}
is a smoothing parameter for defining image coordinates
([`iter_reparam=10`]{style="font-family: terminal, monaco, monospace;background-color: #f0f0f0"}
is recommended).]{style="font-size: 12pt"}

After running [the]{style="color: #000000"} commands, 16 restart files
(`1.rst`, ..., `16.rst`) and 16 PDB files(`1.pdb`, ..., `16.pdb`) [are
created]{style="color: #000000"}. Restart files contain atomistic
coordinates [as well as ]{style="color: #000000"}coordinates of [the
collective variables of]{style="color: #000000"} corresponding images.
PDB files have only atomistic coordinates.

    [INPUT] 
    cvfile       = ../3_steered_md/smd.tor
    pdbfile      = ../1_setup/start.pdb
    dcdfile      = ../3_steered_md/run.dcd
    dcdvelfile   = ../3_steered_md/run.dvl

    [OUTPUT] 
    rstfile      = {}.rst
    pdbfile      = {}.pdb
     
    [RPATH]  
    nreplica     = 16      # number of replicas
    iseed        = 31415
    iter_reparam = 10

In the figure below, the images ([green
circles]{style="color: #00ff00"}) generated by
[`rpath_generator`]{style="font-family: terminal, monaco, monospace"}
are depicted in the collective variable space (Φ and Ψ).[ It can be seen
]{style="color: #000000"}that the images are located in [an
equidistant]{style="color: #000000"} manner in this 2D space.

![](assets/images/2022_03_equil-1.png){.alignnone
.wp-image-20928 .size-medium decoding="async" width="300" height="273"
srcset="wp-content/uploads/2022/03/equil-1-300x273.png 300w, wp-content/uploads/2022/03/equil-1-768x698.png 768w, wp-content/uploads/2022/03/equil-1-20x18.png 20w, wp-content/uploads/2022/03/equil-1-30x27.png 30w, wp-content/uploads/2022/03/equil-1-40x36.png 40w, wp-content/uploads/2022/03/equil-1.png 990w"
sizes="(max-width: 300px) 100vw, 300px"}

#### [5. Equilibration along the initial pathway]{#5_Equilibration_along_the_initial_pathway}

In this step, we perform a two-step equilibration
[using]{style="color: #000000"} 16 replicas (corresponding to 16 images)
to relax the all-atom structures around the images for the subsequent
string method calculation. Weak and strong restraints on the collective
variables (Φ and Ψ) are applied [for]{style="color: #000000"} each
replica [with reference values being specified by corresponding image
coordinates successively.]{style="color: #000000"}

    # Change diretory
    $ cd 5_rpath_equib/

    # Run first equilibration with weak restraint
    $ mpirun -np 16 ../bin/spdyn INP1 > log1

    # Run second equilibration with strong restraint 
    $ mpirun -np 16 ../bin/spdyn INP2 > log2

The content of `INP1` is shown below.
[[`nreplica`]{style="background-color: #f0f0f0"}[[`=16`]{style="background-color: #f0f0f0"}]{style="font-family: terminal, monaco, monospace"}]{style="font-family: terminal, monaco, monospace;background-color: #f0f0f0"} in
[`[RPATH]`]{style="font-family: terminal, monaco, monospace;background-color: #f0f0f0"}
[specifies that]{style="color: #000000"} we perform a simulation with 16
replicas.
[`rpath_period=0`]{style="background-color: #f0f0f0;font-family: terminal, monaco, monospace"}
means that references of restraints (image coordinates) do not change
during the simulation. Force constants for the restraints are specified
[in the lists given]{style="color: #000000"} by
[`constants1`]{style="background-color: #f0f0f0;font-family: terminal, monaco, monospace"} and
[[`constants2`]{style="background-color: #f0f0f0;font-family: terminal, monaco, monospace"} (for
Φ and Ψ, respectively)]{style="color: #000000"} in
[`[RESTRAINTS]`]{style="font-family: terminal, monaco, monospace;background-color: #f0f0f0"}.
Each value [in the list]{style="color: #000000"} specifies the force
constant of the corresponding image. In the first step, we used [weak
restraints
]{style="color: #000000"}([`constant[12]=1.0`]{style="font-family: terminal, monaco, monospace;background-color: #f0f0f0"}
kcal/mol/rad^2^) to relax the system. Then, a strong force constant
([`constant[12]=100.0`]{style="font-family: terminal, monaco, monospace;background-color: #f0f0f0"}
kcal/mol/rad^2^), [as recommended for string simulation,
]{style="color: #000000"}is used in the second equilibration
[step]{style="color: #000000"}. Reference values for the restraints are
specified by
[`reference[12]`]{style="font-family: terminal, monaco, monospace;background-color: #f0f0f0"}.
Here, we set these v[alues to 0.0]{style="color: #000000"} because
GENESIS can automatically replace these values with the coordinates of
images embedded in the restart files (generated by
[`rpath_generator`]{style="font-family: terminal, monaco, monospace"}).
`fix_teminal=YES` tells GENESIS to fix the structure of the start and
end state. This is useful if the terminal images correspond to crystal
structures and users do not want to move them.

    [INPUT] 
    topfile          = ../toppar/top_all36_prot.rtf
    parfile          = ../toppar/par_all36m_prot.prm
    strfile          = ../toppar/toppar_water_ions.str
    psffile          = ../1_setup/wbox.psf
    pdbfile          = ../1_setup/start.pdb
    reffile          = ../4_rpath_generator/{}.pdb 
    rstfile          = ../4_rpath_generator/{}.rst

    [OUTPUT] 
    logfile          = eq1_{}.log
    dcdfile          = eq1_{}.dcd
    rstfile          = eq1_{}.rst
    rpathfile        = eq1_{}.rpath

    [ENERGY] 
    forcefield       = CHARMM
    electrostatic    = PME
    switchdist       = 10.0
    cutoffdist       = 12.0
    pairlistdist     = 13.5
    vdw_force_switch = YES
    pme_nspline      = 4
    pme_max_spacing  = 1.2 
    contact_check    = YES 

    [DYNAMICS] 
    integrator       = VRES
    nsteps           = 40000
    timestep         = 0.0025
    eneout_period    = 2000
    crdout_period    = 2000
    rstout_period    = 2000
    nbupdate_period  = 10

    [ENSEMBLE] 
    ensemble         = NPT
    tpcontrol        = BUSSI
    temperature      = 300.0
    pressure         = 1.0

    [CONSTRAINTS] 
    rigid_bond       = YES

    [BOUNDARY] 
    type             = PBC

    [RPATH] 
    nreplica         = 16     # number of replicas
    rpath_period     = 0      # no update of the image coordinates
    rest_function    = 1 2    # index of the restraint function to be used
    fix_terminal     = YES    # two terminal images are always fixed

    [SELECTION] 
    group1           = ai:15
    group2           = ai:17
    group3           = ai:19
    group4           = ai:25
    group5           = ai:27

    [RESTRAINTS]  
    nfunctions       = 2

    function1        = DIHED
    constant1        = 1.0 1.0 1.0 1.0 1.0 1.0 1.0 1.0 \
                       1.0 1.0 1.0 1.0 1.0 1.0 1.0 1.0  
    reference1       = 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 \
                       0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0   
    select_index1    = 1 2 3 4

    function2        = DIHED
    constant2        = 1.0 1.0 1.0 1.0 1.0 1.0 1.0 1.0 \
                       1.0 1.0 1.0 1.0 1.0 1.0 1.0 1.0  
    reference2       = 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 \
                       0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0  
    select_index2    = 2 3 4 5

#### [6. String simulation]{#6_String_simulation}

[Next]{style="color: #000000"}, we perform the string
[simulation]{style="color: #000000"}.

    # Change diretory
    $ cd ../6_rpath_prod

    # Run string simulation 
    $ mpirun -np 16 ../bin/spdyn INP > log

The content of [`INP`]{style="font-family: terminal, monaco, monospace"}
is shown
below. [`rpath_period`]{style="font-family: terminal, monaco, monospace;background-color: #f0f0f0"}
in the
[`[RPATH]`]{style="font-family: terminal, monaco, monospace;background-color: #f0f0f0"}
specifies the [duration]{style="color: #000000"} of
[the]{style="color: #000000"} time window during which the mean force is
evaluated (by time-averaging). [Here]{style="color: #000000"}, [the
image coordinates are updated according to the estimated mean forces
]{style="color: #000000"}every
[`rpath_period=2000`]{style="font-family: terminal, monaco, monospace;background-color: #f0f0f0"}
steps (corresponding to 5 ps). Typically 500-5000 steps are recommended
for moderate-size biomolecules.
[`delta`]{style="background-color: #f0f0f0;font-family: terminal, monaco, monospace"}
in
[`[RPATH]`]{style="background-color: #f0f0f0;font-family: terminal, monaco, monospace"}
is the step size of the image update. A too-large
[`delta`]{style="font-family: terminal, monaco, monospace;background-color: #f0f0f0"}[
will make the]{style="color: #000000"} structure
unstable[,]{style="color: #000000"} while a small [value
will]{style="color: #000000"} result in slow convergence of the pathway.
[Here too]{style="color: #000000"}, the reference values
[`reference[12]`]{style="background-color: #f0f0f0;font-family: terminal, monaco, monospace"}
for the restraints (the initial coordinates of images) are automatically
replaced with those embedded in the restart files. The output trajectory
files of the image coordinates are specified by
[`rpathfile={}.rpath`]{style="font-family: terminal, monaco, monospace;background-color: #f0f0f0"}
in
[`[OUTPUT]`]{style="font-family: terminal, monaco, monospace;background-color: #f0f0f0"}.

    [INPUT] 
    topfile          = ../toppar/top_all36_prot.rtf
    parfile          = ../toppar/par_all36m_prot.prm
    strfile          = ../toppar/toppar_water_ions.str
    psffile          = ../1_setup/wbox.psf
    pdbfile          = ../1_setup/start.pdb
    rstfile          = ../5_rpath_equil/eq2_{}.rst
     
    [OUTPUT] 
    logfile          = rp_{}.log       
    dcdfile          = rp_{}.dcd       
    rstfile          = rp_{}.rst       
    rpathfile        = rp_{}.rpath   # rpath files containing the image coordinates.

    [ENERGY] 
    forcefield       = CHARMM
    electrostatic    = PME
    switchdist       = 10.0
    cutoffdist       = 12.0
    pairlistdist     = 13.5
    vdw_force_switch = YES
    pme_nspline      = 4
    pme_max_spacing  = 1.2
     
    [DYNAMICS] 
    integrator       = VVER
    nsteps           = 2500000
    timestep         = 0.002 
    eneout_period    = 2000
    crdout_period    = 2000
    rstout_period    = 2000 
    nbupdate_period  = 10
     
    [CONSTRAINTS]
    rigid_bond       = YES
     
    [ENSEMBLE] 
    ensemble         = NPT
    tpcontrol        = BUSSI
    temperature      = 300.0
    pressure         = 1.0
     
    [BOUNDARY] 
    type             = PBC

    [RPATH] 
    nreplica         = 16       # number of replicas
    rpath_period     = 2000     # period for evaluating mean-forces
    delta            = 0.2      # step-size for steepest descent update of images
    smooth           = 0.0      # no smoothing
    rest_function    = 1 2      # index of the restraint function to be used
    fix_terminal     = YES      # two terminal images are always fixed and not updated

    [SELECTION] 
    group1           = ai:15
    group2           = ai:17
    group3           = ai:19
    group4           = ai:25
    group5           = ai:27

    [RESTRAINTS] 
    nfunctions       = 2

    function1        = DIHED
    constant1        = 100.0 100.0 100.0 100.0 100.0 100.0 100.0 100.0 \
                       100.0 100.0 100.0 100.0 100.0 100.0 100.0 100.0
    reference1       =   0.0   0.0   0.0   0.0   0.0   0.0   0.0   0.0 \
                         0.0   0.0   0.0   0.0   0.0   0.0   0.0   0.0
    select_index1    = 1 2 3 4 # PHI

    function2        = DIHED
    constant2        = 100.0 100.0 100.0 100.0 100.0 100.0 100.0 100.0 \
                       100.0 100.0 100.0 100.0 100.0 100.0 100.0 100.0
    reference2       =   0.0   0.0   0.0   0.0   0.0   0.0   0.0   0.0 \
                         0.0   0.0   0.0   0.0   0.0   0.0   0.0   0.0
    select_index2    = 2 3 4 5 # PSI

[The trajector[ies]{style="color: #000000"} of
[the]{style="color: #000000"} image coordinates, in the collective
variables (Φ and Ψ) space, [are]{style="color: #000000"} written in
[the]{style="color: #000000"} 
[`"6_rpath_prod/rp_{}.rpath"`]{style="font-family: terminal, monaco, monospace"}
files. By [plotting]{style="color: #000000"} these collective variable
coordinates explicitly, we can easily [track]{style="color: #000000"}
the evolution of the pathway.]{style="font-size: 12pt"}

![](assets/images/2022_03_rpath-1.png){.alignnone
.wp-image-20925 .size-medium loading="lazy" decoding="async" width="300"
height="273"
srcset="wp-content/uploads/2022/03/rpath-1-300x273.png 300w, wp-content/uploads/2022/03/rpath-1-768x698.png 768w, wp-content/uploads/2022/03/rpath-1-20x18.png 20w, wp-content/uploads/2022/03/rpath-1-30x27.png 30w, wp-content/uploads/2022/03/rpath-1-40x36.png 40w, wp-content/uploads/2022/03/rpath-1.png 990w"
sizes="(max-width: 300px) 100vw, 300px"}

In the figure above, the [green line]{style="color: #00ff00"}
[represents the]{style="color: #000000"} initial pathway, the black
lines represent intermediate pathways during the sampling[~~.~~, and
t]{style="color: #000000"}he [red line]{style="color: #ff0000"} is the
final converged pathway ([representing]{style="color: #000000"} the
minimum free energy path). We also provide a small shell script in the
tutorial to extract the final converged pathway which is necessary for
the subsequent analysis.

    #!/bin/sh

    # Generate the final minimum free energy pathway
    if [ -f "last.path" ]; then
      rm "last.path"
    fi

    for ki in `seq 16`; do
      tail -n 1 rp_${ki}.rpath >> last.path
    done

#### [7. Umbrella sampling along the minimum free energy path]{#7_Umbrella_sampling_along_the_minimum_free_energy_path}

[In the previous step, we obtained]{style="color: #000000"} the
converged pathway (minimum free energy pathway) connecting
[the]{style="color: #000000"} alpha-helix and beta-sheet [structures. In
order]{style="color: #000000"} to characterize
[the]{style="color: #000000"} free energy profile along this pathway.
[we next use the]{style="color: #000000"} umbrella sampling method to
obtain the free energy surface. The umbrella sampling method has been
widely used to calculate the free energy profile of the system along
some collective variables. In this method, several restraint potentials
are applied to the system along those collective variables, and
histograms of the collective variables are obtained by using a
reweighting algorithm. It should be noted that sufficient phase space
overlaps are required for subsequent reweighting analysis, such as MBAR
or WHAM.

    # Change diretory
    $ cd ../7_umbrella/

    # Run umbrella sampling
    $ mpirun -np 16 ../bin/spdyn INP > log

The content of [`INP`]{style="font-family: terminal, monaco, monospace"}
is shown below. Again, the reference values
[`reference[12]`]{style="background-color: #f0f0f0;font-family: terminal, monaco, monospace"}
for the restraints (image coordinates) are automatically replaced with
those embedded in the restart files (in this case, those of the last
snapshots in the string simulation). Please note that
[`rpath_period`]{style="font-family: terminal, monaco, monospace;background-color: #f0f0f0"}
and
[`delta`]{style="background-color: #f0f0f0;font-family: terminal, monaco, monospace"}
in
[`[RPATH]`]{style="background-color: #f0f0f0;font-family: terminal, monaco, monospace"}should
be set to 0 in umbrella sampling. Also, proper force constants are
required for the sufficient phase space overlap between two adjacent
replicas.

    [INPUT] 
    topfile          = ../toppar/top_all36_prot.rtf    
    parfile          = ../toppar/par_all36m_prot.prm    
    strfile          = ../toppar/toppar_water_ions.str 
    psffile          = ../1_setup/wbox.psf             
    pdbfile          = ../1_setup/start.pdb            
    rstfile          = ../6_rpath_prod/rp_{}.rst       
     
    [OUTPUT] 
    logfile          = umb_{}.log 
    dcdfile          = umb_{}.dcd 
    rstfile          = umb_{}.rst 
    rpathfile        = umb_{}.rpath 

    [ENERGY] 
    forcefield       = CHARMM
    electrostatic    = PME
    switchdist       = 10.0
    cutoffdist       = 12.0
    pairlistdist     = 13.5
    vdw_force_switch = YES
    pme_nspline      = 4
    pme_max_spacing  = 1.2
     
    [DYNAMICS] 
    integrator       = VRES
    nsteps           = 2000000
    timestep         = 0.0025
    eneout_period    = 1000
    crdout_period    = 1000
    rstout_period    = 40000
    nbupdate_period  = 10
     
    [CONSTRAINTS] 
    rigid_bond       = YES
     
    [ENSEMBLE] 
    ensemble         = NPT
    tpcontrol        = BUSSI
    temperature      = 300.0
    pressure         = 1.0
     
    [BOUNDARY] 
    type             = PBC

    [RPATH] 
    nreplica         = 16 
    rpath_period     = 0
    delta            = 0
    rest_function    = 1 2
    fix_terminal     = YES

    [SELECTION]
    group1           = ai:15
    group2           = ai:17
    group3           = ai:19
    group4           = ai:25
    group5           = ai:27

    [RESTRAINTS] 
    nfunctions       = 2

    function1        = DIHED
    constant1        = 20.0 20.0 20.0 20.0 20.0 20.0 20.0 20.0 \
                       20.0 20.0 20.0 20.0 20.0 20.0 20.0 20.0
    reference1       =  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0 \
                        0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
    select_index1    = 1 2 3 4 # PHI

    function2        = DIHED
    constant2        = 20.0 20.0 20.0 20.0 20.0 20.0 20.0 20.0 \
                       20.0 20.0 20.0 20.0 20.0 20.0 20.0 20.0
    reference2       =  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0 \
                        0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
    select_index2    = 2 3 4 5 # PSI

We also provide a shell script `dih.sh` in the folder to calculate the
collective variables (Φ and Ψ) with `trj_analysis` tool. By executing
the script file, the collective variable file of each replica will be
generated automatically.

#### [8. MBAR analysis of [the]{style="color: #000000"} free energy profile along with the images]{#8_MBAR_analysis_of_the_free_energy_profile_along_with_the_images}

In the following part of this tutorial, we evaluate the free energy
profile along the converged pathway by using
[the]{style="color: #000000"} `mbar_analysis`, `pathcv_analysis` and
`pmf_analysis` tools in GENESIS. First, we use `mbar_analysis` tool to
[analyze]{style="color: #000000"} the collective variable files and
calculate the relative free energy profile among the images. At the same
time, we obtain the weight of each frame in every replica\[5\].

    # Change directory
    $ cd ../8_mbar/


    # Calculate the relative free energy profile and unbiased weights with MBAR
    $ ../bin/mbar_analysis INP > log

The content of [`INP`]{style="font-family: terminal, monaco, monospace"}
is shown below.
[`pathfile=../6_rpath_prod/last.path`]{style="background-color: #f0f0f0;font-family: terminal, monaco, monospace"}
[refers to ]{style="color: #000000"}the path file of the final converged
pathway (obtained in the previous step).
[`cvfile=../7_umbrella/umb_{}.cv`]{style="background-color: #f0f0f0;font-family: terminal, monaco, monospace"}
[refers to]{style="color: #000000"} the collective variable files.

    [INPUT] 
    psffile            = ../1_setup/wbox.psf
    pdbfile            = ../1_setup/start.pdb
    pathfile           = ../6_rpath_prod/last.path  # file of final pathway
    cvfile             = ../7_umbrella/umb_{}.cv    # collective variable file

    [OUTPUT] 
    fenefile           = fene.dat             # relative free energy file
    weightfile         = {}.weight            # weight file

    [MBAR] 
    dimension          = 1       # dimension of the free energy profile
    num_replicas       = 16      # number of replicas
    input_type         = CV      # type of input file
    nblocks            = 1
    newton_iteration   = 100
    temperature        = 300.0
    target_temperature = 300.0
    tolerance          = 10E-08
    rest_function1     = 1 2
    read_ref_path      = 0       # reference for each replica is defined explicitly

    [RESTRAINTS] 
    constant1          = 0.006092 0.006092 0.006092 0.006092 \
                         0.006092 0.006092 0.006092 0.006092 \
                         0.006092 0.006092 0.006092 0.006092 \
                         0.006092 0.006092 0.006092 0.006092
    reference1         =  -58.08  -64.89  -76.07  -84.94 \
                          -96.24  -94.15  -89.72  -86.64 \
                          -84.33  -78.93  -83.10  -85.77 \
                          -80.74  -68.85  -62.55  -61.05
    is_periodic1       = YES
    box_size1          = 360.0

    constant2          = 0.006092 0.006092 0.006092 0.006092 \
                         0.006092 0.006092 0.006092 0.006092 \
                         0.006092 0.006092 0.006092 0.006092 \
                         0.006092 0.006092 0.006092 0.006092
    reference2         =  -48.95  -35.02  -24.24  -11.48 \
                           -0.95   14.45   29.34   44.57 \
                           59.94   74.49   89.46  104.77 \
                          119.45  129.44  143.63  159.10
    is_periodic2       = YES
    box_size2          = 360.0

<div>

**[ ]{style="color: #ff6600"}**It should be noted that, the unit of the
force constant [for]{style="color: #000000"} dihedral restraint is
[different between `spdyn` and `mbar_analysis`. In `spdyn` it is
]{style="color: #000000"}kcal/mol/rad^n^(n=2 by default), [while in
`mbar_analysis` it is ]{style="color: #000000"}kcal/mol/degree^n^,
[[therefore we need to manually convert the force
constants.]{style="color: #000000"} ]{style="color: #ff0000"} 

[Running the script [will produce the file
`fene.dat`]{style="color: #000000"}]{style="font-size: inherit;text-align: justify"}[[,
which contains the evaluated relative free energies between replicas and
multiple (16 in this tutorial) `*.weight` files containing the unbiased
weights of each snapshot for each replica. We can easily view the
contents by executing the following
command:]{style="font-size: inherit;text-align: justify"}]{style="font-size: inherit;text-align: justify"}

<div>

 

</div>

    $ less fene.dat
         0.0000000000000000
        -0.3488274847237864
        -0.0168096948783365
         0.1146757095587470
        -0.3476903357483172
         0.1914340993220627
         1.9528355578763374
         3.8065227433205662
         3.9759595727924788
         3.6498448820374829
         2.9165617927269176
         1.5828269715965586
         0.1552933280410853
        -1.0700710444742558
        -2.0487692029396110
        -1.3555173225438741

    $ less 1.weight
         1 3.070198503406507E-005
         2 1.433995860895774E-005
         3 2.125093486984715E-005
         4 1.484653430974867E-005
         5 1.487277857447018E-005
         6 1.759031859861885E-005
         7 1.565738966114539E-005
         8 1.713826681786259E-005
         ...

<div>

#### [9. Generate pathcv along the minimum free energy pathway]{#9_Generate_pathcv_along_the_minimum_free_energy_pathway}

</div>

[Next, we call `pathcv_analysis` in GENESIS to obtain the tangential and
perpendicular coordinates to the final minimum free energy
pathway\[4\]. ]{style="font-size: inherit;text-align: justify"}

<div>

 

</div>

    # Change directory
    $ cd ../9_pathcv_analysis/


    # Calculate tangential and orthogonal coordinates to a pathway
    $ ../bin/pathcv_analysis INP > log

The content of [`INP`]{style="font-family: terminal, monaco, monospace"}
is shown below.

    [INPUT]
    pathfile   = ../6_rpath_prod/last.path
    cvfile     = ../7_umbrella/umb_{}.cv

    [OUTPUT]
    pathcvfile = {}.pathcv

    [OPTION]
    nreplica   = 16

[Running the script [will produce the files
`*.pathcv`]{style="color: #000000"}]{style="font-size: inherit;text-align: justify"}[,
which contain the calculated tangential and orthogonal coordinates to a
pathway for each sampled conformation\[4\]. For example, `1.pathcv` is
as follows:]{style="font-size: inherit;text-align: justify"}

</div>

<div>

    $ less 1.pathcv

         1 2.0367752279467917  39.7223852590263604
         2 1.2431857784915374 -13.6652453225284543
         3 1.6107542414331801  11.0016856164995041
         4 1.1703363815449264  -7.1919998044208642
         5 1.2812352527745408 -11.6566657733450807
         6 1.5985929053871142  -7.2291767498512458
         7 1.4110629640701358 -11.8067897485528537
         8 1.8398897407868577 -17.1296149647721840

[The 2nd and 3rd columns are the [tangential and orthogonal coordinates
to the final converged pathway
respectively.]{style="font-size: inherit;text-align: justify"}]{style="color: #000000"}

</div>

<div>

 

</div>

<div>

#### [10. Evaluate the free energy profile along the converged pathway]{#10_Evaluate_the_free_energy_profile_along_the_converged_pathway}

Then, we evaluate free energy profile from the MBAR weights and
tangential coordinates to the pathway by the `pmf_analysis` tool in
GENESIS. This tool reads the outputs (weight files) of `mbar_analysis`
and calculates the potential of mean force (or free energy profile)
along given collective variable coordinates. 

    $ cd ../10_pmf

    # Calculate pmf along pathcv
    $ ../bin/pmf_analysis INP > log

</div>

<div>

[The content of `INP` is shown below.]{style="color: #000000"}

</div>

<div>

 

</div>

<div>

    [INPUT]
    cvfile       = {}.pathcv
    weightfile   = {}.weight
    distfile     = {}.pathdist

    [OUTPUT]
    pmffile      = pathcv.pmf

    [OPTION]
    check_only   = NO
    allow_backup = NO

    nreplica     = 16
    dimension    = 1
    temperature  = 300
    cutoff       = 200

    grids1       = 0.0 17.0 30
    band_width1  = 0.1
    is_periodic1 = NO

</div>

After running the script, we get `pathcv.pmf` whose 1st column contains
pathcv, which is the tangential coordinates to the final converged
pathway, and 2nd column is the free energy profile. The result should
like below.

![](assets/images/2022_04_pmf1d.png){.alignnone
.wp-image-22849 .size-medium loading="lazy" decoding="async" width="300"
height="273"
srcset="wp-content/uploads/2022/04/pmf1d-300x273.png 300w, wp-content/uploads/2022/04/pmf1d-768x698.png 768w, wp-content/uploads/2022/04/pmf1d-20x18.png 20w, wp-content/uploads/2022/04/pmf1d-30x27.png 30w, wp-content/uploads/2022/04/pmf1d-40x36.png 40w, wp-content/uploads/2022/04/pmf1d.png 990w"
sizes="(max-width: 300px) 100vw, 300px"}

This figure shows the free energies profile along the minimum free
energy path connecting the alpha-helix state (pathcv=1) and beta-sheet
state (pathcv=16). Clearly, this is a two-state transition with a high
free energy barrier. Please keep in mind that there might be some errors
in the final free energies profiles due to the limited umbrella sampling
simulation.

#### [References]{#References}

1\. J*. Chem. Phys*. **125**, 024106 (2006)
[](https://aip.scitation.org/doi/10.1063/1.2212942){style="font-size: revert"
target="_blank" rel="noopener noreferrer"}

<div>

2\. [[*J. Phys. Chem. B*]{.cit-title} [2008]{.cit-year-info}[,
**112**]{.cit-volume}[, 11]{.cit-issue}[, 3432--3440
[](https://pubs.acs.org/doi/abs/10.1021/jp0777059){style="font-size: revert"
target="_blank"
rel="noopener noreferrer"}]{.cit-pageRange}]{style="color: initial"}

</div>

<div>

*3. Chemical Physics Letters*,[ **446** (2007) 182--190
[](https://www.sciencedirect.com/science/article/pii/S000926140701086X?via%3Dihub){style="font-size: revert"
target="_blank"
rel="noopener noreferrer"}]{style="font-size: revert;color: initial"}

</div>

<div>

*4. J. Chem. Phys*. **126**, 054103 (2007)
[](https://aip.scitation.org/doi/10.1063/1.2432340){style="font-size: revert"
target="_blank" rel="noopener noreferrer"}

</div>

<div>

5\. *J. Chem. Phys*. **129**, 124105 (2008)
[](https://aip.scitation.org/doi/10.1063/1.2978177){style="font-size: revert"
target="_blank" rel="noopener noreferrer"}

</div>

<div>

 

</div>

------------------------------------------------------------------------

[*Written by Yasuhiro Matsunaga@RIKEN Computational biophysics research
team\
Created August 26, 2016\
Updated By Weitong Ren@RIKEN Theoretical molecular science laboratory\
Nov 15, 2019*]{style="font-size: 10pt"}

[*Updated By Weitong Ren@RIKEN Theoretical molecular science laboratory\
Mar 31, 2022*]{style="font-size: 10pt"}

