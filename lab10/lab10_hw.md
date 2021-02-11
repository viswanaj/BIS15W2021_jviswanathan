---
title: "Lab 10 Homework"
author: "Jayashri Viswanathan"
date: "2021-02-11"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(here)
library(naniar)
```

## Desert Ecology
For this assignment, we are going to use a modified data set on [desert ecology](http://esapubs.org/archive/ecol/E090/118/). The data are from: S. K. Morgan Ernest, Thomas J. Valone, and James H. Brown. 2009. Long-term monitoring and experimental manipulation of a Chihuahuan Desert ecosystem near Portal, Arizona, USA. Ecology 90:1708.

```r
deserts <- read_csv(here("lab10", "data", "surveys_complete.csv"))
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   record_id = col_double(),
##   month = col_double(),
##   day = col_double(),
##   year = col_double(),
##   plot_id = col_double(),
##   species_id = col_character(),
##   sex = col_character(),
##   hindfoot_length = col_double(),
##   weight = col_double(),
##   genus = col_character(),
##   species = col_character(),
##   taxa = col_character(),
##   plot_type = col_character()
## )
```

1. Use the function(s) of your choice to get an idea of its structure, including how NA's are treated. Are the data tidy?  


```r
summary(deserts)
```

```
##    record_id         month             day            year         plot_id     
##  Min.   :    1   Min.   : 1.000   Min.   : 1.0   Min.   :1977   Min.   : 1.00  
##  1st Qu.: 8964   1st Qu.: 4.000   1st Qu.: 9.0   1st Qu.:1984   1st Qu.: 5.00  
##  Median :17762   Median : 6.000   Median :16.0   Median :1990   Median :11.00  
##  Mean   :17804   Mean   : 6.474   Mean   :16.1   Mean   :1990   Mean   :11.34  
##  3rd Qu.:26655   3rd Qu.:10.000   3rd Qu.:23.0   3rd Qu.:1997   3rd Qu.:17.00  
##  Max.   :35548   Max.   :12.000   Max.   :31.0   Max.   :2002   Max.   :24.00  
##                                                                                
##   species_id            sex            hindfoot_length     weight      
##  Length:34786       Length:34786       Min.   : 2.00   Min.   :  4.00  
##  Class :character   Class :character   1st Qu.:21.00   1st Qu.: 20.00  
##  Mode  :character   Mode  :character   Median :32.00   Median : 37.00  
##                                        Mean   :29.29   Mean   : 42.67  
##                                        3rd Qu.:36.00   3rd Qu.: 48.00  
##                                        Max.   :70.00   Max.   :280.00  
##                                        NA's   :3348    NA's   :2503    
##     genus             species              taxa            plot_type        
##  Length:34786       Length:34786       Length:34786       Length:34786      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
## 
```

```r
names(deserts)
```

```
##  [1] "record_id"       "month"           "day"             "year"           
##  [5] "plot_id"         "species_id"      "sex"             "hindfoot_length"
##  [9] "weight"          "genus"           "species"         "taxa"           
## [13] "plot_type"
```

```r
naniar :: miss_var_summary(deserts)
```

```
## # A tibble: 13 x 3
##    variable        n_miss pct_miss
##    <chr>            <int>    <dbl>
##  1 hindfoot_length   3348     9.62
##  2 weight            2503     7.20
##  3 sex               1748     5.03
##  4 record_id            0     0   
##  5 month                0     0   
##  6 day                  0     0   
##  7 year                 0     0   
##  8 plot_id              0     0   
##  9 species_id           0     0   
## 10 genus                0     0   
## 11 species              0     0   
## 12 taxa                 0     0   
## 13 plot_type            0     0
```


2. How many genera and species are represented in the data? What are the total number of observations? Which species is most/ least frequently sampled in the study?

```r
deserts %>%
  summarise(number_genera = n_distinct(genus),
            number_species = n_distinct(species),
            n=n()) 
```

```
## # A tibble: 1 x 3
##   number_genera number_species     n
##           <int>          <int> <int>
## 1            26             40 34786
```

```r
#there are 26 distinct genera and 40 distinct species. there are 34786 observations.

deserts %>%
  select(genus, species) %>%
  group_by(genus, species) %>%
  summarise(n=n()) %>%
  arrange(desc(n))
```

```
## `summarise()` has grouped output by 'genus'. You can override using the `.groups` argument.
```

```
## # A tibble: 48 x 3
## # Groups:   genus [26]
##    genus           species          n
##    <chr>           <chr>        <int>
##  1 Dipodomys       merriami     10596
##  2 Chaetodipus     penicillatus  3123
##  3 Dipodomys       ordii         3027
##  4 Chaetodipus     baileyi       2891
##  5 Reithrodontomys megalotis     2609
##  6 Dipodomys       spectabilis   2504
##  7 Onychomys       torridus      2249
##  8 Perognathus     flavus        1597
##  9 Peromyscus      eremicus      1299
## 10 Neotoma         albigula      1252
## # … with 38 more rows
```

```r
#Dipodomys merriami is the most frequently samples species in this study. 
```

3. What is the proportion of taxa included in this study? Show a table and plot that reflects this count.

```r
#this is the table.
deserts_proportion <- deserts %>%
  count(taxa) %>%
  mutate(total = sum(n)) %>%
  mutate(proportion_taxa = n / total) %>%
  arrange(desc(proportion_taxa))

deserts_proportion
```

```
## # A tibble: 4 x 4
##   taxa        n total proportion_taxa
##   <chr>   <int> <int>           <dbl>
## 1 Rodent  34247 34786        0.985   
## 2 Bird      450 34786        0.0129  
## 3 Rabbit     75 34786        0.00216 
## 4 Reptile    14 34786        0.000402
```

```r
#this is the plot. 
deserts_proportion %>%
  ggplot(aes(x = taxa, y = proportion_taxa))+
  geom_col() +
  labs(title = "Proportion of each Taxon in Deserts Data",
       x = "Taxon",
       y = "Proportion") +
  theme(plot.title = element_text(size = rel(1.5), hjust = 0.5))
```

![](lab10_hw_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

4. For the taxa included in the study, use the fill option to show the proportion of individuals sampled by `plot_type.`


```r
deserts %>%
  ggplot(aes(x = taxa, fill = plot_type))+
  geom_bar(position = "dodge") +
  labs(title = "Proportion of Individuals Sampled by Plot Type",
       x = "Taxon",
       y = "# of Observations") +
  theme(plot.title = element_text(size = rel(1.5), hjust = (0.5)))
```

![](lab10_hw_files/figure-html/unnamed-chunk-6-1.png)<!-- -->


5. What is the range of weight for each species included in the study? Remove any observations of weight that are NA so they do not show up in the plot.


```r
deserts %>%
  filter(weight != "NA") %>%
  ggplot(aes(x = species, y = weight))+
  geom_boxplot() +
  labs(title = "Weight Ranges Organized by Species",
       x = "Species",
       y = "Weight") +
  theme(plot.title = element_text(size = rel(1.5), hjust = 0.5))
```

![](lab10_hw_files/figure-html/unnamed-chunk-7-1.png)<!-- -->


6. Add another layer to your answer from #4 using `geom_point` to get an idea of how many measurements were taken for each species.


```r
deserts %>%
  filter(weight != "NA") %>%
  ggplot(aes(x = species, y = weight))+
  geom_boxplot() +
  geom_point(alpha = 0.5 , color = "blue", position = "jitter") +
  labs(title = "Weight Ranges Organized by Species",
       x = "Species",
       y = "Weight") +
  theme(plot.title = element_text(size = rel(1.5), hjust = 0.5))
```

![](lab10_hw_files/figure-html/unnamed-chunk-8-1.png)<!-- -->


7. [Dipodomys merriami](https://en.wikipedia.org/wiki/Merriam's_kangaroo_rat) is the most frequently sampled animal in the study. How have the number of observations of this species changed over the years included in the study?


```r
DM_year <- deserts %>%
  select(genus, species, year) %>%
  unite("genus_species", genus, species, sep = "_") %>%
  filter(genus_species == "Dipodomys_merriami") %>%
  group_by(genus_species, year) %>%
  summarise(n=n()) %>%
  arrange(year)
```

```
## `summarise()` has grouped output by 'genus_species'. You can override using the `.groups` argument.
```

```r
DM_year %>%
  ggplot(aes(x = year, y = n)) +
  geom_col()
```

![](lab10_hw_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

```r
#over time, the # of observations of DM fluctuates.
```


8. What is the relationship between `weight` and `hindfoot` length? Consider whether or not over plotting is an issue.


```r
deserts %>%
  ggplot(aes(x = weight, y = hindfoot_length))+
  geom_point()
```

```
## Warning: Removed 4048 rows containing missing values (geom_point).
```

![](lab10_hw_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

```r
#there seems to be a positive correlation between weight and hindfoot length.
```


9. Which two species have, on average, the highest weight? Once you have identified them, make a new column that is a ratio of `weight` to `hindfoot_length`. Make a plot that shows the range of this new ratio and fill by sex


```r
deserts %>%
  filter(weight != "NA") %>%
  group_by(species_id) %>%
  summarise(average_weight = mean(weight)) %>%
  arrange(desc(average_weight)) #NL and DS have the highest average weight. 
```

```
## # A tibble: 25 x 2
##    species_id average_weight
##    <chr>               <dbl>
##  1 NL                  159. 
##  2 DS                  120. 
##  3 SS                   93.5
##  4 SH                   73.1
##  5 SF                   58.9
##  6 SO                   55.4
##  7 DO                   48.9
##  8 DM                   43.2
##  9 PB                   31.7
## 10 OL                   31.6
## # … with 15 more rows
```


```r
deserts %>%
  filter(weight != "NA" ,
         hindfoot_length != "NA") %>%
  group_by(species_id) %>%
  mutate(weight_hindfoot_ratio = weight / hindfoot_length)
```

```
## # A tibble: 30,738 x 14
## # Groups:   species_id [24]
##    record_id month   day  year plot_id species_id sex   hindfoot_length weight
##        <dbl> <dbl> <dbl> <dbl>   <dbl> <chr>      <chr>           <dbl>  <dbl>
##  1        63     8    19  1977       3 DM         M                  35     40
##  2        64     8    19  1977       7 DM         M                  37     48
##  3        65     8    19  1977       4 DM         F                  34     29
##  4        66     8    19  1977       4 DM         F                  35     46
##  5        67     8    19  1977       7 DM         M                  35     36
##  6        68     8    19  1977       8 DO         F                  32     52
##  7        69     8    19  1977       2 PF         M                  15      8
##  8        70     8    19  1977       3 OX         F                  21     22
##  9        71     8    19  1977       7 DM         F                  36     35
## 10        74     8    19  1977       8 PF         M                  12      7
## # … with 30,728 more rows, and 5 more variables: genus <chr>, species <chr>,
## #   taxa <chr>, plot_type <chr>, weight_hindfoot_ratio <dbl>
```



```r
deserts %>%
  filter(species_id == "NL" | 
         species_id == "DS") %>%
  filter(weight != "NA" &
         hindfoot_length != "NA") %>%
  group_by(species_id) %>%
  mutate(weight_hindfoot_ratio = weight / hindfoot_length) %>%
  
  ggplot(aes(x = species_id, y = weight_hindfoot_ratio, fill = sex))+
  geom_boxplot() +
  labs(title = "Ratio of Weight to Hindfoot Length of DS and NL",
       x = "Species ID",
       y = "Weight:Hindfoot Length") +
  theme(plot.title = element_text(size = rel(1.5), hjust = 0.5))
```

![](lab10_hw_files/figure-html/unnamed-chunk-13-1.png)<!-- -->


10. Make one plot of your choice! Make sure to include at least two of the aesthetics options you have learned.


```r
deserts %>%
  select(species, weight, sex) %>%
  filter(weight != "NA", sex != "NA") %>%
  ggplot(aes(x = species, y = weight, fill = sex)) + 
  geom_col(position = "dodge") +
  labs(title = "Comparison of Weights Across Species: Male versus Female",
       x = "Species",
       y = "Weight") +
  theme(plot.title = element_text(size = rel(1.5), hjust = 0.5))
```

![](lab10_hw_files/figure-html/unnamed-chunk-14-1.png)<!-- -->


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
