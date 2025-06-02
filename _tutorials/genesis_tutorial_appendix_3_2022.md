---
title: "GENESIS Tutorial Appendix 3 (2022)"
excerpt: ""
last_modified_at: 2025-06-03T00:00:57+09:00
layout: single
toc: true
toc_sticky: true
sidebar:
  nav: sidebar-basic
---

# [Atomistic MD simulations of biomolecules on Fugaku]{lang="EN-US"}

[Fugaku](https://www.r-ccs.riken.jp/en/fugaku/about/) is a supercomputer
at RIKEN Center for Computational Science and has 158, 976 nodes. This
machine has been made available through the
[HPCI](https://www.hpci-office.jp/folders/english) project.

GENESIS was one of the target applications during the development of
this supercomputer and has been developed to maximize the performance of
this supercomputer through co-design. The appendix indicate how to
execute atomistic MD simulations on Fugaku.

 The optimized code for Fugaku is available in
[GitHub](https://github.com/genesis-release-r-ccs/genesis) as release
verson 2.0.0.


```
# Download the tutorial file
$ git clone https://github.com/genesis-release-r-ccs/genesis.git
$ cd genesis
$ autoreconf
$ ./configure --enable-mixed --host=Fugaku
$ make
$ make install

```

To compile GENESIS on Fugaku, please do not forget to add
"`--host=Fugaku`".

A option "`--enable-single/mixed/double`" controls default precision of
real type variables. 

We recommend using "--enable-[mixed]" in terms
of simulation stability and accuracy on
[**Fugaku**]. In particular, it has been
reported that the simulation with "--enable-single" give slightly
different box sizes/temperature from those with "--enable-mixed/double".

You can find further information in doc/GENESIS.pdf

After compile, please execute compile test.


```
$ cd ./tests/regression_test
(make a job script)
$ pjsub test_script.sh

```

Templates of `pjsub` script are shown in the Fugaku portal site. (Only registered users) Please find a script with hybrid parallelization type
(MPI/OpenMP) in the site. Please find the detailed explanation of
`pjsub` options from the portal site.

Due to amount of memory, please assign at least 2 nodes for compile
test.


```
./test.py "mpiexec ${bindir}/spdyn " fugaku > regression.log

```

For normal jobs, please write down the line for the execution of
GENESIS.

mpiexec -stdout run_fep1.out \${bindir}/spdyn inp\
\
Although not as fast, GENESIS 1.5.1 and later versions work on Fugaku.
If you would like to use the versions,  "`--host=Fugaku`"  should be
used for configure, however, only "`--enable-single/double`" are
available in the versions.

 

*Written by Chigusa Kobayashi@RIKEN Center for Computational Science\
July 29, 2022*

