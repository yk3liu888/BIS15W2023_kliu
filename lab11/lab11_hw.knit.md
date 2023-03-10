---
title: "Lab 11 Homework"
author: "Kellie Liu"
date: "2023-02-20"
output:
  html_document:
    theme: spacelab
    toc: no
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

**In this homework, you should make use of the aesthetics you have learned. It's OK to be flashy!**

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(here)
library(naniar)
```

## Resources
The idea for this assignment came from [Rebecca Barter's](http://www.rebeccabarter.com/blog/2017-11-17-ggplot2_tutorial/) ggplot tutorial so if you get stuck this is a good place to have a look.  

## Gapminder
For this assignment, we are going to use the dataset [gapminder](https://cran.r-project.org/web/packages/gapminder/index.html). Gapminder includes information about economics, population, and life expectancy from countries all over the world. You will need to install it before use. This is the same data that we will use for midterm 2 so this is good practice.

```r
#install.packages("gapminder")
library("gapminder")
```

## Questions
The questions below are open-ended and have many possible solutions. Your approach should, where appropriate, include numerical summaries and visuals. Be creative; assume you are building an analysis that you would ultimately present to an audience of stakeholders. Feel free to try out different `geoms` if they more clearly present your results.  

**1. Use the function(s) of your choice to get an idea of the overall structure of the data frame, including its dimensions, column names, variable classes, etc. As part of this, determine how NA's are treated in the data.**  


```r
names(gapminder)
```

```
## [1] "country"   "continent" "year"      "lifeExp"   "pop"       "gdpPercap"
```

```r
dim(gapminder)
```

```
## [1] 1704    6
```

```r
glimpse(gapminder)
```

```
## Rows: 1,704
## Columns: 6
## $ country   <fct> "Afghanistan", "Afghanistan", "Afghanistan", "Afghanistan", ???
## $ continent <fct> Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia, ???
## $ year      <int> 1952, 1957, 1962, 1967, 1972, 1977, 1982, 1987, 1992, 1997, ???
## $ lifeExp   <dbl> 28.801, 30.332, 31.997, 34.020, 36.088, 38.438, 39.854, 40.8???
## $ pop       <int> 8425333, 9240934, 10267083, 11537966, 13079460, 14880372, 12???
## $ gdpPercap <dbl> 779.4453, 820.8530, 853.1007, 836.1971, 739.9811, 786.1134, ???
```

```r
class(gapminder)
```

```
## [1] "tbl_df"     "tbl"        "data.frame"
```

```r
anyNA(gapminder)
```

```
## [1] FALSE
```

```r
gapminder %>% 
  na_if(-999) %>% 
  miss_var_summary()
```

```
## # A tibble: 6 ?? 3
##   variable  n_miss pct_miss
##   <chr>      <int>    <dbl>
## 1 country        0        0
## 2 continent      0        0
## 3 year           0        0
## 4 lifeExp        0        0
## 5 pop            0        0
## 6 gdpPercap      0        0
```


```r
summary(gapminder)
```

```
##         country        continent        year         lifeExp     
##  Afghanistan:  12   Africa  :624   Min.   :1952   Min.   :23.60  
##  Albania    :  12   Americas:300   1st Qu.:1966   1st Qu.:48.20  
##  Algeria    :  12   Asia    :396   Median :1980   Median :60.71  
##  Angola     :  12   Europe  :360   Mean   :1980   Mean   :59.47  
##  Argentina  :  12   Oceania : 24   3rd Qu.:1993   3rd Qu.:70.85  
##  Australia  :  12                  Max.   :2007   Max.   :82.60  
##  (Other)    :1632                                                
##       pop              gdpPercap       
##  Min.   :6.001e+04   Min.   :   241.2  
##  1st Qu.:2.794e+06   1st Qu.:  1202.1  
##  Median :7.024e+06   Median :  3531.8  
##  Mean   :2.960e+07   Mean   :  7215.3  
##  3rd Qu.:1.959e+07   3rd Qu.:  9325.5  
##  Max.   :1.319e+09   Max.   :113523.1  
## 
```

**2. Among the interesting variables in gapminder is life expectancy. How has global life expectancy changed between 1952 and 2007?**

```r
gapminder %>% 
  filter(year>=1952, year<=2007) %>% 
  ggplot(aes(x=year, y=lifeExp, fill=year))+
  geom_col()+
  labs(title="Life Expectancy 1952-2007")
```

![](lab11_hw_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

**3. How do the distributions of life expectancy compare for the years 1952 and 2007?**

```r
gapminder %>% 
  filter(year==1952 | year==2007) %>%
  mutate(year=as_factor(year)) %>% 
  ggplot(aes(x = lifeExp, fill=year)) +
  geom_density(alpha=0.5)+
  labs(title = "Distribution of Life Expectancy in 1952 & 2007",
       x="Life Expectancy")
```

![](lab11_hw_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

**4. Your answer above doesn't tell the whole story since life expectancy varies by region. Make a summary that shows the min, mean, and max life expectancy by continent for all years represented in the data.**

```r
gapminder %>% 
  group_by(continent) %>% 
  summarise(min_lx=min(lifeExp),
            max_lx=max(lifeExp),
            mean_lx=mean(lifeExp))
```

```
## # A tibble: 5 ?? 4
##   continent min_lx max_lx mean_lx
##   <fct>      <dbl>  <dbl>   <dbl>
## 1 Africa      23.6   76.4    48.9
## 2 Americas    37.6   80.7    64.7
## 3 Asia        28.8   82.6    60.1
## 4 Europe      43.6   81.8    71.9
## 5 Oceania     69.1   81.2    74.3
```

**5. How has life expectancy changed between 1952-2007 for each continent?**

```r
gapminder %>% 
  group_by(year, continent) %>% 
  summarize(mean=mean(lifeExp)) %>% 
  ggplot(aes(x=year, y=mean, group=continent, color=continent))+
  geom_line()
```

```
## `summarise()` has grouped output by 'year'. You can override using the
## `.groups` argument.
```

![](lab11_hw_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

**6. We are interested in the relationship between per capita GDP and life expectancy; i.e. does having more money help you live longer?**

```r
gapminder %>% 
  ggplot(aes(x=gdpPercap, y=lifeExp))+
  geom_point(alpha=0.3, color="coral", position = "jitter")+
  labs(title = "GDP vs. Life Expectancy", 
       x="GDP",
       y="Life Expectancy")
```

![](lab11_hw_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

**7. Which countries have had the largest population growth since 1952?**

```r
gapminder %>% 
  select(country, year, pop) %>% 
  filter(year==1952 | year==2007) %>% 
  pivot_wider(names_from = year,
              values_from = pop)
```

```
## # A tibble: 142 ?? 3
##    country       `1952`    `2007`
##    <fct>          <int>     <int>
##  1 Afghanistan  8425333  31889923
##  2 Albania      1282697   3600523
##  3 Algeria      9279525  33333216
##  4 Angola       4232095  12420476
##  5 Argentina   17876956  40301927
##  6 Australia    8691212  20434176
##  7 Austria      6927772   8199783
##  8 Bahrain       120447    708573
##  9 Bangladesh  46886859 150448339
## 10 Belgium      8730405  10392226
## # ??? with 132 more rows
```


```r
gapminder %>% 
  select(country, year, pop) %>% 
  filter(year==1952 | year==2007) %>% 
  pivot_wider(names_from = year,
              values_from = pop) %>% 
  mutate(delta= `2007`-`1952`) %>% 
  arrange(desc(delta)) # China has the largest population
```

```
## # A tibble: 142 ?? 4
##    country          `1952`     `2007`     delta
##    <fct>             <int>      <int>     <int>
##  1 China         556263527 1318683096 762419569
##  2 India         372000000 1110396331 738396331
##  3 United States 157553000  301139947 143586947
##  4 Indonesia      82052000  223547000 141495000
##  5 Brazil         56602560  190010647 133408087
##  6 Pakistan       41346560  169270617 127924057
##  7 Bangladesh     46886859  150448339 103561480
##  8 Nigeria        33119096  135031164 101912068
##  9 Mexico         30144317  108700891  78556574
## 10 Philippines    22438691   91077287  68638596
## # ??? with 132 more rows
```

**8. Use your results from the question above to plot population growth for the top five countries since 1952.**

```r
gapminder %>% 
  select(country, year, pop) %>% 
  filter(year==1952 | year==2007) %>% 
  pivot_wider(names_from = year,
              values_from = pop) %>% 
  mutate(delta= `2007`-`1952`) %>% 
  arrange(desc(delta)) %>% 
  top_n(n=5)
```

```
## Selecting by delta
```

```
## # A tibble: 5 ?? 4
##   country          `1952`     `2007`     delta
##   <fct>             <int>      <int>     <int>
## 1 China         556263527 1318683096 762419569
## 2 India         372000000 1110396331 738396331
## 3 United States 157553000  301139947 143586947
## 4 Indonesia      82052000  223547000 141495000
## 5 Brazil         56602560  190010647 133408087
```

```r
gapminder %>% 
  select(country, year, pop) %>% 
  filter(year>=1952, country==c("China", "India", "United States", "Indonesia", "Brazil")) %>% 
  mutate(year=as_factor(year)) %>% 
  group_by(year, pop, country) %>% 
  summarise(n=n(), .groups='keep') %>% 
  ggplot(aes(x=year, y=pop, group=country, color=country))+
  geom_line()+
  geom_point(shape=1)+
  theme(axis.text.x = element_text(angle = 60, hjust = 1))+
  labs(title = "Top 5 Population Growth Since 1952",
       x = NULL,
       y="Population")
```

```
## Warning in `==.default`(country, c("China", "India", "United States",
## "Indonesia", : longer object length is not a multiple of shorter object length
```

```
## Warning in is.na(e1) | is.na(e2): longer object length is not a multiple of
## shorter object length
```

![](lab11_hw_files/figure-html/unnamed-chunk-18-1.png)<!-- -->

**9. How does per-capita GDP growth compare between these same five countries?**

```r
gapminder %>% 
  select(country, year, gdpPercap) %>% 
  filter(year>=1952, country==c("China", "India", "United States", "Indonesia", "Brazil")) %>% 
  mutate(year=as_factor(year)) %>% 
  group_by(year, gdpPercap, country) %>% 
  summarise(n=n(), .groups='keep') %>% 
  ggplot(aes(x=year, y=gdpPercap, group=country, color=country))+
  geom_line()+
  geom_point(shape=5)+
  theme(axis.text.x = element_text(angle = 60, hjust = 1))+
  labs(title = "GDP Growth since 1952 in Top 5 populated 
       Countries",
       x = NULL,
       y="GDP")
```

```
## Warning in `==.default`(country, c("China", "India", "United States",
## "Indonesia", : longer object length is not a multiple of shorter object length
```

```
## Warning in is.na(e1) | is.na(e2): longer object length is not a multiple of
## shorter object length
```

![](lab11_hw_files/figure-html/unnamed-chunk-19-1.png)<!-- -->

**10. Make one plot of your choice that uses faceting!**


```r
gm <- gapminder %>%
  filter(year>=1952) %>%
  mutate(year=as_factor(year)) %>% 
  ggplot(aes(x=year, y=gdpPercap, fill=continent))+
  geom_col(position="dodge")+
  labs(title = "GDP Growth in each Continent since 1952", 
       x="Year",
       y="GDP")
```

```r
gm+theme_classic()+
  theme(axis.text.x = element_text(angle = 60, hjust=1))
```

![](lab11_hw_files/figure-html/unnamed-chunk-21-1.png)<!-- -->

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
