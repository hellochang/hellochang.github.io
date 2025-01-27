---
layout: post
title: Grade 5 Students in California
subtitle: A Quick Look at Stardardized Test Scores of Grade 5 Students in California
tags: [R, Statistics, Influence]
comments: true
---
Stardardized Test Scores of Grade 5 Students in California
==========================================================

Describing the Data
-------------------

The data contains the average standardized test scores of grade 5
students in each school in California in the school year of 1998 through
1999. On top of the scores, information are collected on areas such as
the number of students enrolled in the school, number of computers per
classroom, the percentage of students in school that qualify for a
reduced price lunch, and the percentage of students whose first language
is not English in the school.

The data is from a population
-----------------------------

This test score is from the standardized test, and all students in
California is required to take this test. Furthermore, the data is from
all 420 schools in different counties in California so this data is
complete. This data also contains everything we want to learn such as
the number of students enrolled in the school, number of computers per
classroom, the percentage of students in school that qualify for a
reduced price lunch, etc. Since the data is complete and contains almost
everything we want to learn, this dataset is a population instead of a
sample.

Interesting and Unique
----------------------

Standardized tests are no longer performed often nowadays or are kept
confidential to avoid hurting students’ feelings. This is an interesting
dataset because this dataset is complete and contains information of
standardized test scores from a long time ago.

Data URL
--------

Choose the title “California Test Score Data” on [this
page](https://vincentarelbundock.github.io/Rdatasets/datasets.html).
Documentation for this dataset is
[here](https://vincentarelbundock.github.io/Rdatasets/doc/AER/CASchools.html).

Units and Variates
------------------

Elements of a population is called an unit. An unit in this dataset is a
school. One variate would be the average standardized test score for
math, another variate would be the average standardized test score for
reading. Both variates are great assessment for academic readiness
because reading and math are fundamental to all disciplines.

    #  Setup files, not included
    directory <-"/Users/chang/OneDrive - University of Waterloo/3A/STAT 341/a1"
    dirsep <-"/"
    filename <-paste(directory,"CASchools.csv",sep=dirsep)
    CASchools <-read.csv(filename,header=TRUE)

    # Change data to NA
    # Adjust for missing maths score
    missing_math <-CASchools[,"math"] == 0.00000
    CASchools[missing_math,"math"] <-NA
    missing_read <-CASchools[,"read"] == 0.00000
    CASchools[missing_read,"read"] <-NA
    missing_english <-CASchools[,"english"] == 0.00000
    CASchools[missing_english,"english"] <-NA

    head(CASchools)

    ##   X district                          school  county grades students
    ## 1 1    75119              Sunol Glen Unified Alameda  KK-08      195
    ## 2 2    61499            Manzanita Elementary   Butte  KK-08      240
    ## 3 3    61549     Thermalito Union Elementary   Butte  KK-08     1550
    ## 4 4    61457 Golden Feather Union Elementary   Butte  KK-08      243
    ## 5 5    61523        Palermo Union Elementary   Butte  KK-08     1335
    ## 6 6    62042         Burrel Union Elementary  Fresno  KK-08      137
    ##   teachers calworks   lunch computer expenditure    income   english  read
    ## 1    10.90   0.5102  2.0408       67    6384.911 22.690001        NA 691.6
    ## 2    11.15  15.4167 47.9167      101    5099.381  9.824000  4.583333 660.5
    ## 3    82.90  55.0323 76.3226      169    5501.955  8.978000 30.000002 636.3
    ## 4    14.00  36.4754 77.0492       85    7101.831  8.978000        NA 651.9
    ## 5    71.50  33.1086 78.4270      171    5235.988  9.080333 13.857677 641.8
    ## 6     6.40  12.3188 86.9565       25    5580.147 10.415000 12.408759 605.7
    ##    math
    ## 1 690.0
    ## 2 661.9
    ## 3 650.9
    ## 4 643.5
    ## 5 639.9
    ## 6 605.4

Graphical Display
-----------------

I plotted each of the variable against the reading and math test score.
Surprisingly, the two variable with the most significant linear
relationship is the math test score and the percentage of students
quality for lunch at a reduced price. The strong negative linear
relationship between variables suggests the schools that score high in
math have fewer economically disadvantaged students qualifying for lunch
at reduce prices.

Although unfortunate, this result makes sense logically, and this
insight suggest that more governmental efforts should be placed in
schools located in lower income district because students in those
schools are having more trouble academically.

    plot(CASchools$lunch, CASchools$math, pch = 19,
         col=adjustcolor("grey",alpha =0.5),
         xlab ="Percentage of Students with Reduced Price Lunch",
         ylab ="Math Test Score",
         main ="California 5th Grade Standardized Test Score")

![](a1q3_files/figure-markdown_strict/unnamed-chunk-3-1.png)

Attribute and Feature
---------------------

**Mean and Median**

We can see the mean and median of the number of students in each school
in California from the summary below. By briefly taking a look at the
range(
*m**a**x* − *m**i**n*
) and inter-quantile range(
*Q*<sub>3</sub> − *Q*<sub>1</sub>
), we could see the variation of the number of students is really large
between schools. As a result, the mean is so different from the median!
Therefore, it’s really important for us to know the limitations of mean
and median, and which situation is more suitable for each.

    summary(CASchools[, c("students")])

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##    81.0   379.0   950.5  2628.8  3008.0 27176.0

**Histogram of Sharing Computers**

The second attribute of my choice is a histogram plotting the number of
students sharing each computer in schools around California in 1999.

This is a great attribute because histograms are most often just plotted
for one variable. However, in some cases, plotting the a histogram of a
function of two variables would give interesting result.

From the graph, we could see that usually, less than 20 students is
sharing each computer, which is really great considering the data was
from 1999!

    hist(CASchools$student/CASchools$computer,col=adjustcolor("grey",alpha =0.5),
         main="5th Grade Students Sharing Each Computer in 1999",
         xlab="Number of Students Sharing Each Computer in Grade 5",breaks=100,prob=TRUE)

    abline(v=c(mean(CASchools$student/CASchools$computer),
               median(CASchools$student/CASchools$computer)),
           col=c("red","orange"),lwd=2,lty=2)

    legend("topright",c("Mean","Median"),lwd=2,lty=2,col=c("red","orange"))

![](a1q3_files/figure-markdown_strict/unnamed-chunk-5-1.png)
