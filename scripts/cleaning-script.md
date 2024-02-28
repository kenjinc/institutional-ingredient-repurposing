cleaning-script
================

## cleaning script

### package loading

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
    ## ✔ ggplot2 3.4.1      ✔ purrr   0.3.4 
    ## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
    ## ✔ tidyr   1.2.1      ✔ stringr 1.4.1 
    ## ✔ readr   2.1.2      ✔ forcats 0.5.2 
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(ggpubr)
```

### data loading

``` r
aggregated_data <- as_tibble(read.csv("/Users/kenjinchang/github/institutional-ingredient-repurposing/parent-data/boston-university.csv",skip=1)) 
aggregated_data
```

    ## # A tibble: 74 × 17
    ##    Site.…¹ Total…² Avail…³ Total…⁴ Produ…⁵ Total…⁶ X     Meal.…⁷ Meal.…⁸ Recip…⁹
    ##    <chr>   <chr>   <chr>   <chr>   <chr>   <chr>   <lgl> <chr>   <chr>   <chr>  
    ##  1 "Produ… (Volum… (Produ… (Volum… "(Prod… (Volum… NA    ""      (when … ""     
    ##  2 "Oct 2… 190     pineap… 85.75   "tops,… 68.75   NA    "Break… 978     "pinea…
    ##  3 ""      60      brocco… 7.25    "stem … 1       NA    "Lunch" 2447    "pinea…
    ##  4 ""      30      cilant… 3       "stem"  0.5     NA    "Dinne… 1755    ""     
    ##  5 "Oct 2… 220     pineap… 99.3    "tops,… 79.7    NA    "Break… 1146    "pinea…
    ##  6 ""      60      brocco… 7.7     "stem … 1.1     NA    "Lunch" 2153    "pinea…
    ##  7 ""      30      cilant… 3.4     "stem"  0.6     NA    "Dinne… 1611    ""     
    ##  8 "Oct 2… 189     pineap… 85.3    "tops,… 68.4    NA    "Break… 1003    "pinea…
    ##  9 ""      42      brocco… 5.4     "stem … 0.7     NA    "Lunch" 2314    "pinea…
    ## 10 ""      60      cilant… 6.7     "stem"  1.1     NA    "Dinne… 1614    "pickl…
    ## # … with 64 more rows, 7 more variables:
    ## #   Volume.of.Repurposed.Ingredient.Used.in.Recipe <chr>,
    ## #   Cost.per.recipe <chr>, Servings.per.recipe <chr>,
    ## #   Displaced.Recipe.Name..Optional..if.replacing.something.on.the.line. <chr>,
    ## #   Volume.that.would.have.been.served <chr>, Cost.per.recipe.1 <chr>,
    ## #   Servings.per.recipe.1 <chr>, and abbreviated variable names ¹​Site..BU,
    ## #   ²​Total.product.purchased, …

### data writing

``` r
write.csv(aggregated_data,file="/Users/kenjinchang/github/institutional-ingredient-repurposing/data/aggregated-data.csv")
```
