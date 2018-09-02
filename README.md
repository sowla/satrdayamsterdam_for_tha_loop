README
================

[![license](https://img.shields.io/github/license/mashape/apistatus.svg)](https://choosealicense.com/licenses/mit/)

### Use `purrr` to feed four cats

Imagine having 4 cats. Four real cats who need food, care and ... to live a happy life. Every now and then it's time to feed them. Our real life algorithm would be:

-   give food to cat 1
-   give food to cat 2
-   ...

In real life we would have to have some place for the cats and their bowls. In R the place for cats and their bowls can be a `data.frame` (or `tibble`)

Turn your for loop into a purrr map function.

We start with cats, because why not.

``` r
library(tibble)
cats <- 
    tribble(
    ~Name, ~Good,
    "Suzie", TRUE,
    "George", TRUE,
    "Fred", FALSE,
    "Donald", FALSE,
    "Tibbles", TRUE
)
```

We completely forgat we have ifelse statements.

Only good cats get premium quality super-awesome-food. Bad cats get normal food.

We don't want to type to much, we are data scientist, and lazy. So we create a function to do our work.

``` r
good_kitty <-function(x){
    if(x){"Premium"} else{
            "Normal"
    }}
good_kitty(TRUE)
```

    ## [1] "Premium"

``` r
good_kitty(FALSE)
```

    ## [1] "Normal"

We start with a for loop to add information to the dataframe. We want to do something on every row. We want to run the good\_kitty function on every row in the Good column.

``` r
cats$Food <- NA
for(cat in 1:nrow(cats)){
    cats$Food[cat] <- good_kitty(cats$Good[cat])
}
cats
```

    ## # A tibble: 5 x 3
    ##   Name    Good  Food   
    ##   <chr>   <lgl> <chr>  
    ## 1 Suzie   TRUE  Premium
    ## 2 George  TRUE  Premium
    ## 3 Fred    FALSE Normal 
    ## 4 Donald  FALSE Normal 
    ## 5 Tibbles TRUE  Premium

Now onto purrr

``` r
library(purrr)
cats$Food <- NA

cats$Food <- purrr::map_chr(.x = cats$Good, .f = good_kitty)
cats
```

    ## # A tibble: 5 x 3
    ##   Name    Good  Food   
    ##   <chr>   <lgl> <chr>  
    ## 1 Suzie   TRUE  Premium
    ## 2 George  TRUE  Premium
    ## 3 Fred    FALSE Normal 
    ## 4 Donald  FALSE Normal 
    ## 5 Tibbles TRUE  Premium
