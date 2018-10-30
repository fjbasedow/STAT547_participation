cm103 Worksheet
================

``` r
suppressPackageStartupMessages(library(tidyverse)) # Loads purrr, too
```

    ## Warning: package 'ggplot2' was built under R version 3.4.4

``` r
library(repurrrsive) # Contains data examples
library(listviewer) # For viewing JSON/lists interactively
```

    ## Warning: package 'listviewer' was built under R version 3.4.4

Resources
---------

This week, we'll be drawing from [Jenny Bryan's `purrr` tutorial](https://jennybc.github.io/purrr-tutorial/). Specifically:

-   The [`map` tutorial](https://jennybc.github.io/purrr-tutorial/ls01_map-name-position-shortcuts.html)
-   The [GitHub users tutorial](https://jennybc.github.io/purrr-tutorial/ls02_map-extraction-advanced.html) is similar.

In addition:

-   Do you feel that you need a refresher on lists and vectors in R? Check out the [Vectors and Lists](https://jennybc.github.io/purrr-tutorial/bk00_vectors-and-lists.html) part of the tutorial.
-   Are you familiar with the `apply` family of functions in Base R? You might find Jenny's ["Relationship to base and plyr functions"](https://jennybc.github.io/purrr-tutorial/bk01_base-functions.html) great for bridging the gap to `purrr`.

Review Vectors and Lists
------------------------

Vectors:

-   hold multiple entries of the same type.
-   coerced to the least-informative data type in the vector.
-   subset with single square brackets

Lists:

-   hold multiple entries of anything.
-   no entries are coerced (as a result of being able to hold anything)
-   subset with `[`, `[[`, or `$`.

Review Vectorization
--------------------

= element-wise application of a function.

Examples:

``` r
(1:10) ^ 2
```

    ##  [1]   1   4   9  16  25  36  49  64  81 100

``` r
(1:10) * (1:2)
```

    ##  [1]  1  4  3  8  5 12  7 16  9 20

``` r
commute <- c(40, 20, 35, 15)
person <- c("Parveen", "Leo", "Shawn", "Emmotions")
str_c(person, " takes ", commute, " minutes to commute to work.")
```

    ## [1] "Parveen takes 40 minutes to commute to work."  
    ## [2] "Leo takes 20 minutes to commute to work."      
    ## [3] "Shawn takes 35 minutes to commute to work."    
    ## [4] "Emmotions takes 15 minutes to commute to work."

`purrr`
-------

`purrr` is great when vectorization does not apply!

Particularly useful if your data is in JSON format.

Example:

1.  Explore the `wesanderson` list (comes with `repurrrsive` package). Hint: `str()` might help. It's a list of vectors.
2.  Use what you know about R to write code that extracts a vector of the first elements of each vector contained in `wesanderson`.

``` r
str(wesanderson)
```

    ## List of 15
    ##  $ GrandBudapest : chr [1:4] "#F1BB7B" "#FD6467" "#5B1A18" "#D67236"
    ##  $ Moonrise1     : chr [1:4] "#F3DF6C" "#CEAB07" "#D5D5D3" "#24281A"
    ##  $ Royal1        : chr [1:4] "#899DA4" "#C93312" "#FAEFD1" "#DC863B"
    ##  $ Moonrise2     : chr [1:4] "#798E87" "#C27D38" "#CCC591" "#29211F"
    ##  $ Cavalcanti    : chr [1:5] "#D8B70A" "#02401B" "#A2A475" "#81A88D" ...
    ##  $ Royal2        : chr [1:5] "#9A8822" "#F5CDB4" "#F8AFA8" "#FDDDA0" ...
    ##  $ GrandBudapest2: chr [1:4] "#E6A0C4" "#C6CDF7" "#D8A499" "#7294D4"
    ##  $ Moonrise3     : chr [1:5] "#85D4E3" "#F4B5BD" "#9C964A" "#CDC08C" ...
    ##  $ Chevalier     : chr [1:4] "#446455" "#FDD262" "#D3DDDC" "#C7B19C"
    ##  $ Zissou        : chr [1:5] "#3B9AB2" "#78B7C5" "#EBCC2A" "#E1AF00" ...
    ##  $ FantasticFox  : chr [1:5] "#DD8D29" "#E2D200" "#46ACC8" "#E58601" ...
    ##  $ Darjeeling    : chr [1:5] "#FF0000" "#00A08A" "#F2AD00" "#F98400" ...
    ##  $ Rushmore      : chr [1:5] "#E1BD6D" "#EABE94" "#0B775E" "#35274A" ...
    ##  $ BottleRocket  : chr [1:7] "#A42820" "#5F5647" "#9B110E" "#3F5151" ...
    ##  $ Darjeeling2   : chr [1:5] "#ECCBAE" "#046C9A" "#D69C4E" "#ABDDDE" ...

``` r
x <- character(0)

for (i in 1:length(wesanderson)) x[i] <- wesanderson[[i]][1]
x
```

    ##  [1] "#F1BB7B" "#F3DF6C" "#899DA4" "#798E87" "#D8B70A" "#9A8822" "#E6A0C4"
    ##  [8] "#85D4E3" "#446455" "#3B9AB2" "#DD8D29" "#FF0000" "#E1BD6D" "#A42820"
    ## [15] "#ECCBAE"

`str()` is not always useful! Try checking the structure of `got_chars` (= Game of Thrones characters):

``` r
#str(got_chars)
```

Exploring lists
---------------

1.  `str()`: embrace `list.len` and `max.level`

``` r
str(got_chars, list.len = 4)#gives first 4 elements, also of sublists
```

    ## List of 30
    ##  $ :List of 18
    ##   ..$ url        : chr "https://www.anapioficeandfire.com/api/characters/1022"
    ##   ..$ id         : int 1022
    ##   ..$ name       : chr "Theon Greyjoy"
    ##   ..$ gender     : chr "Male"
    ##   .. [list output truncated]
    ##  $ :List of 18
    ##   ..$ url        : chr "https://www.anapioficeandfire.com/api/characters/1052"
    ##   ..$ id         : int 1052
    ##   ..$ name       : chr "Tyrion Lannister"
    ##   ..$ gender     : chr "Male"
    ##   .. [list output truncated]
    ##  $ :List of 18
    ##   ..$ url        : chr "https://www.anapioficeandfire.com/api/characters/1074"
    ##   ..$ id         : int 1074
    ##   ..$ name       : chr "Victarion Greyjoy"
    ##   ..$ gender     : chr "Male"
    ##   .. [list output truncated]
    ##  $ :List of 18
    ##   ..$ url        : chr "https://www.anapioficeandfire.com/api/characters/1109"
    ##   ..$ id         : int 1109
    ##   ..$ name       : chr "Will"
    ##   ..$ gender     : chr "Male"
    ##   .. [list output truncated]
    ##   [list output truncated]

``` r
str(got_chars, list.len = 4, max.level =1) #how many levels of sublists do you want to see
```

    ## List of 30
    ##  $ :List of 18
    ##  $ :List of 18
    ##  $ :List of 18
    ##  $ :List of 18
    ##   [list output truncated]

``` r
str(got_chars[[1]])
```

    ## List of 18
    ##  $ url        : chr "https://www.anapioficeandfire.com/api/characters/1022"
    ##  $ id         : int 1022
    ##  $ name       : chr "Theon Greyjoy"
    ##  $ gender     : chr "Male"
    ##  $ culture    : chr "Ironborn"
    ##  $ born       : chr "In 278 AC or 279 AC, at Pyke"
    ##  $ died       : chr ""
    ##  $ alive      : logi TRUE
    ##  $ titles     : chr [1:3] "Prince of Winterfell" "Captain of Sea Bitch" "Lord of the Iron Islands (by law of the green lands)"
    ##  $ aliases    : chr [1:4] "Prince of Fools" "Theon Turncloak" "Reek" "Theon Kinslayer"
    ##  $ father     : chr ""
    ##  $ mother     : chr ""
    ##  $ spouse     : chr ""
    ##  $ allegiances: chr "House Greyjoy of Pyke"
    ##  $ books      : chr [1:3] "A Game of Thrones" "A Storm of Swords" "A Feast for Crows"
    ##  $ povBooks   : chr [1:2] "A Clash of Kings" "A Dance with Dragons"
    ##  $ tvSeries   : chr [1:6] "Season 1" "Season 2" "Season 3" "Season 4" ...
    ##  $ playedBy   : chr "Alfie Allen"

1.  Interactive exploration: `View()` and `listviewer::jsonedit(..., mode = "view")`

``` r
#View(got_chars)
#jsonedit(got_chars, mode="view")
```

1.  Don't be afraid to check out a subset! `names()` comes in handy, too.

``` r
str(got_chars[[1]])
```

    ## List of 18
    ##  $ url        : chr "https://www.anapioficeandfire.com/api/characters/1022"
    ##  $ id         : int 1022
    ##  $ name       : chr "Theon Greyjoy"
    ##  $ gender     : chr "Male"
    ##  $ culture    : chr "Ironborn"
    ##  $ born       : chr "In 278 AC or 279 AC, at Pyke"
    ##  $ died       : chr ""
    ##  $ alive      : logi TRUE
    ##  $ titles     : chr [1:3] "Prince of Winterfell" "Captain of Sea Bitch" "Lord of the Iron Islands (by law of the green lands)"
    ##  $ aliases    : chr [1:4] "Prince of Fools" "Theon Turncloak" "Reek" "Theon Kinslayer"
    ##  $ father     : chr ""
    ##  $ mother     : chr ""
    ##  $ spouse     : chr ""
    ##  $ allegiances: chr "House Greyjoy of Pyke"
    ##  $ books      : chr [1:3] "A Game of Thrones" "A Storm of Swords" "A Feast for Crows"
    ##  $ povBooks   : chr [1:2] "A Clash of Kings" "A Dance with Dragons"
    ##  $ tvSeries   : chr [1:6] "Season 1" "Season 2" "Season 3" "Season 4" ...
    ##  $ playedBy   : chr "Alfie Allen"

``` r
names(got_chars[[1]])
```

    ##  [1] "url"         "id"          "name"        "gender"      "culture"    
    ##  [6] "born"        "died"        "alive"       "titles"      "aliases"    
    ## [11] "father"      "mother"      "spouse"      "allegiances" "books"      
    ## [16] "povBooks"    "tvSeries"    "playedBy"

Exploring `purrr` fundamentals
------------------------------

Apply a function to each element in a list/vector with `map`.

General usage: `purrr::map(VECTOR_OR_LIST, YOUR_FUNCTION)`

Note:

-   `map` always returns a list.
-   `YOUR_FUNCTION` can return anything!

Toy example 1: without using vectorization, take the square root of the following vector:

``` r
x <- 1:10
sqrt(x)
```

    ##  [1] 1.000000 1.414214 1.732051 2.000000 2.236068 2.449490 2.645751
    ##  [8] 2.828427 3.000000 3.162278

``` r
map(x, sqrt)
```

    ## [[1]]
    ## [1] 1
    ## 
    ## [[2]]
    ## [1] 1.414214
    ## 
    ## [[3]]
    ## [1] 1.732051
    ## 
    ## [[4]]
    ## [1] 2
    ## 
    ## [[5]]
    ## [1] 2.236068
    ## 
    ## [[6]]
    ## [1] 2.44949
    ## 
    ## [[7]]
    ## [1] 2.645751
    ## 
    ## [[8]]
    ## [1] 2.828427
    ## 
    ## [[9]]
    ## [1] 3
    ## 
    ## [[10]]
    ## [1] 3.162278

Toy example 2 (functions on-the-fly): without using vectorization, square each component of `x`:

``` r
map(x, function(w) w^2) #or define function beforehand 
```

    ## [[1]]
    ## [1] 1
    ## 
    ## [[2]]
    ## [1] 4
    ## 
    ## [[3]]
    ## [1] 9
    ## 
    ## [[4]]
    ## [1] 16
    ## 
    ## [[5]]
    ## [1] 25
    ## 
    ## [[6]]
    ## [1] 36
    ## 
    ## [[7]]
    ## [1] 49
    ## 
    ## [[8]]
    ## [1] 64
    ## 
    ## [[9]]
    ## [1] 81
    ## 
    ## [[10]]
    ## [1] 100

Want a vector to be returned? Must specify the `typeof()` of the vector. Use `map_dbl()` to specify an output vector of type "double" for the above (check out the documentation `?map` for the acceptable vector types):

``` r
map_dbl(x, sqrt)
```

    ##  [1] 1.000000 1.414214 1.732051 2.000000 2.236068 2.449490 2.645751
    ##  [8] 2.828427 3.000000 3.162278

``` r
map_chr(x, sqrt)
```

    ##  [1] "1.000000" "1.414214" "1.732051" "2.000000" "2.236068" "2.449490"
    ##  [7] "2.645751" "2.828427" "3.000000" "3.162278"

Does your function have other arguments? You can specify them afterwards in the ellipses (`...`).

``` r
map_chr(x, str_c, "potato.", sep="-") #specify other argument of your function
```

    ##  [1] "1-potato."  "2-potato."  "3-potato."  "4-potato."  "5-potato." 
    ##  [6] "6-potato."  "7-potato."  "8-potato."  "9-potato."  "10-potato."

Your Turn: `purrr` fundamentals
-------------------------------

1.  Let's retry the `wesanderson` example: use `purrr` to write code that extracts a vector of the first elements of each vector contained in `wesanderson`. Play around with the following, too:
    -   Use `head` instead of writing your own function.
    -   Try different `map` functions, even the "wrong" types.
    -   Use the ``` ``[`` ``` function if you're feeling daring.

``` r
# Method 1
vec1 <- function(x) x[1]
map_chr(wesanderson, vec1)
```

    ##  GrandBudapest      Moonrise1         Royal1      Moonrise2     Cavalcanti 
    ##      "#F1BB7B"      "#F3DF6C"      "#899DA4"      "#798E87"      "#D8B70A" 
    ##         Royal2 GrandBudapest2      Moonrise3      Chevalier         Zissou 
    ##      "#9A8822"      "#E6A0C4"      "#85D4E3"      "#446455"      "#3B9AB2" 
    ##   FantasticFox     Darjeeling       Rushmore   BottleRocket    Darjeeling2 
    ##      "#DD8D29"      "#FF0000"      "#E1BD6D"      "#A42820"      "#ECCBAE"

``` r
# Method 2: head
map_chr(wesanderson, head, 1)
```

    ##  GrandBudapest      Moonrise1         Royal1      Moonrise2     Cavalcanti 
    ##      "#F1BB7B"      "#F3DF6C"      "#899DA4"      "#798E87"      "#D8B70A" 
    ##         Royal2 GrandBudapest2      Moonrise3      Chevalier         Zissou 
    ##      "#9A8822"      "#E6A0C4"      "#85D4E3"      "#446455"      "#3B9AB2" 
    ##   FantasticFox     Darjeeling       Rushmore   BottleRocket    Darjeeling2 
    ##      "#DD8D29"      "#FF0000"      "#E1BD6D"      "#A42820"      "#ECCBAE"

``` r
# Method 3
map_chr(wesanderson, `[`,1) # # `[` is a function for subsetting: `[`(vector, part_to_subset), e.g. `[`(x, 1) = x[1]
```

    ##  GrandBudapest      Moonrise1         Royal1      Moonrise2     Cavalcanti 
    ##      "#F1BB7B"      "#F3DF6C"      "#899DA4"      "#798E87"      "#D8B70A" 
    ##         Royal2 GrandBudapest2      Moonrise3      Chevalier         Zissou 
    ##      "#9A8822"      "#E6A0C4"      "#85D4E3"      "#446455"      "#3B9AB2" 
    ##   FantasticFox     Darjeeling       Rushmore   BottleRocket    Darjeeling2 
    ##      "#DD8D29"      "#FF0000"      "#E1BD6D"      "#A42820"      "#ECCBAE"

1.  Check that each character's list entry in `got_chars` has the same names as everyone else (that is, list component names, not character names). Here's one way to do it:
    1.  Use the `names` function.
    2.  Then, bind the names together in a single character.
    3.  Then, apply the `table()` function.

Shortcut functions
------------------

We can do the subsetting much easier with these shortcuts: just replace function with either:

-   index you'd like to subset by, or
-   name you'd like to subset by.

``` r
map_chr(wesanderson, 1) %>% unname()
```

    ##  [1] "#F1BB7B" "#F3DF6C" "#899DA4" "#798E87" "#D8B70A" "#9A8822" "#E6A0C4"
    ##  [8] "#85D4E3" "#446455" "#3B9AB2" "#DD8D29" "#FF0000" "#E1BD6D" "#A42820"
    ## [15] "#ECCBAE"

Your turn: shortcut functions
-----------------------------

1.  For the `got_chars` data:

-   What are the titles of each character?
-   Is a vector output appropriate here?
-   Use a pipe.

Note: each character's list entry has a component named `titles` as the 9th entry.

``` r
#map(got_chars, "titles")

got_chars %>% 
  map("titles") %>% 
  head()
```

    ## [[1]]
    ## [1] "Prince of Winterfell"                                
    ## [2] "Captain of Sea Bitch"                                
    ## [3] "Lord of the Iron Islands (by law of the green lands)"
    ## 
    ## [[2]]
    ## [1] "Acting Hand of the King (former)" "Master of Coin (former)"         
    ## 
    ## [[3]]
    ## [1] "Lord Captain of the Iron Fleet" "Master of the Iron Victory"    
    ## 
    ## [[4]]
    ## [1] ""
    ## 
    ## [[5]]
    ## [1] "Captain of the Guard at Sunspear"
    ## 
    ## [[6]]
    ## [1] ""

1.  For the `got_chars` data:

-   Extract a list of the "name" and "born" data for each person.
    -   Use the function ``` ``[`` ``` or `extract()` (from the `magrittr` package, does the same thing) function to do the subsetting
-   What happens when we switch to `map_dfr` instead of `map`? How about `map_dfc`?

``` r
desired_info <- c("name", "born")
map(got_chars, `[`, desired_info) %>% 
  head()
```

    ## [[1]]
    ## [[1]]$name
    ## [1] "Theon Greyjoy"
    ## 
    ## [[1]]$born
    ## [1] "In 278 AC or 279 AC, at Pyke"
    ## 
    ## 
    ## [[2]]
    ## [[2]]$name
    ## [1] "Tyrion Lannister"
    ## 
    ## [[2]]$born
    ## [1] "In 273 AC, at Casterly Rock"
    ## 
    ## 
    ## [[3]]
    ## [[3]]$name
    ## [1] "Victarion Greyjoy"
    ## 
    ## [[3]]$born
    ## [1] "In 268 AC or before, at Pyke"
    ## 
    ## 
    ## [[4]]
    ## [[4]]$name
    ## [1] "Will"
    ## 
    ## [[4]]$born
    ## [1] ""
    ## 
    ## 
    ## [[5]]
    ## [[5]]$name
    ## [1] "Areo Hotah"
    ## 
    ## [[5]]$born
    ## [1] "In 257 AC or before, at Norvos"
    ## 
    ## 
    ## [[6]]
    ## [[6]]$name
    ## [1] "Chett"
    ## 
    ## [[6]]$born
    ## [1] "At Hag's Mire"

``` r
map_dfr(got_chars, `[`, desired_info) # makes output into a dataframe
```

    ## # A tibble: 30 x 2
    ##    name               born                                  
    ##    <chr>              <chr>                                 
    ##  1 Theon Greyjoy      In 278 AC or 279 AC, at Pyke          
    ##  2 Tyrion Lannister   In 273 AC, at Casterly Rock           
    ##  3 Victarion Greyjoy  In 268 AC or before, at Pyke          
    ##  4 Will               ""                                    
    ##  5 Areo Hotah         In 257 AC or before, at Norvos        
    ##  6 Chett              At Hag's Mire                         
    ##  7 Cressen            In 219 AC or 220 AC                   
    ##  8 Arianne Martell    In 276 AC, at Sunspear                
    ##  9 Daenerys Targaryen In 284 AC, at Dragonstone             
    ## 10 Davos Seaworth     In 260 AC or before, at King's Landing
    ## # ... with 20 more rows

Note: as Jenny says, it's always safer (from a programming perspective) to work with output that's more predictable. The following would be safer, and is still readable:

``` r
got_chars %>% {
    tibble(
        name = map_chr(., "name"),
        born = map_chr(., "born")
    )
} #without these brackets, got_chars would be piped into first argument of tibble(). last thing that is executed in curly braces gets return outside of braces, only tibble gets returned
```

    ## # A tibble: 30 x 2
    ##    name               born                                  
    ##    <chr>              <chr>                                 
    ##  1 Theon Greyjoy      In 278 AC or 279 AC, at Pyke          
    ##  2 Tyrion Lannister   In 273 AC, at Casterly Rock           
    ##  3 Victarion Greyjoy  In 268 AC or before, at Pyke          
    ##  4 Will               ""                                    
    ##  5 Areo Hotah         In 257 AC or before, at Norvos        
    ##  6 Chett              At Hag's Mire                         
    ##  7 Cressen            In 219 AC or 220 AC                   
    ##  8 Arianne Martell    In 276 AC, at Sunspear                
    ##  9 Daenerys Targaryen In 284 AC, at Dragonstone             
    ## 10 Davos Seaworth     In 260 AC or before, at King's Landing
    ## # ... with 20 more rows

Note the curly braces "tricks" the object prior to the pipe from entering as the first argument to `tibble`, because ``` ``{`` ``` only returns the last line evaluated. In this case there are two: the above code is equivalent to

``` r
got_chars %>% {
    .
    tibble(
        name = map_chr(., "name"),
        born = map_chr(., "born")
    )
}
```

    ## # A tibble: 30 x 2
    ##    name               born                                  
    ##    <chr>              <chr>                                 
    ##  1 Theon Greyjoy      In 278 AC or 279 AC, at Pyke          
    ##  2 Tyrion Lannister   In 273 AC, at Casterly Rock           
    ##  3 Victarion Greyjoy  In 268 AC or before, at Pyke          
    ##  4 Will               ""                                    
    ##  5 Areo Hotah         In 257 AC or before, at Norvos        
    ##  6 Chett              At Hag's Mire                         
    ##  7 Cressen            In 219 AC or 220 AC                   
    ##  8 Arianne Martell    In 276 AC, at Sunspear                
    ##  9 Daenerys Targaryen In 284 AC, at Dragonstone             
    ## 10 Davos Seaworth     In 260 AC or before, at King's Landing
    ## # ... with 20 more rows
