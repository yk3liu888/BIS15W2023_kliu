---
title: "Lab 6 Homework"
author: "Joel Ledford"
date: "2023-02-01"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(skimr)
```

For this assignment we are going to work with a large data set from the [United Nations Food and Agriculture Organization](http://www.fao.org/about/en/) on world fisheries. These data are pretty wild, so we need to do some cleaning. First, load the data.  

Load the data `FAO_1950to2012_111914.csv` as a new object titled `fisheries`.

```r
fisheries <- readr::read_csv(file = "data/FAO_1950to2012_111914.csv")
```

```
## Rows: 17692 Columns: 71
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (69): Country, Common name, ISSCAAP taxonomic group, ASFIS species#, ASF...
## dbl  (2): ISSCAAP group#, FAO major fishing area
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

1. Do an exploratory analysis of the data (your choice). What are the names of the variables, what are the dimensions, are there any NA's, what are the classes of the variables?  

```r
names(fisheries)
```

```
##  [1] "Country"                 "Common name"            
##  [3] "ISSCAAP group#"          "ISSCAAP taxonomic group"
##  [5] "ASFIS species#"          "ASFIS species name"     
##  [7] "FAO major fishing area"  "Measure"                
##  [9] "1950"                    "1951"                   
## [11] "1952"                    "1953"                   
## [13] "1954"                    "1955"                   
## [15] "1956"                    "1957"                   
## [17] "1958"                    "1959"                   
## [19] "1960"                    "1961"                   
## [21] "1962"                    "1963"                   
## [23] "1964"                    "1965"                   
## [25] "1966"                    "1967"                   
## [27] "1968"                    "1969"                   
## [29] "1970"                    "1971"                   
## [31] "1972"                    "1973"                   
## [33] "1974"                    "1975"                   
## [35] "1976"                    "1977"                   
## [37] "1978"                    "1979"                   
## [39] "1980"                    "1981"                   
## [41] "1982"                    "1983"                   
## [43] "1984"                    "1985"                   
## [45] "1986"                    "1987"                   
## [47] "1988"                    "1989"                   
## [49] "1990"                    "1991"                   
## [51] "1992"                    "1993"                   
## [53] "1994"                    "1995"                   
## [55] "1996"                    "1997"                   
## [57] "1998"                    "1999"                   
## [59] "2000"                    "2001"                   
## [61] "2002"                    "2003"                   
## [63] "2004"                    "2005"                   
## [65] "2006"                    "2007"                   
## [67] "2008"                    "2009"                   
## [69] "2010"                    "2011"                   
## [71] "2012"
```

```r
anyNA(fisheries)
```

```
## [1] TRUE
```

```r
dim(fisheries)
```

```
## [1] 17692    71
```

```r
class(fisheries)
```

```
## [1] "spec_tbl_df" "tbl_df"      "tbl"         "data.frame"
```

2. Use `janitor` to rename the columns and make them easier to use. As part of this cleaning step, change `country`, `isscaap_group_number`, `asfis_species_number`, and `fao_major_fishing_area` to data class factor. 

```r
fisheries <- janitor::clean_names(fisheries)
fisheries
```

```
## # A tibble: 17,692 × 71
##    country common_…¹ issca…² issca…³ asfis…⁴ asfis…⁵ fao_m…⁶ measure x1950 x1951
##    <chr>   <chr>       <dbl> <chr>   <chr>   <chr>     <dbl> <chr>   <chr> <chr>
##  1 Albania Angelsha…      38 Sharks… 10903X… Squati…      37 Quanti… <NA>  <NA> 
##  2 Albania Atlantic…      36 Tunas,… 175010… Sarda …      37 Quanti… <NA>  <NA> 
##  3 Albania Barracud…      37 Miscel… 177100… Sphyra…      37 Quanti… <NA>  <NA> 
##  4 Albania Blue and…      45 Shrimp… 228020… Ariste…      37 Quanti… <NA>  <NA> 
##  5 Albania Blue whi…      32 Cods, … 148040… Microm…      37 Quanti… <NA>  <NA> 
##  6 Albania Bluefish       37 Miscel… 170202… Pomato…      37 Quanti… <NA>  <NA> 
##  7 Albania Bogue          33 Miscel… 170392… Boops …      37 Quanti… <NA>  <NA> 
##  8 Albania Caramote…      45 Shrimp… 228010… Penaeu…      37 Quanti… <NA>  <NA> 
##  9 Albania Catshark…      38 Sharks… 108010… Scylio…      37 Quanti… <NA>  <NA> 
## 10 Albania Common c…      57 Squids… 321020… Sepia …      37 Quanti… <NA>  <NA> 
## # … with 17,682 more rows, 61 more variables: x1952 <chr>, x1953 <chr>,
## #   x1954 <chr>, x1955 <chr>, x1956 <chr>, x1957 <chr>, x1958 <chr>,
## #   x1959 <chr>, x1960 <chr>, x1961 <chr>, x1962 <chr>, x1963 <chr>,
## #   x1964 <chr>, x1965 <chr>, x1966 <chr>, x1967 <chr>, x1968 <chr>,
## #   x1969 <chr>, x1970 <chr>, x1971 <chr>, x1972 <chr>, x1973 <chr>,
## #   x1974 <chr>, x1975 <chr>, x1976 <chr>, x1977 <chr>, x1978 <chr>,
## #   x1979 <chr>, x1980 <chr>, x1981 <chr>, x1982 <chr>, x1983 <chr>, …
```


```r
fisheries <- fisheries %>% 
  mutate(across(c("country", "isscaap_group_number",
                  "asfis_species_number",
                  "fao_major_fishing_area"),as_factor))
fisheries
```

```
## # A tibble: 17,692 × 71
##    country common_…¹ issca…² issca…³ asfis…⁴ asfis…⁵ fao_m…⁶ measure x1950 x1951
##    <fct>   <chr>     <fct>   <chr>   <fct>   <chr>   <fct>   <chr>   <chr> <chr>
##  1 Albania Angelsha… 38      Sharks… 10903X… Squati… 37      Quanti… <NA>  <NA> 
##  2 Albania Atlantic… 36      Tunas,… 175010… Sarda … 37      Quanti… <NA>  <NA> 
##  3 Albania Barracud… 37      Miscel… 177100… Sphyra… 37      Quanti… <NA>  <NA> 
##  4 Albania Blue and… 45      Shrimp… 228020… Ariste… 37      Quanti… <NA>  <NA> 
##  5 Albania Blue whi… 32      Cods, … 148040… Microm… 37      Quanti… <NA>  <NA> 
##  6 Albania Bluefish  37      Miscel… 170202… Pomato… 37      Quanti… <NA>  <NA> 
##  7 Albania Bogue     33      Miscel… 170392… Boops … 37      Quanti… <NA>  <NA> 
##  8 Albania Caramote… 45      Shrimp… 228010… Penaeu… 37      Quanti… <NA>  <NA> 
##  9 Albania Catshark… 38      Sharks… 108010… Scylio… 37      Quanti… <NA>  <NA> 
## 10 Albania Common c… 57      Squids… 321020… Sepia … 37      Quanti… <NA>  <NA> 
## # … with 17,682 more rows, 61 more variables: x1952 <chr>, x1953 <chr>,
## #   x1954 <chr>, x1955 <chr>, x1956 <chr>, x1957 <chr>, x1958 <chr>,
## #   x1959 <chr>, x1960 <chr>, x1961 <chr>, x1962 <chr>, x1963 <chr>,
## #   x1964 <chr>, x1965 <chr>, x1966 <chr>, x1967 <chr>, x1968 <chr>,
## #   x1969 <chr>, x1970 <chr>, x1971 <chr>, x1972 <chr>, x1973 <chr>,
## #   x1974 <chr>, x1975 <chr>, x1976 <chr>, x1977 <chr>, x1978 <chr>,
## #   x1979 <chr>, x1980 <chr>, x1981 <chr>, x1982 <chr>, x1983 <chr>, …
```

We need to deal with the years because they are being treated as characters and start with an X. We also have the problem that the column names that are years actually represent data. We haven't discussed tidy data yet, so here is some help. You should run this ugly chunk to transform the data for the rest of the homework. It will only work if you have used janitor to rename the variables in question 2!

```r
fisheries_tidy <- fisheries %>% 
  pivot_longer(-c(country,common_name,isscaap_group_number,isscaap_taxonomic_group,asfis_species_number,asfis_species_name,fao_major_fishing_area,measure),
               names_to = "year",
               values_to = "catch",
               values_drop_na = TRUE) %>% 
  mutate(year= as.numeric(str_replace(year, 'x', ''))) %>% 
  mutate(catch= str_replace(catch, c(' F'), replacement = '')) %>% 
  mutate(catch= str_replace(catch, c('...'), replacement = '')) %>% 
  mutate(catch= str_replace(catch, c('-'), replacement = '')) %>% 
  mutate(catch= str_replace(catch, c('0 0'), replacement = ''))

fisheries_tidy$catch <- as.numeric(fisheries_tidy$catch)
fisheries_tidy
```

```
## # A tibble: 376,771 × 10
##    country common_…¹ issca…² issca…³ asfis…⁴ asfis…⁵ fao_m…⁶ measure  year catch
##    <fct>   <chr>     <fct>   <chr>   <fct>   <chr>   <fct>   <chr>   <dbl> <dbl>
##  1 Albania Angelsha… 38      Sharks… 10903X… Squati… 37      Quanti…  1995    NA
##  2 Albania Angelsha… 38      Sharks… 10903X… Squati… 37      Quanti…  1996    53
##  3 Albania Angelsha… 38      Sharks… 10903X… Squati… 37      Quanti…  1997    20
##  4 Albania Angelsha… 38      Sharks… 10903X… Squati… 37      Quanti…  1998    31
##  5 Albania Angelsha… 38      Sharks… 10903X… Squati… 37      Quanti…  1999    30
##  6 Albania Angelsha… 38      Sharks… 10903X… Squati… 37      Quanti…  2000    30
##  7 Albania Angelsha… 38      Sharks… 10903X… Squati… 37      Quanti…  2001    16
##  8 Albania Angelsha… 38      Sharks… 10903X… Squati… 37      Quanti…  2002    79
##  9 Albania Angelsha… 38      Sharks… 10903X… Squati… 37      Quanti…  2003     1
## 10 Albania Angelsha… 38      Sharks… 10903X… Squati… 37      Quanti…  2004     4
## # … with 376,761 more rows, and abbreviated variable names ¹​common_name,
## #   ²​isscaap_group_number, ³​isscaap_taxonomic_group, ⁴​asfis_species_number,
## #   ⁵​asfis_species_name, ⁶​fao_major_fishing_area
```

3. How many countries are represented in the data? Provide a count and list their names.

```r
count(fisheries_tidy, country)
```

```
## # A tibble: 203 × 2
##    country                 n
##    <fct>               <int>
##  1 Albania               934
##  2 Algeria              1561
##  3 American Samoa        556
##  4 Angola               2119
##  5 Anguilla              129
##  6 Antigua and Barbuda   356
##  7 Argentina            3403
##  8 Aruba                 172
##  9 Australia            8183
## 10 Bahamas               423
## # … with 193 more rows
```
There are 203 countries.

4. Refocus the data only to include country, isscaap_taxonomic_group, asfis_species_name, asfis_species_number, year, catch.

```r
fisheries_tidy_select <- fisheries_tidy %>% 
  select(country,isscaap_taxonomic_group, asfis_species_name, asfis_species_number, year, catch )
fisheries_tidy_select
```

```
## # A tibble: 376,771 × 6
##    country isscaap_taxonomic_group asfis_species_name asfis_specie…¹  year catch
##    <fct>   <chr>                   <chr>              <fct>          <dbl> <dbl>
##  1 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX      1995    NA
##  2 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX      1996    53
##  3 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX      1997    20
##  4 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX      1998    31
##  5 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX      1999    30
##  6 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX      2000    30
##  7 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX      2001    16
##  8 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX      2002    79
##  9 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX      2003     1
## 10 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX      2004     4
## # … with 376,761 more rows, and abbreviated variable name ¹​asfis_species_number
```

5. Based on the asfis_species_number, how many distinct fish species were caught as part of these data?

```r
n_distinct(fisheries_tidy_select$asfis_species_number)
```

```
## [1] 1551
```

6. Which country had the largest overall catch in the year 2000?

```r
fisheries_tidy_select %>% 
  filter(year==2000) %>% 
  select(country, catch) %>% 
  arrange(desc(catch))
```

```
## # A tibble: 8,793 × 2
##    country                  catch
##    <fct>                    <dbl>
##  1 China                     9068
##  2 Peru                      5717
##  3 Russian Federation        5065
##  4 Viet Nam                  4945
##  5 Chile                     4299
##  6 China                     3288
##  7 China                     2782
##  8 United States of America  2438
##  9 China                     1234
## 10 Philippines                999
## # … with 8,783 more rows
```
China has the largest overall catch.

7. Which country caught the most sardines (_Sardina pilchardus_) between the years 1990-2000?

```r
fisheries_tidy_select %>% 
  filter(2000 >= year, year >= 1999) %>% 
  filter(asfis_species_name == "Sardina pilchardus") %>% 
  group_by(country) %>%  
  summarise(sardines_catch = sum(catch, na.rm = T)) %>% 
  arrange(desc(sardines_catch))
```

```
## # A tibble: 27 × 2
##    country                  sardines_catch
##    <fct>                             <dbl>
##  1 Morocco                            1092
##  2 France                              265
##  3 Spain                               249
##  4 Portugal                            191
##  5 Ukraine                             144
##  6 Algeria                             101
##  7 Saint Vincent/Grenadines             96
##  8 Tunisia                              95
##  9 Albania                              85
## 10 Serbia and Montenegro                85
## # … with 17 more rows
```

Morocco caught the most sardines.

8. Which five countries caught the most cephalopods between 2008-2012?

```r
fisheries_tidy_select %>% 
  filter(2012 >= year, year >= 2008)%>% 
  filter(asfis_species_name == "Cephalopoda") %>% 
  group_by(country) %>%  
  summarise(cephalopoda_catch = sum(catch, na.rm = T)) %>% 
  arrange(desc(cephalopoda_catch)) %>% 
  head(5)
```

```
## # A tibble: 5 × 2
##   country cephalopoda_catch
##   <fct>               <dbl>
## 1 India                 570
## 2 China                 257
## 3 Spain                 198
## 4 Algeria               162
## 5 France                101
```
India, China, France, Algeria, Spain are the top five.

9. Which species had the highest catch total between 2008-2012? (hint: Osteichthyes is not a species)

```r
fisheries_tidy %>% 
  filter(2012 >= year, year >= 2008)%>% 
  group_by(common_name) %>%  
  count(common_name) %>% 
  arrange(desc(n))
```

```
## # A tibble: 1,477 × 2
## # Groups:   common_name [1,477]
##    common_name                        n
##    <chr>                          <int>
##  1 Marine fishes nei               1330
##  2 Yellowfin tuna                  1006
##  3 Bigeye tuna                      923
##  4 Swordfish                        837
##  5 Sharks, rays, skates, etc. nei   761
##  6 Skipjack tuna                    761
##  7 Albacore                         755
##  8 Blue marlin                      584
##  9 Mullets nei                      482
## 10 Rays, stingrays, mantas nei      427
## # … with 1,467 more rows
```
Marine fishes nei has the highest total.

10. Use the data to do at least one analysis of your choice.

```r
fisheries_tidy_select %>% 
  filter(2002 >= year, year>=2000) %>% 
  group_by(country) %>% 
  summarize(mean_catch = mean(catch, na.rm = T)) %>% 
  arrange(desc(mean_catch))
```

```
## # A tibble: 193 × 2
##    country                  mean_catch
##    <fct>                         <dbl>
##  1 Myanmar                       809. 
##  2 Viet Nam                      469. 
##  3 China                         428. 
##  4 Bangladesh                    274. 
##  5 Peru                          207. 
##  6 Chile                         193. 
##  7 Iceland                       119. 
##  8 Korea, Dem. People's Rep       89.1
##  9 French Southern Terr           80  
## 10 Norway                         77.5
## # … with 183 more rows
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
