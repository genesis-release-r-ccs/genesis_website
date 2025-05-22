---
title: "Installation"
excerpt: "Installation of GENESIS."
last_modified_at: 2025-05-08T11:59:26+09:00
layout: single
toc: true
sidebar:
  nav: sidebar-basic
---

## Requirements

The recommended compilers, preprocessors, and libraries for **GENESIS
ver. 1.4.0 or later** are listed below. Please make sure that at least
one of them in each section is installed on your system. If the users do
not use the Intel or Fujitsu compilers, the combination of GCC compiler,
GCC preprocessor, and OpenMPI is recommended. Detailed information about
the requirements for each version is described in the user manual (see
Chapter "Getting started").

-   **Operating systems**
    -   Linux
    -   Mac OSX
    -   Window 10, 11 (spdyn, ver 1.7.1)
-   **Fortran and C compilers**
    -   [GNU compiler](https://gcc.gnu.org/) `gfortran`, `gcc` (version 7.0.0 or
        higher is required.)
    -   Intel compiler `ifort`, `icc` (`ifx`, `icx` are available from
        ver 2.1.3.)
    -   Fujitsu compiler `frtpx`, `fccpx`
    -   Cygwin/mingw (spdyn, ver 1.7.1)
-   **Preprocessors**
    -   GCC preprocessor `cpp`
    -   Intel preprocessor `fpp`
    -   Fujitsu compiler `frtpx`
-   **MPI libraries for parallel computing**
    -   [OpenMPI](https://www.open-mpi.org/) `mpirun`, `mpif90`, `mpicc`
    -   Intel MPI `mpiexec`, `mpiifort`, `mpiicc` (`mpiifx`, `mpiicx`
        are available from ver 2.1.3.)
    -   Fujitsu MPI
    -   Microsoft MPI (spdyn, ver 1.7.1)
-   **Numerical libraries for mathematical algorithms**
    -   [LAPACK/BLAS](http://www.netlib.org/lapack/)
    -   Intel Math Kernel Library (MKL)
    -   Fujitsu Scientific Subroutine Library (SSL II)
    -   OpenBlas
-   **GPU (Optional)**
    -   NVIDIA GPU cards which support [Compute Capability
        (CC)](https://developer.nvidia.com/cuda-gpus) 3.5 or higher (except for 5.X)
    -   The following GPU cards and CUDA versions have been tested by
        the GENESIS developers
        -   NVIDIA K20, K40, P100, TITAN V, GTX 1080, GTX 1080Ti, RTX
            2080, RTX 2080Ti, RTX 3090, RTX A6000, RTX 4500 ada (from
            ver 2.1.3), A30, A100, H100 (from ver 2.1.3) , GH200 (from
            ver 2.1.3)
        -   [CUDA](https://developer.nvidia.com/cuda-toolkit) ver. 8.0, 9.0, 9.1, 9.2, 10.0,
            11.2, 11.4, 11.5, 12.1-12.6
        -   Compute Capability 3.5, 3.7, 6.0, 6.1, 6.2

##  General scheme for installation

The source code of GENESIS is available in the [Download page](/docs/download/).
The users have to first uncompress the download file in an appropriate
directory. In order to compile the source code, the users execute a "configure"
script in the directory. This script automatically detects appropriate
compilers, preprocessors, and libraries in the users' computer, and create
"`Makefile`". The users may have to add some options in the command according to
the user's computer environment. For example, "`--enable-gpu`" should be
appended to turn on the GPGPU calculation (see Chapter "Getting started" in the
user manual). After the "configure" command is successfully finished, type the
following command to compile and install GENESIS. All programs of GENESIS are
compiled and installed into the "`genesis-X.Y.Z/bin`" directory by default. The followings
are example commands to install GENESIS for typical Linux workstations.

    $ mv ~/Downloads/genesis-X.Y.Z.tar.bz2 ./
    $ tar xvfj genesis-X.Y.Z.tar.bz2
    $ cd genesis-X.Y.Z
    $ ./configure
    $ make install

If the users encountered a failure during
the installation, check the error message carefully. In most cases,
errors are caused by invalid path of compilers and libraries. The
followings are possible suggestions to solve frequent problems.

### Trouble shooting 

(Last update: Oct. 10, 2024)

#### Configure

-   If the error message is like "configure: error: Fortran compiler
    cannot create executables", any Fortran compilers might not be
    detected. Basically, the configure script looks for "mpiifort",
    "mpif90", or "mpifrtpx" for Fortran compiler, and "mpiicc", "mpicc",
    or "mpifccpx" for C compiler. If one of them was not detected in
    your computer, the configure will be failed with such error. To
    solve such problems, you may have to set the path to the executables
    and libraries about MPI in "\~/.bashrc" or specify the compilers
    explicitly in the configure command. You should also check the
    typing mistakes in the path.
-   If recommended software is not used for compilation, warning
    messages might be displayed in the terminal when the configure
    command is executed. That message is just a warning (not an error),
    and you can continue the compilation. However, we strongly recommend
    you perform installation check in such cases
-   In some supercomputer systems, "module load \[module\]" command
    might be required before the configure command. See the user guide
    of the supercomputer system.
-   Make an attempt to execute "autoreconf" before the configure. If
    your computer environment is significantly different from the
    developer's one, or if you modified "configure.ac" or "Makefile.am"
    by yourself, this attempt may work well.

#### Make install

-   If the error message is like "/usr/bin/ld: cannot find -lblas" or
    "/usr/bin/ld: cannot find -llapack", make sure that the BLAS or
    LAPACK libraries are installed in the computer. The users may also
    have to set the path to the libraries in the "configure" command
    with the "`LAPACK_LIBS`" or "`LAPACK_PATH`" option.

#### Other minor errors

-   If `Makefile.depends` is lost or broken, parallel compilation using
    `-j` option will fail. If you encounter this situation, please go to
    the directory where the error occurred and check the existence of
    `Makefile.depends`. If Makefile.depends does not exist, you can
    create the file by executing `make depend`.

###  Verify the installation

The users can verify the installation of GENESIS by using "test sets" prepared
by the developers, which are available in the [Download page](/docs/download/).
The following commands verify the installed spdyn. For detailed usage of the
test sets, see Chapter "Getting started" in the user manual.

    $ mv ~/Downloads/tests-X.Y.Z.tar.bz2 ./
    $ tar xvfj tests-X.Y.Z.tar.bz2
    $ cd tests-X.Y.Z/regression_test
    $ export OMP_NUM_THREADS=1
    $ ./test.py "mpirun -np 8 /home/user/genesis-X.Y.Z/bin/spdyn"

##  Installation for various computer systems

-   [Installation/usage on GPU workstations](/docs/usage_on_gpu_workstations/)
-   [Installation/usage on Intel PC cluster with/without GPU](/docs/usage_on_gpu_workstations/)
-   [Installation/usage on Fugaku at RIKEN R-CCS](/tutorials/genesis_tutorial_appendix_3_2022/)

<!--
## Old systems

-   [Installation/usage on HOKUSAI GreatWave (FX100) and BigWaterfall at RIKEN ACCC](/docs/usage_on_hokusai/)
-   [Installation/usage on K-computer at RIKEN R-CCS](/docs/usage_on_k_computer/)
-   [Installation/usage on TSUBAME2.5 at Tokyo Institute of Technology](/docs/usage_on_tsubame/)
-->
