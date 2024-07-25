---
layout: post
title: Bootstrap-t Confidence Interval
subtitle: A Note on Bootstrap-t Confidence Interval
tags: [R, Statistics, Influence]
comments: true
---
Bootstrap-t Confidence Interval
===============================

We want to approximate the sampling distribution of a pivotal quantity
with bootstrap distribution so that we could construct a confidence
interval. The method is similar to approximating a sampling distribution
of a pivotal quantity using a t-distribution.

The Step-By-Step Approach
-------------------------

1.  Given a sample 𝒮, an attribute *a*(𝒮), and standard error
    $$\\widehat {SD}\[\\tilde a(\\mathcal S)\]$$, calculate *a*(𝒮) and
    standard error $$\\widehat {SD}\[\\tilde a(\\mathcal S)\]$$ based on
    the sample.

2.  Generate *B* bootstrap samples
    *s*<sub>1</sub><sup>\*</sup>, ..., *s*<sub>*B*</sub><sup>\*</sup>
    from sample *s* with replacement.

3.  For each of the *B* bootstrap samples, calculate
    *a*(𝒮<sub>*B*</sub><sup>\*</sup>) and error
    $$\\widehat {SD}\[\\tilde a(\\mathcal S\_B^\*)\]$$. Find the value of
    $$z\_B^\*=\\frac{a(\\mathcal S\_B^\*)-a(\\mathcal S)}{\\widehat {SD}\[\\tilde a(\\mathcal S\_B^\*)\]}$$

4.  From the estimates of the sampling distribution
    *z*<sub>1</sub><sup>\*</sup>, ..., *z*<sub>*B*</sub><sup>\*</sup>,
    find the quantiles
    *c*<sub>*l**o**w**e**r*</sub> = *Q*<sub>*z*</sub>(*p*/2) and
    *c*<sub>*u**p**p**e**r*</sub> = *Q*<sub>*z*</sub>(1 − *p*/2).

5.  The 100(1 − *p*)% bootstrap-t confidence interval is
    $$\\left (a(\\mathcal S)-c\_{upper} \\widehat {SD}\[\\tilde a(\\mathcal S)\], (a(\\mathcal S)-c\_{lower} \\widehat {SD}\[\\tilde a(\\mathcal S)\] \\right )$$

-   **Note** the positions and signs of the
    *c*<sub>*l**o**w**e**r*</sub> and *c*<sub>*u**p**p**e**r*</sub> in
    the above definition

Casualty Age Example
--------------------

In this example, when *a*(𝒫) is average age of the casualty in shark
encounters, the sampling distribution of the pivotal quantity with
*n* = 8 is shown below.

First, let’s define some functions to help our future computation.

    casualty <- read.csv("sharks.csv")
    popCasualty <- rownames(casualty)

    # Put n or N in the denominator of SD
    # instead of n-1 or N-1
    sdn <- function( y.pop ) {
    N = length(y.pop)
    sqrt( var(y.pop)*(N-1)/(N) ) }
    se.avg <- function(y.sam) {
    sdn(y.sam)/sqrt(length(y.sam)) * sqrt((65-length(y.sam))/(65-1) )
    }

    popSize <- function(pop) {
    if (is.vector(pop))
    {if (is.logical(pop))
      
    ## Assume TRUE values
    sum(pop) else length(pop)}
    else nrow(pop)
    }

    getSample <- function(pop, size, replace=FALSE) {
    N <- popSize(pop)
    pop[sample(1:N, size, replace = replace)]
    }

We now generate our sampling distribution and the bootstrap estimates of
the sampling distribution. Then, we can also calculate *a*(𝒮),
$\\widehat {SD}\[\\tilde a(\\mathcal S)\]$,
*a*(𝒮<sub>*B*</sub><sup>\*</sup>),
$\\widehat {SD}\[\\tilde a(\\mathcal S\_B^\*)\]$ and
*z*<sub>*B*</sub><sup>\*</sup>.

    M <- 10^5
    n = 8
    set.seed(3421134)
    samples <- sapply(1:M, FUN =function(m) sample(popCasualty, n, replace = FALSE) )
    avePop <- mean(casualty[, "Age"])
    avesSamp <- apply(samples, MARGIN = 2,
    FUN = function(s){mean(casualty[s,"Age"])})
    SEaveSamp <- apply(samples, MARGIN = 2,
    FUN = function(s){se.avg(casualty[s,"Age"])})
    ZPop <- (avesSamp - mean(casualty[,"Age"]))/SEaveSamp

    samCasualty <- sample(popCasualty, n, replace = FALSE)
    samStar <- sapply(1:M, FUN = function(m) sample(samCasualty, n, replace = TRUE))
    aveSam <- mean(casualty[samCasualty, "Age"])
    avesBoot <- apply(samStar, MARGIN = 2, FUN = function(s) {
    mean(casualty[s, "Age"])
    })
    SEaveBoot <- apply(samples, MARGIN = 2, FUN = function(s) {
    se.avg(casualty[s, "Age"])
    })
    ZBoot <- (avesBoot - aveSam)/SEaveSamp

We have our answers. The Bootstrap-t confidence interval using
$\\widehat {SD}\[\\tilde a(\\mathcal S)\]$ for the average age of the
casualty is

    samCasualtyAges = casualty[samCasualty, "Age"]
    zStar.lower = quantile(ZBoot, 0.025)
    zStar.upper = quantile(ZBoot, 0.975)
    round(mean(samCasualtyAges) - c(zStar.upper, zStar.lower) * se.avg(samCasualtyAges),
    2)

    ## 97.5%  2.5% 
    ## 19.69 40.94

And the Bootstrap-t confidence interval using
$\\widehat {SD}\_\*\[\\tilde a(\\mathcal S)\]$ for the average casualty
age is

    round(mean(samCasualtyAges) - quantile(ZBoot, c(0.975, 0.025)) * sd(avesBoot),
    2)

    ## 97.5%  2.5% 
    ## 19.04 41.55
