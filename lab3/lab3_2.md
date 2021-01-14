---
title: "Importing Data Frames"
date: "2021-01-14"
output:
  html_document: 
    theme: spacelab
    toc: yes
    toc_float: yes
    keep_md: yes
  pdf_document:
    toc: yes
---

## Breakout Rooms  
Please take 5-8 minutes to check over your answers to HW 2 in your group. If you are stuck, please remember that you can check the key in [Joel's repository](https://github.com/jmledford3115/BIS15LW2021_jledford).  

## Learning Goals
*At the end of this exercise, you will be able to:*    
1. Import .csv files as data frames using `read_csv()`.  
2. Use summary functions to explore the dimensions, structure, and contents of a data frame.  
3. Use the `select()` command of dplyr to sort data frames.  

## Review
At this point, you should have familiarity in RStudio, GitHub, and basic operations in R. You understand how to do arithmetic, assign values to objects, and work with vectors, data matrices, and data frames. If you are confused or need some extra help, please [email me](mailto: jmledford@ucdavis.edu).  

## Load the tidyverse

```r
library("tidyverse")
```

## Data Frames
For the remainder of the course, we will work exclusively with data frames. Recall that data frames store multiple classes of data. Last time, you were shown how to build data frames using the `data.frame()` command. However, scientists often make their data available as supplementary material associated with a publication. This is excellent scientific practice as it insures repeatability by showing exactly how analyses were performed. As data scientists, we capitalize on this by importing data directly into R.  

## Importing Data
R allows us to import a wide variety of data types. The most common type of file is a .csv file which stands for comma separated values. Spreadsheets are often developed in Excel then saved as .csv files for use in R. There are packages that allow you to open excel files and many other formats directly but .csv is the most common.  

An opinionated word about excel. It is fine to use excel for data entry and basic analysis. But, it often adds formatting that makes excel files difficult to work with in any program besides excel. R can read excel files, but I know of no R programmers that routinely use them. Instead they save copies of their excel files as .csv which strips away formatting but makes them easier to use in a variety of programs. We won't work with excel files in BIS 15L, but we will learn to import them.  

To import any file, first make sure that you are in the correct working directory. If you are not in the correct directory, R will not "see" the file.

```r
getwd()
```

```
## [1] "/Users/viswanaj/Documents/GitHub/BIS15W2021_jviswanathan/lab3"
```

## Load the data
Here we open a .csv file. Since we are using the tidyverse, we open the file using `read_csv()`. `readr` is included in the tidyverse set of packages. We specify the package and function with the `::` symbol. This becomes important if you have multiple packages loaded that contain functions with the same name.  

In the previous part of the lab, you exported a `.csv` of hot springs data. Let's try to reload that `.csv`.  

```r
hot_springs <- readr::read_csv("hsprings_data.csv")
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   temp_C = col_double(),
##   scientist = col_character(),
##   spring = col_character(),
##   depth_ft = col_double()
## )
```

Use the `str()` function to get an idea of the data structure of `hot_springs`.  

```r
str(hot_springs)
```

```
## tibble [9 × 4] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ temp_C   : num [1:9] 36.2 35.4 35.3 35.1 35.4 ...
##  $ scientist: chr [1:9] "Jill" "Susan" "Steve" "Jill" ...
##  $ spring   : chr [1:9] "Buckeye" "Buckeye" "Buckeye" "Benton" ...
##  $ depth_ft : num [1:9] 4.15 4.13 4.12 3.21 3.23 3.2 5.67 5.65 5.66
##  - attr(*, "spec")=
##   .. cols(
##   ..   temp_C = col_double(),
##   ..   scientist = col_character(),
##   ..   spring = col_character(),
##   ..   depth_ft = col_double()
##   .. )
```

```r
glimpse(hot_springs)
```

```
## Rows: 9
## Columns: 4
## $ temp_C    <dbl> 36.25, 35.40, 35.30, 35.15, 35.35, 33.35, 30.70, 29.65, 29.…
## $ scientist <chr> "Jill", "Susan", "Steve", "Jill", "Susan", "Steve", "Jill",…
## $ spring    <chr> "Buckeye", "Buckeye", "Buckeye", "Benton", "Benton", "Bento…
## $ depth_ft  <dbl> 4.15, 4.13, 4.12, 3.21, 3.23, 3.20, 5.67, 5.65, 5.66
```

What is the class of the scientist column? Change it to factor and then show the levels of that factor.  

```r
class(hot_springs$scientist)
```

```
## [1] "character"
```

```r
#now I will reassign the scientist column to be a factor - this will be important later on!!
hot_springs$scientist <- as.factor(hot_springs$scientist)
class(hot_springs$scientist)
```

```
## [1] "factor"
```


```r
hot_springs$scientist <- as.factor(hot_springs$scientist)
class(hot_springs$scientist)
```

```
## [1] "factor"
```


```r
levels(hot_springs$scientist)
```

```
## [1] "Jill"  "Steve" "Susan"
```

## Practice
1. In your lab 3 folder there is another folder titled `data`. Inside the `data` folder there is a `.csv` titled `Gaeta_etal_CLC_data.csv`. Open this data and store them as an object called `fish`.  

The data are from: Gaeta J., G. Sass, S. Carpenter. 2012. Biocomplexity at North Temperate Lakes LTER: Coordinated Field Studies: Large Mouth Bass Growth 2006. Environmental Data Initiative.  [link](https://portal.edirepository.org/nis/mapbrowse?scope=knb-lter-ntl&identifier=267)  

```r
setwd("~/Documents/GitHub/BIS15W2021_jviswanathan/lab3/data")
fish <- readr::read_csv("Gaeta_etal_CLC_data.csv")
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   lakeid = col_character(),
##   fish_id = col_double(),
##   annnumber = col_character(),
##   length = col_double(),
##   radii_length_mm = col_double(),
##   scalelength = col_double()
## )
```

```r
names(fish)
```

```
## [1] "lakeid"          "fish_id"         "annnumber"       "length"         
## [5] "radii_length_mm" "scalelength"
```

2. What is the structure of these data?

```r
str(fish)
```

```
## tibble [4,033 × 6] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ lakeid         : chr [1:4033] "AL" "AL" "AL" "AL" ...
##  $ fish_id        : num [1:4033] 299 299 299 300 300 300 300 301 301 301 ...
##  $ annnumber      : chr [1:4033] "EDGE" "2" "1" "EDGE" ...
##  $ length         : num [1:4033] 167 167 167 175 175 175 175 194 194 194 ...
##  $ radii_length_mm: num [1:4033] 2.7 2.04 1.31 3.02 2.67 ...
##  $ scalelength    : num [1:4033] 2.7 2.7 2.7 3.02 3.02 ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   lakeid = col_character(),
##   ..   fish_id = col_double(),
##   ..   annnumber = col_character(),
##   ..   length = col_double(),
##   ..   radii_length_mm = col_double(),
##   ..   scalelength = col_double()
##   .. )
```

```r
glimpse(fish)
```

```
## Rows: 4,033
## Columns: 6
## $ lakeid          <chr> "AL", "AL", "AL", "AL", "AL", "AL", "AL", "AL", "AL",…
## $ fish_id         <dbl> 299, 299, 299, 300, 300, 300, 300, 301, 301, 301, 301…
## $ annnumber       <chr> "EDGE", "2", "1", "EDGE", "3", "2", "1", "EDGE", "3",…
## $ length          <dbl> 167, 167, 167, 175, 175, 175, 175, 194, 194, 194, 194…
## $ radii_length_mm <dbl> 2.697443, 2.037518, 1.311795, 3.015477, 2.670733, 2.1…
## $ scalelength     <dbl> 2.697443, 2.697443, 2.697443, 3.015477, 3.015477, 3.0…
```

Notice that when the data are imported, you are presented with a message that tells you how R interpreted the column classes. This is also where error messages will appear if there are problems.  

## Summary functions
Once data have been uploaded, you may want to get an idea of its structure, contents, and dimensions. I routinely run one or more of these commands when data are first imported.  

We can summarize our data frame with the`summary()` function.  

```r
summary(fish)
```

```
##     lakeid             fish_id       annnumber             length     
##  Length:4033        Min.   :  1.0   Length:4033        Min.   : 58.0  
##  Class :character   1st Qu.:156.0   Class :character   1st Qu.:253.0  
##  Mode  :character   Median :267.0   Mode  :character   Median :299.0  
##                     Mean   :258.3                      Mean   :293.3  
##                     3rd Qu.:376.0                      3rd Qu.:342.0  
##                     Max.   :478.0                      Max.   :420.0  
##  radii_length_mm    scalelength     
##  Min.   : 0.4569   Min.   : 0.6282  
##  1st Qu.: 2.3252   1st Qu.: 4.2596  
##  Median : 3.5380   Median : 5.4062  
##  Mean   : 3.6589   Mean   : 5.3821  
##  3rd Qu.: 4.8229   3rd Qu.: 6.4145  
##  Max.   :11.0258   Max.   :11.0258
```

`glimpse()` is another useful summary function.

```r
glimpse(fish)
```

```
## Rows: 4,033
## Columns: 6
## $ lakeid          <chr> "AL", "AL", "AL", "AL", "AL", "AL", "AL", "AL", "AL",…
## $ fish_id         <dbl> 299, 299, 299, 300, 300, 300, 300, 301, 301, 301, 301…
## $ annnumber       <chr> "EDGE", "2", "1", "EDGE", "3", "2", "1", "EDGE", "3",…
## $ length          <dbl> 167, 167, 167, 175, 175, 175, 175, 194, 194, 194, 194…
## $ radii_length_mm <dbl> 2.697443, 2.037518, 1.311795, 3.015477, 2.670733, 2.1…
## $ scalelength     <dbl> 2.697443, 2.697443, 2.697443, 3.015477, 3.015477, 3.0…
```

`nrow()` gives the numbers of rows.

```r
nrow(fish) #the number of rows or observations
```

```
## [1] 4033
```

`ncol` gives the number of columns.

```r
ncol(fish) #the number of columns or variables
```

```
## [1] 6
```

`dim()` gives the dimensions.

```r
dim(fish) #total dimensions
```

```
## [1] 4033    6
```

`names` gives the column names.

```r
names(fish) #column names
```

```
## [1] "lakeid"          "fish_id"         "annnumber"       "length"         
## [5] "radii_length_mm" "scalelength"
```

`head()` prints the first n rows of the data frame.

```r
head(fish, n = 10)
```

```
## # A tibble: 10 x 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         299 EDGE         167            2.70        2.70
##  2 AL         299 2            167            2.04        2.70
##  3 AL         299 1            167            1.31        2.70
##  4 AL         300 EDGE         175            3.02        3.02
##  5 AL         300 3            175            2.67        3.02
##  6 AL         300 2            175            2.14        3.02
##  7 AL         300 1            175            1.23        3.02
##  8 AL         301 EDGE         194            3.34        3.34
##  9 AL         301 3            194            2.97        3.34
## 10 AL         301 2            194            2.29        3.34
```

`tail()` prinst the last n rows of the data frame.

```r
tail(fish, n = 10)
```

```
## # A tibble: 10 x 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 WS         180 10           403            8.15        11.0
##  2 WS         180 9            403            7.49        11.0
##  3 WS         180 8            403            6.97        11.0
##  4 WS         180 7            403            6.24        11.0
##  5 WS         180 6            403            5.41        11.0
##  6 WS         180 5            403            4.98        11.0
##  7 WS         180 4            403            4.22        11.0
##  8 WS         180 3            403            3.04        11.0
##  9 WS         180 2            403            2.03        11.0
## 10 WS         180 1            403            1.19        11.0
```

`table()` is useful when you have a limited number of categorical variables. It produces fast counts of the number of observations in a variable. We will come back to this later... 

```r
table(fish$lakeid)
```

```
## 
##  AL  AR  BO  BR  CR  DY  FD  JN  LC  LJ  LR LSG  MN  RD  UB  WS 
## 383 262 197 291 343 355 302 238 173 181 292 143 293 135 191 254
```

We can also click on the `fish` data frame in the Environment tab or type View(fish).

```r
View(fish)
```

## Subset
Subset is a way of pulling out observations that meet specific criteria in a variable.  

```r
little_fish <- subset(fish, length<=100)
little_fish
```

```
## # A tibble: 5 x 6
##   lakeid fish_id annnumber length radii_length_mm scalelength
##   <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
## 1 LSG         58 EDGE          92           1.15        1.15 
## 2 LSG         59 EDGE          64           0.773       0.773
## 3 WS         151 EDGE          58           0.628       0.628
## 4 WS         152 EDGE          74           0.832       0.832
## 5 WS         153 EDGE          78           0.637       0.637
```

## Practice
1. Load the data `mammal_lifehistories_v2.csv` and place it into a new object called `mammals`.

```r
setwd("~/Documents/GitHub/BIS15W2021_jviswanathan/lab3/data")
mammals <- readr::read_csv("mammal_lifehistories_v2.csv")
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   order = col_character(),
##   family = col_character(),
##   Genus = col_character(),
##   species = col_character(),
##   mass = col_double(),
##   gestation = col_double(),
##   newborn = col_double(),
##   weaning = col_double(),
##   `wean mass` = col_double(),
##   AFR = col_double(),
##   `max. life` = col_double(),
##   `litter size` = col_double(),
##   `litters/year` = col_double()
## )
```

2. Provide the dimensions of the data frame.

```r
nrow(mammals)
```

```
## [1] 1440
```

```r
ncol(mammals)
```

```
## [1] 13
```

```r
#The data contain 1440 rows and 13 columns. A quicker way to do this would be to use the following command. 

dim(mammals)
```

```
## [1] 1440   13
```

3. Check the column names in the data frame. 

```r
names(mammals)
```

```
##  [1] "order"        "family"       "Genus"        "species"      "mass"        
##  [6] "gestation"    "newborn"      "weaning"      "wean mass"    "AFR"         
## [11] "max. life"    "litter size"  "litters/year"
```

4. Use `str()` to show the structure of the data frame and its individual columns; compare this to `glimpse()`. 

```r
str(mammals)
```

```
## tibble [1,440 × 13] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ order       : chr [1:1440] "Artiodactyla" "Artiodactyla" "Artiodactyla" "Artiodactyla" ...
##  $ family      : chr [1:1440] "Antilocapridae" "Bovidae" "Bovidae" "Bovidae" ...
##  $ Genus       : chr [1:1440] "Antilocapra" "Addax" "Aepyceros" "Alcelaphus" ...
##  $ species     : chr [1:1440] "americana" "nasomaculatus" "melampus" "buselaphus" ...
##  $ mass        : num [1:1440] 45375 182375 41480 150000 28500 ...
##  $ gestation   : num [1:1440] 8.13 9.39 6.35 7.9 6.8 5.08 5.72 5.5 8.93 9.14 ...
##  $ newborn     : num [1:1440] 3246 5480 5093 10167 -999 ...
##  $ weaning     : num [1:1440] 3 6.5 5.63 6.5 -999 ...
##  $ wean mass   : num [1:1440] 8900 -999 15900 -999 -999 ...
##  $ AFR         : num [1:1440] 13.5 27.3 16.7 23 -999 ...
##  $ max. life   : num [1:1440] 142 308 213 240 -999 251 228 255 300 324 ...
##  $ litter size : num [1:1440] 1.85 1 1 1 1 1.37 1 1 1 1 ...
##  $ litters/year: num [1:1440] 1 0.99 0.95 -999 -999 2 -999 1.89 1 1 ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   order = col_character(),
##   ..   family = col_character(),
##   ..   Genus = col_character(),
##   ..   species = col_character(),
##   ..   mass = col_double(),
##   ..   gestation = col_double(),
##   ..   newborn = col_double(),
##   ..   weaning = col_double(),
##   ..   `wean mass` = col_double(),
##   ..   AFR = col_double(),
##   ..   `max. life` = col_double(),
##   ..   `litter size` = col_double(),
##   ..   `litters/year` = col_double()
##   .. )
```


```r
glimpse(mammals)
```

```
## Rows: 1,440
## Columns: 13
## $ order          <chr> "Artiodactyla", "Artiodactyla", "Artiodactyla", "Artio…
## $ family         <chr> "Antilocapridae", "Bovidae", "Bovidae", "Bovidae", "Bo…
## $ Genus          <chr> "Antilocapra", "Addax", "Aepyceros", "Alcelaphus", "Am…
## $ species        <chr> "americana", "nasomaculatus", "melampus", "buselaphus"…
## $ mass           <dbl> 45375.0, 182375.0, 41480.0, 150000.0, 28500.0, 55500.0…
## $ gestation      <dbl> 8.13, 9.39, 6.35, 7.90, 6.80, 5.08, 5.72, 5.50, 8.93, …
## $ newborn        <dbl> 3246.36, 5480.00, 5093.00, 10166.67, -999.00, 3810.00,…
## $ weaning        <dbl> 3.00, 6.50, 5.63, 6.50, -999.00, 4.00, 4.04, 2.13, 10.…
## $ `wean mass`    <dbl> 8900, -999, 15900, -999, -999, -999, -999, -999, 15750…
## $ AFR            <dbl> 13.53, 27.27, 16.66, 23.02, -999.00, 14.89, 10.23, 20.…
## $ `max. life`    <dbl> 142, 308, 213, 240, -999, 251, 228, 255, 300, 324, 300…
## $ `litter size`  <dbl> 1.85, 1.00, 1.00, 1.00, 1.00, 1.37, 1.00, 1.00, 1.00, …
## $ `litters/year` <dbl> 1.00, 0.99, 0.95, -999.00, -999.00, 2.00, -999.00, 1.8…
```

5. . Try the `table()` command to produce counts of mammal order, family, and genus.  

```r
#The following commands will display how many of each order, family, and genus exist. 

table(mammals$order)
```

```
## 
##   Artiodactyla      Carnivora        Cetacea     Dermoptera     Hyracoidea 
##            161            197             55              2              4 
##    Insectivora     Lagomorpha  Macroscelidea Perissodactyla      Pholidota 
##             91             42             10             15              7 
##       Primates    Proboscidea       Rodentia     Scandentia        Sirenia 
##            156              2            665              7              5 
##  Tubulidentata      Xenarthra 
##              1             20
```

```r
table(mammals$Genus)
```

```
## 
##         Abrocoma         Acinonyx           Acomys            Addax 
##                2                1                4                1 
##        Aepyceros         Aethomys           Agouti       Ailuropoda 
##                1                4                1                1 
##          Ailurus           Akodon       Alcelaphus            Alces 
##                1                9                1                1 
##        Allactaga   Allenopithecus   Allocricetulus           Alopex 
##                4                1                1                1 
##         Alouatta         Alticola         Amblonyx       Amblysomus 
##                4                3                1                1 
##       Ammodorcas Ammospermophilus       Ammotragus       Anomalurus 
##                1                4                1                3 
##       Antidorcas      Antilocapra         Antilope            Aonyx 
##                1                1                1                2 
##            Aotus       Aplodontia         Apodemus        Arborimus 
##                3                1                7                3 
##        Arctictis       Arctocebus    Arctocephalus     Arctogalidia 
##                1                1                6                1 
##         Arctonyx      Arvicanthis         Arvicola         Atelerix 
##                1                1                2                3 
##           Ateles       Atelocynus        Atherurus           Atilax 
##                4                1                2                1 
##       Auliscomys            Avahi             Axis        Babyrousa 
##                1                1                2                1 
##          Baiomys          Balaena     Balaenoptera        Bandicota 
##                1                1                5                2 
##      Bassaricyon      Bassariscus       Bathyergus         Bdeogale 
##                1                2                2                1 
##           Beamys        Berardius            Bison          Blarina 
##                1                2                2                3 
##      Blastocerus          Bolomys              Bos       Boselaphus 
##                1                1                3                1 
##      Brachylagus      Brachyteles         Bradypus          Bubalus 
##                1                1                3                3 
##         Budorcas        Bunolagus        Cabassous          Cacajao 
##                1                1                1                1 
##       Callicebus        Callimico       Callithrix      Callorhinus 
##                3                1                6                1 
##     Callosciurus          Calomys       Calomyscus          Camelus 
##                4                5                2                2 
##            Canis         Cannomys          Caperea            Capra 
##                7                1                1                5 
##        Capreolus       Caprolagus         Capromys          Caracal 
##                2                1                1                1 
##           Castor        Catagonus         Catopuma            Cavia 
##                2                1                1                3 
##            Cebus      Cephalophus  Cephalorhynchus    Ceratotherium 
##                4               11                3                1 
##       Cercocebus    Cercopithecus        Cerdocyon           Cervus 
##                2               13                1                7 
##      Chaetodipus   Chaetophractus     Cheirogaleus       Chinchilla 
##                7                2                2                2 
##        Chionomys     Chiropodomys       Chiropotes       Chiruromys 
##                2                1                2                1 
##      Chlorocebus        Choloepus       Chrotogale    Chrysochloris 
##                1                2                1                2 
##       Chrysocyon     Chrysospalax      Civettictis    Clethrionomys 
##                1                1                1                5 
##          Coendou          Colobus          Colomys        Condylura 
##                1                4                1                1 
##        Conepatus        Conilurus     Connochaetes        Cremnomys 
##                3                1                2                2 
##       Cricetomys       Cricetulus         Cricetus        Crocidura 
##                1                2                1               13 
##          Crocuta      Crossarchus        Cryptomys     Cryptoprocta 
##                1                1                3                1 
##        Cryptotis    Ctenodactylus         Ctenomys             Cuon 
##                1                2                5                1 
##         Cyclopes         Cynictis     Cynocephalus         Cynogale 
##                1                1                2                1 
##          Cynomys       Cystophora             Dama       Damaliscus 
##                5                1                1                3 
##          Dasymys       Dasyprocta          Dasypus      Daubentonia 
##                1                3                5                1 
##   Delphinapterus        Delphinus      Dendrohyrax        Dendromus 
##                1                1                2                5 
##         Dephomys          Desmana   Desmodilliscus      Desmodillus 
##                1                1                1                1 
##     Dicerorhinus          Diceros      Dicrostonyx        Dinaromys 
##                1                1                5                1 
##          Dinomys     Diplomesodon        Dipodomys            Dipus 
##                1                1               14                1 
##       Dolichotis         Dremomys          Dryomys           Dugong 
##                2                1                2                1 
##          Echimys         Echinops      Echinosorex             Eira 
##                1                1                1                1 
##        Elaphodus        Elaphurus     Elephantulus          Elephas 
##                1                1                7                1 
##     Eligmodontia          Eliomys         Ellobius          Enhydra 
##                1                1                2                1 
##        Eolagurus         Epixerus            Equus       Eremitalpa 
##                1                1                6                1 
##       Eremodipus        Erethizon       Erignathus        Erinaceus 
##                1                1                1                2 
##     Erythrocebus     Eschrichtius        Eubalaena          Eulemur 
##                1                1                2                5 
##       Eumetopias         Euoticus       Euphractus         Eupleres 
##                1                1                1                1 
##     Exilisciurus            Felis           Feresa            Fossa 
##                1                4                1                1 
##       Funambulus      Funisciurus           Galago       Galagoides 
##                3                4                3                2 
##            Galea          Galemys        Galerella         Galictis 
##                2                1                2                1 
##          Galidia       Galidictis          Gazella          Genetta 
##                1                1               11                3 
##      Geocapromys          Geogale           Geomys        Georychus 
##                2                1                4                1 
##      Gerbillurus        Gerbillus          Giraffa        Glaucomys 
##                3               10                1                2 
##         Glirulus     Globicephala          Golunda          Gorilla 
##                1                2                1                1 
##        Grammomys          Grampus          Graomys       Graphiurus 
##                3                1                1                3 
##             Gulo      Halichoerus        Hapalemur        Helarctos 
##                1                1                2                1 
##     Heliophobius     Heliosciurus         Helogale     Hemicentetes 
##                1                1                1                1 
##      Hemiechinus        Hemigalus       Hemitragus      Herpailurus 
##                4                1                2                1 
##        Herpestes   Heterocephalus      Heterohyrax        Heteromys 
##                3                1                1                4 
##     Hexaprotodon     Hippocamelus     Hippopotamus      Hippotragus 
##                1                2                1                2 
##          Hodomys       Holochilus         Hoplomys           Hyaena 
##                1                1                1                1 
##          Hybomys     Hydrochaeris     Hydrodamalis         Hydromys 
##                1                1                1                1 
##       Hydropotes         Hydrurga       Hyemoschus        Hylobates 
##                1                1                1                7 
##      Hylochoerus          Hylomys       Hylomyscus        Hylopetes 
##                1                1                2                2 
##           Hyomys      Hyperacrius       Hyperoodon       Hypogeomys 
##                1                2                1                1 
##          Hystrix        Ichneumia          Ictonyx          Idiurus 
##                5                1                2                1 
##            Indri             Inia            Iomys          Jaculus 
##                1                1                1                3 
##    Kannabateomys          Kerodon            Kobus            Kogia 
##                1                1                5                2 
##    Lagenodelphis   Lagenorhynchus         Lagidium       Lagostomus 
##                1                4                2                1 
##        Lagothrix          Lagurus             Lama     Lasiopodomys 
##                1                1                3                1 
##        Leggadina        Lemmiscus           Lemmus      Lemniscomys 
##                2                1                2                2 
##            Lemur   Leontopithecus        Leopardus        Lepilemur 
##                1                1                3                3 
##       Leporillus      Leptailurus    Leptonychotes            Lepus 
##                1                1                1               14 
##        Limnogale           Liomys          Lipotes      Litocranius 
##                1                4                1                1 
##          Lobodon           Lontra        Lophiomys       Lophocebus 
##                1                3                1                1 
##       Lophuromys            Loris        Loxodonta            Lutra 
##                4                1                1                2 
##        Lutrogale           Lycaon             Lynx           Macaca 
##                1                1                4               13 
##    Macroscelides    Macrotarsomys          Madoqua         Makalata 
##                1                1                3                1 
##      Malacothrix       Mandrillus            Manis          Marmota 
##                1                2                7               13 
##           Martes      Massoutiera         Mastomys           Mazama 
##                6                1                2                3 
##     Megadontomys        Megaptera            Meles        Mellivora 
##                1                1                1                1 
##         Melogale          Melomys         Melursus         Mephitis 
##                1                8                1                2 
##         Meriones     Mesembriomys     Mesocricetus       Mesoplodon 
##               10                2                3                3 
##       Microcavia       Microcebus    Microdipodops        Microgale 
##                1                3                2                2 
##         Micromys  Micropotamogale         Microtus        Millardia 
##                1                1               29                1 
##      Miopithecus         Mirounga         Monachus          Monodon 
##                1                2                2                1 
##        Moschiola          Moschus           Mungos      Mungotictis 
##                1                3                1                1 
##        Muntiacus              Mus      Muscardinus          Mustela 
##                2               10                1               10 
##        Myocastor           Myomys        Myoprocta           Myopus 
##                1                3                1                1 
##       Myosciurus         Myosorex        Myospalax           Myoxus 
##                1                3                1                1 
##     Myrmecophaga        Mysateles        Mystromys      Naemorhedus 
##                1                2                1                3 
##         Nandinia      Nannospalax      Napaeozapus          Nasalis 
##                1                1                1                1 
##            Nasua         Neacomys         Nectomys         Neofelis 
##                2                1                2                1 
##         Neofiber           Neomys         Neophoca      Neophocaena 
##                1                2                1                1 
##          Neotoma       Neotomodon        Neotragus          Nesokia 
##               10                1                2                1 
##     Neurotrichus       Niviventer       Notiosorex          Notomys 
##                1                1                1                5 
##      Nyctereutes       Nycticebus         Nyctomys         Ochotona 
##                1                2                1               11 
##       Ochrotomys          Octodon     Octodontomys         Odobenus 
##                1                1                1                1 
##       Odocoileus          Oecomys          Oenomys           Okapia 
##                2                1                1                1 
##     Oligoryzomys      Ommatophoca        Oncifelis          Ondatra 
##                3                1                2                1 
##        Onychomys         Orcaella          Orcinus         Oreamnos 
##                2                1                1                1 
##       Oreotragus      Orthogeomys      Orycteropus      Oryctolagus 
##                1                2                1                1 
##             Oryx         Oryzomys           Otaria       Otocolobus 
##                3                3                1                1 
##          Otocyon         Otolemur           Otomys       Ototylomys 
##                1                2                5                1 
##          Ourebia           Ovibos             Ovis      Oxymycterus 
##                1                1                6                1 
##       Ozotoceros      Pachyuromys           Paguma              Pan 
##                1                1                1                2 
##         Panthera       Pantholops            Papio      Pappogeomys 
##                4                1                1                1 
##     Paracynictis      Paradoxurus       Parahyaena      Parascalops 
##                1                2                1                1 
##        Paraxerus       Pardofelis        Parotomys           Pecari 
##                5                1                1                1 
##          Pedetes            Pelea          Pelomys    Peponocephala 
##                1                1                2                1 
##     Perodicticus      Perognathus       Peromyscus       Petaurista 
##                1                7               24                5 
##        Petinomys      Petrodromus         Petromus      Petromyscus 
##                4                1                1                1 
##     Phacochoerus           Phaner       Phenacomys            Phoca 
##                1                1                2                7 
##       Phocarctos         Phocoena     Phocoenoides         Phodopus 
##                1                2                1                3 
##        Phyllotis         Physeter       Pithecheir         Pithecia 
##                1                1                1                2 
##     Plagiodontia       Platanista          Podomys      Poecilogale 
##                1                2                1                1 
##         Poelagus    Pogonomelomys        Pogonomys            Pongo 
##                1                1                3                1 
##       Pontoporia    Potamochoerus       Potamogale            Potos 
##                1                1                1                1 
##          Praomys        Presbytis       Priodontes     Prionailurus 
##                4                4                1                3 
##        Prionodon         Procapra         Procavia       Procolobus 
##                2                1                1                2 
##          Procyon       Proechimys         Profelis       Pronolagus 
##                2                3                1                3 
##      Propithecus         Proteles        Psammomys      Pseudalopex 
##                3                1                1                3 
##         Pseudois        Pseudomys        Pseudorca         Pteromys 
##                1               17                1                1 
##        Pteronura      Ptilocercus             Pudu             Puma 
##                1                1                2                1 
##        Pygathrix       Pygeretmus         Rangifer       Raphicerus 
##                2                2                1                1 
##           Rattus           Ratufa          Redunca       Reithrodon 
##               14                3                3                1 
##  Reithrodontomys        Rhabdomys       Rhinoceros     Rhinosciurus 
##                5                1                2                1 
##       Rhipidomys         Rhizomys        Rhombomys      Rhynchocyon 
##                2                2                1                1 
##      Romerolagus        Rupicapra      Saccostomus         Saguinus 
##                1                1                1               10 
##            Saiga          Saimiri      Salpingotus         Scalopus 
##                1                2                1                1 
##         Scapanus       Sciurillus          Sciurus       Scotinomys 
##                3                1               10                2 
##       Sekeetamys    Semnopithecus          Setifer          Sicista 
##                1                1                1                2 
##       Sigmoceros         Sigmodon        Solenodon            Sorex 
##                1                5                2               21 
##          Sotalia       Spalacopus           Spalax         Speothos 
##                1                1                1                1 
##  Spermophilopsis     Spermophilus       Sphiggurus        Spilogale 
##                1               31                2                2 
##        Steatomys         Stenella            Steno        Stochomys 
##                2                3                1                1 
##       Stylodipus           Suncus         Sundamys     Sundasciurus 
##                1                3                1                2 
##       Surdisorex         Suricata              Sus       Sylvicapra 
##                2                1                3                1 
##       Sylvilagus       Sylvisorex       Synaptomys         Syncerus 
##                8                1                2                1 
##     Tachyoryctes            Talpa         Tamandua           Tamias 
##                2                2                1               18 
##     Tamiasciurus          Tapirus          Tarsius         Tateomys 
##                2                4                5                1 
##           Tatera       Taterillus      Taurotragus          Taxidea 
##                8                3                2                1 
##          Tayassu           Tenrec       Tetracerus        Thallomys 
##                1                1                1                1 
##        Thamnomys    Theropithecus         Thomomys       Thrichomys 
##                1                1                7                1 
##       Thryonomys       Tolypeutes   Trachypithecus      Tragelaphus 
##                2                1                7                7 
##         Tragulus       Tremarctos       Trichechus      Trogopterus 
##                2                1                3                1 
##           Tupaia         Tursiops          Tylomys            Uncia 
##                5                1                1                1 
##         Uranomys          Urocyon          Urogale           Uromys 
##                1                2                1                1 
##       Urotrichus            Ursus      Vandeleuria          Varecia 
##                1                4                1                1 
##          Vicugna          Viverra      Viverricula          Vormela 
##                1                1                1                1 
##           Vulpes         Wiedomys            Xerus          Zaedyus 
##               10                1                2                1 
##         Zalophus            Zapus        Zelotomys          Ziphius 
##                1                3                1                1 
##     Zygodontomys          Zyzomys 
##                1                3
```

```r
table(mammals$family)
```

```
## 
##     Abrocomidae       Agoutidae    Anomaluridae  Antilocapridae    Aplodontidae 
##               2               1               4               1               1 
##      Balaenidae Balaenopteridae    Bathyergidae         Bovidae    Bradypodidae 
##               3               6               8             103               3 
##  Callitrichidae       Camelidae         Canidae     Capromyidae      Castoridae 
##              18               6              31               6               2 
##        Caviidae         Cebidae Cercopithecidae        Cervidae  Cheirogaleidae 
##               9              27              58              30               6 
##   Chinchillidae Chrysochloridae Ctenodactylidae     Ctenomyidae  Cynocephalidae 
##               5               5               3               5               2 
##     Dasypodidae   Dasyproctidae  Daubentoniidae     Delphinidae      Dinomyidae 
##              12               4               1              23               1 
##       Dipodidae      Dugongidae      Echimyidae    Elephantidae         Equidae 
##              19               2               8               2               6 
##  Erethizontidae     Erinaceidae  Eschrichtiidae         Felidae     Galagonidae 
##               4              11               1              31               8 
##       Geomyidae      Giraffidae     Herpestidae    Heteromyidae  Hippopotamidae 
##              14               2              17              38               2 
##       Hominidae       Hyaenidae  Hydrochaeridae     Hylobatidae     Hystricidae 
##               4               4               1               7               7 
##        Indridae       Lemuridae       Leporidae         Loridae Macroscelididae 
##               5               9              31               5              10 
##         Manidae   Megaladapidae  Megalonychidae    Monodontidae       Moschidae 
##               7               3               2               2               3 
##         Muridae      Mustelidae   Myocastoridae        Myoxidae Myrmecophagidae 
##             376              46               1               9               3 
##   Neobalaenidae     Ochotonidae    Octodontidae      Odobenidae Orycteropodidae 
##               1              11               3               1               1 
##       Otariidae       Pedetidae    Petromuridae        Phocidae     Phocoenidae 
##              12               1               1              18               4 
##    Physeteridae   Platanistidae     Procaviidae     Procyonidae  Rhinocerotidae 
##               3               5               4               8               5 
##       Sciuridae  Solenodontidae       Soricidae          Suidae        Talpidae 
##             130               2              51               7              12 
##       Tapiridae       Tarsiidae     Tayassuidae      Tenrecidae   Thryonomyidae 
##               4               5               3              10               2 
##      Tragulidae    Trichechidae       Tupaiidae         Ursidae      Viverridae 
##               4               3               7               9              20 
##       Ziphiidae 
##               7
```

## Wrap-up
Please review the learning goals and be sure to use the code here as a reference when completing the homework.  
-->[Home](https://jmledford3115.github.io/datascibiol/)
