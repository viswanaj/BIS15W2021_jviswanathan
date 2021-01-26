---
title: "Lab 5 Homework"
author: "Jayashri Viswanathan"
date: "2021-01-26"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the tidyverse

```r
library(tidyverse)
```

## Load the superhero data
These are data taken from comic books and assembled by fans. The include a good mix of categorical and continuous data.  Data taken from: https://www.kaggle.com/claudiodavi/superhero-set  

Check out the way I am loading these data. If I know there are NAs, I can take care of them at the beginning. But, we should do this very cautiously. At times it is better to keep the original columns and data intact.  

```r
superhero_info <- readr::read_csv("data/heroes_information.csv", na = c("", "-99", "-"))
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   name = col_character(),
##   Gender = col_character(),
##   `Eye color` = col_character(),
##   Race = col_character(),
##   `Hair color` = col_character(),
##   Height = col_double(),
##   Publisher = col_character(),
##   `Skin color` = col_character(),
##   Alignment = col_character(),
##   Weight = col_double()
## )
```

```r
superhero_powers <- readr::read_csv("data/super_hero_powers.csv", na = c("", "-99", "-"))
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   .default = col_logical(),
##   hero_names = col_character()
## )
## i Use `spec()` for the full column specifications.
```

## Data tidy
1. Some of the names used in the `superhero_info` data are problematic so you should rename them here.  

```r
superhero_info <- superhero_info %>%
  select_all(tolower) %>%
  select_all(~str_replace(., " ", "_"))
```

Yikes! `superhero_powers` has a lot of variables that are poorly named. We need some R superpowers...

```r
superhero_powers <- superhero_powers %>%
  select_all(tolower) %>%
  select_all(~str_replace(., " ", "_"))
```

## `janitor`
The [janitor](https://garthtarr.github.io/meatR/janitor.html) package is your friend. Make sure to install it and then load the library.  

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

The `clean_names` function takes care of everything in one line! Now that's a superpower!

```r
superhero_powers <- janitor::clean_names(superhero_powers)
```

## `tabyl`
The `janitor` package has many awesome functions that we will explore. Here is its version of `table` which not only produces counts but also percentages. Very handy! Let's use it to explore the proportion of good guys and bad guys in the `superhero_info` data.  

```r
superhero_info %>%
  tabyl(alignment)
```

```
##  alignment   n     percent valid_percent
##        bad 207 0.282016349    0.28473177
##       good 496 0.675749319    0.68225585
##    neutral  24 0.032697548    0.03301238
##       <NA>   7 0.009536785            NA
```

2. Notice that we have some neutral superheros! Who are they?

```r
neutral <- superhero_info %>%
  filter(alignment == "neutral")

neutral$name
```

```
##  [1] "Bizarro"         "Black Flash"     "Captain Cold"    "Copycat"        
##  [5] "Deadpool"        "Deathstroke"     "Etrigan"         "Galactus"       
##  [9] "Gladiator"       "Indigo"          "Juggernaut"      "Living Tribunal"
## [13] "Lobo"            "Man-Bat"         "One-Above-All"   "Raven"          
## [17] "Red Hood"        "Red Hulk"        "Robin VI"        "Sandman"        
## [21] "Sentry"          "Sinestro"        "The Comedian"    "Toad"
```

## `superhero_info`
3. Let's say we are only interested in the variables name, alignment, and "race". How would you isolate these variables from `superhero_info`?

```r
super_hero_new <- superhero_info %>%
  select(name, alignment, race)

super_hero_new
```

```
## # A tibble: 734 x 3
##    name          alignment race             
##    <chr>         <chr>     <chr>            
##  1 A-Bomb        good      Human            
##  2 Abe Sapien    good      Icthyo Sapien    
##  3 Abin Sur      good      Ungaran          
##  4 Abomination   bad       Human / Radiation
##  5 Abraxas       bad       Cosmic Entity    
##  6 Absorbing Man bad       Human            
##  7 Adam Monroe   good      <NA>             
##  8 Adam Strange  good      Human            
##  9 Agent 13      good      <NA>             
## 10 Agent Bob     good      Human            
## # ... with 724 more rows
```

## Not Human
4. List all of the superheros that are not human.

```r
super_hero_new %>%
  filter( race != "Human")
```

```
## # A tibble: 222 x 3
##    name         alignment race             
##    <chr>        <chr>     <chr>            
##  1 Abe Sapien   good      Icthyo Sapien    
##  2 Abin Sur     good      Ungaran          
##  3 Abomination  bad       Human / Radiation
##  4 Abraxas      bad       Cosmic Entity    
##  5 Ajax         bad       Cyborg           
##  6 Alien        bad       Xenomorph XX121  
##  7 Amazo        bad       Android          
##  8 Angel        good      Vampire          
##  9 Angel Dust   good      Mutant           
## 10 Anti-Monitor bad       God / Eternal    
## # ... with 212 more rows
```

## Good and Evil
5. Let's make two different data frames, one focused on the "good guys" and another focused on the "bad guys".

```r
good_guys <- superhero_info %>%
  filter(alignment == "good")

bad_guys <- superhero_info %>%
  filter(alignment == "bad")
```

6. For the good guys, use the `tabyl` function to summarize their "race".

```r
good_guys %>%
  tabyl(race)
```

```
##               race   n     percent valid_percent
##              Alien   3 0.006048387   0.010752688
##              Alpha   5 0.010080645   0.017921147
##             Amazon   2 0.004032258   0.007168459
##            Android   4 0.008064516   0.014336918
##             Animal   2 0.004032258   0.007168459
##          Asgardian   3 0.006048387   0.010752688
##          Atlantean   4 0.008064516   0.014336918
##         Bolovaxian   1 0.002016129   0.003584229
##              Clone   1 0.002016129   0.003584229
##             Cyborg   3 0.006048387   0.010752688
##           Demi-God   2 0.004032258   0.007168459
##              Demon   3 0.006048387   0.010752688
##            Eternal   1 0.002016129   0.003584229
##     Flora Colossus   1 0.002016129   0.003584229
##        Frost Giant   1 0.002016129   0.003584229
##      God / Eternal   6 0.012096774   0.021505376
##             Gungan   1 0.002016129   0.003584229
##              Human 148 0.298387097   0.530465950
##         Human-Kree   2 0.004032258   0.007168459
##      Human-Spartoi   1 0.002016129   0.003584229
##       Human-Vulcan   1 0.002016129   0.003584229
##    Human-Vuldarian   1 0.002016129   0.003584229
##    Human / Altered   2 0.004032258   0.007168459
##     Human / Cosmic   2 0.004032258   0.007168459
##  Human / Radiation   8 0.016129032   0.028673835
##      Icthyo Sapien   1 0.002016129   0.003584229
##            Inhuman   4 0.008064516   0.014336918
##    Kakarantharaian   1 0.002016129   0.003584229
##         Kryptonian   4 0.008064516   0.014336918
##            Martian   1 0.002016129   0.003584229
##          Metahuman   1 0.002016129   0.003584229
##             Mutant  46 0.092741935   0.164874552
##     Mutant / Clone   1 0.002016129   0.003584229
##             Planet   1 0.002016129   0.003584229
##             Saiyan   1 0.002016129   0.003584229
##           Symbiote   3 0.006048387   0.010752688
##           Talokite   1 0.002016129   0.003584229
##         Tamaranean   1 0.002016129   0.003584229
##            Ungaran   1 0.002016129   0.003584229
##            Vampire   2 0.004032258   0.007168459
##     Yoda's species   1 0.002016129   0.003584229
##      Zen-Whoberian   1 0.002016129   0.003584229
##               <NA> 217 0.437500000            NA
```

```r
good_guys
```

```
## # A tibble: 496 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 A-Bo~ Male   yellow    Human No Hair       203 Marvel C~ <NA>       good     
##  2 Abe ~ Male   blue      Icth~ No Hair       191 Dark Hor~ blue       good     
##  3 Abin~ Male   blue      Unga~ No Hair       185 DC Comics red        good     
##  4 Adam~ Male   blue      <NA>  Blond          NA NBC - He~ <NA>       good     
##  5 Adam~ Male   blue      Human Blond         185 DC Comics <NA>       good     
##  6 Agen~ Female blue      <NA>  Blond         173 Marvel C~ <NA>       good     
##  7 Agen~ Male   brown     Human Brown         178 Marvel C~ <NA>       good     
##  8 Agen~ Male   <NA>      <NA>  <NA>          191 Marvel C~ <NA>       good     
##  9 Alan~ Male   blue      <NA>  Blond         180 DC Comics <NA>       good     
## 10 Alex~ Male   <NA>      <NA>  <NA>           NA NBC - He~ <NA>       good     
## # ... with 486 more rows, and 1 more variable: weight <dbl>
```

```r
bad_guys
```

```
## # A tibble: 207 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 Abom~ Male   green     Huma~ No Hair       203 Marvel C~ <NA>       bad      
##  2 Abra~ Male   blue      Cosm~ Black          NA Marvel C~ <NA>       bad      
##  3 Abso~ Male   blue      Human No Hair       193 Marvel C~ <NA>       bad      
##  4 Air-~ Male   blue      <NA>  White         188 Marvel C~ <NA>       bad      
##  5 Ajax  Male   brown     Cybo~ Black         193 Marvel C~ <NA>       bad      
##  6 Alex~ Male   <NA>      Human <NA>           NA Wildstorm <NA>       bad      
##  7 Alien Male   <NA>      Xeno~ No Hair       244 Dark Hor~ black      bad      
##  8 Amazo Male   red       Andr~ <NA>          257 DC Comics <NA>       bad      
##  9 Ammo  Male   brown     Human Black         188 Marvel C~ <NA>       bad      
## 10 Ange~ Female <NA>      <NA>  <NA>           NA Image Co~ <NA>       bad      
## # ... with 197 more rows, and 1 more variable: weight <dbl>
```

7. Among the good guys, Who are the Asgardians?

```r
asgardians <- good_guys %>%
  filter(race == "Asgardian")

asgardians$name
```

```
## [1] "Sif"       "Thor"      "Thor Girl"
```
<style>
div.blue { background-color:#e6f0ff; border-radius: 5px; padding: 20px;}
</style>
<div class = "blue">

8. Among the bad guys, who are the male humans over 200 inches in height?

```r
tall <- bad_guys %>%
  filter(height > 200)

tall$name
```

```
##  [1] "Abomination"    "Alien"          "Amazo"          "Apocalypse"    
##  [5] "Bane"           "Bloodaxe"       "Darkseid"       "Doctor Doom"   
##  [9] "Doctor Doom II" "Doomsday"       "Frenzy"         "Hela"          
## [13] "Killer Croc"    "Kingpin"        "Lizard"         "MODOK"         
## [17] "Omega Red"      "Onslaught"      "Predator"       "Sauron"        
## [21] "Scorpion"       "Solomon Grundy" "Thanos"         "Ultron"        
## [25] "Venom III"
```
</div>

9. OK, so are there more good guys or bad guys that are bald (personal interest)?

```r
bald_good <- good_guys %>%
  filter(hair_color == "No Hair")

bald_bad <- bad_guys %>%
  filter(hair_color == "No Hair")

count(bald_good)
```

```
## # A tibble: 1 x 1
##       n
##   <int>
## 1    37
```

```r
count(bald_bad)
```

```
## # A tibble: 1 x 1
##       n
##   <int>
## 1    35
```

```r
#There are more bald good guys. 
```

10. Let's explore who the really "big" superheros are. In the `superhero_info` data, which have a height over 200 or weight over 300?

```r
big_hero <- superhero_info %>%
  filter(height > 200 | weight > 300)

big_hero$name
```

```
##  [1] "A-Bomb"             "Abomination"        "Alien"             
##  [4] "Amazo"              "Ant-Man"            "Anti-Venom"        
##  [7] "Apocalypse"         "Bane"               "Beta Ray Bill"     
## [10] "Bloodaxe"           "Cable"              "Century"           
## [13] "Cloak"              "Colossus"           "Darkseid"          
## [16] "Destroyer"          "Doctor Doom"        "Doctor Doom II"    
## [19] "Doomsday"           "Drax the Destroyer" "Fin Fang Foom"     
## [22] "Frenzy"             "Galactus"           "Giganta"           
## [25] "Groot"              "Hela"               "Hellboy"           
## [28] "Hulk"               "Juggernaut"         "K-2SO"             
## [31] "Killer Croc"        "Kilowog"            "Kingpin"           
## [34] "Living Brain"       "Lizard"             "Lobo"              
## [37] "Machine Man"        "Man-Thing"          "Martian Manhunter" 
## [40] "Master Chief"       "MODOK"              "Mr Incredible"     
## [43] "Odin"               "Omega Red"          "Onslaught"         
## [46] "Predator"           "Red Hulk"           "Rey"               
## [49] "Rhino"              "Sasquatch"          "Sauron"            
## [52] "Scorpion"           "She-Hulk"           "Sinestro"          
## [55] "Solomon Grundy"     "Spawn"              "Steel"             
## [58] "Thanos"             "Thundra"            "Ultron"            
## [61] "Venom III"          "Venompool"          "Warpath"           
## [64] "Wolfsbane"          "Ymir"
```
<style>
div.blue { background-color:#e6f0ff; border-radius: 5px; padding: 20px;}
</style>
<div class = "blue">

11. Just to be clear on the `|` operator,  have a look at the superheros over 300 in height...


</div>

12. ...and the superheros over 450 in weight. Bonus question! Why do we not have 16 rows in question #10?

```r
superhero_info %>%
  filter(height > 300 | weight > 450)
```

```
## # A tibble: 14 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 Bloo~ Female blue      Human Brown       218   Marvel C~ <NA>       bad      
##  2 Dark~ Male   red       New ~ No Hair     267   DC Comics grey       bad      
##  3 Fin ~ Male   red       Kaka~ No Hair     975   Marvel C~ green      good     
##  4 Gala~ Male   black     Cosm~ Black       876   Marvel C~ <NA>       neutral  
##  5 Giga~ Female green     <NA>  Red          62.5 DC Comics <NA>       bad      
##  6 Groot Male   yellow    Flor~ <NA>        701   Marvel C~ <NA>       good     
##  7 Hulk  Male   green     Huma~ Green       244   Marvel C~ green      good     
##  8 Jugg~ Male   blue      Human Red         287   Marvel C~ <NA>       neutral  
##  9 MODOK Male   white     Cybo~ Brownn      366   Marvel C~ <NA>       bad      
## 10 Onsl~ Male   red       Muta~ No Hair     305   Marvel C~ <NA>       bad      
## 11 Red ~ Male   yellow    Huma~ Black       213   Marvel C~ red        neutral  
## 12 Sasq~ Male   red       <NA>  Orange      305   Marvel C~ <NA>       good     
## 13 Wolf~ Female green     <NA>  Auburn      366   Marvel C~ <NA>       good     
## 14 Ymir  Male   white     Fros~ No Hair     305.  Marvel C~ white      good     
## # ... with 1 more variable: weight <dbl>
```
<style>
div.blue { background-color:#e6f0ff; border-radius: 5px; padding: 20px;}
</style>
<div class = "blue">

## Height to Weight Ratio
13. It's easy to be strong when you are heavy and tall, but who is heavy and short? Which superheros have the highest height to weight ratio?

```r
superhero_info <- superhero_info %>%
  transform(new = height/weight)
```

</div>

## `superhero_powers`
Have a quick look at the `superhero_powers` data frame.  

```r
glimpse(superhero_powers) 
```

```
## Rows: 667
## Columns: 168
## $ hero_names                   <chr> "3-D Man", "A-Bomb", "Abe Sapien", "Ab...
## $ agility                      <lgl> TRUE, FALSE, TRUE, FALSE, FALSE, FALSE...
## $ accelerated_healing          <lgl> FALSE, TRUE, TRUE, FALSE, TRUE, FALSE,...
## $ lantern_power_ring           <lgl> FALSE, FALSE, FALSE, TRUE, FALSE, FALS...
## $ dimensional_awareness        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRU...
## $ cold_resistance              <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALS...
## $ durability                   <lgl> FALSE, TRUE, TRUE, FALSE, FALSE, FALSE...
## $ stealth                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ energy_absorption            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ flight                       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRU...
## $ danger_sense                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ underwater_breathing         <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALS...
## $ marksmanship                 <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALS...
## $ weapons_master               <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALS...
## $ power_augmentation           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ animal_attributes            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ longevity                    <lgl> FALSE, TRUE, TRUE, FALSE, FALSE, FALSE...
## $ intelligence                 <lgl> FALSE, FALSE, TRUE, FALSE, TRUE, TRUE,...
## $ super_strength               <lgl> TRUE, TRUE, TRUE, FALSE, TRUE, TRUE, T...
## $ cryokinesis                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ telepathy                    <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALS...
## $ energy_armor                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ energy_blasts                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ duplication                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ size_changing                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRU...
## $ density_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ stamina                      <lgl> TRUE, TRUE, TRUE, FALSE, TRUE, FALSE, ...
## $ astral_travel                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ audio_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ dexterity                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ omnitrix                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ super_speed                  <lgl> TRUE, FALSE, FALSE, FALSE, TRUE, TRUE,...
## $ possession                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ animal_oriented_powers       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ weapon_based_powers          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ electrokinesis               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ darkforce_manipulation       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ death_touch                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ teleportation                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRU...
## $ enhanced_senses              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ telekinesis                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ energy_beams                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ magic                        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRU...
## $ hyperkinesis                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ jump                         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ clairvoyance                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ dimensional_travel           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRU...
## $ power_sense                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ shapeshifting                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ peak_human_condition         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ immortality                  <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, TRUE...
## $ camouflage                   <lgl> FALSE, TRUE, FALSE, FALSE, FALSE, FALS...
## $ element_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ phasing                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ astral_projection            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ electrical_transport         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ fire_control                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ projection                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ summoning                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ enhanced_memory              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ reflexes                     <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALS...
## $ invulnerability              <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, TRUE...
## $ energy_constructs            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ force_fields                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ self_sustenance              <lgl> FALSE, TRUE, FALSE, FALSE, FALSE, FALS...
## $ anti_gravity                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ empathy                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ power_nullifier              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ radiation_control            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ psionic_powers               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ elasticity                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ substance_secretion          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ elemental_transmogrification <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ technopath_cyberpath         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ photographic_reflexes        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ seismic_power                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ animation                    <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, FALS...
## $ precognition                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ mind_control                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ fire_resistance              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ power_absorption             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ enhanced_hearing             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ nova_force                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ insanity                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ hypnokinesis                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ animal_control               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ natural_armor                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ intangibility                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ enhanced_sight               <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALS...
## $ molecular_manipulation       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRU...
## $ heat_generation              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ adaptation                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ gliding                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ power_suit                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ mind_blast                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ probability_manipulation     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ gravity_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ regeneration                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ light_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ echolocation                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ levitation                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ toxin_and_disease_control    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ banish                       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ energy_manipulation          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRU...
## $ heat_resistance              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ natural_weapons              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ time_travel                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ enhanced_smell               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ illusions                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ thirstokinesis               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ hair_manipulation            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ illumination                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ omnipotent                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ cloaking                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ changing_armor               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ power_cosmic                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRU...
## $ biokinesis                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ water_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ radiation_immunity           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ vision_telescopic            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ toxin_and_disease_resistance <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ spatial_awareness            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ energy_resistance            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ telepathy_resistance         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ molecular_combustion         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ omnilingualism               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ portal_creation              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ magnetism                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ mind_control_resistance      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ plant_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ sonar                        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ sonic_scream                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ time_manipulation            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ enhanced_touch               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ magic_resistance             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ invisibility                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ sub_mariner                  <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALS...
## $ radiation_absorption         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ intuitive_aptitude           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ vision_microscopic           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ melting                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ wind_control                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ super_breath                 <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, FALS...
## $ wallcrawling                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ vision_night                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ vision_infrared              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ grim_reaping                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ matter_absorption            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ the_force                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ resurrection                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ terrakinesis                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ vision_heat                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ vitakinesis                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ radar_sense                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ qwardian_power_ring          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ weather_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ vision_x_ray                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ vision_thermal               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ web_creation                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ reality_warping              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ odin_force                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ symbiote_costume             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ speed_force                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ phoenix_force                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ molecular_dissipation        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ vision_cryo                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ omnipresent                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ omniscient                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
```

```r
names(superhero_powers)
```

```
##   [1] "hero_names"                   "agility"                     
##   [3] "accelerated_healing"          "lantern_power_ring"          
##   [5] "dimensional_awareness"        "cold_resistance"             
##   [7] "durability"                   "stealth"                     
##   [9] "energy_absorption"            "flight"                      
##  [11] "danger_sense"                 "underwater_breathing"        
##  [13] "marksmanship"                 "weapons_master"              
##  [15] "power_augmentation"           "animal_attributes"           
##  [17] "longevity"                    "intelligence"                
##  [19] "super_strength"               "cryokinesis"                 
##  [21] "telepathy"                    "energy_armor"                
##  [23] "energy_blasts"                "duplication"                 
##  [25] "size_changing"                "density_control"             
##  [27] "stamina"                      "astral_travel"               
##  [29] "audio_control"                "dexterity"                   
##  [31] "omnitrix"                     "super_speed"                 
##  [33] "possession"                   "animal_oriented_powers"      
##  [35] "weapon_based_powers"          "electrokinesis"              
##  [37] "darkforce_manipulation"       "death_touch"                 
##  [39] "teleportation"                "enhanced_senses"             
##  [41] "telekinesis"                  "energy_beams"                
##  [43] "magic"                        "hyperkinesis"                
##  [45] "jump"                         "clairvoyance"                
##  [47] "dimensional_travel"           "power_sense"                 
##  [49] "shapeshifting"                "peak_human_condition"        
##  [51] "immortality"                  "camouflage"                  
##  [53] "element_control"              "phasing"                     
##  [55] "astral_projection"            "electrical_transport"        
##  [57] "fire_control"                 "projection"                  
##  [59] "summoning"                    "enhanced_memory"             
##  [61] "reflexes"                     "invulnerability"             
##  [63] "energy_constructs"            "force_fields"                
##  [65] "self_sustenance"              "anti_gravity"                
##  [67] "empathy"                      "power_nullifier"             
##  [69] "radiation_control"            "psionic_powers"              
##  [71] "elasticity"                   "substance_secretion"         
##  [73] "elemental_transmogrification" "technopath_cyberpath"        
##  [75] "photographic_reflexes"        "seismic_power"               
##  [77] "animation"                    "precognition"                
##  [79] "mind_control"                 "fire_resistance"             
##  [81] "power_absorption"             "enhanced_hearing"            
##  [83] "nova_force"                   "insanity"                    
##  [85] "hypnokinesis"                 "animal_control"              
##  [87] "natural_armor"                "intangibility"               
##  [89] "enhanced_sight"               "molecular_manipulation"      
##  [91] "heat_generation"              "adaptation"                  
##  [93] "gliding"                      "power_suit"                  
##  [95] "mind_blast"                   "probability_manipulation"    
##  [97] "gravity_control"              "regeneration"                
##  [99] "light_control"                "echolocation"                
## [101] "levitation"                   "toxin_and_disease_control"   
## [103] "banish"                       "energy_manipulation"         
## [105] "heat_resistance"              "natural_weapons"             
## [107] "time_travel"                  "enhanced_smell"              
## [109] "illusions"                    "thirstokinesis"              
## [111] "hair_manipulation"            "illumination"                
## [113] "omnipotent"                   "cloaking"                    
## [115] "changing_armor"               "power_cosmic"                
## [117] "biokinesis"                   "water_control"               
## [119] "radiation_immunity"           "vision_telescopic"           
## [121] "toxin_and_disease_resistance" "spatial_awareness"           
## [123] "energy_resistance"            "telepathy_resistance"        
## [125] "molecular_combustion"         "omnilingualism"              
## [127] "portal_creation"              "magnetism"                   
## [129] "mind_control_resistance"      "plant_control"               
## [131] "sonar"                        "sonic_scream"                
## [133] "time_manipulation"            "enhanced_touch"              
## [135] "magic_resistance"             "invisibility"                
## [137] "sub_mariner"                  "radiation_absorption"        
## [139] "intuitive_aptitude"           "vision_microscopic"          
## [141] "melting"                      "wind_control"                
## [143] "super_breath"                 "wallcrawling"                
## [145] "vision_night"                 "vision_infrared"             
## [147] "grim_reaping"                 "matter_absorption"           
## [149] "the_force"                    "resurrection"                
## [151] "terrakinesis"                 "vision_heat"                 
## [153] "vitakinesis"                  "radar_sense"                 
## [155] "qwardian_power_ring"          "weather_control"             
## [157] "vision_x_ray"                 "vision_thermal"              
## [159] "web_creation"                 "reality_warping"             
## [161] "odin_force"                   "symbiote_costume"            
## [163] "speed_force"                  "phoenix_force"               
## [165] "molecular_dissipation"        "vision_cryo"                 
## [167] "omnipresent"                  "omniscient"
```
<style>
div.blue { background-color:#e6f0ff; border-radius: 5px; padding: 20px;}
</style>
<div class = "blue">

14. How many superheros have a combination of accelerated healing, durability, and super strength?


</div>

<style>
div.blue { background-color:#e6f0ff; border-radius: 5px; padding: 20px;}
</style>
<div class = "blue">
## `kinesis`
15. We are only interested in the superheros that do some kind of "kinesis". How would we isolate them from the `superhero_powers` data?

```r
kinesis <- superhero_powers %>%
  filter(kinesis = TRUE)

kinesis
```

```
## # A tibble: 667 x 168
##    hero_names agility accelerated_hea~ lantern_power_r~ dimensional_awa~
##    <chr>      <lgl>   <lgl>            <lgl>            <lgl>           
##  1 3-D Man    TRUE    FALSE            FALSE            FALSE           
##  2 A-Bomb     FALSE   TRUE             FALSE            FALSE           
##  3 Abe Sapien TRUE    TRUE             FALSE            FALSE           
##  4 Abin Sur   FALSE   FALSE            TRUE             FALSE           
##  5 Abominati~ FALSE   TRUE             FALSE            FALSE           
##  6 Abraxas    FALSE   FALSE            FALSE            TRUE            
##  7 Absorbing~ FALSE   FALSE            FALSE            FALSE           
##  8 Adam Monr~ FALSE   TRUE             FALSE            FALSE           
##  9 Adam Stra~ FALSE   FALSE            FALSE            FALSE           
## 10 Agent Bob  FALSE   FALSE            FALSE            FALSE           
## # ... with 657 more rows, and 163 more variables: cold_resistance <lgl>,
## #   durability <lgl>, stealth <lgl>, energy_absorption <lgl>, flight <lgl>,
## #   danger_sense <lgl>, underwater_breathing <lgl>, marksmanship <lgl>,
## #   weapons_master <lgl>, power_augmentation <lgl>, animal_attributes <lgl>,
## #   longevity <lgl>, intelligence <lgl>, super_strength <lgl>,
## #   cryokinesis <lgl>, telepathy <lgl>, energy_armor <lgl>,
## #   energy_blasts <lgl>, duplication <lgl>, size_changing <lgl>,
## #   density_control <lgl>, stamina <lgl>, astral_travel <lgl>,
## #   audio_control <lgl>, dexterity <lgl>, omnitrix <lgl>, super_speed <lgl>,
## #   possession <lgl>, animal_oriented_powers <lgl>, weapon_based_powers <lgl>,
## #   electrokinesis <lgl>, darkforce_manipulation <lgl>, death_touch <lgl>,
## #   teleportation <lgl>, enhanced_senses <lgl>, telekinesis <lgl>,
## #   energy_beams <lgl>, magic <lgl>, hyperkinesis <lgl>, jump <lgl>,
## #   clairvoyance <lgl>, dimensional_travel <lgl>, power_sense <lgl>,
## #   shapeshifting <lgl>, peak_human_condition <lgl>, immortality <lgl>,
## #   camouflage <lgl>, element_control <lgl>, phasing <lgl>,
## #   astral_projection <lgl>, electrical_transport <lgl>, fire_control <lgl>,
## #   projection <lgl>, summoning <lgl>, enhanced_memory <lgl>, reflexes <lgl>,
## #   invulnerability <lgl>, energy_constructs <lgl>, force_fields <lgl>,
## #   self_sustenance <lgl>, anti_gravity <lgl>, empathy <lgl>,
## #   power_nullifier <lgl>, radiation_control <lgl>, psionic_powers <lgl>,
## #   elasticity <lgl>, substance_secretion <lgl>,
## #   elemental_transmogrification <lgl>, technopath_cyberpath <lgl>,
## #   photographic_reflexes <lgl>, seismic_power <lgl>, animation <lgl>,
## #   precognition <lgl>, mind_control <lgl>, fire_resistance <lgl>,
## #   power_absorption <lgl>, enhanced_hearing <lgl>, nova_force <lgl>,
## #   insanity <lgl>, hypnokinesis <lgl>, animal_control <lgl>,
## #   natural_armor <lgl>, intangibility <lgl>, enhanced_sight <lgl>,
## #   molecular_manipulation <lgl>, heat_generation <lgl>, adaptation <lgl>,
## #   gliding <lgl>, power_suit <lgl>, mind_blast <lgl>,
## #   probability_manipulation <lgl>, gravity_control <lgl>, regeneration <lgl>,
## #   light_control <lgl>, echolocation <lgl>, levitation <lgl>,
## #   toxin_and_disease_control <lgl>, banish <lgl>, energy_manipulation <lgl>,
## #   heat_resistance <lgl>, ...
```

</div>

16. Pick your favorite superhero and let's see their powers!

```r
superhero_powers %>%
  filter(hero_names == "Abraxas")
```

```
## # A tibble: 1 x 168
##   hero_names agility accelerated_hea~ lantern_power_r~ dimensional_awa~
##   <chr>      <lgl>   <lgl>            <lgl>            <lgl>           
## 1 Abraxas    FALSE   FALSE            FALSE            TRUE            
## # ... with 163 more variables: cold_resistance <lgl>, durability <lgl>,
## #   stealth <lgl>, energy_absorption <lgl>, flight <lgl>, danger_sense <lgl>,
## #   underwater_breathing <lgl>, marksmanship <lgl>, weapons_master <lgl>,
## #   power_augmentation <lgl>, animal_attributes <lgl>, longevity <lgl>,
## #   intelligence <lgl>, super_strength <lgl>, cryokinesis <lgl>,
## #   telepathy <lgl>, energy_armor <lgl>, energy_blasts <lgl>,
## #   duplication <lgl>, size_changing <lgl>, density_control <lgl>,
## #   stamina <lgl>, astral_travel <lgl>, audio_control <lgl>, dexterity <lgl>,
## #   omnitrix <lgl>, super_speed <lgl>, possession <lgl>,
## #   animal_oriented_powers <lgl>, weapon_based_powers <lgl>,
## #   electrokinesis <lgl>, darkforce_manipulation <lgl>, death_touch <lgl>,
## #   teleportation <lgl>, enhanced_senses <lgl>, telekinesis <lgl>,
## #   energy_beams <lgl>, magic <lgl>, hyperkinesis <lgl>, jump <lgl>,
## #   clairvoyance <lgl>, dimensional_travel <lgl>, power_sense <lgl>,
## #   shapeshifting <lgl>, peak_human_condition <lgl>, immortality <lgl>,
## #   camouflage <lgl>, element_control <lgl>, phasing <lgl>,
## #   astral_projection <lgl>, electrical_transport <lgl>, fire_control <lgl>,
## #   projection <lgl>, summoning <lgl>, enhanced_memory <lgl>, reflexes <lgl>,
## #   invulnerability <lgl>, energy_constructs <lgl>, force_fields <lgl>,
## #   self_sustenance <lgl>, anti_gravity <lgl>, empathy <lgl>,
## #   power_nullifier <lgl>, radiation_control <lgl>, psionic_powers <lgl>,
## #   elasticity <lgl>, substance_secretion <lgl>,
## #   elemental_transmogrification <lgl>, technopath_cyberpath <lgl>,
## #   photographic_reflexes <lgl>, seismic_power <lgl>, animation <lgl>,
## #   precognition <lgl>, mind_control <lgl>, fire_resistance <lgl>,
## #   power_absorption <lgl>, enhanced_hearing <lgl>, nova_force <lgl>,
## #   insanity <lgl>, hypnokinesis <lgl>, animal_control <lgl>,
## #   natural_armor <lgl>, intangibility <lgl>, enhanced_sight <lgl>,
## #   molecular_manipulation <lgl>, heat_generation <lgl>, adaptation <lgl>,
## #   gliding <lgl>, power_suit <lgl>, mind_blast <lgl>,
## #   probability_manipulation <lgl>, gravity_control <lgl>, regeneration <lgl>,
## #   light_control <lgl>, echolocation <lgl>, levitation <lgl>,
## #   toxin_and_disease_control <lgl>, banish <lgl>, energy_manipulation <lgl>,
## #   heat_resistance <lgl>, ...
```
