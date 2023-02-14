---
title: "Lab 9 Homework"
author: "Kellie Liu"
date: "2023-02-14"
output:
  html_document:
    theme: spacelab
    toc: no
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(here)
library(naniar)
```

For this homework, we will take a departure from biological data and use data about California colleges. These data are a subset of the national college scorecard (https://collegescorecard.ed.gov/data/). Load the `ca_college_data.csv` as a new object called `colleges`.

```r
college <- read_csv(here("lab9", "data", "ca_college_data.csv"))
```

```
## Rows: 341 Columns: 10
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): INSTNM, CITY, STABBR, ZIP
## dbl (6): ADM_RATE, SAT_AVG, PCIP26, COSTT4_A, C150_4_POOLED, PFTFTUG1_EF
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

The variables are a bit hard to decipher, here is a key:  

INSTNM: Institution name  
CITY: California city  
STABBR: Location state  
ZIP: Zip code  
ADM_RATE: Admission rate  
SAT_AVG: SAT average score  
PCIP26: Percentage of degrees awarded in Biological And Biomedical Sciences  
COSTT4_A: Annual cost of attendance  
C150_4_POOLED: 4-year completion rate  
PFTFTUG1_EF: Percentage of undergraduate students who are first-time, full-time degree/certificate-seeking undergraduate students  

1. Use your preferred function(s) to have a look at the data and get an idea of its structure. Make sure you summarize NA's and determine whether or not the data are tidy. You may also consider dealing with any naming issues.

```r
glimpse(college)
```

```
## Rows: 341
## Columns: 10
## $ INSTNM        <chr> "Grossmont College", "College of the Sequoias", "College…
## $ CITY          <chr> "El Cajon", "Visalia", "San Mateo", "Ventura", "Oxnard",…
## $ STABBR        <chr> "CA", "CA", "CA", "CA", "CA", "CA", "CA", "CA", "CA", "C…
## $ ZIP           <chr> "92020-1799", "93277-2214", "94402-3784", "93003-3872", …
## $ ADM_RATE      <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ SAT_AVG       <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ PCIP26        <dbl> 0.0016, 0.0066, 0.0038, 0.0035, 0.0085, 0.0151, 0.0000, …
## $ COSTT4_A      <dbl> 7956, 8109, 8278, 8407, 8516, 8577, 8580, 9181, 9281, 93…
## $ C150_4_POOLED <dbl> NA, NA, NA, NA, NA, NA, 0.2334, NA, NA, NA, NA, 0.1704, …
## $ PFTFTUG1_EF   <dbl> 0.3546, 0.5413, 0.3567, 0.3824, 0.2753, 0.4286, 0.2307, …
```


```r
college <- janitor::clean_names(college)
```


```r
names(college)
```

```
##  [1] "instnm"        "city"          "stabbr"        "zip"          
##  [5] "adm_rate"      "sat_avg"       "pcip26"        "costt4_a"     
##  [9] "c150_4_pooled" "pftftug1_ef"
```

2. Which cities in California have the highest number of colleges?

```r
college %>% 
  count(city) %>% 
  slice_max(n, n=1) %>% 
  ggplot(aes(x=city, y=n))+
  geom_col()
```

![](lab9_hw_files/figure-html/unnamed-chunk-6-1.png)<!-- -->
Los Angeles has the highest number of colleges.

3. Based on your answer to #2, make a plot that shows the number of colleges in the top 10 cities.

```r
college %>% 
  count(city) %>% 
  slice_max(n, n=10) %>% 
  ggplot(aes(x=city, y=n))+
  geom_col()+
  coord_flip()
```

![](lab9_hw_files/figure-html/unnamed-chunk-7-1.png)<!-- -->
4. The column `COSTT4_A` is the annual cost of each institution. Which city has the highest average cost? Where is it located?

```r
college %>% 
  group_by(city)%>% 
  summarise(mean_costt4_a = mean(costt4_a, na.rm = T)) %>% 
  slice_max(mean_costt4_a, n=1) %>% 
  ggplot(aes(x=city, mean_costt4_a))+
  geom_col()+
  coord_flip()
```

![](lab9_hw_files/figure-html/unnamed-chunk-8-1.png)<!-- -->
Claremont has the highest average cost.

5. Based on your answer to #4, make a plot that compares the cost of the individual colleges in the most expensive city. Bonus! Add UC Davis here to see how it compares :>).

```r
college %>% 
  group_by(instnm) %>%
  summarise(mean_costt4_a = mean(costt4_a, na.rm = T)) %>% 
  slice_max(mean_costt4_a, n=5) %>%
  ggplot(aes(x=instnm, mean_costt4_a))+
  geom_col()+
  coord_flip()
```

![](lab9_hw_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

6. The column `ADM_RATE` is the admissions rate by college and `C150_4_POOLED` is the four-year completion rate. Use a scatterplot to show the relationship between these two variables. What do you think this means?

```r
college %>% 
  ggplot(aes(x=adm_rate, y=c150_4_pooled))+geom_jitter()
```

```
## Warning: Removed 251 rows containing missing values (`geom_point()`).
```

![](lab9_hw_files/figure-html/unnamed-chunk-10-1.png)<!-- -->
It seems like higher admission rate tend to have lower four-year completion rate.

7. Is there a relationship between cost and four-year completion rate? (You don't need to do the stats, just produce a plot). What do you think this means?

```r
college %>% 
  ggplot(aes(x=costt4_a, y=c150_4_pooled))+geom_jitter()
```

```
## Warning: Removed 225 rows containing missing values (`geom_point()`).
```

![](lab9_hw_files/figure-html/unnamed-chunk-11-1.png)<!-- -->
It seems like higher cost tend to have higher four-year completion rate.

8. The column titled `INSTNM` is the institution name. We are only interested in the University of California colleges. Make a new data frame that is restricted to UC institutions. You can remove `Hastings College of Law` and `UC San Francisco` as we are only interested in undergraduate institutions.

```r
college %>% 
   filter_all(any_vars(str_detect(., pattern = "University of California")))
```

```
## # A tibble: 10 × 10
##    instnm      city  stabbr zip   adm_r…¹ sat_avg pcip26 costt…² c150_…³ pftft…⁴
##    <chr>       <chr> <chr>  <chr>   <dbl>   <dbl>  <dbl>   <dbl>   <dbl>   <dbl>
##  1 University… La J… CA     92093   0.357    1324  0.216   31043   0.872   0.662
##  2 University… Irvi… CA     92697   0.406    1206  0.107   31198   0.876   0.725
##  3 University… Rive… CA     92521   0.663    1078  0.149   31494   0.73    0.811
##  4 University… Los … CA     9009…   0.180    1334  0.155   33078   0.911   0.661
##  5 University… Davis CA     9561…   0.423    1218  0.198   33904   0.850   0.605
##  6 University… Sant… CA     9506…   0.578    1201  0.193   34608   0.776   0.786
##  7 University… Berk… CA     94720   0.169    1422  0.105   34924   0.916   0.709
##  8 University… Sant… CA     93106   0.358    1281  0.108   34998   0.816   0.708
##  9 University… San … CA     9410…  NA          NA NA          NA  NA      NA    
## 10 University… San … CA     9414…  NA          NA NA          NA  NA      NA    
## # … with abbreviated variable names ¹​adm_rate, ²​costt4_a, ³​c150_4_pooled,
## #   ⁴​pftftug1_ef
```

Remove `Hastings College of Law` and `UC San Francisco` and store the final data frame as a new object `univ_calif_final`.

```r
univ_calif_final<-college %>% 
  filter_all(any_vars(str_detect(., pattern = "University of 
                                 California"))) %>% 
  filter(instnm != c("University of California-Hastings College of Law", 
                     "University of California-San Francisco"))
univ_calif_final
```

```
## # A tibble: 0 × 10
## # … with 10 variables: instnm <chr>, city <chr>, stabbr <chr>, zip <chr>,
## #   adm_rate <dbl>, sat_avg <dbl>, pcip26 <dbl>, costt4_a <dbl>,
## #   c150_4_pooled <dbl>, pftftug1_ef <dbl>
```

Use `separate()` to separate institution name into two new columns "UNIV" and "CAMPUS".

```r
univ_calif_final %>% 
  separate(instnm, into= c("UNIV", "CAMPUS"))
```

```
## # A tibble: 0 × 11
## # … with 11 variables: UNIV <chr>, CAMPUS <chr>, city <chr>, stabbr <chr>,
## #   zip <chr>, adm_rate <dbl>, sat_avg <dbl>, pcip26 <dbl>, costt4_a <dbl>,
## #   c150_4_pooled <dbl>, pftftug1_ef <dbl>
```

9. The column `ADM_RATE` is the admissions rate by campus. Which UC has the lowest and highest admissions rates? Produce a numerical summary and an appropriate plot.

```r
univ_calif_final %>% 
  group_by(instnm) %>% 
  select(instnm, adm_rate) %>%
  arrange(-adm_rate)
```

```
## # A tibble: 0 × 2
## # Groups:   instnm [0]
## # … with 2 variables: instnm <chr>, adm_rate <dbl>
```
Berkeley has the lowest, Riverside has the highest.


```r
univ_calif_final %>% 
  ggplot(aes(x=instnm, y=adm_rate))+
  geom_col()+
  coord_flip() 
```

![](lab9_hw_files/figure-html/unnamed-chunk-16-1.png)<!-- -->

10. If you wanted to get a degree in biological or biomedical sciences, which campus confers the majority of these degrees? Produce a numerical summary and an appropriate plot.

```r
univ_calif_final %>% 
  group_by(instnm) %>% 
  select(instnm, pcip26) %>%
  arrange(-pcip26)
```

```
## # A tibble: 0 × 2
## # Groups:   instnm [0]
## # … with 2 variables: instnm <chr>, pcip26 <dbl>
```
San Diego campus confers the majority of biological or biomedical sciences degree.


```r
univ_calif_final %>% 
  ggplot(aes(x=instnm, y=pcip26))+
  geom_col()+
  coord_flip() 
```

![](lab9_hw_files/figure-html/unnamed-chunk-18-1.png)<!-- -->

## Knit Your Output and Post to [GitHub](https://github.com/FRS417-DataScienceBiologists)
