Anatomy of a Significance Test
==============================

The Goal
--------

We want to test the difference between attributes of 2 sub-populations
relative to randomly mixed sub-populations and provide numerical
evidence.

The Null Hypothesis
-------------------

The following equivalent statements are the **null hypothesis**,
*H*<sub>0</sub> that we are testing.

-   *H*<sub>0</sub>:The sub-populations *P*<sub>1</sub> and
    *P*<sub>2</sub> were randomly draw from the same population
-   *H*<sub>0</sub>:The sub-populations *P*<sub>1</sub> and
    *P*<sub>2</sub> were created randomly by assigning units in the same
    population to each of *P*<sub>1</sub> and *P*<sub>2</sub>
-   *H*<sub>0</sub>:The sub-populations *P*<sub>1</sub> and
    *P*<sub>2</sub> were randomly generated.

Note that that *H*<sub>0</sub> is weaker to be stated in the form of
*a*(𝒫<sub>1</sub>) = *a*(𝒫<sub>2</sub>), although still correct. That’s
why we shouldn’t state *H*<sub>0</sub> in terms of equivalence of
attribute value.

*H*<sub>*A*</sub>, the **alternate hypothesis** is the complement of
*H*<sub>0</sub>.

The Discrepency Measure
-----------------------

A discrepancy measure, 𝒟(𝒫<sub>1</sub>, 𝒫<sub>2</sub>), is

-   also called a **test statistic**
-   defined to be a specific population attribute for the population
    𝒫 = {𝒫<sub>1</sub>, 𝒫<sub>2</sub>} to measure properties such as
    equivariance and invariance
-   used to quantify the inconsistency of our data against the null
    hypothesis
-   larger discrepancy measures indicate stronger evidence against the
    null hypothesis
-   forms of 𝒟(𝒫<sub>1</sub>, 𝒫<sub>2</sub>) is typically based on
    differences for measures of location, and based on ratios for the
    measures of spread. For example:
    -   A discrepancy measure for hypothesizing that the averages from
        the two sub-populations are the same might be
        𝒟(𝒫<sub>1</sub>, 𝒫<sub>2</sub>) = |*y*<sub>1</sub> − *y*<sub>2</sub>|
    -   A discrepancy measure for hypothesizing that the standard
        deviation from the two sub-populations are the same might be
        $\\mathcal D(\\mathcal P\_1,\\mathcal P\_2) = \\left\\lvert 1- \\frac{SD(\\mathcal P\_1)}{SD(\\mathcal P\_2)} \\right \\rvert$
    -   A discrepancy measure for hypothesizing that the average from
        the first sub-population is larger than the average from the
        second sub-population might be
        𝒟(𝒫<sub>1</sub>, 𝒫<sub>2</sub>) = *y*<sub>2</sub> − *y*<sub>1</sub>

The Observed Discrepancy
------------------------

The discrepancy measure, 𝒟 = (𝒫<sub>1</sub>, 𝒫<sub>2</sub>), calculated
on the two unshuffled(observed) sub population is called the observed
discrepancy,
*d*<sub>*o**b**s*</sub> = 𝒟(𝒫<sub>1</sub>, 𝒫<sub>2</sub>)

-   the discrepancy measure measures only one type of discrepancy
    between the populations at a time for a null hypothesis, all other
    differences are ignored
-   for instance, *P*<sub>1</sub> and *P*<sub>2</sub> could have similar
    standard deviation but different skewness

The Observed p-value
--------------------

The probability that a random sub-population chosen has a discrepancy
measure at least as large as the observed discrepancy is called the
observed p-value
p-value  = *P*(𝒟 ≤ *d*<sub>*o**b**s*</sub> ∣ *H*<sub>0</sub> is true)

-   To approximate the p-value, we use *M*, which is a large number of
    shuffled pairs.

    -   M is shuffled pairs is generated as
        (𝒫<sub>1, 1</sub><sup>\*</sup>, 𝒫<sub>2, 1</sub><sup>\*</sup>), (𝒫<sub>1, 2</sub><sup>\*</sup>, 𝒫<sub>2, 2</sub><sup>\*</sup>), ..., (𝒫<sub>1, *M*</sub><sup>\*</sup>, 𝒫<sub>2, *M*</sub><sup>\*</sup>)

-   The approximated p-value is
    $\\widehat{\\text{p-value}} = \\frac{1}{M} \\sum\_{i=1}^{M} I\\left(\\mathcal D \\left (\\mathcal P\_{1,i}^{\*}, \\mathcal P\_{2,i}^{\*} \\right) \\leq d\_{obs} \\right)$,
    where *d*<sub>*o**b**s*</sub> = 𝒟(𝒫<sub>1</sub>, 𝒫<sub>2</sub>)

-   To calculate the exact p-value instead of an approximation, use
    $M = {N\_1 +N\_2 \\choose N\_1}= {N\_1 +N\_2 \\choose N\_2}$, where
    all possible shuffles are considered. The exact p-value is less
    commonly calculated because there are way too many possible
    permutations and it is inefficient.

-   Smaller p-value indicate more evidence against the null hypothesis.

    -   p-value &lt; 0.001 shows very strong evidence against
        *H*<sub>0</sub>
    -   0.001 &lt; p-value &lt; 0.01 shows that there is strong evidence
        against *H*<sub>0</sub>
    -   0.01 &lt; p-value &lt; 0.05 shows that there is evidence against
        *H*<sub>0</sub>
    -   0.05 &lt; p-value &lt; 0.1 shows that there is weak evidence
        against *H*<sub>0</sub>
    -   p-value &gt; 0.1 shows that there is no evidence against
        *H*<sub>0</sub>
    -   If p-value = 0, we have a proof by contradiction because
        something impossible is observed and the hypothesis must be
        false.

Example
-------

This example compares the final exam grade of male and female students.
First, let’s set things up!

    Marks = read.csv("Marks.csv", header=TRUE)
    Final.Female = Marks$Final[Marks$Gender=="Female"]
    Final.Male = Marks$Final[Marks$Gender=="Male"]
    pop = list(pop1 = Final.Female , pop2 = Final.Male)

We will consider the following 2 discrepancy measure,
$\\mathcal D\_1(\\mathcal P\_1,\\mathcal P\_2)=\\frac{SD(\\mathcal P\_1)}{SD(\\mathcal P\_2)}-1$
and
$\\mathcal D\_2(\\mathcal P\_1,\\mathcal P\_2)=\\frac{\\bar{Y\_1} - \\bar{Y\_2}}{\\tilde{\\sigma} \\sqrt{\\frac{1}{n\_1}+\\frac{1}{n\_2}}}-1$.
Note that 𝒟<sub>1</sub> is a measure of spread and 𝒟<sub>2</sub> is a
measure of central tendency.

First, let’s define a function to generate random shuffles in R.

    mixRandomly <- function(pop) {
      pop1 <- pop$pop1;  n_pop1 <- length(pop1)
      pop2 <- pop$pop2; n_pop2 <- length(pop2); mix <- c(pop1,pop2)
      select4pop1 <- sample(1:(n_pop1 + n_pop2), n_pop1,  replace = FALSE)
      new_pop1 = mix[select4pop1]; new_pop2 = mix[-select4pop1]
      list(pop1=new_pop1, pop2=new_pop2)
    }

Now,we make functions that calculate our discrepancy measures.

    # D1 as a function
    D1Fn <- function(pop) {
        ## First sub-population
        pop1 <- pop$pop1; n1 = length(pop1); SD1 <- sqrt(var(pop1)*(n1-1)/n1)
        ## Second sub-population
        pop2 <- pop$pop2; n2 = length(pop2); SD2 <- sqrt(var(pop2)*(n2-1)/n2)
        ## Determine the test statistic
        temp <- SD1/SD2 - 1
        temp
    }

    # D2 as a function
    D2Fn <- function(pop) {
        ## First sub-population
        pop1 <- pop$pop1; n1 <- length(pop1); m1 <- mean(pop1); v1 <- var(pop1)
        ## Second sub-population
        pop2 <- pop$pop2; n2 <- length(pop2); m2 <- mean(pop2); v2 <- var(pop2)
        ## Pool the variances
        v <- ((n1 - 1) * v1 + (n2 - 1) * v2)/(n1 + n2 - 2)
        ## Determine the t-statistic
        temp <- (m1 - m2) / sqrt(v * ( (1/n1) + (1/n2) ) )
        temp
    }

Here, we generated the histograms of our 2 discrepancy measures based on
5000 shuffles of the two sub-populations.

    par(mfrow = c(1,2))

    # Plot for D1
    D1Vals <- sapply(1:5000, FUN = function(...){
      D1Fn(mixRandomly(pop))})
    hist(D1Vals, breaks=40, probability = TRUE, 
         main = "Permuted populations", xlab="D1 statistic")
    abline(v=D1Fn(pop), col = "red", lwd=2)

    # Plot for D2
    D2Vals <- sapply(1:5000, FUN = function(...){ D2Fn(mixRandomly(pop)) })
    hist(D2Vals, breaks=40, probability = TRUE,  
         main = "Permuted populations", xlab="D2 statistic")
    abline(v=D2Fn(pop), col = "red", lwd=2)

![](a4q3_files/figure-markdown_strict/unnamed-chunk-4-1.png)

From this graph, we could see that our two sub-populations of male vs
female final exam grade seems to be a random mix of the parent
population when considering measures of the spread, 𝒟<sub>1</sub>. On
the other hand, the two sub-populations of male vs female final exam
grade seems to *not* be a random mix of the parent population when
considering measures of the central tendency, 𝒟<sub>2</sub>.

Now, let’s test the null hypothesis and check whether our two
sub-population is indeed generated by random mixing for
𝒟<sub>2</sub>(𝒫<sub>1</sub>, 𝒫<sub>2</sub>).

    D2obs = D2Fn(pop) 
    mean(abs(D2Vals) >= abs(D2obs))

    ## [1] 0.0054

Since our value here is very small and much less than 0.01. We know that
there is strong evidence against the null hypothesis. In the context of
our situation, there is strong evidence that the final marks of male
students vs. female students is *not* a random mix of the parent
population.
