---
layout: post
title: What is Influence?
subtitle: A brief introduction of influence
tags: [R, Statistics, Influence]
comments: true
---
Influence
=========

Influence Definition
--------------------

**Influence** of a variate *y*<sub>*u*</sub> on the population attribute
when a variate *y*<sub>*u*</sub> is removed from the population is
measured by the following expression:

*Δ*(*α*,*u*) = *α*(*y*<sub>1</sub>, …, *y*<sub>*u* − 1</sub>, *y*<sub>*u*</sub>, *y*<sub>*u* + 1</sub>, …, *y*<sub>*N*</sub>) − *α*(*y*<sub>1</sub>, …, *y*<sub>*u* − 1</sub>, *y*<sub>*u* + 1</sub>, …, *y*<sub>*N*</sub>)
for each unit *u* in the population, and *α* is an arbitrary population
attribute. Note the first part of the equation contains the unit *u*,
and the unit *u* is removed in the second part of the equation.

Influence Interpretation
------------------------

Ideally, each unit should have the same influence. However this is often
not the case. If a unit has different unit than the rest of the
population, then this unit could be an error, or, this unit can be an
interesting unit to look into to determine why this unit has
larger/smaller impact than the rest of the units when removed from the
population.

Influence Examples
------------------

We can find the influence with calculations by hand or use R. Below, an
example of finding influence on the attribute population mean is showed.

### Hand Calculation

For the population mean attribute, the population mean without unit *u*
can be written as
$$\\begin{aligned}\\alpha(y\_1,\\ldots,y\_{u-1},y\_{u+1},\\ldots,y\_N) = \\frac{1}{N-1} \\sum\_{k\\in\\mathcal{P},k \\neq u}y\_k = \\frac{\\sum\_{k\\in\\mathcal{P}}{y\_k} - y\_u}{N -1}= \\frac{N \\bar{y}-y\_u}{N -1}\\end{aligned}$$
So, the influence *Δ*(*α*, *u*) for the population mean is
$\\begin{aligned}
\\Delta(\\alpha,u) 
  &= \\alpha(y\_1,\\ldots,y\_{u-1},y\_u,y\_{u+1},\\ldots,y\_N) - \\alpha(y\_1,\\ldots,y\_{u-1},y\_{u+1},\\ldots,y\_N) \\\\
  &= \\bar{y} - \\frac{N \\bar{y}-y\_u}{N -1} = \\frac{N \\bar{y} - \\bar{y}- N \\bar{y}+y\_u}{N -1} =\\frac {y\_u- \\bar{y}}{N -1}
\\end{aligned}$
When we encounter a word problem, we just need to plug in the values to
$\\Delta(\\alpha,u)=\\frac {y\_u- \\bar{y}}{N -1}$ to find the influence
for the given *u* for population mean.

### Calculation and Plotting using R

There are three main ways to approach this problem using R.

1.  Looping through each value to calculate *Δ*.
2.  Create a matrix and use the `apply` function.
3.  Summing numeric vectors. We will use Approach 3 for this example.
    Refer to the course notes on 2.2.3 to see all the approaches.

<!-- -->

    # Setup
    directory <-"/Users/chang/OneDrive - University of Waterloo/3A/STAT 341/a2"
    dirsep <-"/"
    filename <-paste(directory,"agpop_data.csv",sep=dirsep)
    agpop <-read.csv(filename,header=TRUE)

After setting up, the following is approach 3 for calculating inference.

    y = agpop$farms87
    ybar = mean(y)

    # Note that y is a vector and we calculated inference of every value here
    delta = (y-ybar)/(length(y)-1)

Normally, we would only plug in the value for one *y*<sub>*u*</sub>. The
above R calculation finds inference for all *y*<sub>*u*</sub> for
plotting.

Now, let’s plot the influence graph for each unit *u* with the following
R code,

    par(mfrow =c(1,2))

    plot(delta,main ="Influence for Average",pch =19,
         col =adjustcolor("black",alpha =0.2),
         xlab ="Index",ylab =bquote(Delta))

    plot(y, delta,main ="Influence for Average",pch =19,
         col =adjustcolor("black",alpha =0.2),
         xlab ="Farm Size (y)",ylab =bquote(Delta))

<img src="a2q3_files/figure-markdown_strict/unnamed-chunk-3-1.png" style="display: block; margin: auto;" />

### Result Interpretations

In this example, we could see that there are a couple of large farms
that have much more influence than other units to population mean.

    # Gives unit number for the largest values
    which(delta > 1.5)

    ## [1] 172 199 216

So, unit 172 (Fresno), 199(San Diego), and 216 (Tulare) have the largest
influences. These three farms are also the largest in size.

The three largest farms have the most influence (i.e. the three most
extreme values on the right plot). It makes sense for the three largest
farms to have the biggest influence in this case because large values
would impact population mean more.

In general, while interpreting the results for influence, it’s useful to
plot graphs for each unit *u* using R. Then, we should determine the
units with the largest influence on the specific population attribute
and find out why. The influence plot often provides useful insights on
the population attribute.
