Problem Set 02
Instructions

    Use RStudio to create a file named something like lastname_firstname_ps02.Rmd

    Set up your file with a code chunk that loads the tidyverse and socviz packages.

    Check to see that organdata is available:

library(tidyverse)

## ── Attaching packages ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── tidyverse 1.3.1 ──

## ✔ ggplot2 3.3.5     ✔ purrr   0.3.4
## ✔ tibble  3.1.6     ✔ dplyr   1.0.8
## ✔ tidyr   1.2.0     ✔ stringr 1.4.0
## ✔ readr   2.1.2     ✔ forcats 0.5.1

## ── Conflicts ──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ readr::edition_get()   masks testthat::edition_get()
## ✖ dplyr::filter()        masks stats::filter()
## ✖ purrr::is_null()       masks testthat::is_null()
## ✖ dplyr::lag()           masks stats::lag()
## ✖ readr::local_edition() masks testthat::local_edition()
## ✖ dplyr::matches()       masks tidyr::matches(), testthat::matches()

library(socviz)

## 
## Attaching package: 'socviz'

## The following object is masked from 'package:kjhutils':
## 
##     %nin%

organdata

## # A tibble: 238 × 21
##    country   year       donors   pop pop_dens   gdp gdp_lag health health_lag
##    <chr>     <date>      <dbl> <int>    <dbl> <int>   <int>  <dbl>      <dbl>
##  1 Australia NA          NA    17065    0.220 16774   16591   1300       1224
##  2 Australia 1991-01-01  12.1  17284    0.223 17171   16774   1379       1300
##  3 Australia 1992-01-01  12.4  17495    0.226 17914   17171   1455       1379
##  4 Australia 1993-01-01  12.5  17667    0.228 18883   17914   1540       1455
##  5 Australia 1994-01-01  10.2  17855    0.231 19849   18883   1626       1540
##  6 Australia 1995-01-01  10.2  18072    0.233 21079   19849   1737       1626
##  7 Australia 1996-01-01  10.6  18311    0.237 21923   21079   1846       1737
##  8 Australia 1997-01-01  10.3  18518    0.239 22961   21923   1948       1846
##  9 Australia 1998-01-01  10.5  18711    0.242 24148   22961   2077       1948
## 10 Australia 1999-01-01   8.67 18926    0.244 25445   24148   2231       2077
## # … with 228 more rows, and 12 more variables: pubhealth <dbl>, roads <dbl>,
## #   cerebvas <int>, assault <int>, external <int>, txp_pop <dbl>, world <chr>,
## #   opt <chr>, consent_law <chr>, consent_practice <chr>, consistent <chr>,
## #   ccode <chr>

    Using the organdata data set, carry out the tasks below, showing both your code and its output.

Tasks

    Draw a scatterplot (with geom_point()) showing year on the x-axis and the donation rate on the y-axis for all countries. Fit a line to the data with geom_smooth(). Does this seem like a useful way to look at the data? Say in a sentence why or why not.
    Facet your scatterplot by country, ordered from highest to lowest average donation rate. Experiment with using a linear fit instead of the default smoother and say whether it seems useful or not.
    Make a new scatterplot that facets the data by one of the categorical measures in the data, such as world, consent_law or consent_practice. Do donation rates seem to differ by group on average?
    Experiment with the data to see if there is an association between the donation rate and measures like population density (pop_dens), public health spending (pubhealth), road accident fatalities (roads), assault deaths (assault), cerebrovascular deaths (cerebvas), or transplant centers (txp_pop). What do the differences between these scatterplots (e.g. the contrast between putting pubhealth, assault, and txp_pop on the x-axis) tell you about the data?
    Using group_by, group the data by consent_law and calculate the average donation rate for Informed Consent vs Presumed Consent countries.
    Again using group_by, group the data by consent law and year, calculate the average donation rate, and plot the result as a line graph.
    Finally, redo task 6 but this time try calculating the standard deviation (sd()) of the donation rate as well as the mean. Use the result to plot a rough error bar showing plus or minus one standard deviation around the mean. (Hint: use geom_ribbon())
