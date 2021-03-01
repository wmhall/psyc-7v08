hw03-hint
================
Will Hall
01/03/2021

### HW03 hint

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
q1_coding_df <- 
    tibble(q = c("Strongly Agree", 
                                "Agree",
                                "Undecided",
                                "Disagree",
                                "Strongly Disagree"), 
                 q_num = 5:1)

q2_coding_df <- 
    tibble(q = c("yes", "no"), 
                 q_num = c(0, 1))

coding_df <- 
    bind_rows(q1 = q1_coding_df, q2 = q2_coding_df, .id = "name") %>% 
    rename(value = q)

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
    left_join(coding_df) %>% 
    select(-value) %>% 
    pivot_wider(names_from = name, values_from = q_num)
```

    ## Joining, by = c("name", "value")

<div class="kable-table">

| ps\_id |  q1 |  q2 |
|-------:|----:|----:|
|      1 |   4 |   1 |
|      2 |   5 |   0 |
|      3 |   1 |   1 |
|      4 |   4 |   1 |
|      5 |   5 |   1 |
|      6 |   2 |   0 |
|      7 |   1 |   1 |
|      8 |   1 |   0 |
|      9 |   3 |   0 |
|     10 |   3 |   0 |
|     11 |   1 |   1 |
|     12 |   5 |   1 |
|     13 |   4 |   0 |
|     14 |   2 |   0 |
|     15 |   4 |   0 |

</div>
