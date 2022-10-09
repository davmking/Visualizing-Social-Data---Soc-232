Problem Set 04
Instructions

        Use RStudio to create a file named something like lastname_firstname_ps04.Rmd

    2.From last week, you should already have a Census API key and have the the tidycensus package installed. Reread Walker Chapter 2 for more details.
        Set up your file with a code chunk that loads the tidyverse, socviz, tidycensus, sf, and tigris packages.
        Put the line

options(tigris_use_cache = TRUE)

after you load your packages. (This will prevent downloading Census data more than once.)

Next, we will get some population measures from the American Community Survey.

We make two little tables of the official variable name and a more readable short name for it.

pop_names <- tribble(
    ~varname, ~clean_name,
    "B01003_001", "pop",
    "B01001B_001", "black",
    "B01001A_001", "white",
    "B01001H_001", "nh_white",
    "B01001I_001", "hispanic",
    "B01001D_001", "asian"
  )
  
pop_names

## # A tibble: 6 × 2
##   varname     clean_name
##   <chr>       <chr>     
## 1 B01003_001  pop       
## 2 B01001B_001 black     
## 3 B01001A_001 white     
## 4 B01001H_001 nh_white  
## 5 B01001I_001 hispanic  
## 6 B01001D_001 asian

inc_names <- tribble(
    ~varname, ~clean_name,
    "S1901_C01_012", "median_hh_inc")

Now we download county-level data for these variables and make some tables of the results:

## Population groups
## Code nerds note the use of `reduce` here 
fips_pop <- get_acs(geography = "county", 
                    variables = pop_names$varname, 
                    cache_table = TRUE) %>% 
  mutate(variable = reduce2(pop_names$varname, 
                            pop_names$clean_name, 
                            str_replace, 
                            .init = variable)) %>% 
  select(-moe) %>% 
  pivot_wider(names_from = variable, values_from = estimate) %>% 
  rename(fips = GEOID, name = NAME) 

## Getting data from the 2016-2020 5-year ACS

fips_pop

## # A tibble: 3,221 × 8
##    fips  name                      white black asian nh_white hispanic    pop
##    <chr> <chr>                     <dbl> <dbl> <dbl>    <dbl>    <dbl>  <dbl>
##  1 01001 Autauga County, Alabama   42150 10866   649    41160     1601  55639
##  2 01003 Baldwin County, Alabama  186504 19153  2033   180955     9947 218289
##  3 01005 Barbour County, Alabama   11587 11929   122    11332     1110  25026
##  4 01007 Bibb County, Alabama      17138  5045    56    16650      600  22374
##  5 01009 Blount County, Alabama    54271   808   236    50065     5362  57755
##  6 01011 Bullock County, Alabama    2663  6980   137     2162      824  10173
##  7 01013 Butler County, Alabama    10183  8795   261    10125      290  19726
##  8 01015 Calhoun County, Alabama   83801 24190   925    82049     4406 114324
##  9 01017 Chambers County, Alabama  19015 13372   368    18413      843  33427
## 10 01019 Cherokee County, Alabama  23970  1250    26    23779      432  26035
## # … with 3,211 more rows

## Income data
fips_inc <- get_acs(geography = "county", 
                    variables = inc_names$varname,  
                    cache_table = TRUE) %>% 
  mutate(variable = str_replace(variable, 
                            inc_names$varname, 
                            inc_names$clean_name)) %>% 
  rename(fips = GEOID, name = NAME) 

## Getting data from the 2016-2020 5-year ACS

## Using the ACS Subject Tables

Next we grab the spatial data that will let us draw county-level maps.

fips_map <- get_acs(geography = "county", 
                    variables = "B01001_001", 
                    geometry = TRUE,
                    resolution = "20m",
                    cache_table = TRUE) %>%
  shift_geometry() %>%
  select(GEOID, NAME, geometry) %>% 
  rename(fips = GEOID, name = NAME)

fips_map

## Simple feature collection with 3221 features and 2 fields
## Geometry type: GEOMETRY
## Dimension:     XY
## Bounding box:  xmin: -3112200 ymin: -1697728 xmax: 2258154 ymax: 1558935
## Projected CRS: USA_Contiguous_Albers_Equal_Area_Conic
## First 10 features:
##     fips                          name                       geometry
## 1  05121     Randolph County, Arkansas MULTIPOLYGON (((407670.6 -1...
## 2  08069      Larimer County, Colorado MULTIPOLYGON (((-848738.5 4...
## 3  26105        Mason County, Michigan MULTIPOLYGON (((756117.8 77...
## 4  28153     Wayne County, Mississippi MULTIPOLYGON (((664464.4 -6...
## 5  38075 Renville County, North Dakota MULTIPOLYGON (((-447168.5 1...
## 6  39125         Paulding County, Ohio MULTIPOLYGON (((928784.5 47...
## 7  21095       Harlan County, Kentucky MULTIPOLYGON (((1101261 484...
## 8  48063            Camp County, Texas MULTIPOLYGON (((78719.39 -5...
## 9  40101     Muskogee County, Oklahoma MULTIPOLYGON (((20934.8 -18...
## 10 29039        Cedar County, Missouri MULTIPOLYGON (((168374.4 17...

Questions

        Using the fips_pop table, calculate the proportion of each racial or ethnic category per 100,000 people. You should end up with new columns showing that number (e.g. hispanic_prop) for each county.
        Join the table of population information to the spatial data using left_join(). How do you know your join worked?
        Draw maps of the proportion of county residents by race or ethnicity for any two of the categories. Choose a color palette that you think works well for showing these data.
        Read the R help page for the cut_interval() function in ggplot2. Use cut_interval() to turn the proportional measures into binned groups and map any two of those. Do the resulting maps look more or less informative than the continuous versions? Why or why not?
        Draw a map of the Median Household Income data.
        What are the ten richest counties in the U.S. according to this data? What are the five poorest?
