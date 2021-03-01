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
                                                     name == "q2" & value == "no" ~ 0)) %>% 
    pivot_wider(names_from = name, values_from = value)
```

<div class="kable-table">

| ps\_id |  q1 |  q2 |
|-------:|----:|----:|
|      1 |   3 |   1 |
|      2 |   4 |   0 |
|      3 |   4 |   1 |
|      4 |   3 |   1 |
|      5 |   1 |   1 |
|      6 |   4 |   0 |
|      7 |   2 |   1 |
|      8 |   5 |   0 |
|      9 |   4 |   1 |
|     10 |   3 |   1 |
|     11 |   4 |   0 |
|     12 |   5 |   0 |
|     13 |   1 |   1 |
|     14 |   4 |   0 |
|     15 |   2 |   0 |

</div>
