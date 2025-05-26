---
title: "Installation"
excerpt: "Installation of GENESIS."
last_modified_at: 2025-05-08T11:59:26+09:00
layout: single
toc: true
toc_sticky: true
sidebar:
  nav: sidebar-basic
---

## Requirements

The recommended compilers, preprocessors, and libraries for **GENESIS version
1.4.0 or later** are listed below. Please ensure that at least one option from
each category is installed on your system. If you are not using the Intel or
Fujitsu compilers, we recommend the combination of the GCC compiler, GCC
preprocessor, and OpenMPI.

For detailed version-specific requirements, please refer to the User Manual,
Chapter “Getting Started”.

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

Instructions for obtaining the GENESIS source code are provided on the [Download page](/docs/download/).
After downloading, first extract the archive to an appropriate directory.

To compile the source code, run the `configure` script in the extracted directory. 
This script will automatically detect suitable compilers, preprocessors, and libraries on your system, and then generate a `Makefile`. 
Depending on your environment, you may need to add additional options to the configure command. 
For example, to enable GPGPU acceleration, use the `--enable-gpu` option (see the "Getting Started" chapter in the User Manual for details).

Once configure completes successfully, compile and install GENESIS by executing the following command. 
By default, all programs will be installed in the `genesis/bin` directory.

Below are example commands for installing GENESIS on typical Linux workstations.

```bash
$ mv ~/Downloads/genesis-X.Y.Z.tar.bz2 ./
$ tar xvfj genesis-X.Y.Z.tar.bz2
$ cd genesis-X.Y.Z
$ ./configure
$ make install
```

If you encounter an error during installation, please review the error message
carefully. In most cases, the issue is caused by incorrect paths to compilers or
libraries.


### Trouble shooting 

(Last update: Oct. 10, 2024)

#### Configure

- If you encounter an error such as "configure: error: Fortran compiler cannot create executables", it is likely that no suitable Fortran compiler was detected. By default, the configure script looks for `mpiifort`, `mpif90`, or `mpifrtpx` as Fortran compilers, and `mpiicc`, `mpicc`, or `mpifccpx` for C compilers. If none of these are available in your environment, the configuration process will fail. To resolve this, ensure that the appropriate compiler and MPI library paths are correctly set in your `~/.bashrc` (or equivalent shell configuration file), or specify them explicitly in the configure command. Double-check for any typos in the paths as well.
- If you are using compilers or libraries other than the recommended ones, warning messages may appear during the configure step. These are only warnings (not errors), and compilation can still proceed. However, we strongly recommend performing an installation check in such cases.
- On some supercomputing systems, you may need to run `module load [module]` before executing the `configure` command. Please consult the user guide of your system for more details.
- If you experience unexpected configuration errors, especially after editing `configure.ac` or `Makefile.am`, or if your environment differs significantly from the developer’s, try running `autoreconf` before executing `configure`. This may help resolve compatibility issues.


#### Make install

- If you see an error such as "`/usr/bin/ld: cannot find -lblas`" or "`/usr/bin/ld: cannot find -llapack`", it indicates that the BLAS or LAPACK libraries are not found on your system. Please ensure that these libraries are properly installed. You may also need to specify their path explicitly in the `configure` command using the `LAPACK_LIBS` or `LAPACK_PATH` options.

#### Other minor errors

- If `Makefile.depends` is missing or corrupted, parallel compilation using the `-j` option will fail.  To resolve this issue, navigate to the directory where the error occurred and check whether the `Makefile.depends` file exists. If it does not, you can generate it by running the command `make depend`.


### Verify the installation


Users can verify the successful installation of GENESIS using the official test sets provided by the developers, included in the GENESIS package on [GitHub](https://github.com/genesis-release-r-ccs/genesis/tree/main/tests).  
The following commands demonstrate how to validate the spdyn program. For more details on using the test sets, please refer to the "Getting Started" chapter in the user manual.

```bash
$ mv ~/Downloads/tests-X.Y.Z.tar.bz2 ./
$ tar xvfj tests-X.Y.Z.tar.bz2
$ cd tests-X.Y.Z/regression_test
$ export OMP_NUM_THREADS=1
$ ./test.py "mpirun -np 8 /home/user/genesis-X.Y.Z/bin/spdyn"
```

## Platform-Specific Installation Instructions

- [Installation/usage on GPU workstations](/docs/usage_on_gpu_workstations/)
- [Installation/usage on Intel PC cluster with/without GPU](/docs/usage_on_gpu_workstations/)
- [Installation/usage on Fugaku at RIKEN R-CCS](/tutorials/genesis_tutorial_appendix_3_2022/)

<!--
## Old systems
-   [Installation/usage on HOKUSAI GreatWave (FX100) and BigWaterfall at RIKEN ACCC](/docs/usage_on_hokusai/)
-   [Installation/usage on K-computer at RIKEN R-CCS](/docs/usage_on_k_computer/)
-   [Installation/usage on TSUBAME2.5 at Tokyo Institute of Technology](/docs/usage_on_tsubame/)
-->
