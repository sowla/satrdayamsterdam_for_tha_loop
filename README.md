README
================

Autohrs: Reigo Hendrikson

[![license](https://img.shields.io/github/license/mashape/apistatus.svg)](https://choosealicense.com/licenses/mit/)

### Use `purrr` to feed four cats

Imagine having 4 cats. Four real cats who need food, care and love to live a happy life. They start to meow, so it's time to feed them. Our real life algorithm would be:

-   give food to cat 1
-   give food to cat 2
-   ...

But life is not that simple. Some of your cats are good cats and some are bad. You want to spoil the good cats and give normal food to the bad cats.

### Elements for the four cats problem

In real life we would have to have some place for the cats and their bowls. In R the place for cats and their bowls can be a `data.frame` (or `tibble`)

We start with cats. The cats have a name and a Goodness.

``` r
library(tibble)
cats <- 
    tribble(
    ~Name, ~Good,
    "Suzie", TRUE,
    "Margaret", FALSE,
    "Donald", FALSE,
    "Tibbles", TRUE
)

cats$Name <- paste(emo::ji("cat"), cats$Name)
cats$Good <- ifelse(cats$Good, emo::ji("heart"), emo::ji("lightning"))
cats
```

    ## # A tibble: 4 x 2
    ##   Name        Good 
    ##   <chr>       <chr>
    ## 1 🐱 Suzie    ❤️   
    ## 2 🐱 Margaret ⚡   
    ## 3 🐱 Donald   ⚡   
    ## 4 🐱 Tibbles  ❤️

Now we have to feed the cats. Only good cats get premium quality super-awesome-food. Bad cats get normal food.

Now if you have used dplyr or ifelse statements before, this example is super silly and inefficient. But for people coming from different programming languages and people who started out with for loops, this is the approach you might use.

To feed the cat, we have to determine if the cat is Good or not. We don't want to type to much, we are lazy data scientist after all. So we create a function to do our work.

``` r
# if the thing that goes in is TRUE you return "Premium", otherwise return "Normal"
good_kitty <- function(x) {
    if(x == emo::ji("heart")) {
        emo::ji("cake")
    } else {
        emo::ji("bread")
    }
}
good_kitty(TRUE)
```

    ## 🍞

``` r
good_kitty(FALSE) 
```

    ## 🍞

``` r
# you try for yourself what happens 
# when you put in other values like numbers, letters, missings etc.
```

The for loop
------------

We start with a for loop to add information to the dataframe. We want to do something on every row. We want to run the good\_kitty function on every row in the Good column.

``` r
cats$Food <- NA
for(cat in 1:nrow(cats)){
    cats$Food[cat] <- good_kitty(cats$Good[cat])
}
cats
```

    ## # A tibble: 4 x 3
    ##   Name        Good  Food 
    ##   <chr>       <chr> <chr>
    ## 1 🐱 Suzie    ❤️    🍰   
    ## 2 🐱 Margaret ⚡    🍞   
    ## 3 🐱 Donald   ⚡    🍞   
    ## 4 🐱 Tibbles  ❤️    🍰

The purrr solution
------------------

Now onto purrr:

``` r
library(purrr)
cats$Food <- NA
cats$Food <- purrr::map_chr(.x = cats$Good, .f = good_kitty)
cats
```

    ## # A tibble: 4 x 3
    ##   Name        Good  Food 
    ##   <chr>       <chr> <chr>
    ## 1 🐱 Suzie    ❤️    🍰   
    ## 2 🐱 Margaret ⚡    🍞   
    ## 3 🐱 Donald   ⚡    🍞   
    ## 4 🐱 Tibbles  ❤️    🍰

What if we do multiple things?
==============================

Some of our cats don't want cuddles.

``` r
pet_cat <- 
    function(name){
    if(name %in% c("Margaret", "Tibbles")){
        "pet"
    }else{
        "don't pet, stay away!"
    }
}
pet_cat("Donald")
```

    ## [1] "don't pet, stay away!"

``` r
pet_cat("Tibbles")
```

    ## [1] "pet"

The for loop

``` r
cats$Food <- NA
cats$Petting <- NA
for (cat in 1:nrow(cats)){
    cats$Food[cat] <- good_kitty(cats$Good[cat])
    cats$Petting[cat] <- pet_cat(cats$Name[cat])
}
cats
```

    ## # A tibble: 4 x 4
    ##   Name        Good  Food  Petting              
    ##   <chr>       <chr> <chr> <chr>                
    ## 1 🐱 Suzie    ❤️    🍰    don't pet, stay away!
    ## 2 🐱 Margaret ⚡    🍞    don't pet, stay away!
    ## 3 🐱 Donald   ⚡    🍞    don't pet, stay away!
    ## 4 🐱 Tibbles  ❤️    🍰    don't pet, stay away!

``` r
cats$Food <- NA
cats$Petting <- NA
cats$Food <- map_chr(cats$Good, good_kitty)
cats$Petting <- map_chr(cats$Name, pet_cat)
cats
```

    ## # A tibble: 4 x 4
    ##   Name        Good  Food  Petting              
    ##   <chr>       <chr> <chr> <chr>                
    ## 1 🐱 Suzie    ❤️    🍰    don't pet, stay away!
    ## 2 🐱 Margaret ⚡    🍞    don't pet, stay away!
    ## 3 🐱 Donald   ⚡    🍞    don't pet, stay away!
    ## 4 🐱 Tibbles  ❤️    🍰    don't pet, stay away!
