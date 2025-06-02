---
title: "GENESIS Tutorial Appendix 2 (2022)"
excerpt: ""
last_modified_at: 2025-06-03T00:00:57+09:00
layout: single
toc: true
toc_sticky: true
sidebar:
  nav: sidebar-basic
---

# [Atomistic MD simulations of biomolecules on GPU clusters]{lang="EN-US"}

Graphic Processing Unit (GPU) can accelerate the MD simulations from CPU
only systems. GENESIS is now making use of GPUs by assigning real-sapce
non-bonded interactions on GPU, and bonded and reciprocal-space
non-bonded interactions on CPU. This appendix explain how to execute
atomistic MD simulations on GPU clusters.  Please note that we are only
considering NVIDIA GPU cards in this tutorial.

Before compiling with GPU, please check if GPU cards are available and
CUDA libraires are installed. 


```
# Check NVIDIA GPU cards
$ nvidia-smi
Wed Mar 30 10:25:18 2022
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 495.29.05     Driver Version: 495.29.05       CUDA Version: 11.5 |
|-------------------------------+----------------------+----------------------+
| GPU Name Persistence-M        | Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan Temp Perf Pwr:Usage/Cap   | Memory-Usage         | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
| 0 NVIDIA GeForce   ...    Off | 00000000:3B:00.0 Off |                  N/A |
| 45%   55C   P2    158W / 350W |   2092MiB / 24268MiB |     80%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
| 1 NVIDIA GeForce   ...    Off | 00000000:B1:00.0 Off |                  N/A |
| 51%   56C   P2    170W / 350W |   2098MiB / 24268MiB |     77%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+

# Check NVIDA compiler
$ nvcc --version
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2021 NVIDIA Corporation
Built on Mon_Sep_13_19:13:29_PDT_2021
Cuda compilation tools, release 11.5, V11.5.50
Build cuda_11.5.r11.5/compiler.30411180_0

```

If you execute MD jobs from host node, the GPU card information might
not be visible on the host node. In this case, please check if you can
see GPU card information in the computational node. 

The recent GENESIS code for GPU clusters can be downloaded from
[GitHub](https://github.com/genesis-release-r-ccs/genesis-2.0). 


```
# Download the tutorial file 
$ git clone https://github.com/genesis-release-r-ccs/genesis-2.0.git 
$ cd genesis-2.0 
$ autoreconf 
$ ./configure --enable-single --enable-gpu 
$ make 
$ make install

```

To accelerate speed with GPU, we need to use single precision in
real-space non-bonded interactions. Please type "`--enable-single`" or
"`--enable-mixed`" when executing configure.

If you run MD simulations for an unstable structure, you might use
`nonb_limiter` option. In the case of genesis 1.7.X, compiling without
gpu option is required. However, with genesis 2.0b, you don't need to do
it. Even the program is compiled with gpu option, you can select
`nonbond_kernel=generic` to assign the option.

[To fully make use of GPU cards, please check the number of GPU cards in each node and decide the MPI numbers such that the MPI process number is the multiple of GPU card number in each node.]

------------------------------------------------------------------------

*Written by Jaewoon Jung@RIKEN Center for Computational Science\
June, 2022*

