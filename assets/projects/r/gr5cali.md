Mean, Median, and Grade 5 Students in California
================================================

Data URL
--------

Choose the title “California Test Score Data” on [this
page](https://vincentarelbundock.github.io/Rdatasets/datasets.html).
Documentation for this dataset is
[here](https://vincentarelbundock.github.io/Rdatasets/doc/AER/CASchools.html).

    #  Setup files, not included
    directory <-"/Users/chang/OneDrive - University of Waterloo/3A/STAT 341/a5"
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

Background
----------

Let’s first briefly examine the data. The data contains the average
standardized test scores of grade 5 students in each school in
California in the school year of 1998 through 1999. On top of the
scores, information are collected on areas such as the number of
students enrolled in the school, number of computers per classroom, the
percentage of students in school that qualify for a reduced price lunch,
and the percentage of students whose first language is not English in
the school.

    head(CASchools)

    ##   X district                          school  county grades students
    ## 1 1    75119              Sunol Glen Unified Alameda  KK-08        0
    ## 2 2    61499            Manzanita Elementary   Butte  KK-08        0
    ## 3 3    61549     Thermalito Union Elementary   Butte  KK-08        1
    ## 4 4    61457 Golden Feather Union Elementary   Butte  KK-08        0
    ## 5 5    61523        Palermo Union Elementary   Butte  KK-08        1
    ## 6 6    62042         Burrel Union Elementary  Fresno  KK-08        0
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

The Effects of Skewness
-----------------------

Mean and median are both measure as central tendency, and they are the
most commonly used measures of population! Therefore, it is vital for
statisticians to understand when it is suitable to use median and median
in the specific context of the population. Let’s look at an example
here.

    summary(CASchools[, c("computer")])

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##     0.0    46.0   117.5   303.4   375.2  3324.0

We can see the mean and median of the computers per classroom in each
school in California from the summary above. The mean is almost triple
the median, and the difference is huge! By briefly taking a look at the
range(
*m**a**x* − *m**i**n*
) and inter-quantile range(
*Q*<sub>3</sub> − *Q*<sub>1</sub>
), we could see the variation of the number of students is really large
between schools. As a result, the mean is so different from the median!
Therefore, it’s really important for us to know the limitations of mean
and median, and which situation is more suitable for each.

Let’s examine the mean and median of the computer in classroom in more
detail. From the histogram below, we could see the mean (red) is being
dragged in the direction of the skewness compared to the median
(yellow). From the graph, we could clearly see that the median is a
better measure of central tendency because the data is so skewed. In
this specific case, the number of computers per classroom is really high
according to the mean, which gives false impression of lack of need for
computers fin schools. In real life, this fact of misusing the mean
could make the government give less grants to schools and making
students in California more disadvantaged about earlier Computer Science
education!

    hist(CASchools$computer,col=adjustcolor("grey",alpha =0.5),
         main="Number of Computers in Grade 5 Classrooms",
         xlab="Number of Computers per Classroom in Grade 5",breaks=100,prob=TRUE)

    abline(v=c(mean(CASchools$computer),
               median(CASchools$computer)),
           col=c("red","orange"),lwd=2,lty=2)

    legend("topright",c("Mean","Median"),lwd=2,lty=2,col=c("red","orange"))

![](a4q2_files/figure-markdown_strict/unnamed-chunk-4-1.png)

So far, it seems like that median works better for skewed data. Let’s
see if this is still the case for normally distributed population! The
following histogram shows the standardized reading score for grade 5 in
each school district in California. From the following graph, we could
see the mean and median is really close and there wasn’t much
difference! In normally distributed data or for any symmetrically
distributed (not skewed) data, we know mean and median gave similar
values and are both good measures of central tendency. However, we
should use mean as a measure anyways because mean takes into account of
all values in the population and any change in any of the scores will
affect the value of the mean!

    summary(CASchools[, c("read")])

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   604.5   640.4   655.8   655.0   668.7   704.0

    hist(CASchools$read,col=adjustcolor("grey",alpha =0.5),
         main="Standardized Reading Score for Grade 5 in California",
         xlab="Standardized Reading Score",breaks=100,prob=TRUE)

    abline(v=c(mean(CASchools$read),
               median(CASchools$read)),
           col=c("red","orange"),lwd=2,lty=2)

    legend("topright",c("Mean","Median"),lwd=2,lty=2,col=c("red","orange"))

![](a4q2_files/figure-markdown_strict/unnamed-chunk-6-1.png) In
conclusion, the mean is better used for the measure of central tendency
if the data is symmetrical, and the median is a better measure of
central tendency if the data is skewed.
