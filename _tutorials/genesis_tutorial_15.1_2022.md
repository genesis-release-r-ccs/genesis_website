---
title: "GENESIS Tutorial 15.1 (2022)"
excerpt: ""
last_modified_at: 2025-06-03T00:00:57+09:00
layout: single
toc: true
toc_sticky: true
sidebar:
  nav: sidebar-basic
---

# What is QM/MM? 

QM/MM method is a multiscale/multiphysics approach, first proposed in
seminal papers by Warshel and Karplus [^1] and Warshel and Levitt [^2], which treats a chemically
important region by the electronic structure theory (QM) and the
surrounding environment by a molecular mechanics force field (MM). The
method was recently implemented into GENESIS [^3] [^4].
In this section, we give a brief introduction to the theory.

## 1. MM force field 

The potential energy is a pre-requisite to perform molecular
simulations. In the MM force field, the potential energy is divided into
bonded and non-bonded contribution,

\\[
V^{\mathrm{MM}} = V_\mathrm{bonded} + V_\mathrm{non-bonded}.
\\]

Although the precise functional forms are different in every MM force
fields, the former is often represented as a function of bond distances
(*r*), bond angles (\\( \\theta \\)), and torsion angles(\\( \\phi \\))
as,

\\[ 
\begin{aligned} 
V_\mathrm{bonded}  &=  \sum_{\mathrm{bond}(r)} \frac{k_r}{2} (r - r_0)^2 \\\ 
&+\sum_{\mathrm{angle}(\theta)} \frac{k_{\theta}}{2}(\theta - \theta_0)^2 \\\ 
&+ \sum_{\mathrm{torsion}(\phi)} k_\phi [1-\cos(n\phi+\delta)], 
\end{aligned} 
\\]

and the latter is a sum of the Coulomb interaction between atomic
charges and the van der Waals interaction,

\\[
V_\mathrm{non-bonded} = \sum_{m,m^\prime} \frac{q_m q_{m^\prime}}{|r_m - r_{m^\prime}|} + \sum_{m,{m^\prime}} \epsilon_{m m^\prime} \left[ \left( \frac{R_{m m^\prime}^\mathrm{min}}{r_{m m^\prime}} \right)^{12} - 2 \left( \frac{R_{m m^\prime}^\mathrm{min}}{r_{m m^\prime}} \right)^6 \right].
\\]

The physical simplicity and careful parameterization have led to the
enormous success of the MM force field today. However, there are
limitations that are evident from its function form: (1) The bond
connectivity is pre-defined, so that bond breaking/forming events, i.e.,
chemical reactions, do not happen during the simulation. (2) The atomic
charge is fixed, neglecting the polarization and charge transfer
effects. (3) The parameters are usually derived for the electronic
ground state, so that the simulation involving electronic excited states
is not readily feasible. The motivation of QM/MM method is to overcome
the limitations of MM force field.

## 2. QM/MM potential 

In the QM/MM method, the system is spatially divided into QM and MM
regions. In the QM region, the motion of electrons is explicitly taken
into account based on quantum mechanics. The electronic Schrödindger
equation reads in atomic unit,

\\[
\hat{H}_e = -\frac{1}{2} \sum_i \nabla_i^2 + \sum\_{i>i^\prime} \frac{1}{|r_i - r_i^\prime|} - \sum\_{i,a} \frac{Z_a}{|r_i - r_a|} - \sum\_{i,m} \frac{q_m}{|r_i - r_m|},
\\]

\\[
\hat{H}_e |\Psi_e \rangle = E_e |\Psi_e \rangle,
\\]

where \\(i\\), \\(a\\), and \\(m\\) are indices for electrons, nucleus, and MM atoms,
respectively. As in the usual electronic structure theory, the first
three terms in the Hamiltonian represent the kinetic energy of
electrons, the electron-electron repulsion, and the electron-nulceus
attraction. The last term represents the Coulomb interaction between the
electrons and MM atomic charges, and is unique in the QM/MM method. Note
that the interaction is a 1-electron operator, which hardly affects the
scaling of QM calculations.

The electronic energy is a function of nuclear and atomic coordinates,

\\[
E_e = E_e (\mathbf{r}_a, \mathbf{r}_m).
\\]

The potential energy of the QM/MM system is then written as,

\\[
V = V^\mathrm{QM} ( \mathbf{r}_a, \mathbf{r}_m ) + 
V\_\mathrm{LJ}^\mathrm{QM/MM} ( \mathbf{r}_a, \mathbf{r}_m ) + V^{\mathrm{MM}} ( \mathbf{r}_m ), 
\\]

where \\( V^\mathrm{QM} \\) is the QM energy given as,

\\[
V^\mathrm{QM} = E_e ( \mathbf{r}_a, \mathbf{r}_m ) + \sum\_{a>a^\prime} \frac{Z_a Z\_{a^\prime}}{|r_a - r\_{a^\prime}|} + \sum\_{a,m} \frac{Z_a q_m}{|r_a - r_m|},
\\]

and \\( V_\mathrm{LJ}^\mathrm{QM/MM} \\) is the Lennard-Jones interaction between QM and MM atoms,

\\[
V_\mathrm{LJ}^\mathrm{QM/MM} = \sum\_{a,m} \epsilon\_{am} \left[ \left( \frac{R\_{am}^\mathrm{min}}{r\_{am}} \right)^{12} - 2 \left( \frac{R\_{am}^\mathrm{min}}{r\_{am}} \right)^{6} \right].
\\]

The derivatives of the QM/MM potential give the forces acting on the QM and MM atoms,

\\[
\begin{aligned} 
\mathbf{F}_a &= -\frac{\partial V}{\partial \mathbf{r}_a} = -\nabla_a V^\mathrm{QM} - \nabla_a V\_\mathrm{LJ}^\mathrm{QM/MM}, \\\ 
\mathbf{F}_m &= -\frac{\partial V}{\partial \mathbf{r}_m} = -\nabla_m V^\mathrm{QM} - \nabla_m V\_\mathrm{LJ}^\mathrm{QM/MM} - \nabla_m V^{\mathrm{MM}}
\end{aligned} 
\\]

Among the QM/MM potential, \\( V^{\mathrm{MM}} \\), \\( V\_\mathrm{LJ}^\mathrm{QM/MM} \\),
and their derivatives are calculated internally by GENESIS. On the other
hand, we expect that \\( V^\mathrm{QM} \\) and its derivatives are provided by
an external QM program that is capable of solving the electronic
Schrödindger equation. Therefore, the QM/MM calculation requires a QM
program in addition to GENESIS.

## 3. Interface with QM programs 

GENESIS has a general interface, which makes feasible to easily connect
with any QM programs. The structure of the interface is sketched in the
figure below,

![](/assets/images/2019_02_qmmm_scheme.png){: width="400" .align-center}

`qmcnt` and `qmexe` are the options specified in a \[QMMM\] section of
the control file. `qmcnt` is a template file to generate input files for
the QM program and `qmexe` is a script file to invoke the QM program.

In the current version, GENESIS is interfaced with

- [DFTB+](https://dftbplus.org/), Version 18, 19, and 20.
- [Q-Chem](https://www.q-chem.com/), Version 4.2, 4.3, and 4.4.
- [Gaussian](https://gaussian.com/), Version 09 and 16.
- [TeraChem](http://www.petachem.com/index.html), Version 1.93P and 1.94V.
- [QSimulate](https://qsimulate.com/)

Examples of `qmcnt` and `qmexe` files for these programs are found in
our [github repository](https://github.com/yagikiyoshi/QMMMscripts).

*Updated by Kiyoshi Yagi@RIKEN Theoretical molecular science laboratory\
Feb., 20, 2022*\
*Written by Kiyoshi Yagi@RIKEN Theoretical molecular science laboratory\
Dec., 5, 2020*
{: .notice}

## References 

[^1]:  [A. Warshel and M. Karplus, J. Am. Chem. Soc., **94**, 5612-5625 (1972).](https://doi.org/10.1021/ja00771a014)

[^2]:  [A. Warshel and M. Levitt, *J. Mol. Biol.*, **103**, 227-249 (1976).](https://doi.org/10.1016/0022-2836(76)90311-9)

[^3]:  [K. Yagi, K. Yamada, C. Kobayashi, and Y. Sugita, J. Chem. Theory Comput. **15**, 1924-1938 (2019).](https://pubs.acs.org/doi/10.1021/acs.jctc.8b01193)

[^4]:  [K. Yagi, S. Ito, and Y. Sugita, J. Phys. Chem. B 125, 4701-4713 (2021).](https://pubs.acs.org/doi/10.1021/acs.jpcb.1c01862)
