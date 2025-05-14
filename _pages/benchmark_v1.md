---
permalink: /benchmark_v1/
title: "Benchmark (GENESIS ver ≤1.4)"
excerpt: "Benchmark of GENESIS ver ≤1.4."
last_modified_at: 2025-05-13T11:59:59+09:00
toc: true
toc_sticky: true
---




## Benchmark of GENESIS 1.3 and 1.4 on PC clusters

- MD conditions:
    - **Force field**: CHARMM
    - **Integrator**: VVER (PME every 2 steps)
    - **Ensemble**: NVE
    - **Time step**: 2 fs
- Hardware and compilation options:
    - **CPU**: Intel Xeon Gold 6142 (2.6 GHz)
    - **GPU**: Geforce GTX 1080 Ti
    - **Precision**: Mixed precision

### DHFR (23,558 atoms) 

![](/assets/images/2019_06_dhfr_charmm-1.jpg){: width="600px" .align-center}
- **switch**, **cutoff**, **pairlist** = 10, 12, 13.5 Å 


### STMV (1,066,628 atoms) 

![](/assets/images/2019_06_stmv_charmm.jpg){: width="600px" .align-center}
- **switch**, **cutoff**, **pairlist** = 10, 12, 13.5 Å 


##  Benchmark of GENESIS 1.0 and 1.1

- MD conditions:
    - **Integrator**: VVER or RESPA (PME every 2 steps)
    - **Ensemble**: NVE
    - **Time step**: 2 fs
- Hardware and compilation options:
    - **Intel and GPU benchmarks (CPU, CPU+GPU)**
        - CPU: Intel(R) Xeon(R) CPU E5-2670 0 @ 2.60GHz (2 CPU x 8 cores/node)
        - GPU: NVIDIA Tesla K20x (2 GPUs/node)
        - Compiler: openmpi 1.10.1 + intel (fortran/C) 15.0.5.223 + cuda 6.5
        - Precision : double precision (dp) or mixed precision (sp)
    - **K computer benchmarks**
        - CPU: SPARC64(TM) VIIIFx (8 cores/node)
        - Compiler: Fujitsu compiler
        - Precision: double precision

### MSPA (216,726 atoms) on Intel

![](/assets/images/2016_02_uun_NVE.png){: width="600px" .align-center}
- **Force field**: CHARMM c36
- **PME grid** = 144, 144, 144
- **switch**, **cutoff**, **pairlist** = 10, 12, 13.5 Å 

### STMV (1,066,628 atoms) on Intel

![](/assets/images/2016_02_stmv_NVE.png){: width="600px" .align-center}
- **Force field**: CHARMM
- **PME grid** = 256, 256, 256
- **switch**, **cutoff**, **pairlist** = 9, 10, 11.5 Å 

### STMV (1,066,628 atoms) on Intel

![](/assets/images/2016_02_stmv_NVE.png){: width="600px" .align-center}
- **Force field**: CHARMM
- **PME grid** = 256, 256, 256
- **switch**, **cutoff**, **pairlist** = 9, 10, 11.5 Å 

### 27 × STMV (28,798,956 atoms) on K

![](/assets/images/2016_02_stmv27_K_NVE.png){: width="600px" .align-center}
- **Force field**: CHARMM
- **PME grid** = 512, 512, 512
- **switch**, **cutoff**, **pairlist** = 9, 10, 11.5 Å 

