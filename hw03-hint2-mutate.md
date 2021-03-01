hw03-hint2-mutate
================
Will Hall
01/03/2021

``` r
library(tidyverse)
```

    ## ── Attaching packages ──────────────────────────────────────── tidyverse 1.3.0 ──

    ## ✓ ggplot2 3.3.3     ✓ purrr   0.3.4
    ## ✓ tibble  3.0.5     ✓ dplyr   1.0.3
    ## ✓ tidyr   1.1.2     ✓ stringr 1.4.0
    ## ✓ readr   1.4.0     ✓ forcats 0.5.0

    ## ── Conflicts ─────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
data_df <- 
    tibble(ps_id = 1:15, 
                 q1 = sample(c("Strongly Agree", 
                                            "Agree",
                                            "Undecided",
                                            "Disagree",
                                            "Strongly Disagree"), 1, size = 15), 
                 q2 = sample(c("yes", "no"), 1, size =15))


data_df %>% 
    pivot_longer(-ps_id) %>% 
    mutate(value = case_when(name == "q1" & value == "Strongly Agree" ~ 5, 
                                                     name == "q1" & value == "Agree" ~ 4, 
                                                     name == "q1" & value == "Undecided" ~ 3,
                                                     name == "q1" & value == "Disagree" ~ 2,
                                                     name == "q1" & value == "Strongly Disagree" ~ 1, 
                                                     name == "q2" & value == "yes" ~ 1, 
                                                     name == "q2" & value == "no" ~ 0))
```

<div class="kable-table">

| ps\_id | name | value |
|-------:|:-----|------:|
|      1 | q1   |     1 |
|      1 | q2   |     1 |
|      2 | q1   |     3 |
|      2 | q2   |     0 |
|      3 | q1   |     1 |
|      3 | q2   |     1 |
|      4 | q1   |     2 |
|      4 | q2   |     0 |
|      5 | q1   |     4 |
|      5 | q2   |     0 |
|      6 | q1   |     3 |
|      6 | q2   |     1 |
|      7 | q1   |     5 |
|      7 | q2   |     1 |
|      8 | q1   |     4 |
|      8 | q2   |     1 |
|      9 | q1   |     3 |
|      9 | q2   |     0 |
|     10 | q1   |     4 |
|     10 | q2   |     0 |
|     11 | q1   |     5 |
|     11 | q2   |     0 |
|     12 | q1   |     3 |
|     12 | q2   |     0 |
|     13 | q1   |     1 |
|     13 | q2   |     0 |
|     14 | q1   |     4 |
|     14 | q2   |     0 |
|     15 | q1   |     5 |
|     15 | q2   |     1 |

</div>
