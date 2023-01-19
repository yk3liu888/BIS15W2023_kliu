---
title: "Lab 2 Homework"
author: "Kellie Liu"
date: "2023-01-17"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

1. What is a vector in R?  

Vector is a organization of variables. It can be integer, numeric, complex, character, or logical.

2. What is a data matrix in R? 

Data matrix is a series of vectors.

3. Below are data collected by three scientists (Jill, Steve, Susan in order) measuring temperatures of eight hot springs. Run this code chunk to create the vectors.  

```r
spring_1 <- c(36.25, 35.40, 35.30)
spring_2 <- c(35.15, 35.35, 33.35)
spring_3 <- c(30.70, 29.65, 29.20)
spring_4 <- c(39.70, 40.05, 38.65)
spring_5 <- c(31.85, 31.40, 29.30)
spring_6 <- c(30.20, 30.65, 29.75)
spring_7 <- c(32.90, 32.50, 32.80)
spring_8 <- c(36.80, 36.45, 33.15)
```

4. Build a data matrix that has the springs as rows and the columns as scientists.  

```r
hot_springs <- c(spring_1, spring_2, spring_3, spring_4, spring_5, spring_6, spring_7, spring_8)
hot_springs
```

```
##  [1] 36.25 35.40 35.30 35.15 35.35 33.35 30.70 29.65 29.20 39.70 40.05 38.65
## [13] 31.85 31.40 29.30 30.20 30.65 29.75 32.90 32.50 32.80 36.80 36.45 33.15
```

```r
hot_springs_matrix <- matrix(hot_springs, nrow = 8, byrow = T)
hot_springs_matrix
```

```
##       [,1]  [,2]  [,3]
## [1,] 36.25 35.40 35.30
## [2,] 35.15 35.35 33.35
## [3,] 30.70 29.65 29.20
## [4,] 39.70 40.05 38.65
## [5,] 31.85 31.40 29.30
## [6,] 30.20 30.65 29.75
## [7,] 32.90 32.50 32.80
## [8,] 36.80 36.45 33.15
```

```r
scientist <- c("Jill", "Steve", "Susan")
scientist
```

```
## [1] "Jill"  "Steve" "Susan"
```

```r
titles <- c("spring_1", "spring_2", "spring_3", "spring_4", "spring_5", "spring_6", "spring_7", "spring_8")
titles
```

```
## [1] "spring_1" "spring_2" "spring_3" "spring_4" "spring_5" "spring_6" "spring_7"
## [8] "spring_8"
```

```r
colnames(hot_springs_matrix) <- scientist
```

```r
rownames(hot_springs_matrix) <- titles
```

```r
hot_springs_matrix
```

```
##           Jill Steve Susan
## spring_1 36.25 35.40 35.30
## spring_2 35.15 35.35 33.35
## spring_3 30.70 29.65 29.20
## spring_4 39.70 40.05 38.65
## spring_5 31.85 31.40 29.30
## spring_6 30.20 30.65 29.75
## spring_7 32.90 32.50 32.80
## spring_8 36.80 36.45 33.15
```
region

5. The names of the springs are 1.Bluebell Spring, 2.Opal Spring, 3.Riverside Spring, 4.Too Hot Spring, 5.Mystery Spring, 6.Emerald Spring, 7.Black Spring, 8.Pearl Spring. Name the rows and columns in the data matrix. Start by making two new vectors with the names, then use `colnames()` and `rownames()` to name the columns and rows.


```r
Bluebell_Spring <- c(36.25, 35.40, 35.30)
Opal_Spring <- c(35.15, 35.35, 33.35)
Riverside_Spring <- c(30.70, 29.65, 29.20)
Too_Hot_Spring <- c(39.70, 40.05, 38.65)
Mystery_Spring <- c(31.85, 31.40, 29.30)
Emerald_Spring <- c(30.20, 30.65, 29.75)
Black_Spring <- c(32.90, 32.50, 32.80)
Pearl_Spring <- c(36.80, 36.45, 33.15)
```

```r
hot_springs <- c(Bluebell_Spring, Opal_Spring, Riverside_Spring, Too_Hot_Spring, Mystery_Spring, Emerald_Spring, Black_Spring, Pearl_Spring)
hot_springs
```

```
##  [1] 36.25 35.40 35.30 35.15 35.35 33.35 30.70 29.65 29.20 39.70 40.05 38.65
## [13] 31.85 31.40 29.30 30.20 30.65 29.75 32.90 32.50 32.80 36.80 36.45 33.15
```

```r
springs_matrix <- matrix(hot_springs, nrow = 8, byrow = T)
springs_matrix
```

```
##       [,1]  [,2]  [,3]
## [1,] 36.25 35.40 35.30
## [2,] 35.15 35.35 33.35
## [3,] 30.70 29.65 29.20
## [4,] 39.70 40.05 38.65
## [5,] 31.85 31.40 29.30
## [6,] 30.20 30.65 29.75
## [7,] 32.90 32.50 32.80
## [8,] 36.80 36.45 33.15
```


```r
scientist <- c("Jill", "Steve", "Susan")
scientist
```

```
## [1] "Jill"  "Steve" "Susan"
```

```r
springs <- c("Bluebell_Spring", "Opal_Spring", "Riverside_Spring", "Too_Hot_Spring", "Mystery_Spring", "Emerald_Spring", "Black_Spring", "Pearl_Spring")
springs
```

```
## [1] "Bluebell_Spring"  "Opal_Spring"      "Riverside_Spring" "Too_Hot_Spring"  
## [5] "Mystery_Spring"   "Emerald_Spring"   "Black_Spring"     "Pearl_Spring"
```

```r
colnames(springs_matrix) <- scientist
```

```r
rownames(springs_matrix) <- springs
```


```r
springs_matrix
```

```
##                   Jill Steve Susan
## Bluebell_Spring  36.25 35.40 35.30
## Opal_Spring      35.15 35.35 33.35
## Riverside_Spring 30.70 29.65 29.20
## Too_Hot_Spring   39.70 40.05 38.65
## Mystery_Spring   31.85 31.40 29.30
## Emerald_Spring   30.20 30.65 29.75
## Black_Spring     32.90 32.50 32.80
## Pearl_Spring     36.80 36.45 33.15
```

6. Calculate the mean temperature of all eight springs.

```r
springs_means <- rowMeans(springs_matrix)
springs_means
```

```
##  Bluebell_Spring      Opal_Spring Riverside_Spring   Too_Hot_Spring 
##         35.65000         34.61667         29.85000         39.46667 
##   Mystery_Spring   Emerald_Spring     Black_Spring     Pearl_Spring 
##         30.85000         30.20000         32.73333         35.46667
```
7. Add this as a new column in the data matrix.  

```r
all_springs_matrix <- cbind(springs_matrix, springs_means)
all_springs_matrix
```

```
##                   Jill Steve Susan springs_means
## Bluebell_Spring  36.25 35.40 35.30      35.65000
## Opal_Spring      35.15 35.35 33.35      34.61667
## Riverside_Spring 30.70 29.65 29.20      29.85000
## Too_Hot_Spring   39.70 40.05 38.65      39.46667
## Mystery_Spring   31.85 31.40 29.30      30.85000
## Emerald_Spring   30.20 30.65 29.75      30.20000
## Black_Spring     32.90 32.50 32.80      32.73333
## Pearl_Spring     36.80 36.45 33.15      35.46667
```
8. Show Susan's value for Opal Spring only.

```r
springs_matrix[2,3]
```

```
## [1] 33.35
```
9. Calculate the mean for Jill's column only.  

```r
Jill_mean <- all_springs_matrix[ ,1]
mean(Jill_mean)
```

```
## [1] 34.19375
```

10. Use the data matrix to perform one calculation or operation of your interest.

Calculate the mean for Steve's and Susan's measurements for Mystery Springs.

```r
Steve_MT <- all_springs_matrix[5,2]
Steve_MT
```

```
## [1] 31.4
```

```r
Susan_MT <- all_springs_matrix[5,3]
Susan_MT
```

```
## [1] 29.3
```

```r
mean(Steve_MT, Susan_MT)
```

```
## [1] 31.4
```


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.  
