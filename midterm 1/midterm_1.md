---
title: "Midterm 1"
author: "Jayashri Viswanathan"
date: "2021-01-29"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code should be organized, clean, and run free from errors. Be sure to **add your name** to the author header above. You may use any resources to answer these questions (including each other), but you may not post questions to Open Stacks or external help sites. There are 12 total questions.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

This exam is due by **12:00p on Thursday, January 28**.  

## Load the tidyverse
If you plan to use any other libraries to complete this assignment then you should load them here.

```r
library(tidyverse)
```

## Questions
**1. (2 points) Briefly explain how R, RStudio, and GitHub work together to make work flows in data science transparent and repeatable. What is the advantage of using RMarkdown in this context?**  

```r
#R is the language we are using for this application in data science. RStudio is a more user-friendly 'GUI' for someone to analyze data sets. RMarkdown is a file format that makes the code easier to read and understand because R Markdown files can be viewed on GitHub without requiring the viewer to download the file. Also, R Markdown files can also be easily converted into a pdf or word document, which makes the code easier to share with others even if they don't have R. 
```


**2. (2 points) What are the three types of `data structures` that we have discussed? Why are we using data frames for BIS 15L?**

```r
#Data frames, vectors, and matrices. We are using data frames because they are easier and more intuitive to work with using the packages we use in this class. 
```


In the midterm 1 folder there is a second folder called `data`. Inside the `data` folder, there is a .csv file called `ElephantsMF`. These data are from Phyllis Lee, Stirling University, and are related to Lee, P., et al. (2013), "Enduring consequences of early experiences: 40-year effects on survival and success among African elephants (Loxodonta africana)," Biology Letters, 9: 20130011. [kaggle](https://www.kaggle.com/mostafaelseidy/elephantsmf).  

**3. (2 points) Please load these data as a new object called `elephants`. Use the function(s) of your choice to get an idea of the structure of the data. Be sure to show the class of each variable.**


```r
setwd("~/Documents/GitHub/BIS15W2021_jviswanathan/midterm 1/data")
elephants <- readr::read_csv("ElephantsMF.csv")
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   Age = col_double(),
##   Height = col_double(),
##   Sex = col_character()
## )
```

```r
names(elephants)
```

```
## [1] "Age"    "Height" "Sex"
```


**4. (2 points) Change the names of the variables to lower case and change the class of the variable `sex` to a factor.**


```r
library("janitor")
```

```
## 
## Attaching package: 'janitor'
```

```
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

```r
library("tidyverse")
library("skimr")
library("dplyr")

elephants <- elephants %>%
  clean_names() %>%
  mutate(sex = as.factor(sex))
```


**5. (2 points) How many male and female elephants are represented in the data?**


```r
elephants %>%
  group_by(sex) %>%
  summarise(n=n())
```

```
## # A tibble: 2 x 2
##   sex       n
## * <fct> <int>
## 1 F       150
## 2 M       138
```

```r
# 138 males and 150 females.
```


**6. (2 points) What is the average age all elephants in the data?**


```r
elephants %>%
  summarise(average_age = mean(age))
```

```
## # A tibble: 1 x 1
##   average_age
##         <dbl>
## 1        11.0
```

```r
#The average age is 10.97132 years. 
```


**7. (2 points) How does the average age and height of elephants compare by sex?**


```r
elephants %>%
  group_by(sex) %>%
  summarise(average_age = mean(age),
            average_height = mean(height), n=n())
```

```
## # A tibble: 2 x 4
##   sex   average_age average_height     n
## * <fct>       <dbl>          <dbl> <int>
## 1 F           12.8            190.   150
## 2 M            8.95           185.   138
```

```r
# females on average are older and taller. 
```


**8. (2 points) How does the average height of elephants compare by sex for individuals over 25 years old. Include the min and max height as well as the number of individuals in the sample as part of your analysis.**


```r
elephants %>%
  group_by(sex) %>%
  filter(age > 25) %>%
  summarise(average_height = mean(height),
            minimum_height = min(height),
            maximum_height = max(height), 
            n=n())
```

```
## # A tibble: 2 x 5
##   sex   average_height minimum_height maximum_height     n
##   <fct>          <dbl>          <dbl>          <dbl> <int>
## 1 F               233.           206.           278.    25
## 2 M               273.           237.           304.     8
```

```r
#for elephants over 25 year of age, males are taller on average. 
```


For the next series of questions, we will use data from a study on vertebrate community composition and impacts from defaunation in [Gabon, Africa](https://en.wikipedia.org/wiki/Gabon). One thing to notice is that the data include 24 separate transects. Each transect represents a path through different forest management areas.  

Reference: Koerner SE, Poulsen JR, Blanchard EJ, Okouyi J, Clark CJ. Vertebrate community composition and diversity declines along a defaunation gradient radiating from rural villages in Gabon. _Journal of Applied Ecology_. 2016. This paper, along with a description of the variables is included inside the midterm 1 folder.  

**9. (2 points) Load `IvindoData_DryadVersion.csv` and use the function(s) of your choice to get an idea of the overall structure. Change the variables `HuntCat` and `LandUse` to factors.**


```r
setwd("~/Documents/GitHub/BIS15W2021_jviswanathan/midterm 1/data")
ivindo <- readr::read_csv("IvindoData_DryadVersion.csv")
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   .default = col_double(),
##   HuntCat = col_character(),
##   LandUse = col_character()
## )
## ℹ Use `spec()` for the full column specifications.
```

```r
#Structure
glimpse(ivindo)
```

```
## Rows: 24
## Columns: 26
## $ TransectID              <dbl> 1, 2, 2, 3, 4, 5, 6, 7, 8, 9, 13, 14, 15, 16,…
## $ Distance                <dbl> 7.14, 17.31, 18.32, 20.85, 15.95, 17.47, 24.0…
## $ HuntCat                 <chr> "Moderate", "None", "None", "None", "None", "…
## $ NumHouseholds           <dbl> 54, 54, 29, 29, 29, 29, 29, 54, 25, 73, 46, 5…
## $ LandUse                 <chr> "Park", "Park", "Park", "Logging", "Park", "P…
## $ Veg_Rich                <dbl> 16.67, 15.75, 16.88, 12.44, 17.13, 16.50, 14.…
## $ Veg_Stems               <dbl> 31.20, 37.44, 32.33, 29.39, 36.00, 29.22, 31.…
## $ Veg_liana               <dbl> 5.78, 13.25, 4.75, 9.78, 13.25, 12.88, 8.38, …
## $ Veg_DBH                 <dbl> 49.57, 34.59, 42.82, 36.62, 41.52, 44.07, 51.…
## $ Veg_Canopy              <dbl> 3.78, 3.75, 3.43, 3.75, 3.88, 2.50, 4.00, 4.0…
## $ Veg_Understory          <dbl> 2.89, 3.88, 3.00, 2.75, 3.25, 3.00, 2.38, 2.7…
## $ RA_Apes                 <dbl> 1.87, 0.00, 4.49, 12.93, 0.00, 2.48, 3.78, 6.…
## $ RA_Birds                <dbl> 52.66, 52.17, 37.44, 59.29, 52.62, 38.64, 42.…
## $ RA_Elephant             <dbl> 0.00, 0.86, 1.33, 0.56, 1.00, 0.00, 1.11, 0.4…
## $ RA_Monkeys              <dbl> 38.59, 28.53, 41.82, 19.85, 41.34, 43.29, 46.…
## $ RA_Rodent               <dbl> 4.22, 6.04, 1.06, 3.66, 2.52, 1.83, 3.10, 1.2…
## $ RA_Ungulate             <dbl> 2.66, 12.41, 13.86, 3.71, 2.53, 13.75, 3.10, …
## $ Rich_AllSpecies         <dbl> 22, 20, 22, 19, 20, 22, 23, 19, 19, 19, 21, 2…
## $ Evenness_AllSpecies     <dbl> 0.793, 0.773, 0.740, 0.681, 0.811, 0.786, 0.8…
## $ Diversity_AllSpecies    <dbl> 2.452, 2.314, 2.288, 2.006, 2.431, 2.429, 2.5…
## $ Rich_BirdSpecies        <dbl> 11, 10, 11, 8, 8, 10, 11, 11, 11, 9, 11, 11, …
## $ Evenness_BirdSpecies    <dbl> 0.732, 0.704, 0.688, 0.559, 0.799, 0.771, 0.8…
## $ Diversity_BirdSpecies   <dbl> 1.756, 1.620, 1.649, 1.162, 1.660, 1.775, 1.9…
## $ Rich_MammalSpecies      <dbl> 11, 10, 11, 11, 12, 12, 12, 8, 8, 10, 10, 11,…
## $ Evenness_MammalSpecies  <dbl> 0.736, 0.705, 0.650, 0.619, 0.736, 0.694, 0.7…
## $ Diversity_MammalSpecies <dbl> 1.764, 1.624, 1.558, 1.484, 1.829, 1.725, 1.9…
```

```r
names(ivindo)
```

```
##  [1] "TransectID"              "Distance"               
##  [3] "HuntCat"                 "NumHouseholds"          
##  [5] "LandUse"                 "Veg_Rich"               
##  [7] "Veg_Stems"               "Veg_liana"              
##  [9] "Veg_DBH"                 "Veg_Canopy"             
## [11] "Veg_Understory"          "RA_Apes"                
## [13] "RA_Birds"                "RA_Elephant"            
## [15] "RA_Monkeys"              "RA_Rodent"              
## [17] "RA_Ungulate"             "Rich_AllSpecies"        
## [19] "Evenness_AllSpecies"     "Diversity_AllSpecies"   
## [21] "Rich_BirdSpecies"        "Evenness_BirdSpecies"   
## [23] "Diversity_BirdSpecies"   "Rich_MammalSpecies"     
## [25] "Evenness_MammalSpecies"  "Diversity_MammalSpecies"
```

```r
#Change variables to factors.
ivindo <- ivindo %>%
  clean_names() %>%
  mutate(hunt_cat = as.factor(hunt_cat)) %>%
  mutate(land_use = as.factor(land_use))
```


**10. (4 points) For the transects with high and moderate hunting intensity, how does the average diversity of birds and mammals compare?**


```r
#Cleaning up the variable names before answering the question. 
ivindo <- ivindo %>%
  clean_names()

#Answering the question.
ivindo %>%
  filter(hunt_cat == "High" |  hunt_cat == "Moderate") %>%
  summarise(average_diversity_birds = mean(diversity_bird_species), 
            average_diversity_mammals = mean(diversity_mammal_species), 
            n=n())
```

```
## # A tibble: 1 x 3
##   average_diversity_birds average_diversity_mammals     n
##                     <dbl>                     <dbl> <int>
## 1                    1.64                      1.71    15
```

```r
#Of the animals with high and moderate huting intensity, mammals have a higher diversity value than birds. 
```


**11. (4 points) One of the conclusions in the study is that the relative abundance of animals drops off the closer you get to a village. Let's try to reconstruct this (without the statistics). How does the relative abundance (RA) of apes, birds, elephants, monkeys, rodents, and ungulates compare between sites that are less than 5km from a village to sites that are greater than 20km from a village? The variable `Distance` measures the distance of the transect from the nearest village. Hint: try using the `across` operator.**  


```r
short_sites <- ivindo %>%
  filter(distance < 5) %>%
  summarize(across(contains("ra"), mean, na.rm=T))


long_sites <- ivindo %>%
  filter(distance > 20) %>%
  summarize(across(contains("ra"), mean, na.rm=T))

short_sites
```

```
## # A tibble: 1 x 7
##   transect_id ra_apes ra_birds ra_elephant ra_monkeys ra_rodent ra_ungulate
##         <dbl>   <dbl>    <dbl>       <dbl>      <dbl>     <dbl>       <dbl>
## 1        19.7    0.08     70.4      0.0967       24.1      3.66        1.59
```

```r
long_sites
```

```
## # A tibble: 1 x 7
##   transect_id ra_apes ra_birds ra_elephant ra_monkeys ra_rodent ra_ungulate
##         <dbl>   <dbl>    <dbl>       <dbl>      <dbl>     <dbl>       <dbl>
## 1          11    7.21     44.5       0.557       40.1      2.68        4.98
```

**12. (4 points) Based on your interest, do one exploratory analysis on the `gabon` data of your choice. This analysis needs to include a minimum of two functions in `dplyr.`**

```r
#exploring the relationship between species richness and vegetation  richness.  
ivindo %>%
  arrange(desc(num_households)) %>%
  group_by(rich_all_species) %>%
  summarise(average_veg_rich = mean(veg_rich))
```

```
## # A tibble: 8 x 2
##   rich_all_species average_veg_rich
## *            <dbl>            <dbl>
## 1               15             14.9
## 2               16             13.2
## 3               19             13.4
## 4               20             14.8
## 5               21             15.6
## 6               22             16.3
## 7               23             14.8
## 8               24             16.8
```





