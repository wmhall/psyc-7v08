Lab 03: Introduction to dplyr
================

-   [Before you start](#before-you-start)
-   [Intro](#intro)
    -   [Load `tidyverse` and
        `gapminder`](#load-tidyverse-and-gapminder)
    -   [Load the Gapminder data](#load-the-gapminder-data)
-   [Use `filter()` to subset data
    row-wise.](#use-filter-to-subset-data-row-wise.)
-   [Meet the new pipe operator](#meet-the-new-pipe-operator)
-   [Use `select()` to subset the data on variables or
    columns.](#use-select-to-subset-the-data-on-variables-or-columns.)
-   [Revel in the convenience](#revel-in-the-convenience)
-   [Pause to reflect](#pause-to-reflect)
-   [Use `mutate()` to add new
    variables](#use-mutate-to-add-new-variables)
-   [Use `arrange()` to row-order data in a principled
    way](#use-arrange-to-row-order-data-in-a-principled-way)
-   [Use `rename()` to rename
    variables](#use-rename-to-rename-variables)
-   [`group_by()` is a mighty weapon](#group_by-is-a-mighty-weapon)
    -   [Counting things up](#counting-things-up)
    -   [General summarization](#general-summarization)
-   [But I want to do more!](#but-i-want-to-do-more)
-   [Resources](#resources)

### Before you start

If you haven’t worked through [lab
1](https://github.com/wmhall/psyc-7v08/blob/main/posted/labs/lab-01/lab-01-getting-started.md)
and [lab 2](lab-02.md), go ahead and finish those first.

### Intro

`dplyr` is a `tidyverse` package for data manipulation. It is built to
be fast while also resulting in highly readable code. It was initially
developed by Hadley Wickham and Romain Francois.

`dplyr`’s roots are in an earlier, still-very-useful package called
[`plyr`](http://plyr.had.co.nz). Where `plyr` covers a diverse set of
inputs and outputs (e.g., arrays, data.frames, lists), `dplyr` has a
laser-like focus on data frames.

#### Load `tidyverse` and `gapminder`

``` r
library(tidyverse)
library(gapminder)
```

#### Load the Gapminder data

Take a quick look at the Gapminder data frame.

``` r
str(gapminder)
```

    ## tibble [1,704 × 6] (S3: tbl_df/tbl/data.frame)
    ##  $ country  : Factor w/ 142 levels "Afghanistan",..: 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ continent: Factor w/ 5 levels "Africa","Americas",..: 3 3 3 3 3 3 3 3 3 3 ...
    ##  $ year     : int [1:1704] 1952 1957 1962 1967 1972 1977 1982 1987 1992 1997 ...
    ##  $ lifeExp  : num [1:1704] 28.8 30.3 32 34 36.1 ...
    ##  $ pop      : int [1:1704] 8425333 9240934 10267083 11537966 13079460 14880372 12881816 13867957 16317921 22227415 ...
    ##  $ gdpPercap: num [1:1704] 779 821 853 836 740 ...

``` r
head(gapminder)
```

    ## # A tibble: 6 x 6
    ##   country     continent  year lifeExp      pop gdpPercap
    ##   <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
    ## 1 Afghanistan Asia       1952    28.8  8425333      779.
    ## 2 Afghanistan Asia       1957    30.3  9240934      821.
    ## 3 Afghanistan Asia       1962    32.0 10267083      853.
    ## 4 Afghanistan Asia       1967    34.0 11537966      836.
    ## 5 Afghanistan Asia       1972    36.1 13079460      740.
    ## 6 Afghanistan Asia       1977    38.4 14880372      786.

### Use `filter()` to subset data row-wise.

`filter()` takes logical expressions and returns the rows for which all
are `TRUE`.

``` r
filter(gapminder, lifeExp < 29)
```

    ## # A tibble: 2 x 6
    ##   country     continent  year lifeExp     pop gdpPercap
    ##   <fct>       <fct>     <int>   <dbl>   <int>     <dbl>
    ## 1 Afghanistan Asia       1952    28.8 8425333      779.
    ## 2 Rwanda      Africa     1992    23.6 7290203      737.

``` r
filter(gapminder, country == "Rwanda")
```

    ## # A tibble: 12 x 6
    ##    country continent  year lifeExp     pop gdpPercap
    ##    <fct>   <fct>     <int>   <dbl>   <int>     <dbl>
    ##  1 Rwanda  Africa     1952    40   2534927      493.
    ##  2 Rwanda  Africa     1957    41.5 2822082      540.
    ##  3 Rwanda  Africa     1962    43   3051242      597.
    ##  4 Rwanda  Africa     1967    44.1 3451079      511.
    ##  5 Rwanda  Africa     1972    44.6 3992121      591.
    ##  6 Rwanda  Africa     1977    45   4657072      670.
    ##  7 Rwanda  Africa     1982    46.2 5507565      882.
    ##  8 Rwanda  Africa     1987    44.0 6349365      848.
    ##  9 Rwanda  Africa     1992    23.6 7290203      737.
    ## 10 Rwanda  Africa     1997    36.1 7212583      590.
    ## 11 Rwanda  Africa     2002    43.4 7852401      786.
    ## 12 Rwanda  Africa     2007    46.2 8860588      863.

``` r
filter(gapminder, country %in% c("Rwanda", "Afghanistan"))
```

    ## # A tibble: 24 x 6
    ##    country     continent  year lifeExp      pop gdpPercap
    ##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
    ##  1 Afghanistan Asia       1952    28.8  8425333      779.
    ##  2 Afghanistan Asia       1957    30.3  9240934      821.
    ##  3 Afghanistan Asia       1962    32.0 10267083      853.
    ##  4 Afghanistan Asia       1967    34.0 11537966      836.
    ##  5 Afghanistan Asia       1972    36.1 13079460      740.
    ##  6 Afghanistan Asia       1977    38.4 14880372      786.
    ##  7 Afghanistan Asia       1982    39.9 12881816      978.
    ##  8 Afghanistan Asia       1987    40.8 13867957      852.
    ##  9 Afghanistan Asia       1992    41.7 16317921      649.
    ## 10 Afghanistan Asia       1997    41.8 22227415      635.
    ## # … with 14 more rows

Compare with some base R code to accomplish the same things

``` r
gapminder[gapminder$lifeExp < 29, ] ## repeat `gapminder`, [i, j] indexing is distracting
subset(gapminder, country == "Rwanda") ## almost same as filter ... but wait ...
```

### Meet the new pipe operator

Before we go any further, we should exploit the new pipe operator that
`dplyr` imports from the
[`magrittr`](https://magrittr.tidyverse.org/articles/magrittr.html)
package. This is going to change your data analytical life. You no
longer need to enact multi-operation commands by nesting them inside
each other, like so many [Russian nesting
dolls](https://upload.wikimedia.org/wikipedia/commons/thumb/4/41/Floral_matryoshka_set_2_smallest_doll_nested.JPG/1024px-Floral_matryoshka_set_2_smallest_doll_nested.JPG).
This new syntax leads to code that is much easier to write and to read.

Here’s what it looks like: `%>%`. The RStudio keyboard shortcut: Ctrl +
Shift + M (Windows), Cmd + Shift + M (Mac).

Let’s demo then I’ll explain:

``` r
gapminder %>% head
```

    ## # A tibble: 6 x 6
    ##   country     continent  year lifeExp      pop gdpPercap
    ##   <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
    ## 1 Afghanistan Asia       1952    28.8  8425333      779.
    ## 2 Afghanistan Asia       1957    30.3  9240934      821.
    ## 3 Afghanistan Asia       1962    32.0 10267083      853.
    ## 4 Afghanistan Asia       1967    34.0 11537966      836.
    ## 5 Afghanistan Asia       1972    36.1 13079460      740.
    ## 6 Afghanistan Asia       1977    38.4 14880372      786.

This is equivalent to `head(gapminder)`. This pipe operator takes the
thing on the left-hand-side and **pipes** it into the function call on
the right-hand-side – literally, drops it in as the first argument.

Never fear, you can still specify other arguments to this function! To
see the first 3 rows of Gapminder, we could say `head(gapminder, 3)` or
this:

``` r
gapminder %>% head(3)
```

    ## # A tibble: 3 x 6
    ##   country     continent  year lifeExp      pop gdpPercap
    ##   <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
    ## 1 Afghanistan Asia       1952    28.8  8425333      779.
    ## 2 Afghanistan Asia       1957    30.3  9240934      821.
    ## 3 Afghanistan Asia       1962    32.0 10267083      853.

Whenever you see the assignment operator (`<-`), you might think to your
self “gets”. Similarly, you should think “and then”, whenever you see
the pipe operator, `%>%`.

You are probably not impressed yet, but the magic will soon happen.

### Use `select()` to subset the data on variables or columns.

Back to `dplyr` …

Use `select()` to subset the data on variables or columns. Here’s a
conventional call:

``` r
select(gapminder, year, lifeExp) ## tbl_df prevents TMI from printing
```

    ## # A tibble: 1,704 x 2
    ##     year lifeExp
    ##    <int>   <dbl>
    ##  1  1952    28.8
    ##  2  1957    30.3
    ##  3  1962    32.0
    ##  4  1967    34.0
    ##  5  1972    36.1
    ##  6  1977    38.4
    ##  7  1982    39.9
    ##  8  1987    40.8
    ##  9  1992    41.7
    ## 10  1997    41.8
    ## # … with 1,694 more rows

And here’s a similar operation, but written with the pipe operator and
piped through `head`:

``` r
gapminder %>%
  select(year, lifeExp) %>%
  head(4)
```

    ## # A tibble: 4 x 2
    ##    year lifeExp
    ##   <int>   <dbl>
    ## 1  1952    28.8
    ## 2  1957    30.3
    ## 3  1962    32.0
    ## 4  1967    34.0

Think: “Take `gapminder`, *and then* select the variables year and
lifeExp, *and then* show the first 4 rows.”

### Revel in the convenience

Here’s the data for Canada, but only certain variables:

``` r
gapminder %>%
  filter(country == "Canada") %>%
  select(year, lifeExp)
```

    ## # A tibble: 12 x 2
    ##     year lifeExp
    ##    <int>   <dbl>
    ##  1  1952    68.8
    ##  2  1957    70.0
    ##  3  1962    71.3
    ##  4  1967    72.1
    ##  5  1972    72.9
    ##  6  1977    74.2
    ##  7  1982    75.8
    ##  8  1987    76.9
    ##  9  1992    78.0
    ## 10  1997    78.6
    ## 11  2002    79.8
    ## 12  2007    80.7

and what a typical base R call would look like:

``` r
gapminder[gapminder$country == "Canada", c("year", "lifeExp")]
```

    ## # A tibble: 12 x 2
    ##     year lifeExp
    ##    <int>   <dbl>
    ##  1  1952    68.8
    ##  2  1957    70.0
    ##  3  1962    71.3
    ##  4  1967    72.1
    ##  5  1972    72.9
    ##  6  1977    74.2
    ##  7  1982    75.8
    ##  8  1987    76.9
    ##  9  1992    78.0
    ## 10  1997    78.6
    ## 11  2002    79.8
    ## 12  2007    80.7

or, possibly?, a nicer look using base R’s `subset()` function:

``` r
subset(gapminder, country == "Canada", select = c(year, lifeExp))
```

    ## # A tibble: 12 x 2
    ##     year lifeExp
    ##    <int>   <dbl>
    ##  1  1952    68.8
    ##  2  1957    70.0
    ##  3  1962    71.3
    ##  4  1967    72.1
    ##  5  1972    72.9
    ##  6  1977    74.2
    ##  7  1982    75.8
    ##  8  1987    76.9
    ##  9  1992    78.0
    ## 10  1997    78.6
    ## 11  2002    79.8
    ## 12  2007    80.7

### Pause to reflect

We’ve barely scratched the surface of `dplyr` but I want to point out
something you may start to appreciate: These functions take the same
flavor of object as input and output. Namely, a `data.frame` or one of
the other data receptacles `dplyr` supports. And finally, the data is
**always** the very first argument of the verb functions.

This set of deliberate design choices, together with the pipe operator,
produces a highly effective, low friction tools for data analysis.

### Use `mutate()` to add new variables

Imagine we wanted to recover each country’s GDP. After all, the
Gapminder data has a variable for population and GDP per capita. Let’s
multiply them together.

``` r
gapminder_gdp <- gapminder %>%
  mutate(gdp = pop * gdpPercap)
```

Hmmmm … those GDP numbers are almost uselessly large and abstract.
Consider the [advice of Randall Munroe of
xkcd](http://fivethirtyeight.com/datalab/xkcd-randall-munroe-qanda-what-if/):
“One thing that bothers me is large numbers presented without context…
‘If I added a zero to this number, would the sentence containing it mean
something different to me?’ If the answer is ‘no,’ maybe the number has
no business being in the sentence in the first place.”

Maybe it would be more meaningful to report GDP per capita in billions.
Lets give it a try.

``` r
gapminder_gdp_billions <- 
gapminder %>%
  mutate(gdp_billion = gdpPercap*pop/10^9)
```

### Use `arrange()` to row-order data in a principled way

Imagine you wanted this data ordered by the variable you just made.

``` r
gapminder_gdp_billions %>%
  arrange(gdp_billion)
```

    ## # A tibble: 1,704 x 7
    ##    country               continent  year lifeExp    pop gdpPercap gdp_billion
    ##    <fct>                 <fct>     <int>   <dbl>  <int>     <dbl>       <dbl>
    ##  1 Sao Tome and Principe Africa     1952    46.5  60011      880.      0.0528
    ##  2 Sao Tome and Principe Africa     1957    48.9  61325      861.      0.0528
    ##  3 Sao Tome and Principe Africa     1962    51.9  65345     1072.      0.0700
    ##  4 Equatorial Guinea     Africa     1952    34.5 216964      376.      0.0815
    ##  5 Sao Tome and Principe Africa     1967    54.4  70787     1385.      0.0980
    ##  6 Equatorial Guinea     Africa     1957    36.0 232922      426.      0.0992
    ##  7 Sao Tome and Principe Africa     1972    56.5  76595     1533.      0.117 
    ##  8 Gambia                Africa     1952    30   284320      485.      0.138 
    ##  9 Equatorial Guinea     Africa     1962    37.5 249220      583.      0.145 
    ## 10 Sao Tome and Principe Africa     1977    58.6  86796     1738.      0.151 
    ## # … with 1,694 more rows

Or maybe you want just the data from 2007, sorted on life expectancy?

``` r
gapminder %>%
  filter(year == 2007) %>%
  arrange(lifeExp)
```

    ## # A tibble: 142 x 6
    ##    country                  continent  year lifeExp      pop gdpPercap
    ##    <fct>                    <fct>     <int>   <dbl>    <int>     <dbl>
    ##  1 Swaziland                Africa     2007    39.6  1133066     4513.
    ##  2 Mozambique               Africa     2007    42.1 19951656      824.
    ##  3 Zambia                   Africa     2007    42.4 11746035     1271.
    ##  4 Sierra Leone             Africa     2007    42.6  6144562      863.
    ##  5 Lesotho                  Africa     2007    42.6  2012649     1569.
    ##  6 Angola                   Africa     2007    42.7 12420476     4797.
    ##  7 Zimbabwe                 Africa     2007    43.5 12311143      470.
    ##  8 Afghanistan              Asia       2007    43.8 31889923      975.
    ##  9 Central African Republic Africa     2007    44.7  4369038      706.
    ## 10 Liberia                  Africa     2007    45.7  3193942      415.
    ## # … with 132 more rows

Oh, you’d like to sort on life expectancy in descending order? Then use
`desc()`.

``` r
gapminder %>%
  filter(year == 2007) %>%
  arrange(desc(lifeExp))
```

    ## # A tibble: 142 x 6
    ##    country          continent  year lifeExp       pop gdpPercap
    ##    <fct>            <fct>     <int>   <dbl>     <int>     <dbl>
    ##  1 Japan            Asia       2007    82.6 127467972    31656.
    ##  2 Hong Kong, China Asia       2007    82.2   6980412    39725.
    ##  3 Iceland          Europe     2007    81.8    301931    36181.
    ##  4 Switzerland      Europe     2007    81.7   7554661    37506.
    ##  5 Australia        Oceania    2007    81.2  20434176    34435.
    ##  6 Spain            Europe     2007    80.9  40448191    28821.
    ##  7 Sweden           Europe     2007    80.9   9031088    33860.
    ##  8 Israel           Asia       2007    80.7   6426679    25523.
    ##  9 France           Europe     2007    80.7  61083916    30470.
    ## 10 Canada           Americas   2007    80.7  33390141    36319.
    ## # … with 132 more rows

I advise that your analyses NEVER rely on rows or variables being in a
specific order. But it’s still true that human beings write the code and
the interactive development process can be much nicer if you reorder the
rows of your data as you go along. Also, once you are preparing tables
for human eyeballs, it is imperative that you step up and take control
of row order.

### Use `rename()` to rename variables

Notice that the Gapminder excerpt has variables in
[`camelCase`](http://en.wikipedia.org/wiki/CamelCase), but what if we
want them in [`snake_case`](http://en.wikipedia.org/wiki/Snake_case).
Let’s rename some variables!

``` r
gapminder_snake_case <- 
gapminder %>%
  rename(life_exp = lifeExp,
         gdp_percap = gdpPercap)
```

You can also use `select()` to **select and rename** variables, but
notice the difference between the resulting data frames.

``` r
gapminder %>%
  select(life_exp = lifeExp,
         gdp_percap = gdpPercap)
```

    ## # A tibble: 1,704 x 2
    ##    life_exp gdp_percap
    ##       <dbl>      <dbl>
    ##  1     28.8       779.
    ##  2     30.3       821.
    ##  3     32.0       853.
    ##  4     34.0       836.
    ##  5     36.1       740.
    ##  6     38.4       786.
    ##  7     39.9       978.
    ##  8     40.8       852.
    ##  9     41.7       649.
    ## 10     41.8       635.
    ## # … with 1,694 more rows

### `group_by()` is a mighty weapon

When combined with `group_by()`, the `dplyr` verbs you allow you to
solve an extremely diverse set of problems with relative ease.

For example, combination of `group_by()` and `summarize()` can be very
powerful.

Remember:

-   `group_by()` adds extra structure to your dataset – grouping
    information – which lays the groundwork for computations within the
    groups.
-   `summarize()` takes a dataset with *n* observations, computes
    requested summaries, and returns a dataset with 1 observation.

#### Counting things up

Let’s start with simple counting. How many observations do we have per
continent?

``` r
gapminder %>%
  group_by(continent) %>%
  summarize(n_obs = n())
```

    ## # A tibble: 5 x 2
    ##   continent n_obs
    ## * <fct>     <int>
    ## 1 Africa      624
    ## 2 Americas    300
    ## 3 Asia        396
    ## 4 Europe      360
    ## 5 Oceania      24

The `count()` function is a convenience function for this sort of thing.

``` r
gapminder %>%
  group_by(continent) %>%
  count()
```

    ## # A tibble: 5 x 2
    ## # Groups:   continent [5]
    ##   continent     n
    ##   <fct>     <int>
    ## 1 Africa      624
    ## 2 Americas    300
    ## 3 Asia        396
    ## 4 Europe      360
    ## 5 Oceania      24

What if we wanted to add the number of unique countries for each
continent?

``` r
gapminder %>%
  group_by(continent) %>%
  summarize(n_obs = n(),
            n_countries = n_distinct(country))
```

    ## # A tibble: 5 x 3
    ##   continent n_obs n_countries
    ## * <fct>     <int>       <int>
    ## 1 Africa      624          52
    ## 2 Americas    300          25
    ## 3 Asia        396          33
    ## 4 Europe      360          30
    ## 5 Oceania      24           2

#### General summarization

The functions you’ll apply within `summarize()` include classical
statistical summaries, like `mean()`, `median()`, `sd()`, and `IQR`.
Remember they are functions that take *n* inputs and distill them down
into 1 output.

Let’s compute the average life expectancy by continent.

``` r
gapminder %>%
  group_by(continent) %>%
  summarize(avg_lifeExp = mean(lifeExp))
```

    ## # A tibble: 5 x 2
    ##   continent avg_lifeExp
    ## * <fct>           <dbl>
    ## 1 Africa           48.9
    ## 2 Americas         64.7
    ## 3 Asia             60.1
    ## 4 Europe           71.9
    ## 5 Oceania          74.3

We can compute other types of statistics using `summarize()`. Let’s say
we wanted to focus on Europe and ask what are the minimum and maximum
life expectancies seen by year, we could use code like this:

``` r
gapminder %>%
  filter(continent == "Europe") %>%
  group_by(year) %>%
  summarize(min_lifeExp = min(lifeExp), max_lifeExp = max(lifeExp))
```

    ## # A tibble: 12 x 3
    ##     year min_lifeExp max_lifeExp
    ##  * <int>       <dbl>       <dbl>
    ##  1  1952        43.6        72.7
    ##  2  1957        48.1        73.5
    ##  3  1962        52.1        73.7
    ##  4  1967        54.3        74.2
    ##  5  1972        57.0        74.7
    ##  6  1977        59.5        76.1
    ##  7  1982        61.0        77.0
    ##  8  1987        63.1        77.4
    ##  9  1992        66.1        78.8
    ## 10  1997        68.8        79.4
    ## 11  2002        70.8        80.6
    ## 12  2007        71.8        81.8

Of course it would be much more interesting to see *which* countries
contribute these extreme observations. Is the minimum (maximum) always
coming from the same country?

Lets consider the whole dataset and find out if certain countries look
like they are over-represented in having the lowest life expectancy each
year of data collection. The code below will help you do this.

``` r
gapminder %>% 
  group_by(year) %>% 
  filter(lifeExp == min(lifeExp)) %>% 
  group_by(country) %>% 
  count() %>% 
  arrange(desc(n))
```

    ## # A tibble: 7 x 2
    ## # Groups:   country [7]
    ##   country          n
    ##   <fct>        <int>
    ## 1 Afghanistan      4
    ## 2 Rwanda           2
    ## 3 Sierra Leone     2
    ## 4 Angola           1
    ## 5 Cambodia         1
    ## 6 Swaziland        1
    ## 7 Zambia           1

To understand how the code is working, run small chunks of it. The key
line using the `filter()` to find the lowest life expectancy for each
group of years.

`count()` then lets you see if certain countries are occurring more
frequently than others.

We could ask the same question about countries with the highest life
expectancy.

``` r
gapminder %>% 
  group_by(year) %>% 
  filter(lifeExp == max(lifeExp)) %>% 
  group_by(country) %>% 
  count() %>% 
  arrange(desc(n))
```

    ## # A tibble: 4 x 2
    ## # Groups:   country [4]
    ##   country     n
    ##   <fct>   <int>
    ## 1 Japan       6
    ## 2 Iceland     3
    ## 3 Sweden      2
    ## 4 Norway      1

Ponder the minimum life expectancy data for a moment. The subject matter
and the code. Mostly you’re seeing what genocide looks like in dry
statistics on average life expectancy.

Break the code into pieces, starting at the top, and inspect the
intermediate results. That’s certainly how I was able to *write* such a
thing. These commands do not [leap fully formed out of anyone’s
forehead](http://tinyurl.com/athenaforehead) – they are built up
gradually, with lots of errors and refinements along the way. I’m not
even sure it’s a great idea to do so much manipulation in one fell
swoop. Is the statement above really hard for you to read? If yes, then
by all means break it into pieces and make some intermediate objects.
Your code should be easy to write and read when you’re done.

### But I want to do more!

I’ve introduced you to six (maybe more?) `dplyr()` verbs, but this is
just the start of your journey. You should spend time practicing with
the functions I taught you. When you feel ready, take a look at some of
the functions below to move things to the next level:

-   Master `select()` by getting to know the [selection
    helpers](https://tidyselect.r-lib.org/reference/starts_with.html).
-   Supercharge `summarize()` with
    [`across()`](https://dplyr.tidyverse.org/articles/colwise.html).
-   Get to know
    [`rowwise()`](https://dplyr.tidyverse.org/articles/rowwise.html),
    you won’t regret it!

### Resources

`dplyr` stuff

-   `dplyr()` tidyverse [webpage](https://dplyr.tidyverse.org/)
-   `dplyr()`
    [vingette](https://dplyr.tidyverse.org/articles/dplyr.html) on the
    tidyverse webpage
-   `dplyr()` section of your
    [textbook](https://r4ds.had.co.nz/transform.html)

`%>%` stuff

-   An excellent Rstudio conf (2017)
    [talk](https://rstudio.com/resources/rstudioconf-2017/writing-readable-code-with-pipes/)
    on Writing Readable Code with Pipes
-   [r4ds](https://r4ds.had.co.nz/pipes.html) chapter on pipes.
