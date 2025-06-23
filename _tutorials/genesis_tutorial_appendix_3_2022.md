---
title: "GENESIS Tutorial Appendix 3 (2022)"
gpos: 999.003
excerpt: ""
last_modified_at: 2025-06-03T00:00:57+09:00
layout: single
toc: true
toc_sticky: true
sidebar:
  nav: sidebar-basic
---

# Atomistic MD simulations of biomolecules on Fugaku

[Fugaku](https://www.r-ccs.riken.jp/en/fugaku/about/) is a supercomputer
at RIKEN Center for Computational Science and has 158, 976 nodes. This
machine has been made available through the
[HPCI](https://www.hpci-office.jp/folders/english) project.


GENESIS was selected as one of the target applications during the development of
the Fugaku supercomputer, and has been co-designed to maximize its performance
on this platform.  The appendix provides instructions on how to run atomistic MD
simulations on Fugaku.

The optimized GENESIS code for Fugaku is available on 
[GitHub](https://github.com/genesis-release-r-ccs/genesis)
in version 2.x.

```bash
# Download the source code
$ git clone https://github.com/genesis-release-r-ccs/genesis.git
$ cd genesis
$ autoreconf
$ ./configure --enable-mixed --host=Fugaku
$ make
$ make install
```

To compile GENESIS on Fugaku, please do not forget to add `--host=Fugaku`.

A option `--enable-single/mixed/double` controls default precision of
real type variables. 

We recommend using `--enable-mixed` for improved simulation stability and
accuracy on Fugaku. In particular, it has been reported that simulations
compiled with `--enable-single` may produce slightly different box sizes and
temperatures compared to those with `--enable-mixed` or `--enable-double`.
{: .notice--warning}

Please refer to `doc/GENESIS.pdf` for more information.

After the compilation, please execute `compile test`.
```bash
$ cd ./tests/regression_test
(make a job script)
$ pjsub test_script.sh
```

Templates for `pjsub` scripts are available on the Fugaku portal site (accessible
to registered users only).  Please refer to the site to find a script configured
for hybrid parallelization (MPI/OpenMP). Detailed explanations of the available
`pjsub` options can also be found there.

Due to amount of memory, please assign at least 2 nodes for compile
test.

```bash
./test.py "mpiexec ${bindir}/spdyn " fugaku > regression.log
```

For normal jobs, please write down the line for the execution of
GENESIS.
```bash
mpiexec -stdout run_fep1.out ${bindir}/spdyn inp
```

Although not as fast as the optimized version, GENESIS 1.5.1 and later are
compatible with Fugaku. If you wish to use these versions, please specify
`--host=Fugaku` during configuration. Note, however, that only `--enable-single`
or `--enable-double` are available in these versions, whereas `--enable-mixed`
is not supported.

*Written by Chigusa Kobayashi@RIKEN Center for Computational Science\
July 29, 2022*
{: .notice}

