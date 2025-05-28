### 9.2 Restraints in the NPT ensemble

Contents

-   [1. Preparation](genesis_tutorial_9.2.md#1_Preparation)
-   [2. Minimization](genesis_tutorial_9.2.md#2_Minimization)
-   [3. Equilibration](genesis_tutorial_9.2.md#3_Equilibration)
    -   [3.1. Equilibration without positional restraint contribution to
        pressure](genesis_tutorial_9.2.md#31_Equilibration_without_positional_restraint_contribution_to_pressure)
    -   [3.2. Equilibration with positional restraint contribution to
        pressure](genesis_tutorial_9.2.md#32_Equilibration_with_positional_restraint_contribution_to_pressure)
-   [4. Production and
    Analysis](genesis_tutorial_9.2.md#4_Production_and_Analysis)

In this tutorial, we will describe how to deal with positional
restraints in the NPT ensemble. Our target biological system here is a
GPCR system, and we investigate the effect of virial calculation from
positional restraint in performing NPT simulations.  

#### [1. Preparation]{#1_Preparation}

For this tutorial, first please download the tutorial
file [tutorial22-9.2.tar.gz](assets/tutorial_files/2022_05_tutorial22-9.2.tar.gz){.mtli_attachment
.mtli_zip}. After downloading, you can see five directories and two
files.

    $ unzip tutorial22-9.2.zip
    $ cd tutorial-9.2
    $ ls
    1.min     2.equil_pressure_virial step5_input.pdb toppar
    2.equil   restraints              step5_input.psf

System and force fields information are described in
[toppar[,]{style="color: #000000;"} restraints[, step5_input.pdb,
]{style="color: #000000;"}]{style="color: #0000ff;"}and
[[step5_input.psf. ]{style="color: #000000;"}]{style="color: #0000ff;"}

#### [2. Minimization]{#2_Minimization}

[[[Let's consider the first step
(]{style="font-size: revert; color: initial;"}*step 1*[) in our
tutorial: minimization. The
i]{style="font-size: revert; color: initial;"}nput file for minimization
is in the [1.min
[directory]{style="color: #000000;"}]{style="color: #0000ff;"}. Please
run the minimization with the following
command:]{style="color: #000000;"}]{style="color: #0000ff;"}

    $ cd 1.min
    $ mpirun -np 8 {PATH}/spdyn inp > out

[Here,]{style="font-size: revert; color: initial;"}[ we assign
positional restraints in three selection groups: (1) heavy backbone
atoms, (2) heavy sidechain atoms, and (3) phosphate atoms in POPC
lipids. They are written as]{style="font-size: revert; color: initial;"}

    # Selection group
    [SELECTION]

    group1 = (sid:PROA) and backbone
    group2 = (sid:PROA) and not backbone and not hydrogen
    group3 = sid:MEMB and ((rnam:POPC and (an:P)))

    # Positional restraint function
    [RESTRAINTS]
    nfunctions    = 3
    # Restraint function of the first group
    function1     = POSI
    constant1     = 10.0
    select_index1 = 1
    # Restraint function of the second group
    function2     = POSI
    constant2     = 5.0
    select_index2 = 2
    # Restraint function of the third group
    function3     = POSI
    direction3    = Z
    constant3     = 2.5
    select_index3 = 3

For more information on how to setup the rest of the options, please see
Tutorial 3.2. 

#### [3. Equilibration]{#3_Equilibration}

[[We perform equilibration with two different conditions: with and
without pressure from positional restraint force. In
[2.equil]{style="color: #0000ff;"}, the virial contribution from the
positional restraint force is not considered in the pressure evaluation
of the NPT simulations (It is the default in GENESIS 1.X and 2.0). On
the other hand, in [2.equil_pressure_virial]{style="color: #0000ff;"},
virial from positional restraint
]{style="color: #000000;"}]{style="color: #0000ff;"}force is included.

##### [3.1. Equilibration without positional restraint contribution to pressure]{#31_Equilibration_without_positional_restraint_contribution_to_pressure}

In the equilibration run, we are considering the following steps:

*step 2: NVT simulation with 1 fs time step (125000 steps)*

*step 3: NVT simulation with 1 fs time step (125000 steps)*

*step 4: NPT simulation with 1 fs time step (125000 steps)*

*step 5: NPT simulation with 2 fs time step (250000 steps)*

*step 6: NPT simulation with 2 fs time step (250000 steps)*

*step 7: NPT simulation with 1 fs time step (25000 steps)*

In [[[2.equil, ]{style="color: #0000ff;"}we can see input files as
following:]{style="color: #000000;"}]{style="color: #0000ff;"}

    $ cd 2.equil
    $ ls
    box.py inp10   inp12   inp2   inp4   inp6   inp8
    inp1   inp11   inp13   inp3   inp5   inp7   inp9

inp1 to inp6 are input files for step2 to step 7, respectively. They all
have positional restraint functions on the same three groups previously
selected in the minimization. The restraint forces are weaken as we move
on the next step.

*step 2: NVT simulation with 1 fs time step*

    $ ls inp1

    # restraint function in input file inp1
    ...
    [RESTRAINTS]
    nfunctions    = 3
    function1     = POSI
    constant1     = 10.0
    select_index1 = 1
    function2     = POSI
    constant2     = 5.0
    select_index2 = 2
    function3     = POSI
    direction3    = Z
    constant3     = 2.5
    select_index3 = 3

*step 3: NVT simulation with 2 fs time step*

    $ ls inp2

    # restraint function in input file inp2
    ...
    [RESTRAINTS]
    nfunctions    = 3
    function1     = POSI
    constant1     = 5.0
    select_index1 = 1
    function2     = POSI
    constant2     = 2.5
    select_index2 = 2
    function3     = POSI
    direction3    = Z
    constant3     = 2.5
    select_index3 = 3

*step 4: NPT simulation with 1 fs time step*

    $ ls inp3

    # restraint function in input file inp3
    ...
    [RESTRAINTS]
    nfunctions    = 3
    function1     = POSI
    constant1     = 2.5
    select_index1 = 1
    function2     = POSI
    constant2     = 1.0
    select_index2 = 2
    function3     = POSI
    direction3    = Z
    constant3     = 1.0
    select_index3 = 3

*step 5: NPT simulation with 2 fs time step*

    $ ls inp4

    # restraint function in input file inp4
    ...
    [RESTRAINTS]
    nfunctions    = 3
    function1     = POSI
    constant1     = 1.0
    select_index1 = 1
    function2     = POSI
    constant2     = 0.5
    select_index2 = 2
    function3     = POSI
    direction3    = Z
    constant3     = 0.5
    select_index3 = 3

*step 6: NPT simulation with 2 fs time step*

    $ ls inp5

    # restraint function in input file inp5
    ...
    [RESTRAINTS]
    nfunctions    = 3
    function1     = POSI
    constant1     = 0.5
    select_index1 = 1
    function2     = POSI
    constant2     = 0.1
    select_index2 = 2
    function3     = POSI
    direction3    = Z
    constant3     = 0.1
    select_index3 = 3

*step 7: NPT simulation with 1 fs time step*

    $ ls inp6

    # restraint function in input file inp6
    ...
    [RESTRAINTS]
    nfunctions    = 3
    function1     = POSI
    constant1     = 0.1
    select_index1 = 1
    function2     = POSI
    constant2     = 0.0
    select_index2 = 2
    function3     = POSI
    direction3    = Z
    constant3     = 0.0
    select_index3 = 3

You can perform these by executing genesis with each input file:

    $ cd 2.equil 
    $ mpirun -np 8 {PATH}/spdyn inp1 > out1 #step 2
    $ mpirun -np 8 {PATH}/spdyn inp2 > out2 #step 3
    $ mpirun -np 8 {PATH}/spdyn inp3 > out3 #step 4
    $ mpirun -np 8 {PATH}/spdyn inp4 > out4 #step 5
    $ mpirun -np 8 {PATH}/spdyn inp5 > out5 #step 6
    $ mpirun -np 8 {PATH}/spdyn inp6 > out6 #step 7

##### [3.2. Equilibration with positional restraint contribution to pressure]{#32_Equilibration_with_positional_restraint_contribution_to_pressure}

[[We do the same equilibration procedure with positional restraint
contribution to pressure in
[2.equil_pressure_virial]{style="color: #0000ff;"}]{style="color: #000000;"}]{style="color: #0000ff;"}[[:]{style="color: #000000;"}]{style="color: #0000ff;"} 

    $ cd 2.equil_pressure_virial
    $ mpirun -np 8 {PATH}/spdyn inp1 > out1 #step 2
    $ mpirun -np 8 {PATH}/spdyn inp2 > out2 #step 3
    $ mpirun -np 8 {PATH}/spdyn inp3 > out3 #step 4
    $ mpirun -np 8 {PATH}/spdyn inp4 > out4 #step 5
    $ mpirun -np 8 {PATH}/spdyn inp5 > out5 #step 6
    $ mpirun -np 8 {PATH}/spdyn inp6 > out6 #step 7

Input files from inp1 to inp6 are the same as those in 2.equil except
for one thing: to allow pressure evaluation from positional restraint
force, we write 

    pressure_position = YES

in the \[RESTRAINTS\] section.

#### [4. Production and Analysis]{#4_Production_and_Analysis}

After finishing the equilibration, we perform the produciton run without
positional restraints (*step 8*). It can be done by running genesis with
inp7 and inp8. In the case of [2.equil]{style="color: #0000ff;"}, the
produciton run is done by the following commands:

    $ cd 2.equil 
    $ mpirun -np 8 {PATH}/spdyn inp7 > out7 
    $ mpirun -np 8 {PATH}/spdyn inp8 > out8 

Similary, we can do the same thing in
[2.equil_pressure_virial]{style="color: #0000ff;"}:

    $ cd 2.equil_pressure_virial 
    $ mpirun -np 8 {PATH}/spdyn inp7 > out7 
    $ mpirun -np 8 {PATH}/spdyn inp8 > out8 

Finally, we investigate system size changes (x and z dimensions) during
the equilibration and production steps. In each directory, [2.equil
[and]{style="color: #000000;"} 2.equil_pressure_virial, [please execute
the python
script]{style="color: #000000;"}]{style="color: #0000ff;"}[:]{style="color: #000000;"}

    $ python box.py > box.dat

In the file, box.dat, the first, second, and third columns represent
time (ps), system size in x dimension, and system size in the z
dimension, respectively. In the case of
[2.equil]{style="color: #0000ff;"}, the system size as a function of the
MD simulation time is shown as

![](assets/images/2022_01_no_pos_viri1.jpg){.wp-image-18823
.aligncenter fetchpriority="high" decoding="async" width="424"
height="317"
srcset="wp-content/uploads/2022/01/no_pos_viri1.jpg 2256w, wp-content/uploads/2022/01/no_pos_viri1-300x224.jpg 300w, wp-content/uploads/2022/01/no_pos_viri1-1024x766.jpg 1024w, wp-content/uploads/2022/01/no_pos_viri1-768x575.jpg 768w, wp-content/uploads/2022/01/no_pos_viri1-1536x1149.jpg 1536w, wp-content/uploads/2022/01/no_pos_viri1-2048x1532.jpg 2048w, wp-content/uploads/2022/01/no_pos_viri1-20x15.jpg 20w, wp-content/uploads/2022/01/no_pos_viri1-30x22.jpg 30w, wp-content/uploads/2022/01/no_pos_viri1-40x30.jpg 40w"
sizes="(max-width: 424px) 100vw, 424px"}

We can easily understand that there is a large change of the system size
during the equilibration and production. This means that the system is
not well equilibrated when we neglect pressure contribution from
positional restraint forces. Unlike [2.equil]{style="color: #0000ff;"},
the system size does not change during the equilibration and production
steps in
[2.equil_pressure_virial[. ]{style="color: #000000;"}]{style="color: #0000ff;"}

![](assets/images/2022_01_pos_viri1.jpg){.wp-image-18822
.aligncenter decoding="async" width="414" height="310"
srcset="wp-content/uploads/2022/01/pos_viri1.jpg 2254w, wp-content/uploads/2022/01/pos_viri1-300x225.jpg 300w, wp-content/uploads/2022/01/pos_viri1-1024x767.jpg 1024w, wp-content/uploads/2022/01/pos_viri1-768x575.jpg 768w, wp-content/uploads/2022/01/pos_viri1-1536x1150.jpg 1536w, wp-content/uploads/2022/01/pos_viri1-2048x1534.jpg 2048w, wp-content/uploads/2022/01/pos_viri1-20x15.jpg 20w, wp-content/uploads/2022/01/pos_viri1-30x22.jpg 30w, wp-content/uploads/2022/01/pos_viri1-40x30.jpg 40w"
sizes="(max-width: 414px) 100vw, 414px"}

Therefore, we should be careful when performing NPT simulations with
positional restraints. To obtain a reliable structure, we recommend
including the virial contribution from the positional restraints by
writing `pressure_position = YES`. Similarly, we recommend writing
`pressure_rmsd = YES` when performing NPT simulations with RMSD
restraints.

 

*Written by Jaewoon Jung@RIKEN R-CCS. March, 2022*

*Updated by Jaewoon Jung@RIKEN R-CCS. May, 16, 2022*

