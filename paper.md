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

In mammals, the risk of mortality is not constant over time. Rather, it increases log-linearly with age [@MuellerGompertzequationpredictive1995; FinchSlowmortalityrate1990]. Using only ages of death or at loss-to-followup (censored events), the Gompertz probability density function can be used to obtain the maximum likelihood estimates for the initial risk of mortality (IMR or $\lambda$ ) and the rate at which it accelertates (RoA or $\gamma$ ). On a log scale these correspond to a slope and an intercept of mortality risk over time. These parameters can then be plugged back into the observed data to its likelihood. When comparing a test group with a control group the null hypothesis is that their lifespans are no different from each other and therefore the same pair of IMR and RoA parameters are sufficient to fit the observed data. However, if the two groups have different IMRs, a model with three parameters-- IMR~control, IMR~treated, and RoA -- will have a larger likelihood and this is the basis of the likelihood ratio test as used here. The same principle applies a difference in the RoAs between the groups, or differences in both parameters (now the likelihood from the two-parameter null model is being comparted to the likelihood from a four-parameter model where everything is different between the two groups, adjusted for the additional difference in degrees of freedom).

We recently reported that the Weibull and the Cox Proportional Hazard tests had roughly similar performance to a likelihood ratio test using estimates of Gompertz model parameters obtained via Nelder-Mead optimization [@NelderSimplexMethodFunctionMinimization1965]. The Weibull model was found to be slighly less sensitive and slightly more resistant against false positives than the others [@bokov2017riskmodels]. However, when the IMR increased (i.e. higher initial hazard) and RoA decreased (i.e. hazard increases more slowly) in a test group compared to the control group, or vice versa, only the Gompertz likelihood ratio test could detect differences even though the survival curves visibly diverged. Our simulations were based on hazard estimates for laboratory mice.

But we cannot assume that these conclusions are automatically applicable to humans. Clearly humans are orders of magnitude longer-lived so human IMR and RoA parameters will be smaller and harder to detect. Furthermore, laboratory animals are kept in as homogeneous an environment as possible, while humans are in some sense a 'wild' population so in addition to IMR and RoA, a third parameter may need to be included to account for extrinsic hazards-- EH, or *c* . This extended model is called Gompertz-Makeham and was used here.

# Methods

## Data

In our earlier work we used combined results for just the control groups of multiple longevity studies carried out using non-mutant C57Bl6 mice to obtain parameters with which to generate simulated data representative of this species. For humans we used life-tables for the combined US population from 2013 [@arias_united_2017b]. These are the second to most recent available, and we are deliberately holding out the 2014 installment published this year [@arias_united_2017b] as a validation dataset for upcoming work. 

The life table data is in one-year increments for 99,316 individuals with all deaths over 100 years binned together. We extracted these data out into individual time-points and smoothed out the bins to monthly bins rather than yearly, with all deaths above 100 years given a censoring indicator. In Figure 1 we present the raw data and the smoothed data on the same scale, along with a simulated survival curve generated using the parameters estimated from this data (IMR = 3.024956e-06, RoA = 7.669709e-03, and EH = 1.970425e-05). It can be seen from the figure that smoothing the time intervals to months (necessary to avoid numeric artifacts caused by too many tied events) does not significantly bias the data. It also shows how closely the simulated distribution of deaths resembles the real data, though the only information from the data that was used by the simlation were the three hazard parameters.

![Real, smoothed, and simulated human survival data. ](pt2017_figure01.pdf){ width=50% }

In our rodent results [@bokov2017riskmodels] we found that below a sample size of 60, there was an increasing bias in the estimates of both the IMR and RoA parameters. In order to find the corresponding threshold for humans we used the above parameter estimates to simulate one million replicates of this cohort, varying the sample size between 100 and 500. For each sample we estimated the three Gompertz-Makeham parameters and compared to the known values used by the simulation in order to assess how variability and estimation bias changes with sample size. We found the threshold for a human population binned into 1-month intervals to be 200. (Figure 2.)

![Variance and bias versus sample size. ](pt2017_figure01.pdf){ width=50% }

## Software


Our simulation framework for power analysis and exploration of distributional properties of data is called PowerTrip ( link ). It is implemented as a package for the R statistical language [@Rlanguage; @survival-package; @survival-book] and has the following other packages as its essential dependencies: eha [@eha], MASS [@MASS], SparseM [@SparseM], and broom [@broom]. Some of the figures in this manuscript were prepared using the plot3D [@plot3D] and rgl [@rgl] packages. 

At initialization, the user sets upper and lower bounds on all model parameters, and the reference parameters that will be used to simulate the control group for every comparison. The parameter-space is centered on the reference point and treatment effects are modeled as deviations from it. Here, the parameter deviations the three hazards: IMR, RoA, and EH. An increase in any of them would result in a shorter survival time relative to the control group, *ceteris paribus*. Likewise a decrease would be associated with a longer survival-time. One of the distinguishing features of our approach is that instead of making assumptions about the direction of change due to an experimental intervantion, we probe every possible direction and continuously refine our estimate of the multi-dimensional surface that represents the combinations of paramter differences that are detectable by the proposed statistical method (or more generally, decision algorithm) at the specified threshold (here we picked the tradition 80% resolving power).

As we did previously [@bokov2017riskmodels], we 

## Statistics

# Results

## How large must a single-parameter difference be in order to be detectable?

## How large are differences between real human populations?

## Is the Makeham parameter detectable and practically relevant?

# Conclusion and future work

## Retaining individual results for kriging

## Age at recruitment, limited followup

## Censoring

## Sample size as a dimension

## Logistic-Makeham
