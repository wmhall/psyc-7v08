Lab 06
================

-   [How to approach this lab](#how-to-approach-this-lab)
-   [Vectors are everywhere](#vectors-are-everywhere)
-   [Indexing a vector](#indexing-a-vector)
-   [Lists](#lists)
-   [Data frames](#data-frames)
-   [Putting it all together … implications for
    data.frames](#putting-it-all-together-implications-for-data.frames)
-   [I want to do more!](#i-want-to-do-more)

### How to approach this lab

For this lab we’ll be sticking entirely with base R. Although the focus
of this lab is vectors, you’ll see me use a number of functions (e.g.,
`rnorm()`) that aren’t focal to our discussion of vectors but are useful
to know as you continue to develop your R abilities. If you see a
function you don’t know, spend some time researching it and learning
about what it does.

None of what I’m showing you in today’s seminar should be used to
replace approaches I’ve taught you from the `tidyverse`. Our discussion
of vectors is designed to give you a better sense of the different data
types that make up the R language.

### Vectors are everywhere

Most objects you meet in R will be vectors.

Square brackets are used for isolating elements of a vector for
inspection, modification, etc. This is often called **indexing**. Go
through the following code carefully, as it’s really rather surprising.
Indexing begins at 1 in R, unlike many other languages that index from
0.

``` r
x <- 3 * 4
x
```

    ## [1] 12

``` r
is.vector(x)
```

    ## [1] TRUE

``` r
length(x)
```

    ## [1] 1

``` r
x[2] <- 100
x
```

    ## [1]  12 100

``` r
x[5] <- 3
x
```

    ## [1]  12 100  NA  NA   3

``` r
x[11]
```

    ## [1] NA

R is built to work with vectors. Many operations (functions) are
*vectorized*, i.e. by default a fucntion will be applied to a whole
vector when given a vector as input. People coming from other
programming languages often don’t internalize or exploit this and they
write lots of unnecessary `for` loops.

``` r
x <- 1:4
## which would you rather write and read?
## the vectorized version ...
y <- x^2
y
```

    ## [1]  1  4  9 16

``` r
## or the for loop version?
z <- vector(mode = mode(x), length = length(x))
for(i in seq_along(x)) {
  z[i] <- x[i]^2
}
identical(y, z)
```

    ## [1] TRUE

When reading function documentation, keep your eyes peeled for arguments
that can be vectors. You’ll be surprised how common they are. For
example, the mean and standard deviation of random normal variates
(`rnorm` - take a look at the help page for this function or do a quick
google) can be provided as vectors.

``` r
rnorm(5, mean = 10^(1:5))
```

    ## [1]     9.015855    99.757956  1000.944286  9998.790356 99999.253307

``` r
round(rnorm(5, sd = 10^(0:4)), 2)
```

    ## [1]     0.45    11.74    22.02   315.65 -9041.17

This could be awesome in some settings, but dangerous in others, i.e. if
you exploit this by mistake and get no warning. This is one of the
reasons it’s so important to keep close tabs on your R objects: are they
what you expect in terms of their type and length or dimensions? Check
early and check often.

Notice that R also recycles vectors, if they are not the necessary
length. You will get a warning if R suspects recycling is unintended,
i.e. when one length is not an integer multiple of another, but
recycling is silent if it seems like you know what you’re doing. Can be
a beautiful thing when you’re doing this deliberately, but devastating
when you don’t.

``` r
y <- 1:3
z <- 3:7
y + z
```

    ## Warning in y + z: longer object length is not a multiple of shorter object
    ## length

    ## [1] 4 6 8 7 9

``` r
y <- 1:10
z <- 3:7
y + z
```

    ##  [1]  4  6  8 10 12  9 11 13 15 17

The catenate function `c()` is your go-to function for making vectors.
Whenever I see `c()`, I think *combine* or *collect*.

``` r
str(c("hello", "world"))
```

    ##  chr [1:2] "hello" "world"

``` r
str(c(1:3, 100, 150))
```

    ##  num [1:5] 1 2 3 100 150

R has four (there actually six, but for our purposes, we’ll focus on
four) object types that are called “atomic vectors”. An absolute
requirement is that all the bits of info they hold are of the same
flavor, i.e. all numeric or logical or character. If that’s not already
true upon creation, the elements will be coerced to the same type, using
a “lowest common denominator” approach (usually character). This is
another stellar opportunity for you to create an object of one flavor
without meaning to do so and to remain unaware of that for a long time.
Check early, check often.

``` r
x <- c("brock", pi, TRUE, 4.3)
str(x)
```

    ##  chr [1:4] "brock" "3.14159265358979" "TRUE" "4.3"

``` r
length(x)
```

    ## [1] 4

``` r
typeof(x)
```

    ## [1] "character"

The most important atomic vector types are:

-   logical: TRUE’s AND FALSE’s, easily coerced into 1’s and 0’s
-   numeric: numbers and, yes, integers and double-precision floating
    point numbers are different but you can live happily for a long time
    without worrying about this
-   character

Let’s create some simple vectors for more demos below. I’ve posed some
questions for you to answer in the comments. Explore the new functions
that you haven’t met before.

``` r
n <- 8L

# Take a look at the rnorm function. 
w <- round(rnorm(n), 2)
w
```

    ## [1]  0.45  1.13 -0.35  0.35  0.64  0.07 -0.10 -0.90

``` r
# numeric floating point
x <- 1L:n # numeric integer
x
```

    ## [1] 1 2 3 4 5 6 7 8

``` r
## another way to accomplish by hand is x <- c(1L, 2L, 3L, 4L, 5L, 6L, 7L, 8L)

#what is LETTERS?
y <- LETTERS[1:n] # character
y
```

    ## [1] "A" "B" "C" "D" "E" "F" "G" "H"

``` r
#what does the runif function do?
z <- runif(n) > 0.3 # logical
z
```

    ## [1] FALSE FALSE  TRUE  TRUE  TRUE  TRUE  TRUE FALSE

Use `str()` and any other functions you wish to inspect these objects,
such as `length()`, `typeof()`, `is.numeric()`, `is.logical()`, etc.
Like the `is.xxx()` family of functions, there are also `as.xxx()`
functions you can experiment with.

``` r
str(w)
```

    ##  num [1:8] 0.45 1.13 -0.35 0.35 0.64 0.07 -0.1 -0.9

``` r
length(x)
```

    ## [1] 8

``` r
is.logical(y)
```

    ## [1] FALSE

``` r
as.numeric(z)
```

    ## [1] 0 0 1 1 1 1 1 0

### Indexing a vector

We’ve said, and even seen, that square brackets are used to index a
vector. There is great flexibility in what one can put inside the square
brackets and it’s worth understanding the many options. They are all
useful, just in different contexts.

Most common, useful ways to index a vector:

-   vector of positive integers specifying the keepers
-   vector of negative integers specifying who to remove
-   character vector, naming the keepers
-   logical vector: keep elements associated with TRUEs, ditch the
    FALSEs. I didn’t cover this in lecture so make sure you can
    understand it from the code below.

Go through each of the indexing options below and make sure you
understand them.

``` r
#In the code below I'm using a few base R functions you haven't met before. 
#Spend time figuring out what they do. 

#for example, what does seq_along() do? 
# To find out, you might first have to run seq_along(w) and inspect the output

w
```

    ## [1]  0.45  1.13 -0.35  0.35  0.64  0.07 -0.10 -0.90

``` r
names(w) <- letters[seq_along(w)]
w
```

    ##     a     b     c     d     e     f     g     h 
    ##  0.45  1.13 -0.35  0.35  0.64  0.07 -0.10 -0.90

``` r
w < 0
```

    ##     a     b     c     d     e     f     g     h 
    ## FALSE FALSE  TRUE FALSE FALSE FALSE  TRUE  TRUE

``` r
#What is which() doing?
which(w < 0)
```

    ## c g h 
    ## 3 7 8

``` r
# Here is where I index using a logical vector. 
# Do you understand what is going on?

w[w < 0]
```

    ##     c     g     h 
    ## -0.35 -0.10 -0.90

``` r
seq(from = 1, to = length(w), by = 2)
```

    ## [1] 1 3 5 7

``` r
w[seq(from = 1, to = length(w), by = 2)]
```

    ##     a     c     e     g 
    ##  0.45 -0.35  0.64 -0.10

``` r
w[-c(2, 5)]
```

    ##     a     c     d     f     g     h 
    ##  0.45 -0.35  0.35  0.07 -0.10 -0.90

``` r
w[c('c', 'a', 'f')]
```

    ##     c     a     f 
    ## -0.35  0.45  0.07

### Lists

Lists are like a vector, but with no requirement that the elements be of
the same type

Here are a few things that make lists important:

-   data.frames are lists! They are a special case where each element is
    an atomic vector, all having the same length.
-   many functions will return lists to you and you will want to extract
    goodies from them, such as the p-value for a hypothesis test or the
    estimated error variance in a regression model.

Here we repeat an assignment from above, using `list()` instead of `c()`
to combine things and you’ll notice that the different types of the
constituent parts are retained this time.

``` r
a <- list("brock", pi, TRUE, 4.3)
str(a)
```

    ## List of 4
    ##  $ : chr "brock"
    ##  $ : num 3.14
    ##  $ : logi TRUE
    ##  $ : num 4.3

``` r
length(a)
```

    ## [1] 4

``` r
typeof(a)
```

    ## [1] "list"

List components can also have names. You can create or change names
after a list already exists or this can be integrated into the initial
assignment.

``` r
names(a)
```

    ## NULL

``` r
names(a) <- c("uni", "dessert", "myAim", "number")
a
```

    ## $uni
    ## [1] "brock"
    ## 
    ## $dessert
    ## [1] 3.141593
    ## 
    ## $myAim
    ## [1] TRUE
    ## 
    ## $number
    ## [1] 4.3

``` r
a <- list(uni = "brock", dessert = pi, myAim = TRUE, number = 4.3)
names(a)
```

    ## [1] "uni"     "dessert" "myAim"   "number"

Indexing a list is similar to indexing a vector but it is necessarily
more complex. The fundamental issue is this: if you request a single
element from the list, do you want a list of length 1 containing only
that element or do you want the element itself? For the former (desired
return value is a list), we use single square brackets, `[` and `]`,
just like indexing a vector. For the latter (desired return value is a
single element), we use a dollar sign `$`, which you’ve already used to
get one variable from a data.frame, or double square brackets, `[[` and
`]]`.

A slightly more complicated list will make our demos more educational.
Now we really see that the elements can differ in type and length.

``` r
a <- list(uni = c("brock", "ubc"),
           tNum = c(pi, exp(1), sqrt(2)),
           myAim = TRUE,
           joeNum = 2:6)
str(a)
```

    ## List of 4
    ##  $ uni   : chr [1:2] "brock" "ubc"
    ##  $ tNum  : num [1:3] 3.14 2.72 1.41
    ##  $ myAim : logi TRUE
    ##  $ joeNum: int [1:5] 2 3 4 5 6

``` r
length(a)
```

    ## [1] 4

``` r
typeof(a)
```

    ## [1] "list"

Here’s are ways to get a single list element:

``` r
a[[2]] # index with a positive integer
```

    ## [1] 3.141593 2.718282 1.414214

``` r
a$myAim # use dollar sign and element name
```

    ## [1] TRUE

``` r
str(a$myAim) # we get myAim itself, a length 1 logical vector
```

    ##  logi TRUE

``` r
a[["tNum"]] # index with length 1 character vector 
```

    ## [1] 3.141593 2.718282 1.414214

``` r
str(a[["tNum"]]) # we get tNum itself, a length 3 numeric vector
```

    ##  num [1:3] 3.14 2.72 1.41

``` r
i_want_this <- "joeNum" # indexing with length 1 character object
a[[i_want_this]] # we get joeNum itself, a length 5 integer vector
```

    ## [1] 2 3 4 5 6

``` r
a[[c("joeNum", "uni")]] # does not work! can't get > 1 elements! see below
```

    ## Error in a[[c("joeNum", "uni")]]: subscript out of bounds

A case when you must use the double bracket approach, as opposed to the
dollar sign, is when the indexing object itself is an R object
(`a[[i_want_this]]`); we show that above.

What if you want more than one element? You must index vector-style with
single square brackets. Note that the return value will always be a
list, unlike the return value from double square brackets, even if you
only request 1 element.

``` r
names(a)
```

    ## [1] "uni"    "tNum"   "myAim"  "joeNum"

``` r
a[c("tNum", "uni")] # indexing by length 2 character vector
```

    ## $tNum
    ## [1] 3.141593 2.718282 1.414214
    ## 
    ## $uni
    ## [1] "brock" "ubc"

``` r
str(a[c("tNum", "uni")]) # returns list of length 2
```

    ## List of 2
    ##  $ tNum: num [1:3] 3.14 2.72 1.41
    ##  $ uni : chr [1:2] "brock" "ubc"

``` r
a["uni"] # indexing by length 1 character vector
```

    ## $uni
    ## [1] "brock" "ubc"

``` r
str(a["uni"])# returns list of length 1
```

    ## List of 1
    ##  $ uni: chr [1:2] "brock" "ubc"

``` r
length(a["uni"]) # really, it does!
```

    ## [1] 1

``` r
length(a["uni"][[1]]) # contrast with length of the veg vector itself
```

    ## [1] 2

### Data frames

In data analysis, we often import data from an external file (e.g., a
csv). But you can also construct a data.frame directly using
`data.frame()` (or `tibble()` in the `tidyverse`).

``` r
# I'm using a number of functions (rnorm, runif, rep) to generate data that you might not have met before. 
# If you don't know what they do, spend some time learning about them.
# E.g., what does rnorm(8, 2) return?

n <- 8
my_data_frame <- data.frame(w = round(rnorm(n), 2),
                    x = 1:n,
                    y = LETTERS[1:n],
                    z = runif(n) > 0.3,
                    v = rep(LETTERS[9:12], each = 2))

my_data_frame
```

<div class="kable-table">

|     w |   x | y   | z     | v   |
|------:|----:|:----|:------|:----|
|  1.11 |   1 | A   | TRUE  | I   |
| -0.96 |   2 | B   | TRUE  | I   |
|  0.80 |   3 | C   | FALSE | J   |
|  0.13 |   4 | D   | FALSE | J   |
| -1.11 |   5 | E   | TRUE  | K   |
| -0.29 |   6 | F   | TRUE  | K   |
|  1.86 |   7 | G   | FALSE | L   |
| -1.79 |   8 | H   | TRUE  | L   |

</div>

``` r
str(my_data_frame)
```

    ## 'data.frame':    8 obs. of  5 variables:
    ##  $ w: num  1.11 -0.96 0.8 0.13 -1.11 -0.29 1.86 -1.79
    ##  $ x: int  1 2 3 4 5 6 7 8
    ##  $ y: chr  "A" "B" "C" "D" ...
    ##  $ z: logi  TRUE TRUE FALSE FALSE TRUE TRUE ...
    ##  $ v: chr  "I" "I" "J" "J" ...

``` r
typeof(my_data_frame) #It's a list!!
```

    ## [1] "list"

``` r
class(my_data_frame) #But wait, it's a data frame!!
```

    ## [1] "data.frame"

data.frames really are lists! Double square brackets can be used to get
individual variables. Single square brackets can be used to get one or
more variables, returned as a data.frame (how would you do this in the
`tidyverse`?).

``` r
is.list(my_data_frame) # data.frames are lists
```

    ## [1] TRUE

``` r
my_data_frame[[5]] # this works but I prefer ...
```

    ## [1] "I" "I" "J" "J" "K" "K" "L" "L"

``` r
my_data_frame$v # using dollar sign and name, when possible
```

    ## [1] "I" "I" "J" "J" "K" "K" "L" "L"

``` r
my_data_frame[c("x", "z")] # get multiple variables
```

<div class="kable-table">

|   x | z     |
|----:|:------|
|   1 | TRUE  |
|   2 | TRUE  |
|   3 | FALSE |
|   4 | FALSE |
|   5 | TRUE  |
|   6 | TRUE  |
|   7 | FALSE |
|   8 | TRUE  |

</div>

``` r
str(my_data_frame[c("x", "z")]) # returns a data.frame
```

    ## 'data.frame':    8 obs. of  2 variables:
    ##  $ x: int  1 2 3 4 5 6 7 8
    ##  $ z: logi  TRUE TRUE FALSE FALSE TRUE TRUE ...

Remember data.frames are lists. You can convert a list to a data.frame
using the `as.data.frame()` function. But remember, it is an absolute
requirement that the constituent vectors have the same length. Explore
the code below and make sure you understand why `second_list` can’t be
made into a data.frame. Fix the code below so that the `second_list`
object can be converted into a data.frame.

``` r
first_list <- list(w = round(rnorm(n), 2),
                    x = 1:n,
                    y = LETTERS[1:n],
                    z = runif(n) > 0.3,
                    v = rep(LETTERS[9:12], each = 2))

second_list <-  list(w = round(rnorm(7), 2),
                    x = 1:n,
                    y = LETTERS[1:n],
                    z = runif(n) > 0.3,
                    v = rep(LETTERS[9:12], each = 2))


as.data.frame(first_list)
```

<div class="kable-table">

|     w |   x | y   | z     | v   |
|------:|----:|:----|:------|:----|
|  0.86 |   1 | A   | TRUE  | I   |
| -0.74 |   2 | B   | FALSE | I   |
|  0.01 |   3 | C   | TRUE  | J   |
| -0.58 |   4 | D   | TRUE  | J   |
| -0.27 |   5 | E   | TRUE  | K   |
| -0.51 |   6 | F   | TRUE  | K   |
| -0.70 |   7 | G   | TRUE  | L   |
| -1.23 |   8 | H   | TRUE  | L   |

</div>

``` r
as.data.frame(second_list)
```

    ## Error in (function (..., row.names = NULL, check.rows = FALSE, check.names = TRUE, : arguments imply differing number of rows: 7, 8

### Putting it all together … implications for data.frames

This behind the scenes tour is still aimed at making you a better data
analyst. Hopefully the slog through vectors and lists will be redeemed
by greater prowess at manipulating data.frames. Why should this be true?

-   a data.frame is a *list*
-   the list elements are the variables and they are *atomic vectors*
-   next week we’ll meet data frames that have lists nested within them

Please spend some time reviewing the different indexing styles for data
frames. Note, these indexing approaches should not replace your use of
the `dplyr()` verbs that we covered earlier in the course. You should
know about these indexing techniques because one day you’ll find that
yourself in a situation where you’ll need them.

##### List-style indexing of a data.frame:

``` r
my_data_frame
```

<div class="kable-table">

|     w |   x | y   | z     | v   |
|------:|----:|:----|:------|:----|
|  1.11 |   1 | A   | TRUE  | I   |
| -0.96 |   2 | B   | TRUE  | I   |
|  0.80 |   3 | C   | FALSE | J   |
|  0.13 |   4 | D   | FALSE | J   |
| -1.11 |   5 | E   | TRUE  | K   |
| -0.29 |   6 | F   | TRUE  | K   |
|  1.86 |   7 | G   | FALSE | L   |
| -1.79 |   8 | H   | TRUE  | L   |

</div>

``` r
my_data_frame$z
```

    ## [1]  TRUE  TRUE FALSE FALSE  TRUE  TRUE FALSE  TRUE

``` r
i_want_this <- "z"
my_data_frame[[i_want_this]]
```

    ## [1]  TRUE  TRUE FALSE FALSE  TRUE  TRUE FALSE  TRUE

``` r
str(my_data_frame[[i_want_this]]) # we get an atomic vector
```

    ##  logi [1:8] TRUE TRUE FALSE FALSE TRUE TRUE ...

##### Reviewing vector-style indexing of a data.frame:

``` r
my_data_frame["y"]
```

<div class="kable-table">

| y   |
|:----|
| A   |
| B   |
| C   |
| D   |
| E   |
| F   |
| G   |
| H   |

</div>

``` r
str(my_data_frame["y"]) # we get a data.frame with one variable, y
```

    ## 'data.frame':    8 obs. of  1 variable:
    ##  $ y: chr  "A" "B" "C" "D" ...

``` r
i_want_this <- c("w", "v")
my_data_frame[i_want_this] # index with a vector of variable names
```

<div class="kable-table">

|     w | v   |
|------:|:----|
|  1.11 | I   |
| -0.96 | I   |
|  0.80 | J   |
|  0.13 | J   |
| -1.11 | K   |
| -0.29 | K   |
|  1.86 | L   |
| -1.79 | L   |

</div>

``` r
str(my_data_frame[c("w", "v")])
```

    ## 'data.frame':    8 obs. of  2 variables:
    ##  $ w: num  1.11 -0.96 0.8 0.13 -1.11 -0.29 1.86 -1.79
    ##  $ v: chr  "I" "I" "J" "J" ...

##### Matrix-style indexing (I called this matrix subsetting in class) of a data.frame:

``` r
my_data_frame[ , "v"]
```

    ## [1] "I" "I" "J" "J" "K" "K" "L" "L"

``` r
str(my_data_frame[ , "v"])
```

    ##  chr [1:8] "I" "I" "J" "J" "K" "K" "L" "L"

``` r
my_data_frame[ , "v"]
```

    ## [1] "I" "I" "J" "J" "K" "K" "L" "L"

``` r
str(my_data_frame[ , "v"])
```

    ##  chr [1:8] "I" "I" "J" "J" "K" "K" "L" "L"

``` r
my_data_frame[c(2, 4, 7), c(1, 4)] # awful and difficult to understand but syntax works
```

<div class="kable-table">

|     |     w | z     |
|-----|------:|:------|
| 2   | -0.96 | TRUE  |
| 4   |  0.13 | FALSE |
| 7   |  1.86 | FALSE |

</div>

``` r
my_data_frame[my_data_frame$z, ]
```

<div class="kable-table">

|     |     w |   x | y   | z    | v   |
|-----|------:|----:|:----|:-----|:----|
| 1   |  1.11 |   1 | A   | TRUE | I   |
| 2   | -0.96 |   2 | B   | TRUE | I   |
| 5   | -1.11 |   5 | E   | TRUE | K   |
| 6   | -0.29 |   6 | F   | TRUE | K   |
| 8   | -1.79 |   8 | H   | TRUE | L   |

</div>

### I want to do more!

A data type that we didn’t cover is matrices because I think you’ll find
that you rarely use them. However, if you want to learn about matrices,
you can do so by reading these resources: [See section 4.9 of this
chapter](https://bookdown.org/rdpeng/rprogdatascience/r-nuts-and-bolts.html)
and [Section 9.2 of this
chapter](https://bookdown.org/rdpeng/rprogdatascience/subsetting-r-objects.html).
When learning about matrices, think about how they relate to vectors and
data frames.

Continue working on [hw03](hw03.md).
