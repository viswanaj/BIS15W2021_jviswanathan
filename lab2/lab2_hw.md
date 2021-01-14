---
title: "Lab 2 Homework"
author: "Jayashri Viswanathan"
date: "2021-01-14"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

1. What is a vector in R?  

```r
#A vector is a mode of data storage in R, which displays like am ordered list of numbers. 
```

2. What is a data matrix in R?  

```r
# A data matrix is also a type of data structure in R, organized into a defined number of rows and columns. 
```


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
spring_data <- c(spring_1, spring_2, spring_3, spring_4, spring_5, spring_6, spring_7, spring_8)
spring_data_matrix <- matrix(spring_data, nrow = 8, byrow = T)
```

5. The names of the springs are 1.Bluebell Spring, 2.Opal Spring, 3.Riverside Spring, 4.Too Hot Spring, 5.Mystery Spring, 6.Emerald Spring, 7.Black Spring, 8.Pearl Spring. Name the rows and columns in the data matrix. Start by making two new vectors with the names, then use `colnames()` and `rownames()` to name the columns and rows.

```r
scientists <- c("Jill", "Steve", "Susan")
springs <- c("bluebell spring", "opal spring", "riverside spring", "too hot spring", "mystery spring", "emerald spring", "black spring", "pearl spring")
rownames(spring_data_matrix) <- springs
colnames(spring_data_matrix) <- scientists
spring_data_matrix
```

```
##                   Jill Steve Susan
## bluebell spring  36.25 35.40 35.30
## opal spring      35.15 35.35 33.35
## riverside spring 30.70 29.65 29.20
## too hot spring   39.70 40.05 38.65
## mystery spring   31.85 31.40 29.30
## emerald spring   30.20 30.65 29.75
## black spring     32.90 32.50 32.80
## pearl spring     36.80 36.45 33.15
```


6. Calculate the mean temperature of all three springs.

```r
mean_temp <- rowMeans(spring_data_matrix)
mean_temp
```

```
##  bluebell spring      opal spring riverside spring   too hot spring 
##         35.65000         34.61667         29.85000         39.46667 
##   mystery spring   emerald spring     black spring     pearl spring 
##         30.85000         30.20000         32.73333         35.46667
```


7. Add this as a new column in the data matrix.  

```r
all_spring_data_matrix <- cbind(spring_data_matrix, mean_temp)
all_spring_data_matrix
```

```
##                   Jill Steve Susan mean_temp
## bluebell spring  36.25 35.40 35.30  35.65000
## opal spring      35.15 35.35 33.35  34.61667
## riverside spring 30.70 29.65 29.20  29.85000
## too hot spring   39.70 40.05 38.65  39.46667
## mystery spring   31.85 31.40 29.30  30.85000
## emerald spring   30.20 30.65 29.75  30.20000
## black spring     32.90 32.50 32.80  32.73333
## pearl spring     36.80 36.45 33.15  35.46667
```


8. Show Susan's value for Opal Spring only.

```r
susan_opal_spring <- all_spring_data_matrix[2,3]
susan_opal_spring
```

```
## [1] 33.35
```

9. Calculate the mean for Jill's column only.  

```r
jill <- all_spring_data_matrix[ ,1]
mean_jill <- mean(jill)
mean_jill
```

```
## [1] 34.19375
```

10. Use the data matrix to perform one calculation or operation of your interest.


```r
#standard deviation of the measured temps at bluebell springs. 
bluebell_sd <- sd(spring_data_matrix[1, ])
bluebell_sd
```

```
## [1] 0.5220153
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.  
