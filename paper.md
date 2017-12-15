---
title: "Simulations for Power Analysis and Study Design: do Lab Animal Findings About the Gompertz-Makeham Model Extend to Human Populations?"
journal: "Journal of Network and Computer Applications"
author:
- name: 'Alex F. Bokov\corref{cor1}'
  email: bokov@uthscsa.edu
  group: deb
  address: "UT Health, San Antonio"
- name: Jonathan A. Gelfond
  group: deb
  address: "UT Health, San Antonio"
- name: Alfredo Tirado-Ramos
  group: deb
  address: "UT Health, San Antonio"
- name: Scott D. Pletcher
  group: um
  address: "University of Michigan, Ann Arbor"
biblio-files: paper
options: "review, authoryear"
abstract:
  "In clinical trials as well as in basic research, critical decisions about experimental design need to be made in advance, often when little is known about the population being studied. Effect sizes, variability, and other statistical properties are not reliably portable from one study population or research topic to another. By definition, the more novel an experiment is the fewer 'similar' previous experiments it can draw on the the more acute this problem. We have extended our earlier survival simulation work from laboratory rodents to human populations. Moreover we generalized from a two dimensional parameter space to three dimensions in order to determine whether or not the Gompertz-Makeham model has any advantage over simpler and more commmonly used survival models. "
keywords: "survival analysis, predictive modeling, statistics"
---

# Introduction

In planning an experiment in basic or clinical research,
power analysis is important for determining feasibility and
budgeting for a sample size sufficient to avoid an indeterminate
result. Conversely, accurate power analysis also helps contain
costs by avoiding excessive sample sizes. Power analysis
depends on a probability density function over all possible
outcomes for an experiment given a particular set of
parameters. Real-world processes, especially in the life sciences, often have density functions that either lack a
closed-form solution or are complex to implement.
Furthermore, it is not always clear how large of a sample size is sufficient to meet the asysmptotic assumptions of one's statistical tests. A promising approach is to generate simulated data spanning the relevant parameter-space.

This allows the researcher to realisticall budget for follow-up intervals, participant recruitment targets, and choice of analysis method prior to initiating an experiment or clincal trial.

Existing Monte Carlo methods to characterizing distributions [@fang08; @schoemann14] depend on hardcoded assumptions about the type of
phenomenon being studied and the statistical model being
fitted to the data. We have developed and are continuing to improve an open
source library for the R statistical language that implements a simpulation and test framework that can be adapted to various populations, study designs, and outcomes being measured. Instead of interpreting treatement effect as a simplistic uni-directional difference our approach is to accrue data by repeatedly running statistical workflows and decision rules representative of what the researcher is considering for their actual experiment. These virtual experiments are imlemented as modules with a automated code-generation function that makes it easier to create new ones or adjust existing ones to the needs of a given project.

# Background

In mammals, the risk of mortality is not constant over time. Rather, it increases log-linearly with age [@MuellerGompertzequationpredictive1995; FinchSlowmortalityrate1990]. Using only ages of death or at loss-to-followup (censored events), the Gompertz probability density function can be used to obtain the maximum likelihood estimates for the initial risk of mortality (IMR or $\lambda$ ) and the rate at which it accelertates (RoA or $\gamma$ ). On a log scale these correspond to a slope and an intercept of mortality risk over time. 

We recently reported that the Weibull and the Cox Proportional Hazard tests had roughly similar performance to a likelihood ratio test using estimates of Gompertz model parameters obtained via Nelder-Mead optimization [@NelderSimplexMethodFunctionMinimization1965]. The Weibull model was found to be slighly less sensitive and slightly more resistant against false positives than the others [@bokov2017riskmodels]. However, when the IMR increased (i.e. higher initial hazard) and RoA decreased (i.e. hazard increases more slowly) in a test group compared to the control group, or vice versa, only the Gompertz likelihood ratio test could detect differences even though the survival curves visibly diverged. Our simulations were based on hazard estimates for laboratory mice.

But we cannot assume that these conclusions are automatically applicable to humans. Obviously the IMR and RoA parameters in humans will be much smaller and harder to detect. Furthermore, 


# Methods

## Data

## Statistics

## Software design

# Results

## How large of a single-paramter difference is detectable?

## How large are differences between real human populations?

## Is the Makeham parameter detectable and practically relevant?

# Conclusion and future work

## Retaining individual results for kriging

## Age at recruitment, limited followup

## Censoring

## Sample size as a dimension
