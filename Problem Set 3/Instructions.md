Problem Set 03
Instructions

        Use RStudio to create a file named something like lastname_firstname_ps03.Rmd
        Set up your file with a code chunk that loads the tidyverse, socviz, slider, and covdata packages. If you do not have covdata installed, then learn more about the package here. At a minimum, install it with,

install.packages("slider")

remotes::install_github("kjhealy/covdata@main")

Remember, you only have to install these packages once, so do not include the lines above in your RMarkdown file.

Then, in your Rmd file:

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

library(slider)
library(socviz)

## 
## Attaching package: 'socviz'

## The following object is masked from 'package:kjhutils':
## 
##     %nin%

library(covdata)

## 
## Attaching package: 'covdata'

## The following object is masked from 'package:socviz':
## 
##     %nin%

## The following object is masked from 'package:datasets':
## 
##     uspop

## The following object is masked from 'package:kjhutils':
## 
##     %nin%

        Take the time to look at the documentation for the package.
        Carry out the tasks below, showing both your code and its output.

Note: The covdata package contains several different datasets. We will be workign with stmf, nytcovstate, and apple_mobility
Tasks
Section A. Short Term Mortality Fluctuations

Read the package documentation for the stmf data set.

Here is part of the stmf data:

stmf %>% 
  select(cname, year:sex, approx_date, age_group, death_rate, rate_total)

## # A tibble: 557,490 × 8
##    cname      year  week sex   approx_date age_group death_rate rate_total
##    <chr>     <dbl> <dbl> <chr> <date>      <chr>          <dbl>      <dbl>
##  1 Australia  2015     1 m     2015-01-04  0-14        0.000113    0.00533
##  2 Australia  2015     1 m     2015-01-04  15-64       0.00140     0.00533
##  3 Australia  2015     1 m     2015-01-04  65-74       0.0107      0.00533
##  4 Australia  2015     1 m     2015-01-04  75-84       0.0417      0.00533
##  5 Australia  2015     1 m     2015-01-04  85+         0.119       0.00533
##  6 Australia  2015     1 f     2015-01-04  0-14        0.000160    0.00564
##  7 Australia  2015     1 f     2015-01-04  15-64       0.000929    0.00564
##  8 Australia  2015     1 f     2015-01-04  65-74       0.00787     0.00564
##  9 Australia  2015     1 f     2015-01-04  75-84       0.0288      0.00564
## 10 Australia  2015     1 f     2015-01-04  85+         0.119       0.00564
## # … with 557,480 more rows

The death_rate variable is the rate that week for each age_group within sex. The rate_total is the total rate for all ages within sex that week, which is e.g. why it is the same number for the first five rows of the dataset: that’s the overall rate for Australian males in week one of 2015. Meanwhile row one of the dataset is Australian males aged 0-14 for week 1 of 2015. Now,

        Reproduce the following figure:

Excess mortality in Belgium

(Hint: read the article in the documentation on “Excess Mortality Data”).

        Choose one country in the dataset. For 2020 only, compare the mortality rates for men and women in that country. Does it seem like there are any differences beween men and women?
        For the same country, consider people between the ages of 75 and 84 only. Now compare the mortality rate for men and women again. Does it seem like there are any differences between men and women?

Section B. US Deaths due to COVID

Examine the nytcovstate data:

nytcovstate

## # A tibble: 41,390 × 5
##    date       state      fips  cases deaths
##    <date>     <chr>      <chr> <dbl>  <dbl>
##  1 2020-01-21 Washington 53        1      0
##  2 2020-01-22 Washington 53        1      0
##  3 2020-01-23 Washington 53        1      0
##  4 2020-01-24 Illinois   17        1      0
##  5 2020-01-24 Washington 53        1      0
##  6 2020-01-25 California 06        1      0
##  7 2020-01-25 Illinois   17        1      0
##  8 2020-01-25 Washington 53        1      0
##  9 2020-01-26 Arizona    04        1      0
## 10 2020-01-26 California 06        2      0
## # … with 41,380 more rows

The data are organized by date and state. Use group_by() and then calculate daily counts:

nytcovstate %>% 
  group_by(state) %>% 
  mutate(daily_cases = cases - lag(cases, order_by = date), 
         daily_deaths = deaths - lag(deaths, order_by = date)) 

## # A tibble: 41,390 × 7
## # Groups:   state [56]
##    date       state      fips  cases deaths daily_cases daily_deaths
##    <date>     <chr>      <chr> <dbl>  <dbl>       <dbl>        <dbl>
##  1 2020-01-21 Washington 53        1      0          NA           NA
##  2 2020-01-22 Washington 53        1      0           0            0
##  3 2020-01-23 Washington 53        1      0           0            0
##  4 2020-01-24 Illinois   17        1      0          NA           NA
##  5 2020-01-24 Washington 53        1      0           0            0
##  6 2020-01-25 California 06        1      0          NA           NA
##  7 2020-01-25 Illinois   17        1      0           0            0
##  8 2020-01-25 Washington 53        1      0           0            0
##  9 2020-01-26 Arizona    04        1      0          NA           NA
## 10 2020-01-26 California 06        2      0           1            0
## # … with 41,380 more rows

Create this table of U.S. State populations:

state_pops <- uspop %>%
  filter(sex_id == "totsex", hisp_id == "tothisp") %>%
  select(state_abbr, statefips, pop, state) %>%
  rename(name = state, 
         state = state_abbr, fips = statefips) %>%
  mutate(state = replace(state, fips == "11", "DC"))

state_pops

## # A tibble: 51 × 4
##    state fips       pop name                
##    <chr> <chr>    <dbl> <chr>               
##  1 AL    01     4887871 Alabama             
##  2 AK    02      737438 Alaska              
##  3 AZ    04     7171646 Arizona             
##  4 AR    05     3013825 Arkansas            
##  5 CA    06    39557045 California          
##  6 CO    08     5695564 Colorado            
##  7 CT    09     3572665 Connecticut         
##  8 DE    10      967171 Delaware            
##  9 DC    11      702455 District of Columbia
## 10 FL    12    21299325 Florida             
## # … with 41 more rows

Now,

    Join this table of state populations to nytcovstate using left_join().

    Calculate the case rate and the death rate per 100,000 people for each state.

    Plot the trend in daily cases for three states of your choice.

    Use the slide_dbl() function to calculate a weekly moving average. Draw the plot you made in step 3 again, adding the moving average as a new layer.

Section C. Mobility Data

Apple’s mobility data looks like this:

apple_mobility

## # A tibble: 3,743,418 × 8
##    geo_type       region  transportation_ty… alternative_name sub_region country
##    <chr>          <chr>   <chr>              <chr>            <chr>      <chr>  
##  1 country/region Albania driving            <NA>             <NA>       <NA>   
##  2 country/region Albania driving            <NA>             <NA>       <NA>   
##  3 country/region Albania driving            <NA>             <NA>       <NA>   
##  4 country/region Albania driving            <NA>             <NA>       <NA>   
##  5 country/region Albania driving            <NA>             <NA>       <NA>   
##  6 country/region Albania driving            <NA>             <NA>       <NA>   
##  7 country/region Albania driving            <NA>             <NA>       <NA>   
##  8 country/region Albania driving            <NA>             <NA>       <NA>   
##  9 country/region Albania driving            <NA>             <NA>       <NA>   
## 10 country/region Albania driving            <NA>             <NA>       <NA>   
## # … with 3,743,408 more rows, and 2 more variables: date <date>, score <dbl>

There are daily scores for driving, transit, and walking transportation types.

    Choose one region or city (e.g., somewhere near where you are from, if it is in the data). To do a quick search to see if somewhere is in there, try e.g.

apple_mobility %>% 
  filter(stringr::str_detect(region, "Cork"))

## # A tibble: 1,596 × 8
##    geo_type   region transportation_type alternative_name sub_region country
##    <chr>      <chr>  <chr>               <chr>            <chr>      <chr>  
##  1 sub-region Cork   driving             <NA>             <NA>       Ireland
##  2 sub-region Cork   driving             <NA>             <NA>       Ireland
##  3 sub-region Cork   driving             <NA>             <NA>       Ireland
##  4 sub-region Cork   driving             <NA>             <NA>       Ireland
##  5 sub-region Cork   driving             <NA>             <NA>       Ireland
##  6 sub-region Cork   driving             <NA>             <NA>       Ireland
##  7 sub-region Cork   driving             <NA>             <NA>       Ireland
##  8 sub-region Cork   driving             <NA>             <NA>       Ireland
##  9 sub-region Cork   driving             <NA>             <NA>       Ireland
## 10 sub-region Cork   driving             <NA>             <NA>       Ireland
## # … with 1,586 more rows, and 2 more variables: date <date>, score <dbl>

    Draw a graph of the mobility trends for your place of choice.

    What do you think this sort of data tells us about what’s happening in these places? What if we wanted to compare two or more places?
