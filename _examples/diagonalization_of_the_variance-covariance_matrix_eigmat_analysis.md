---
title: "GENESIS Input Example: Diagonalization of the variance-covariance matrix (`eigmat_analysis`)"
excerpt: ""
last_modified_at: 2025-06-11T22:49:37+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---


This tool is used to diagonalyze the variance-covariance matrix obtained
by the `flccrd_analysis`. This tool is used for the principal component
analysis (PCA). 
See [Tutorials 2.1](/tutorials/genesis_tutorial_2.1_2022/) and
[4.4](/tutorials/genesis_tutorial_4.4_2022/).

## Diagonalization of the variance-covariance matrix

```toml
[INPUT]
pcafile    = ../2_flccrd/output.pca  # PCA file obtained by flccrd_analysis
 
[OUTPUT]
valfile   = output.val               # VAL file
vecfile   = output.vec               # VEC file
cntfile   = output.cnt               # CNT file
```
