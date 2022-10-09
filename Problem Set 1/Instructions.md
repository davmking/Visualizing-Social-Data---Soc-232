Problem Set 1

Instructions

    Use RStudio to create a file named something like lastname_firstname_ps01.Rmd

    Set up your file with a code chunk that loads the tidyverse, gapminder, and palmerpenguins packages along with socviz:

library(tidyverse)
library(gapminder)
library(palmerpenguins)
library(socviz)

    Then use R to draw these plots:

GDP and Life Expectancy 1

Figure 1: GDP and Life Expectancy 1
GDP and Life Expectancy 2

Figure 2: GDP and Life Expectancy 2
GDP and Life Expectancy 3

Figure 3: GDP and Life Expectancy 3
GDP and Life Expectancy 4

Figure 4: GDP and Life Expectancy 4

    Discuss in a few words

        What the mappings are in each plot.
        How the plots do or do not illustrate the principles Tufte discusses.
        The differences between Figures 2, 3, and 4, and what difference if any those differences make to your interpretation of the figure.

    Consider the penguins data:

penguins

## # A tibble: 344 × 8
##    species island    bill_length_mm bill_depth_mm flipper_length_mm body_mass_g
##    <fct>   <fct>              <dbl>         <dbl>             <int>       <int>
##  1 Adelie  Torgersen           39.1          18.7               181        3750
##  2 Adelie  Torgersen           39.5          17.4               186        3800
##  3 Adelie  Torgersen           40.3          18                 195        3250
##  4 Adelie  Torgersen           NA            NA                  NA          NA
##  5 Adelie  Torgersen           36.7          19.3               193        3450
##  6 Adelie  Torgersen           39.3          20.6               190        3650
##  7 Adelie  Torgersen           38.9          17.8               181        3625
##  8 Adelie  Torgersen           39.2          19.6               195        4675
##  9 Adelie  Torgersen           34.1          18.1               193        3475
## 10 Adelie  Torgersen           42            20.2               190        4250
## # … with 334 more rows, and 2 more variables: sex <fct>, year <int>

    Use what you’ve learned so far about plotting to explore this data and make a figure that shows something interesting about the data. Say in a sentence or two why it’s interesting.

Submitting your work

    By 5pm on Tuesday, submit your finished work to the Drop Box on the Sakai Site for this course.
    You should include two files. (1) An RMarkdown (Rmd) file with a name of the form lastname_firstname_ps01.Rmd. (2) Either an HTML, or a PDF, or a Word file with the name e.g. lastname_firstname_ps01.html that is the knitted/completed version of the Rmd file.
