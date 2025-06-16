---
title: "GENESIS Tutorial Appendix 5 (2022)"
gpos: 999.005
excerpt: ""
last_modified_at: 2025-06-03T00:00:57+09:00
layout: single
toc: true
toc_sticky: true
sidebar:
  nav: sidebar-basic
---

# Automatic Parameter Tuning for REMD, REUS, REST

## Strategy 

In REMD simulations, we need to define "good" parameter set to achieve
random walk in parameter space to perform calculations efficiently. In
case of T-REMD, there is a good way to determine the parameter set. But
in REUS or REST, no general ways are available so far. This is one of
the most annoying points in those calculations. To solve this problem,
an intuitive parameter tuning mechanism is implemented in GENESIS (both of ATDYN and SPDYN).

Assume we have \\(n\\) replicas characterized by parameters (such as temperatures) \\(V\_{0}, V\_{1}, V\_{2}, \\cdots, V\_{n-1}\\). In this setup, we have
\\(n-1\\) deltas between adjacent values \\(X\_{0,1}, X\_{1,2}, X\_{2,3}, \\cdots, X\_{n-2,n-1}\\). Here we try to change these \\(X\\)
values to get a good set of parameters, while one of terminal values is
fixed.

Deviation of the exchange probability between replicas \\(i\\) and \\(j\\),
\\(\\Delta p\_{ij}\\), from the target exchange probability, \\(p\_\\mathrm{tgt}\\), can be written as:

\\[\\Delta p\_{ij} = p\_{ij} - p\_\\mathrm{tgt} - \\mathrm{sign}(p\_{ij} - p\_\\mathrm{tgt}) \\cdot \\mathrm{min}(\\mathrm{abs}(p\_{ij} - p\_\\mathrm{tgt}),p\_\\mathrm{mgn})\\]

where \\(p\_{ij}\\) is the exchange probability between replicas \\(i\\) and
\\(j\\), and margin is also considered. If the exchange probability is within
the range of \\(p\_\\mathrm{tgt}-p\_\\mathrm{mgn} \< p\_{ij} \< p\_\\mathrm{tgt}+p\_\\mathrm{mgn}\\), the exchange works fine. This formula says that
if the exchange probability is larger (smaller) than the target value,
\\(\\Delta p\_{ij}\\) is large (small).

\\(X\_{0,1}, X\_{1,2}, X\_{2,3}, \\cdots, X\_{n-2,n-1}\\) will
be updated according to the following equation:

\\[\\Delta X\_{i,j} = \\Delta p\_{ij} \\cdot 100 \\cdot v\_g\\]

where \\(v_g\\) is the grid size in parameter space. If the exchange
probability deviation is large, large change will happen in the
parameter values. Parameter set is recalculated from the fixed terminal
value and the \\(X\\)s upon update. Repeating update according to this scheme,
the exchange probabilities will get close to the target value.

## List of Parameters in GENESIS

These parameters are available in **[REMD]** section. In the following
list, **N** should be replaced to an integer number like other params.

### param_tuningN = *YES / NO (default: NO)*

This parameter defines whether the parameter tuning is enabled.

### tgt_exc_probN = *Real (default: 0.25)*

This parameter defines target exchange probability \\(p\_\\mathrm{tgt}\\). The valid value
range is **0.0-1.0**.

### mgn_exc_probN = *Real (default: 0.05)*

This parameter defines the margin for the target exchange probability \\(p\_\\mathrm{mgn}\\).
Parameters would not change if the following condition is satisfied:
**tgt_exc_prob - mgn_exc_prob \< exchange probability \< tgt_exc_prob +
mgn_exc_prob**.

### trial_freqN = *Integer (default: 48)*

Number of trials used for single update of parameter set. In the real
calculations, update of parameter set takes place after **(trial_freqN \* 2)** exchange trials.
This is because there are two exchange patterns in GENESIS.

### eq_cycleN = *Integer (default: 2)*

Number of equilibration cycles after the parameter set update. As
**trial_freqN** above, **(eq_cycleN \* 2)** exchange trials will be ignored.

### param_gridN = *Real (default: 0.1)*

Grid of parameter space \\(v_g\\).

### max_param_shiftN = *Real (default: 20.0)*

Maximum parameter shift in a single update. If you specify large values,
this value is virtually ignored.

### fix_terminalN = *BOTTOM / TOP (default: BOTTOM)*

Determines parameter value to be fixed. In case of **BOTTOM**, the first
value in the parameter list is fixed. On the other hand, the last value
in the parameter list is fixed if **TOP** is specified.

## Examples

### General notes
- **exchange_period** must not be 0 if you perform parameter tuning.
- **exchange_period** should be small for the first tuning stage.
- Updated parameters are not saved in restart files. You should write new values in your next input file.
- If you want to do fine tuning, increase **exchange_period** and **trial_freqN** values.

### T-REMD example

(Parameter update takes place every 5,000 steps.)


```toml
[REMD]
dimension        = 1
exchange_period  = 50

type1            = TEMPERATURE
nreplica1        = 4
parameters1      = 300 301 301.7 303.2

param_tuning1    = YES
tgt_exc_prob1    = 0.25
mgn_exc_prob1    = 0.05
trial_freq1      = 48
eq_cycle1        = 2
param_grid1      = 0.1
max_param_shift1 = 20.0 # virtually ignored
fix_terminal1    = BOTTOM
```

Sample log message (extracted from the main log file):


```bash
REMD> New parameter set: 300.00000 303.00000 303.10000 311.60000
REMD> New parameter set: 300.00000 301.00000 308.10000 314.60000
```

### REUS example

[(only reference values will change upon update.)]


```toml
[REMD]
dimension        = 1
exchange_period  = 50

type1            = RESTRAINT
nreplica1        = 4
cyclic_params1   = NO
rest_function1   = 1

param_tuning1    = YES
tgt_exc_prob1    = 0.25
mgn_exc_prob1    = 0.05
trial_freq1      = 48
eq_cycle1        = 2
param_grid1      = 0.01
max_param_shift1 = 0.2
fix_terminal1    = BOTTOM

[SELECTION]
group1           = an:H62 and resno: 9
group2           = an:H62 and resno: 10

[RESTRAINTS]
nfunctions       = 1
function1        = DIST          # restraint function type
constant1        = 15 15 15 15   # force constants
reference1       = 4 4.1 4.3 4.5 # references
select_index1    = 1 2           # restrained groups
```

Sample log message (extracted from the main log file):


```bash
REMD> New parameter set: 4.00000 4.30000 4.31000 4.32000
REMD> New parameter set: 4.00000 4.50000 4.71000 4.92000
```

### REST example

(almost the same as T-REMD)


```toml
[REMD]
dimension        = 1
exchange_period  = 50

type1            = REST
nreplica1        = 4
parameters1      = 300 301 301.7 303.2
select_index1    = 1

param_tuning1    = YES
tgt_exc_prob1    = 0.25
mgn_exc_prob1    = 0.05
trial_freq1      = 48
eq_cycle1        = 2
param_grid1      = 0.1
max_param_shift1 = 20.0
fix_terminal1    = BOTTOM

[SELECTION]
group1           = ai:1-638
```

Sample log message (extracted from the main log file):


```bash
REMD> New parameter set: 300.00000 303.00000 310.70000 319.20000
REMD> New parameter set: 300.00000 305.00000 314.70000 330.20000
```

------------------------------------------------------------------------

*Written by Motoshi Kamiya@RIKEN R-CCS\
Jun. 19, 2018*
{: .notice}
