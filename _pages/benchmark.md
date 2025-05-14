---
permalink: /benchmark/
title: "Benchmark"
excerpt: "Benchmark of GENESIS."
last_modified_at: 2025-05-12T11:59:26+09:00
toc: true
toc_sticky: true
---

## Benchmark of GENESIS 2.0 beta / 1.6 on FUGAKU

- **CPU**: A64FX (2.0 GHz)
- **Compile conditions**: Fujitsu compiler 4.3.0a tcsds-1.2.28 / Single precision
- **Integrator**: Velocity Verlet (VV) and Multiple Time Step (MTS)
- **Time step**: 2 fs (VV), 2.5 fs / 5.0 fs (MTS, ver. 1.6), 3.5 fs / 7.0 fs (MTS, ver. 2.0)


### DHFR (27,346 atoms) 

#### NVT
{: .no_toc }

![](/assets/images/2020_12_jac_fugaku.jpg){: width="600px" .align-center}
- **Force field**: AMBER
- **switch**, **cutoff**, **pairlist** = 8, 8, 9.5 Å 

#### NPT
{: .no_toc }

![](/assets/images/2020_12_jac_fugaku_npt.jpg){: width="600px" .align-center}
- **Force field**: AMBER
- **switch**, **cutoff**, **pairlist** = 8, 8, 9.5 Å 

### ApoA1 (92,224 atoms)

#### NVT
{: .no_toc }

![](/assets/images/2020_12_apoa1_fugaku.jpg){: width="600px" .align-center}
- **Force field**: CHARMM
- **switch**, **cutoff**, **pairlist** = 10, 12, 13.5 Å 

#### NPT
{: .no_toc }

![](/assets/images/2020_12_apoa1_fugaku_npt.jpg){: width="600px" .align-center}
- **Force field**: CHARMM
- **switch**, **cutoff**, **pairlist** = 10, 12, 13.5 Å 

### UUN (216,726 atoms)

#### NVT
{: .no_toc }

![](/assets/images/2020_12_uun_fugaku.jpg){: width="600px" .align-center}
- **Force field**: CHARMM
- **switch**, **cutoff**, **pairlist** = 10, 12, 13.5 Å 

#### NPT
{: .no_toc }

![](/assets/images/2020_12_uun_fugaku_npt.jpg){: width="600px" .align-center}
- **Force field**: CHARMM
- **switch**, **cutoff**, **pairlist** = 10, 12, 13.5 Å 


### Crowding System (1,578,958 atoms)

#### NVT
{: .no_toc }

![](/assets/images/2020_12_1.5M_fugaku.jpg){: width="600px" .align-center}
- **Force field**: CHARMM
- **switch**, **cutoff**, **pairlist** = 10, 12, 13.5 Å 

#### NPT
{: .no_toc }

![](/assets/images/2020_12_1.5M_Fugaku_npt.jpg){: width="600px" .align-center}
- **Force field**: CHARMM
- **switch**, **cutoff**, **pairlist** = 10, 12, 13.5 Å 


### Huge Crowding System 

#### NVT
{: .no_toc }

![](/assets/images/2020_12_crowding_huge.jpg){: width="600px" .align-center}
- **Force field**: CHARMM
- **switch**, **cutoff**, **pairlist** = 10, 12, 13.5 Å 



## Benchmark of GENESIS 2.0 beta on CPU+GPU cluster

- **CPU**: Intel Xeon Gold 6142 (2.6 GHz)
- **GPU**: GeForce GTX 1080Ti
- **Compile conditions**: Intel MPI ver. 19.1 / Intel compiler ver. 19.1 / Single precision
- **Integrator**: Velocity Verlet (VV) and Multiple Time Step (MTS)
- **Time step**: 2 fs (VV),  3.5 fs / 7.0 fs (MTS)


### DHFR (27,346 atoms)

#### NVT
{: .no_toc }

![](/assets/images/2020_12_jac_genesis2.0-2.jpg){: width="600px" .align-center}
- **Force field**: AMBER
- **switch**, **cutoff**, **pairlist** = 8, 8, 9.5 Å 

#### NPT
{: .no_toc }

![](/assets/images/2020_12_jac_genesis2.0_npt.jpg){: width="600px" .align-center}
- **Force field**: AMBER
- **switch**, **cutoff**, **pairlist** = 8, 8, 9.5 Å 

### ApoA1 (92,224 atoms)

#### NVT
{: .no_toc }

![](/assets/images/2020_12_apoa1_genesis2.0.jpg){: width="600px" .align-center}
- **Force field**: CHARMM
- **switch**, **cutoff**, **pairlist** = 10, 12, 13.5 Å 

#### NPT
{: .no_toc }

![](/assets/images/2020_12_apoa1_genesis2.0_npt.jpg){: width="600px" .align-center}
- **Force field**: CHARMM
- **switch**, **cutoff**, **pairlist** = 10, 12, 13.5 Å 

### UUN (216,726 atoms)

#### NVT
{: .no_toc }

![](/assets/images/2020_12_uun_genesis2.0-1.jpg){: width="600px" .align-center}
- **Force field**: CHARMM
- **switch**, **cutoff**, **pairlist** = 10, 12, 13.5 Å 

#### NPT
{: .no_toc }

![](/assets/images/2020_12_uun_genesis2.0_npt.jpg){: width="600px" .align-center}
- **Force field**: CHARMM
- **switch**, **cutoff**, **pairlist** = 10, 12, 13.5 Å 


### Crowding System (1,578,958 atoms)

#### NVT
{: .no_toc }

![](/assets/images/2020_12_1.5M_genesis2.0.jpg){: width="600px" .align-center}
- **Force field**: CHARMM
- **switch**, **cutoff**, **pairlist** = 10, 12, 13.5 Å 

#### NPT
{: .no_toc }

![](/assets/images/2020_12_uun_genesis2.0_npt.jpg){: width="600px" .align-center}
- **Force field**: CHARMM
- **switch**, **cutoff**, **pairlist** = 10, 12, 13.5 Å 



## Benchmark of GENESIS 1.6 on CPU+GPU cluster

- **CPU**: Intel Xeon Gold 6142 (2.6 GHz)
- **GPU**: GeForce GTX 1080Ti
- **Compile conditions**: Intel MPI ver. 19.1 / Intel compiler ver. 19.1 / Mixed precision
- **Integrator**: Velocity Verlet (VV) and Multiple Time Step (MTS)
- **Time step**: 2 fs (VV),  2.5 fs / 5.0 fs (MTS)



### DHFR (27,346 atoms)

#### NVT
{: .no_toc }

![](/assets/images/2020_12_jac_genesis1.6.jpg){: width="600px" .align-center}
- **Force field**: AMBER
- **switch**, **cutoff**, **pairlist** = 8, 8, 9.5 Å 

#### NPT
{: .no_toc }

![](/assets/images/2020_12_jac_genesis1.6_npt.jpg){: width="600px" .align-center}
- **Force field**: AMBER
- **switch**, **cutoff**, **pairlist** = 8, 8, 9.5 Å 

### ApoA1 (92,224 atoms)

#### NVT
{: .no_toc }

![](/assets/images/2020_12_apoa1_genesis1.6.jpg){: width="600px" .align-center}
- **Force field**: CHARMM
- **switch**, **cutoff**, **pairlist** = 10, 12, 13.5 Å 

#### NPT
{: .no_toc }

![](/assets/images/2020_12_apoa1_genesis1.6_npt.jpg){: width="600px" .align-center}
- **Force field**: CHARMM
- **switch**, **cutoff**, **pairlist** = 10, 12, 13.5 Å 

### UUN (216,726 atoms)

#### NVT
{: .no_toc }

![](/assets/images/2020_12_uun_genesis1.6-1.jpg){: width="600px" .align-center}
- **Force field**: CHARMM
- **switch**, **cutoff**, **pairlist** = 10, 12, 13.5 Å 

#### NPT
{: .no_toc }

![](/assets/images/2020_12_uun_genesis1.6_npt.jpg){: width="600px" .align-center}
- **Force field**: CHARMM
- **switch**, **cutoff**, **pairlist** = 10, 12, 13.5 Å 


### Crowding System (1,578,958 atoms)

#### NVT
{: .no_toc }

![](/assets/images/2020_12_1.5M_genesis1.6.jpg){: width="600px" .align-center}
- **Force field**: CHARMM
- **switch**, **cutoff**, **pairlist** = 10, 12, 13.5 Å 

#### NPT
{: .no_toc }

![](/assets/images/2020_12_uun_genesis1.6_npt.jpg){: width="600px" .align-center}
- **Force field**: CHARMM
- **switch**, **cutoff**, **pairlist** = 10, 12, 13.5 Å 





## Input and control files of the benchmark

- [Benchmark sets for DHFR, ApoA1, and UUN](/assets/fundamental/2020_12_benchmark_mkl_ver4_nocrowding.tar.gz)
 

## Previous versions

- [Benchmark of GENESIS 1.0-1.4](/benchmark_v1/)

