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

### data loading and aggregation

- boston university

we begin by loading in the data associated with the first of the 11
listed sites, omitting rows one and three, which house additional
variable-specifying information that will ultimately be captured in the
metadata

``` r
bu_data <- as_tibble(read.csv("/Users/kenjinchang/github/institutional-ingredient-repurposing/parent-data/boston-university.csv",skip=1)) %>%
  filter(!row_number() %in% c(1)) 
```

we will then need to remove the embedded separating column, `X`, and
rename each of the variables according the conventions laid out in our
codebook.

``` r
bu_data <- bu_data %>%
  select(!X) %>%
  rename("date"=Site..BU,"prod_vol_purch"=Total.product.purchased,"prod_type"=Available.Product.Type..to.Prepare.for.Service.,"prod_vol_avail_upcyc"=Total.available.product.to.repurpose..after.primary.fabrication.or.preparation.,"prod_type_unavail_upcyc"=Product.Unable.to.Repurpose,"prod_vol_unavail_upcyc"=Total.available.product.left.that.cannot.be.repurposed..remaining.product.waste.,"meal_period"=Meal.Period.Utilization.Recipe.Served,"meal_swipes"=Meal.swipes.per.period,"recipe_served"=Recipe.Name,"prod_vol_in_recipe"=Volume.of.Repurposed.Ingredient.Used.in.Recipe,"recipe_cost"=Cost.per.recipe,"recipe_servings"=Servings.per.recipe,"displaced_recipe"=Displaced.Recipe.Name..Optional..if.replacing.something.on.the.line.,"displaced_recipe_vol"=Volume.that.would.have.been.served,"displaced_recipe_cost"=Cost.per.recipe.1,"displaced_recipe_servings"=Servings.per.recipe.1)
```

from this point, we will need to add an adjoining column specifying the
site where these data correspond to

``` r
bu_data <- bu_data %>% 
  mutate(site="boston university",
         .before="date")
```

now, we will perform the same series of changes to each of the remaining
10 sites

- culinary institute of america

``` r
cia_data <- as_tibble(read.csv("/Users/kenjinchang/github/institutional-ingredient-repurposing/parent-data/culinary-institute-of-america.csv",skip=1)) %>%
  filter(!row_number() %in% c(1)) %>%
  select(!X) %>%
  rename("date"=Site..CIA,"prod_vol_purch"=Total.product.purchased,"prod_type"=Available.Product.Type..to.Prepare.for.Service.,"prod_vol_avail_upcyc"=Total.available.product.to.repurpose..after.primary.fabrication.or.preparation.,"prod_type_unavail_upcyc"=Product.Unable.to.Repurpose,"prod_vol_unavail_upcyc"=Total.available.product.left.that.cannot.be.repurposed..remaining.product.waste.,"meal_period"=Meal.Period.Utilization.Recipe.Served,"meal_swipes"=Meal.swipes.per.period,"recipe_served"=Recipe.Name,"prod_vol_in_recipe"=Volume.of.Repurposed.Ingredient.Used.in.Recipe,"recipe_cost"=Cost.per.recipe,"recipe_servings"=Servings.per.recipe,"displaced_recipe"=Displaced.Recipe.Name..Optional..if.replacing.something.on.the.line.,"displaced_recipe_vol"=Volume.that.would.have.been.served,"displaced_recipe_cost"=Cost.per.recipe.1,"displaced_recipe_servings"=Servings.per.recipe.1) %>%
  mutate(site="culinary institute of america",
         .before="date")
```

- rutgers university

``` r
ru_data <- as_tibble(read.csv("/Users/kenjinchang/github/institutional-ingredient-repurposing/parent-data/rutgers-university.csv",skip=1)) %>%
  filter(!row_number() %in% c(1)) %>%
  select(!X) %>%
  rename("date"=Site..Rutgers,"prod_vol_purch"=Total.product.purchased,"prod_type"=Available.Product.Type..to.Prepare.for.Service.,"prod_vol_avail_upcyc"=Total.available.product.to.repurpose..after.primary.fabrication.or.preparation.,"prod_type_unavail_upcyc"=Product.Unable.to.Repurpose,"prod_vol_unavail_upcyc"=Total.available.product.left.that.cannot.be.repurposed..remaining.product.waste.,"meal_period"=Meal.Period.Utilization.Recipe.Served,"meal_swipes"=Meal.swipes.per.period,"recipe_served"=Recipe.Name,"prod_vol_in_recipe"=Volume.of.Repurposed.Ingredient.Used.in.Recipe,"recipe_cost"=Cost.per.recipe,"recipe_servings"=Servings.per.recipe,"displaced_recipe"=Displaced.Recipe.Name..Optional..if.replacing.something.on.the.line.,"displaced_recipe_vol"=Volume.that.would.have.been.served,"displaced_recipe_cost"=Cost.per.recipe.1,"displaced_recipe_servings"=Servings.per.recipe.1) %>%
  mutate(site="rutgers university",
         .before="date")
```

- san jose state university

``` r
sjsu_data <- as_tibble(read.csv("/Users/kenjinchang/github/institutional-ingredient-repurposing/parent-data/san-jose-state-university.csv",skip=1)) %>%
  filter(!row_number() %in% c(1)) %>%
  select(!X) %>%
  rename("date"=Site..SJSU,"prod_vol_purch"=Total.product.purchased,"prod_type"=Available.Product.Type..to.Prepare.for.Service.,"prod_vol_avail_upcyc"=Total.available.product.to.repurpose..after.primary.fabrication.or.preparation.,"prod_type_unavail_upcyc"=Product.Unable.to.Repurpose,"prod_vol_unavail_upcyc"=Total.available.product.left.that.cannot.be.repurposed..remaining.product.waste.,"meal_period"=Meal.Period.Utilization.Recipe.Served,"meal_swipes"=Meal.swipes.per.period,"recipe_served"=Recipe.Name,"prod_vol_in_recipe"=Volume.of.Repurposed.Ingredient.Used.in.Recipe,"recipe_cost"=Cost.per.recipe,"recipe_servings"=Servings.per.recipe,"displaced_recipe"=Displaced.Recipe.Name..Optional..if.replacing.something.on.the.line.,"displaced_recipe_vol"=Volume.that.would.have.been.served,"displaced_recipe_cost"=Cost.per.recipe.1,"displaced_recipe_servings"=Servings.per.recipe.1) %>%
  mutate(site="san jose state university",
         .before="date")
```

- stanford university

``` r
su_data <- as_tibble(read.csv("/Users/kenjinchang/github/institutional-ingredient-repurposing/parent-data/stanford-university.csv",skip=1)) %>%
  filter(!row_number() %in% c(1)) %>%
  select(!X) %>%
  rename("date"=Site..Stanford,"prod_vol_purch"=Total.product.purchased,"prod_type"=Available.Product.Type..to.Prepare.for.Service.,"prod_vol_avail_upcyc"=Total.available.product.to.repurpose..after.primary.fabrication.or.preparation.,"prod_type_unavail_upcyc"=Product.Unable.to.Repurpose,"prod_vol_unavail_upcyc"=Total.available.product.left.that.cannot.be.repurposed..remaining.product.waste.,"meal_period"=Meal.Period.Utilization.Recipe.Served,"meal_swipes"=Meal.swipes.per.period,"recipe_served"=Recipe.Name,"prod_vol_in_recipe"=Volume.of.Repurposed.Ingredient.Used.in.Recipe,"recipe_cost"=Cost.per.recipe,"recipe_servings"=Servings.per.recipe,"displaced_recipe"=Displaced.Recipe.Name..Optional..if.replacing.something.on.the.line.,"displaced_recipe_vol"=Volume.that.would.have.been.served,"displaced_recipe_cost"=Cost.per.recipe.1,"displaced_recipe_servings"=Servings.per.recipe.1) %>%
  mutate(site="stanford university",
         .before="date")
```

- university of bristol

``` r
ub_data <- as_tibble(read.csv("/Users/kenjinchang/github/institutional-ingredient-repurposing/parent-data/university-of-bristol.csv",skip=1)) %>%
  filter(!row_number() %in% c(1)) %>%
  select(!X) %>%
  rename("date"=Site..Bristol,"prod_vol_purch"=Total.product.purchased,"prod_type"=Available.Product.Type..to.Prepare.for.Service.,"prod_vol_avail_upcyc"=Total.available.product.to.repurpose..after.primary.fabrication.or.preparation.,"prod_type_unavail_upcyc"=Product.Unable.to.Repurpose,"prod_vol_unavail_upcyc"=Total.available.product.left.that.cannot.be.repurposed..remaining.product.waste.,"meal_period"=Meal.Period.Utilization.Recipe.Served,"meal_swipes"=Meal.swipes.per.period,"recipe_served"=Recipe.Name,"prod_vol_in_recipe"=Volume.of.Repurposed.Ingredient.Used.in.Recipe,"recipe_cost"=Cost.per.recipe,"recipe_servings"=Servings.per.recipe,"displaced_recipe"=Displaced.Recipe.Name..Optional..if.replacing.something.on.the.line.,"displaced_recipe_vol"=Volume.that.would.have.been.served,"displaced_recipe_cost"=Cost.per.recipe.1,"displaced_recipe_servings"=Servings.per.recipe.1) %>%
  mutate(site="university of bristol",
         .before="date")
```

- university of california los angeles

``` r
ucla_data <- as_tibble(read.csv("/Users/kenjinchang/github/institutional-ingredient-repurposing/parent-data/university-of-california-los-angeles.csv",skip=1)) %>%
  filter(!row_number() %in% c(1)) %>%
  select(!X) %>%
  rename("date"=Site..UCLA,"prod_vol_purch"=Total.product.purchased,"prod_type"=Available.Product.Type..to.Prepare.for.Service.,"prod_vol_avail_upcyc"=Total.available.product.to.repurpose..after.primary.fabrication.or.preparation.,"prod_type_unavail_upcyc"=Product.Unable.to.Repurpose,"prod_vol_unavail_upcyc"=Total.available.product.left.that.cannot.be.repurposed..remaining.product.waste.,"meal_period"=Meal.Period.Utilization.Recipe.Served,"meal_swipes"=Meal.swipes.per.period,"recipe_served"=Recipe.Name,"prod_vol_in_recipe"=Volume.of.Repurposed.Ingredient.Used.in.Recipe,"recipe_cost"=Cost.per.recipe,"recipe_servings"=Servings.per.recipe,"displaced_recipe"=Displaced.Recipe.Name..Optional..if.replacing.something.on.the.line.,"displaced_recipe_vol"=Volume.that.would.have.been.served,"displaced_recipe_cost"=Cost.per.recipe.1,"displaced_recipe_servings"=Servings.per.recipe.1) %>%
  mutate(site="university of california los angeles",
         .before="date")
```

- university of michigan

``` r
um_data <- as_tibble(read.csv("/Users/kenjinchang/github/institutional-ingredient-repurposing/parent-data/university-of-michigan.csv",skip=1)) %>%
  filter(!row_number() %in% c(1)) %>%
  select(!X) %>%
  rename("date"=Site..University.of.Michigan,"prod_vol_purch"=Total.product.purchased,"prod_type"=Available.Product.Type..to.Prepare.for.Service.,"prod_vol_avail_upcyc"=Total.available.product.to.repurpose..after.primary.fabrication.or.preparation.,"prod_type_unavail_upcyc"=Product.Unable.to.Repurpose,"prod_vol_unavail_upcyc"=Total.available.product.left.that.cannot.be.repurposed..remaining.product.waste.,"meal_period"=Meal.Period.Utilization.Recipe.Served,"meal_swipes"=Meal.swipes.per.period,"recipe_served"=Recipe.Name,"prod_vol_in_recipe"=Volume.of.Repurposed.Ingredient.Used.in.Recipe,"recipe_cost"=Cost.per.recipe,"recipe_servings"=Servings.per.recipe,"displaced_recipe"=Displaced.Recipe.Name..Optional..if.replacing.something.on.the.line.,"displaced_recipe_vol"=Volume.that.would.have.been.served,"displaced_recipe_cost"=Cost.per.recipe.1,"displaced_recipe_servings"=Servings.per.recipe.1) %>%
  mutate(site="university of michigan",
         .before="date")
```

- university of north texas

``` r
unt_data <- as_tibble(read.csv("/Users/kenjinchang/github/institutional-ingredient-repurposing/parent-data/university-of-north-texas.csv",skip=1)) %>%
  filter(!row_number() %in% c(1)) %>%
  select(!X) %>%
  rename("date"=Site..UNT,"prod_vol_purch"=Total.product.purchased,"prod_type"=Available.Product.Type..to.Prepare.for.Service.,"prod_vol_avail_upcyc"=Total.available.product.to.repurpose..after.primary.fabrication.or.preparation.,"prod_type_unavail_upcyc"=Product.Unable.to.Repurpose,"prod_vol_unavail_upcyc"=Total.available.product.left.that.cannot.be.repurposed..remaining.product.waste.,"meal_period"=Meal.Period.Utilization.Recipe.Served,"meal_swipes"=Meal.swipes.per.period,"recipe_served"=Recipe.Name,"prod_vol_in_recipe"=Volume.of.Repurposed.Ingredient.Used.in.Recipe,"recipe_cost"=Cost.per.recipe,"recipe_servings"=Servings.per.recipe,"displaced_recipe"=Displaced.Recipe.Name..Optional..if.replacing.something.on.the.line.,"displaced_recipe_vol"=Volume.that.would.have.been.served,"displaced_recipe_cost"=Cost.per.recipe.1,"displaced_recipe_servings"=Servings.per.recipe.1) %>%
  mutate(site="university of north texas",
         .before="date")
```

- university of reading

``` r
ur_data <- as_tibble(read.csv("/Users/kenjinchang/github/institutional-ingredient-repurposing/parent-data/university-of-reading.csv",skip=1)) %>%
  filter(!row_number() %in% c(1)) %>%
  select(!X) %>%
  rename("date"=Site..University.of.Reading,"prod_vol_purch"=Total.product.purchased,"prod_type"=Available.Product.Type..to.Prepare.for.Service.,"prod_vol_avail_upcyc"=Total.available.product.to.repurpose..after.primary.fabrication.or.preparation.,"prod_type_unavail_upcyc"=Product.Unable.to.Repurpose,"prod_vol_unavail_upcyc"=Total.available.product.left.that.cannot.be.repurposed..remaining.product.waste.,"meal_period"=Meal.Period.Utilization.Recipe.Served,"meal_swipes"=Meal.swipes.per.period,"recipe_served"=Recipe.Name,"prod_vol_in_recipe"=Volume.of.Repurposed.Ingredient.Used.in.Recipe,"recipe_cost"=Cost.per.recipe,"recipe_servings"=Servings.per.recipe,"displaced_recipe"=Displaced.Recipe.Name..Optional..if.replacing.something.on.the.line.,"displaced_recipe_vol"=Volume.that.would.have.been.served,"displaced_recipe_cost"=Cost.per.recipe.1,"displaced_recipe_servings"=Servings.per.recipe.1) %>%
  mutate(site="university of reading",
         .before="date")
```

- vanderbilt university

``` r
vu_data <- as_tibble(read.csv("/Users/kenjinchang/github/institutional-ingredient-repurposing/parent-data/vanderbilt-university.csv",skip=1)) %>%
  filter(!row_number() %in% c(1)) %>%
  select(!X) %>%
  rename("date"=Site..Vanderbilt,"prod_vol_purch"=Total.product.purchased,"prod_type"=Available.Product.Type..to.Prepare.for.Service.,"prod_vol_avail_upcyc"=Total.available.product.to.repurpose..after.primary.fabrication.or.preparation.,"prod_type_unavail_upcyc"=Product.Unable.to.Repurpose,"prod_vol_unavail_upcyc"=Total.available.product.left.that.cannot.be.repurposed..remaining.product.waste.,"meal_period"=Meal.Period.Utilization.Recipe.Served,"meal_swipes"=Meal.swipes.per.period,"recipe_served"=Recipe.Name,"prod_vol_in_recipe"=Volume.of.Repurposed.Ingredient.Used.in.Recipe,"recipe_cost"=Cost.per.recipe,"recipe_servings"=Servings.per.recipe,"displaced_recipe"=Displaced.Recipe.Name..Optional..if.replacing.something.on.the.line.,"displaced_recipe_vol"=Volume.that.would.have.been.served,"displaced_recipe_cost"=Cost.per.recipe.1,"displaced_recipe_servings"=Servings.per.recipe.1) %>%
  mutate(site="vanderbilt university",
         .before="date")
```

now that we have loaded in all of the data from the participating sites,
we can begin the process of aggregating the individual site-level data

``` r
aggregated_data <- bind_rows(bu_data,cia_data,sjsu_data,su_data,ub_data,ucla_data,um_data,unt_data,ur_data,vu_data)
aggregated_data
```

    ## # A tibble: 745 × 17
    ##    site    date  prod_…¹ prod_…² prod_…³ prod_…⁴ prod_…⁵ meal_…⁶ meal_…⁷ recip…⁸
    ##    <chr>   <chr> <chr>   <chr>   <chr>   <chr>   <chr>   <chr>   <chr>   <chr>  
    ##  1 boston… Oct … "190"   pineap… "85.75" "tops,… "68.75" Breakf… 978     "pinea…
    ##  2 boston… Oct … "60"    brocco… "7.25"  "stem … "1"     Lunch   2447    "pinea…
    ##  3 boston… Oct … "30"    cilant… "3"     "stem"  "0.5"   Dinner  1755    ""     
    ##  4 boston… Oct … "220"   pineap… "99.3"  "tops,… "79.7"  Breakf… 1146    "pinea…
    ##  5 boston… Oct … "60"    brocco… "7.7"   "stem … "1.1"   Lunch   2153    "pinea…
    ##  6 boston… Oct … "30"    cilant… "3.4"   "stem"  "0.6"   Dinner  1611    ""     
    ##  7 boston… Oct … "189"   pineap… "85.3"  "tops,… "68.4"  Breakf… 1003    "pinea…
    ##  8 boston… Oct … "42"    brocco… "5.4"   "stem … "0.7"   Lunch   2314    "pinea…
    ##  9 boston… Oct … "60"    cilant… "6.7"   "stem"  "1.1"   Dinner  1614    "pickl…
    ## 10 boston… Oct … ""      pineap… ""      ""      ""      Breakf… 1085    "pinea…
    ## # … with 735 more rows, 7 more variables: prod_vol_in_recipe <chr>,
    ## #   recipe_cost <chr>, recipe_servings <chr>, displaced_recipe <chr>,
    ## #   displaced_recipe_vol <chr>, displaced_recipe_cost <chr>,
    ## #   displaced_recipe_servings <chr>, and abbreviated variable names
    ## #   ¹​prod_vol_purch, ²​prod_type, ³​prod_vol_avail_upcyc,
    ## #   ⁴​prod_type_unavail_upcyc, ⁵​prod_vol_unavail_upcyc, ⁶​meal_period,
    ## #   ⁷​meal_swipes, ⁸​recipe_served

## data cleaning

now that we’ve aggregated the data from the participating sites into one
consolidated table, we’ll need to modify the structure of the data
and—in some cases—the inputted values, both to align the dataset with
our analyses and to map site-provided responses with site-implemented
practices

in addition, there are other reporting discrepancies that require
standardization

we trace, detail, and justify each of these modifications below:

- to address this initial concern, we will convert the columns
  containing numeric data from character strings (i.e., `<chr>`) to
  double strings (i.e., `<dbl>`)

more specifically, this includes the `prod_vol_purch`,
`prod_vol_avail_upcyc`, `prod_vol_unavail_upcyc`, `meal_swipes`,
`prod_vol_in_recipe`, `recipe_cost`, `recipe_servings`,
`displaced_recipe_vol`, `displaced_recipe_cost`,
`displaced_recipe_servings` variables.

``` r
aggregated_data <- aggregated_data %>% 
  mutate(prod_vol_purch=as.numeric(prod_vol_purch)) %>%
  mutate(prod_vol_avail_upcyc=as.numeric(prod_vol_avail_upcyc)) %>%
  mutate(prod_vol_unavail_upcyc=as.numeric(prod_vol_unavail_upcyc)) %>%
  mutate(meal_swipes=as.numeric(meal_swipes)) %>%
  mutate(prod_vol_in_recipe=as.numeric(prod_vol_in_recipe)) %>%
  mutate(recipe_cost=as.numeric(recipe_cost)) %>%
  mutate(recipe_servings=as.numeric(recipe_servings)) %>%
  mutate(displaced_recipe_vol=as.numeric(displaced_recipe_vol)) %>%
  mutate(displaced_recipe_cost=as.numeric(displaced_recipe_cost)) %>%
  mutate(displaced_recipe_servings=as.numeric(displaced_recipe_servings)) 
```

    ## Warning in mask$eval_all_mutate(quo): NAs introduced by coercion

    ## Warning in mask$eval_all_mutate(quo): NAs introduced by coercion

    ## Warning in mask$eval_all_mutate(quo): NAs introduced by coercion

    ## Warning in mask$eval_all_mutate(quo): NAs introduced by coercion

    ## Warning in mask$eval_all_mutate(quo): NAs introduced by coercion

    ## Warning in mask$eval_all_mutate(quo): NAs introduced by coercion

- unit conversions

in addition, for the two participating sites in the UK (i.e., university
of bristol and university of reading), we will need to perform
conversions for the provided cost and weight values, transforming them
from costs represented in pound sterlings to costs represented in USD
and from weights represented in kilograms to weights represented in
pounds

to accomplish this, we’ll perform a conditional mutate that applies
conversion factors of 2.205 (4sf) and 1.27 (3sf) to weight and cost
values, respectively, that map onto those two institutions

more specifically, we will do this to each of the variables representing
cost indices (i.e., `recipe_cost` and `displaced_recipe_cost`) and each
of the variables representing weight indices (i.e., `prod_vol_purch`,
`prod_vol_avail_upcyc`, `prod_vol_unavail_upcyc`, `prod_vol_in_recipe`,
and `displaced_recipe_vol`)

``` r
aggregated_data <- aggregated_data %>%
  mutate(case_when(site=="university of bristol" ~ recipe_cost==recipe_cost*1.27)) %>%
  mutate(case_when(site=="university of bristol" ~ displaced_recipe_cost==displaced_recipe_cost*1.27)) %>%
  mutate(case_when(site=="university of bristol" ~ prod_vol_purch==prod_vol_purch*2.205)) %>%
  mutate(case_when(site=="university of bristol" ~ prod_vol_avail_upcyc==prod_vol_avail_upcyc*2.205)) %>%
  mutate(case_when(site=="university of bristol" ~ prod_vol_unavail_upcyc==prod_vol_unavail_upcyc*2.205)) %>%
  mutate(case_when(site=="university of bristol" ~ prod_vol_in_recipe==prod_vol_in_recipe*2.205)) %>%
  mutate(case_when(site=="university of reading" ~ recipe_cost==recipe_cost*1.27)) %>%
  mutate(case_when(site=="university of reading" ~ displaced_recipe_cost==displaced_recipe_cost*1.27)) %>%
  mutate(case_when(site=="university of reading" ~ prod_vol_purch==prod_vol_purch*2.205)) %>%
  mutate(case_when(site=="university of reading" ~ prod_vol_avail_upcyc==prod_vol_avail_upcyc*2.205)) %>%
  mutate(case_when(site=="university of reading" ~ prod_vol_unavail_upcyc==prod_vol_unavail_upcyc*2.205)) %>%
  mutate(case_when(site=="university of reading" ~ prod_vol_in_recipe==prod_vol_in_recipe*2.205)) 
```

—- ^^^ still need to spot check whether this worked as intended ^^^ —-

- populating residual cells at day level

because the tibble contains data that correspond to different time
intervals (i.e., some data that correspond to a day and some data that
correspond to a meal period), we also need to align respondents’
day-level data inputs with all cells that correspond to that same day.
said differently, there are either three or four rows corresponding to
each day for each site, and the inputs that correspond to that day are
frequently represented under just one of those three or four sets of
cells.

### data writing and codebook documentation

``` r
write.csv(aggregated_data,file="/Users/kenjinchang/github/institutional-ingredient-repurposing/data/aggregated-data.csv")
```

| Variable ID               | Variable Description                                                                                                                               |
|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|
| site                      | The site at which the data was collected.                                                                                                          |
| date                      | The date during which the data was collected.                                                                                                      |
| prod_vol_purch            | The total amount of the ingredient, or product, that was purchased by weight (in pounds).                                                          |
| prod_type                 | The type of ingredient, or product, that was available to be repurposed, or upcycled.                                                              |
| prod_vol_avail_upcyc      | The total amount of the ingredient, or product, that was available to be repurposed, or upcycled, by weight after primary fabrication (in pounds). |
| prod_type_unavail_upcyc   | The segments or portions of the available product that were not able to be repurposed, or upcycled.                                                |
| prod_vol_unavail_upcyc    | The total amount of the ingredient, or product, that was unable to be repurposed, or upcycled, by weight (in pounds).                              |
| meal_period               | The meal period, or interval, where the recipe was served                                                                                          |
| meal_swipes               | The number of entrance swipes that occurred during the corresponding meal period where the upcycled recipe was served (by count).                  |
| recipe_served             | The name of the recipe served.                                                                                                                     |
| prod_vol_in_recipe        | The total amount of the upcycled, or repurposed, ingredient used in the recipe by weight (in pounds).                                              |
| recipe_cost               | The estimated cost of the served recipe (in USD).                                                                                                  |
| recipe_servings           | The estimated number of servings yielded by the recipe.                                                                                            |
| displaced_recipe          | The name of the recipe that would have otherwise been served had the repurposed recipe not been made.                                              |
| displaced_recipe_vol      | The total amount of recipe that would have otherwise been served by weight (in pounds).                                                            |
| displaced_recipe_cost     | The estimated cost of the recipe that would have otherwise been served (in USD).                                                                   |
| displaced_recipe_servings | The estimated number of servings yielded by the recipe that would have otherwise been served.                                                      |

metadata \<- 1:17tibble(rows=c())
