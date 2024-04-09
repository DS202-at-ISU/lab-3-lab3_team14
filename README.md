
<!-- README.md is generated from README.Rmd. Please edit the README.Rmd file -->

# Lab report \#3 - instructions

Follow the instructions posted at
<https://ds202-at-isu.github.io/labs.html> for the lab assignment. The
work is meant to be finished during the lab time, but you have time
until Monday evening to polish things.

Include your answers in this document (Rmd file). Make sure that it
knits properly (into the md file). Upload both the Rmd and the md file
to your repository.

All submissions to the github repo will be automatically uploaded for
grading once the due date is passed. Submit a link to your repository on
Canvas (only one submission per team) to signal to the instructors that
you are done with your submission.

# Lab 3: Avenger’s Peril

## As a team

Extract from the data below two data sets in long form `deaths` and
`returns`

``` r
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ forcats   1.0.0     ✔ readr     2.1.5
    ## ✔ ggplot2   3.4.4     ✔ stringr   1.5.1
    ## ✔ lubridate 1.9.3     ✔ tibble    3.2.1
    ## ✔ purrr     1.0.2     ✔ tidyr     1.3.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
av <- read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/avengers/avengers.csv", stringsAsFactors = FALSE)
head(av)
```

    ##                                                       URL
    ## 1           http://marvel.wikia.com/Henry_Pym_(Earth-616)
    ## 2      http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)
    ## 3       http://marvel.wikia.com/Anthony_Stark_(Earth-616)
    ## 4 http://marvel.wikia.com/Robert_Bruce_Banner_(Earth-616)
    ## 5        http://marvel.wikia.com/Thor_Odinson_(Earth-616)
    ## 6       http://marvel.wikia.com/Richard_Jones_(Earth-616)
    ##                    Name.Alias Appearances Current. Gender Probationary.Introl
    ## 1   Henry Jonathan "Hank" Pym        1269      YES   MALE                    
    ## 2              Janet van Dyne        1165      YES FEMALE                    
    ## 3 Anthony Edward "Tony" Stark        3068      YES   MALE                    
    ## 4         Robert Bruce Banner        2089      YES   MALE                    
    ## 5                Thor Odinson        2402      YES   MALE                    
    ## 6      Richard Milhouse Jones         612      YES   MALE                    
    ##   Full.Reserve.Avengers.Intro Year Years.since.joining Honorary Death1 Return1
    ## 1                      Sep-63 1963                  52     Full    YES      NO
    ## 2                      Sep-63 1963                  52     Full    YES     YES
    ## 3                      Sep-63 1963                  52     Full    YES     YES
    ## 4                      Sep-63 1963                  52     Full    YES     YES
    ## 5                      Sep-63 1963                  52     Full    YES     YES
    ## 6                      Sep-63 1963                  52 Honorary     NO        
    ##   Death2 Return2 Death3 Return3 Death4 Return4 Death5 Return5
    ## 1                                                            
    ## 2                                                            
    ## 3                                                            
    ## 4                                                            
    ## 5    YES      NO                                             
    ## 6                                                            
    ##                                                                                                                                                                              Notes
    ## 1                                                                                                                Merged with Ultron in Rage of Ultron Vol. 1. A funeral was held. 
    ## 2                                                                                                  Dies in Secret Invasion V1:I8. Actually was sent tto Microverse later recovered
    ## 3 Death: "Later while under the influence of Immortus Stark committed a number of horrible acts and was killed.'  This set up young Tony. Franklin Richards later brought him back
    ## 4                                                                               Dies in Ghosts of the Future arc. However "he had actually used a hidden Pantheon base to survive"
    ## 5                                                      Dies in Fear Itself brought back because that's kind of the whole point. Second death in Time Runs Out has not yet returned
    ## 6                                                                                                                                                                             <NA>

Get the data into a format where the five columns for Death\[1-5\] are
replaced by two columns: Time, and Death. Time should be a number
between 1 and 5 (look into the function `parse_number`); Death is a
categorical variables with values “yes”, “no” and ““. Call the resulting
data set `deaths`.

``` r
deaths <- av |>
  pivot_longer(cols = starts_with("Death"),
               names_to = "Time",
               values_to = "Death") |>
  mutate(Time = parse_number(str_remove(Time,"Death")))
#Average number of deaths
sum(deaths$Death == "YES")/length(unique(av$Name.Alias))
```

    ## [1] 0.5460123

``` r
#Joshua Morningstar Individual Part
#Out of 173 avengers, 69 died at least one time
firstDeath <- filter(deaths, Time == 1)
sum(firstDeath$Death == "YES")
```

    ## [1] 69

``` r
#This is proven true, it gives 69
```

Similarly, deal with the returns of characters.

``` r
returns <- av |>
  pivot_longer(cols = starts_with("Return"),
               names_to = "Time",
               values_to = "Return") |>
  mutate(TimeReturn = parse_number(str_remove(Time,"Return")))
```

Based on these datasets calculate the average number of deaths an
Avenger suffers.

**An avenger suffers an average of 0.546 deaths**

## Individually

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

### FiveThirtyEight Statement

> Quote the statement you are planning to fact-check.

### Include the code

Make sure to include the code to derive the (numeric) fact for the
statement

### Include your answer

Include at least one sentence discussing the result of your
fact-checking endeavor.

Upload your changes to the repository. Discuss and refine answers as a
team.

### Aidan Bobholz

Statement: Out of 173 listed Avengers, my analysis found that 69 had
died at least one time after they joined the team.

``` r
one_death <- av %>% 
  count(av$Death1 == "YES")

print(one_death)
```

    ##   av$Death1 == "YES"   n
    ## 1              FALSE 104
    ## 2               TRUE  69

*Running this we can see that indeed 69 of the 173 Avengers had died
atleast once proving the statement to be true.*

Vanessa:

“I counted 89 total deaths — some unlucky Avengers7 are basically Meat
Loaf with an E-ZPass”

``` r
totalDeaths <- sum(
  av |>
    filter(Death1 == "YES") |>
    nrow(),
  av |>
    filter(Death2 == "YES") |>
    nrow(),
  av |>
    filter(Death3 == "YES") |>
    nrow(),
  av |>
    filter(Death4 == "YES") |>
    nrow(),
  av |>
    filter(Death5 == "YES") |>
    nrow())
totalDeaths
```

    ## [1] 89

There are 89 total deaths, so the statement is true.

### Luis Hinkhouse

Statement: “In fact, Hawkeye died twice!”

``` r
hawkeye_deaths1 <- av %>%
  count(av$Name.Alias == 'Clinton Francis Barton' & av$Death1 == 'YES')
print(hawkeye_deaths1)
```

    ##   av$Name.Alias == "Clinton Francis Barton" & av$Death1 == "YES"   n
    ## 1                                                          FALSE 172
    ## 2                                                           TRUE   1

``` r
hawkeye_deaths2 <- av %>%
  count(av$Name.Alias == 'Clinton Francis Barton' & av$Death2 == 'YES')
print(hawkeye_deaths2)
```

    ##   av$Name.Alias == "Clinton Francis Barton" & av$Death2 == "YES"   n
    ## 1                                                          FALSE 172
    ## 2                                                           TRUE   1

``` r
hawkeye_deaths3 <- av %>%
  count(av$Name.Alias == 'Clinton Francis Barton' & av$Death3 == 'YES')
print(hawkeye_deaths3)
```

    ##   av$Name.Alias == "Clinton Francis Barton" & av$Death3 == "YES"   n
    ## 1                                                          FALSE 173

As you can see the statement is True. We can see that there is a true
for both hawkeye has died at least 1 time and 2 times, but not 3 times.
