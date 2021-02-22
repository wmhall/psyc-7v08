Lab 05
================

-   [`tidyr`’s pivot functions](#tidyrs-pivot-functions)
    -   [`relig_income` data](#relig_income-data)
    -   [`us_rent_income` dateset](#us_rent_income-dateset)
-   [Relational data](#relational-data)
    -   [The data](#the-data)
    -   [Mutating joins](#mutating-joins)
        -   [left\_join(superheroes,
            publishers)](#left_joinsuperheroes-publishers)
        -   [inner\_join(superheroes,
            publishers)](#inner_joinsuperheroes-publishers)
        -   [left\_join(publishers,
            superheroes)](#left_joinpublishers-superheroes)
        -   [inner\_join(publishers,
            superheroes)](#inner_joinpublishers-superheroes)
        -   [full\_join(superheroes,
            publishers)](#full_joinsuperheroes-publishers)
    -   [Filtering joins](#filtering-joins)
        -   [semi\_join(superheroes,
            publishers)](#semi_joinsuperheroes-publishers)
        -   [anti\_join(superheroes,
            publishers)](#anti_joinsuperheroes-publishers)
        -   [semi\_join(publishers,
            superheroes)](#semi_joinpublishers-superheroes)
        -   [anti\_join(publishers,
            superheroes)](#anti_joinpublishers-superheroes)
-   [But I want to do more!](#but-i-want-to-do-more)

## `tidyr`’s pivot functions

### `relig_income` data

The `relig_income` dataset stores counts based on a survey that asked
people about their religion and annual income. You have access to the
dataset via the `tidyr` package. If you load the `tidyverse` and type
`relig_income`, you will see the data. To learn more about the dataset
type `?relig_income`.

``` r
library(tidyverse)

relig_income %>% 
  head(10)
```

<div class="kable-table">

| religion                | &lt;$10k | $10-20k | $20-30k | $30-40k | $40-50k | $50-75k | $75-100k | $100-150k | &gt;150k | Don’t know/refused |
|:------------------------|---------:|--------:|--------:|--------:|--------:|--------:|---------:|----------:|---------:|-------------------:|
| Agnostic                |       27 |      34 |      60 |      81 |      76 |     137 |      122 |       109 |       84 |                 96 |
| Atheist                 |       12 |      27 |      37 |      52 |      35 |      70 |       73 |        59 |       74 |                 76 |
| Buddhist                |       27 |      21 |      30 |      34 |      33 |      58 |       62 |        39 |       53 |                 54 |
| Catholic                |      418 |     617 |     732 |     670 |     638 |    1116 |      949 |       792 |      633 |               1489 |
| Don’t know/refused      |       15 |      14 |      15 |      11 |      10 |      35 |       21 |        17 |       18 |                116 |
| Evangelical Prot        |      575 |     869 |    1064 |     982 |     881 |    1486 |      949 |       723 |      414 |               1529 |
| Hindu                   |        1 |       9 |       7 |       9 |      11 |      34 |       47 |        48 |       54 |                 37 |
| Historically Black Prot |      228 |     244 |     236 |     238 |     197 |     223 |      131 |        81 |       78 |                339 |
| Jehovah’s Witness       |       20 |      27 |      24 |      24 |      21 |      30 |       15 |        11 |        6 |                 37 |
| Jewish                  |       19 |      19 |      25 |      25 |      30 |      95 |       69 |        87 |      151 |                162 |

</div>

We will use the `relig_income` dataset to practice `tidyr`’s `pivot`
functions.

**Pick a religion and use a `pivot` function to help you generate a plot
that visualizes the counts for the each income bracket.**

I picked Buddhists and made this plot. Can you make a similar plot?

<img src="/Users/wmh/Documents/Documents - William’s MacBook Air/class/r/psyc-7v08/budhist-income.png" width="2099" />

**Next, find out which religion has the highest count in the top income
bracket.**

### `us_rent_income` dateset

The `us_rent_income` dataset is included as part of the `tidyr` package.
It contains information about median income and rent for each state in
the US for 2017.

To see the data type `us_rent_income`. To learn more about the package,
type `?us_rent_income`

**Using a `pivot` to help you along the way, find out which state has
the largest difference between income and rent?**

**Stretch goal: create a plot visualizing the income data, including the
90% margin of error in the plot. Here’s one I made earlier. Can you make
something similar?**

<img src="/Users/wmh/Documents/Documents - William’s MacBook Air/class/r/psyc-7v08/state-income.png" width="2487" />

## Relational data

### The data

To demonstrate the different joins we will work with two small data
frames: `superheroes` and `publishers.` Copy the code below into your
RStudio code window and run it to get the `superheroes` and `publishers`
data frames.

``` r
library(tidyverse)

superheroes <- tibble::tribble(
       ~name, ~alignment,  ~gender,          ~publisher,
   "Magneto",      "bad",   "male",            "Marvel",
     "Storm",     "good", "female",            "Marvel",
  "Mystique",      "bad", "female",            "Marvel",
    "Batman",     "good",   "male",                "DC",
     "Joker",      "bad",   "male",                "DC",
  "Catwoman",      "bad", "female",                "DC",
   "Hellboy",     "good",   "male", "Dark Horse Comics"
  )

publishers <- tibble::tribble(
  ~publisher, ~yr_founded,
        "DC",       1934L,
    "Marvel",       1939L,
     "Image",       1992L
  )
```

Run through all of the code below. Try and anticipate what the join will
do.

### Mutating joins

#### left\_join(superheroes, publishers)

> left\_join(x, y): Return all rows from x, and all columns from x and
> y. If there are multiple matches between x and y, all combination of
> the matches are returned. This is a mutating join.

``` r
left_join(superheroes, publishers)
```

<div class="kable-table">

| name     | alignment | gender | publisher         | yr\_founded |
|:---------|:----------|:-------|:------------------|------------:|
| Magneto  | bad       | male   | Marvel            |        1939 |
| Storm    | good      | female | Marvel            |        1939 |
| Mystique | bad       | female | Marvel            |        1939 |
| Batman   | good      | male   | DC                |        1934 |
| Joker    | bad       | male   | DC                |        1934 |
| Catwoman | bad       | female | DC                |        1934 |
| Hellboy  | good      | male   | Dark Horse Comics |          NA |

</div>

We basically get `x = superheroes` back, but with the addition of
variable `yr_founded`, which is unique to `y = publishers`. Hellboy,
whose publisher does not appear in `y = publishers`, has an `NA` for
`yr_founded`.

#### inner\_join(superheroes, publishers)

> inner\_join(x, y): Return all rows from x where there are matching
> values in y, and all columns from x and y. If there are multiple
> matches between x and y, all combination of the matches are returned.
> This is a mutating join.

``` r
inner_join(superheroes, publishers)
```

<div class="kable-table">

| name     | alignment | gender | publisher | yr\_founded |
|:---------|:----------|:-------|:----------|------------:|
| Magneto  | bad       | male   | Marvel    |        1939 |
| Storm    | good      | female | Marvel    |        1939 |
| Mystique | bad       | female | Marvel    |        1939 |
| Batman   | good      | male   | DC        |        1934 |
| Joker    | bad       | male   | DC        |        1934 |
| Catwoman | bad       | female | DC        |        1934 |

</div>

We lose Hellboy in the join because, although he appears in
`x = superheroes`, his publisher Dark Horse Comics does not appear in
`y = publishers`. The join result has all variables from
`x = superheroes` plus `yr_founded`, from `y`.

#### left\_join(publishers, superheroes)

> left\_join(x, y): Return all rows from x, and all columns from x and
> y. If there are multiple matches between x and y, all combination of
> the matches are returned. This is a mutating join.

``` r
left_join(publishers, superheroes)
```

<div class="kable-table">

| publisher | yr\_founded | name     | alignment | gender |
|:----------|------------:|:---------|:----------|:-------|
| DC        |        1934 | Batman   | good      | male   |
| DC        |        1934 | Joker    | bad       | male   |
| DC        |        1934 | Catwoman | bad       | female |
| Marvel    |        1939 | Magneto  | bad       | male   |
| Marvel    |        1939 | Storm    | good      | female |
| Marvel    |        1939 | Mystique | bad       | female |
| Image     |        1992 | NA       | NA        | NA     |

</div>

We get a similar result as with `inner_join()` but the publisher Image
survives in the join, even though no superheroes from Image appear in
`y = superheroes`. As a result, Image has `NA`s for `name`, `alignment`,
and `gender`.

#### inner\_join(publishers, superheroes)

> inner\_join(x, y): Return all rows from x where there are matching
> values in y, and all columns from x and y. If there are multiple
> matches between x and y, all combination of the matches are returned.
> This is a mutating join.

``` r
inner_join(publishers, superheroes)
```

<div class="kable-table">

| publisher | yr\_founded | name     | alignment | gender |
|:----------|------------:|:---------|:----------|:-------|
| DC        |        1934 | Batman   | good      | male   |
| DC        |        1934 | Joker    | bad       | male   |
| DC        |        1934 | Catwoman | bad       | female |
| Marvel    |        1939 | Magneto  | bad       | male   |
| Marvel    |        1939 | Storm    | good      | female |
| Marvel    |        1939 | Mystique | bad       | female |

</div>

Every publisher that has a match in `y = superheroes` appears multiple
times in the result, once for each match. In fact, we’re getting the
same result as with `inner_join(superheroes, publishers)`, up to
variable order (which you should also never rely on in an analysis).

#### full\_join(superheroes, publishers)

> full\_join(x, y): Return all rows and all columns from both x and y.
> Where there are not matching values, returns NA for the one missing.
> This is a mutating join.

``` r
full_join(superheroes, publishers)
```

<div class="kable-table">

| name     | alignment | gender | publisher         | yr\_founded |
|:---------|:----------|:-------|:------------------|------------:|
| Magneto  | bad       | male   | Marvel            |        1939 |
| Storm    | good      | female | Marvel            |        1939 |
| Mystique | bad       | female | Marvel            |        1939 |
| Batman   | good      | male   | DC                |        1934 |
| Joker    | bad       | male   | DC                |        1934 |
| Catwoman | bad       | female | DC                |        1934 |
| Hellboy  | good      | male   | Dark Horse Comics |          NA |
| NA       | NA        | NA     | Image             |        1992 |

</div>

We get all rows of `x = superheroes` plus a new row from
`y = publishers`, containing the publisher Image. We get all variables
from `x = superheroes` AND all variables from `y = publishers`. Any row
that derives solely from one table or the other carries `NA`s in the
variables found only in the other table.

### Filtering joins

#### semi\_join(superheroes, publishers)

> semi\_join(x, y): Return all rows from x where there are matching
> values in y, keeping just columns from x. A semi join differs from an
> inner join because an inner join will return one row of x for each
> matching row of y, where a semi join will never duplicate rows of x.
> This is a filtering join.

``` r
semi_join(superheroes, publishers)
```

<div class="kable-table">

| name     | alignment | gender | publisher |
|:---------|:----------|:-------|:----------|
| Magneto  | bad       | male   | Marvel    |
| Storm    | good      | female | Marvel    |
| Mystique | bad       | female | Marvel    |
| Batman   | good      | male   | DC        |
| Joker    | bad       | male   | DC        |
| Catwoman | bad       | female | DC        |

</div>

We get a similar result as with `inner_join()` but the join result
contains only the variables originally found in `x = superheroes`. But
note the row order has changed.

#### anti\_join(superheroes, publishers)

> anti\_join(x, y): Return all rows from x where there are not matching
> values in y, keeping just columns from x. This is a filtering join.

``` r
anti_join(superheroes, publishers)
```

<div class="kable-table">

| name    | alignment | gender | publisher         |
|:--------|:----------|:-------|:------------------|
| Hellboy | good      | male   | Dark Horse Comics |

</div>

We keep **only** Hellboy now (and do not get `yr_founded`).

#### semi\_join(publishers, superheroes)

> semi\_join(x, y): Return all rows from x where there are matching
> values in y, keeping just columns from x. A semi join differs from an
> inner join because an inner join will return one row of x for each
> matching row of y, where a semi join will never duplicate rows of x.
> This is a filtering join.

``` r
semi_join(x = publishers, y = superheroes)
```

<div class="kable-table">

| publisher | yr\_founded |
|:----------|------------:|
| DC        |        1934 |
| Marvel    |        1939 |

</div>

Now the effects of switching the `x` and `y` roles is more clear. The
result resembles `x = publishers`, but the publisher Image is lost,
because there are no observations where `publisher == "Image"` in
`y = superheroes`.

#### anti\_join(publishers, superheroes)

> anti\_join(x, y): Return all rows from x where there are not matching
> values in y, keeping just columns from x. This is a filtering join.

``` r
anti_join(publishers, superheroes)
```

<div class="kable-table">

| publisher | yr\_founded |
|:----------|------------:|
| Image     |        1992 |

</div>

We keep **only** publisher Image now (and the variables found in
`x = publishers`).

## But I want to do more!

Start working on [hw03](hw03.md).
