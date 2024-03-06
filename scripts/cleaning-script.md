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
bu_data <- as_tibble(read.csv("/Users/kenjinchang/github/institutional-ingredient-repurposing/parent-data/csvs/boston-university.csv",skip=1)) %>%
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
cia_data <- as_tibble(read.csv("/Users/kenjinchang/github/institutional-ingredient-repurposing/parent-data/csvs/culinary-institute-of-america.csv",skip=1)) %>%
  filter(!row_number() %in% c(1)) %>%
  select(!X) %>%
  rename("date"=Site..CIA,"prod_vol_purch"=Total.product.purchased,"prod_type"=Available.Product.Type..to.Prepare.for.Service.,"prod_vol_avail_upcyc"=Total.available.product.to.repurpose..after.primary.fabrication.or.preparation.,"prod_type_unavail_upcyc"=Product.Unable.to.Repurpose,"prod_vol_unavail_upcyc"=Total.available.product.left.that.cannot.be.repurposed..remaining.product.waste.,"meal_period"=Meal.Period.Utilization.Recipe.Served,"meal_swipes"=Meal.swipes.per.period,"recipe_served"=Recipe.Name,"prod_vol_in_recipe"=Volume.of.Repurposed.Ingredient.Used.in.Recipe,"recipe_cost"=Cost.per.recipe,"recipe_servings"=Servings.per.recipe,"displaced_recipe"=Displaced.Recipe.Name..Optional..if.replacing.something.on.the.line.,"displaced_recipe_vol"=Volume.that.would.have.been.served,"displaced_recipe_cost"=Cost.per.recipe.1,"displaced_recipe_servings"=Servings.per.recipe.1) %>%
  mutate(site="culinary institute of america",
         .before="date")
```

- rutgers university

``` r
ru_data <- as_tibble(read.csv("/Users/kenjinchang/github/institutional-ingredient-repurposing/parent-data/csvs/rutgers-university.csv",skip=1)) %>%
  filter(!row_number() %in% c(1)) %>%
  select(!X) %>%
  rename("date"=Site..Rutgers,"prod_vol_purch"=Total.product.purchased,"prod_type"=Available.Product.Type..to.Prepare.for.Service.,"prod_vol_avail_upcyc"=Total.available.product.to.repurpose..after.primary.fabrication.or.preparation.,"prod_type_unavail_upcyc"=Product.Unable.to.Repurpose,"prod_vol_unavail_upcyc"=Total.available.product.left.that.cannot.be.repurposed..remaining.product.waste.,"meal_period"=Meal.Period.Utilization.Recipe.Served,"meal_swipes"=Meal.swipes.per.period,"recipe_served"=Recipe.Name,"prod_vol_in_recipe"=Volume.of.Repurposed.Ingredient.Used.in.Recipe,"recipe_cost"=Cost.per.recipe,"recipe_servings"=Servings.per.recipe,"displaced_recipe"=Displaced.Recipe.Name..Optional..if.replacing.something.on.the.line.,"displaced_recipe_vol"=Volume.that.would.have.been.served,"displaced_recipe_cost"=Cost.per.recipe.1,"displaced_recipe_servings"=Servings.per.recipe.1) %>%
  mutate(site="rutgers university",
         .before="date")
```

- san jose state university

``` r
sjsu_data <- as_tibble(read.csv("/Users/kenjinchang/github/institutional-ingredient-repurposing/parent-data/csvs/san-jose-state-university.csv",skip=1)) %>%
  filter(!row_number() %in% c(1)) %>%
  select(!X) %>%
  rename("date"=Site..SJSU,"prod_vol_purch"=Total.product.purchased,"prod_type"=Available.Product.Type..to.Prepare.for.Service.,"prod_vol_avail_upcyc"=Total.available.product.to.repurpose..after.primary.fabrication.or.preparation.,"prod_type_unavail_upcyc"=Product.Unable.to.Repurpose,"prod_vol_unavail_upcyc"=Total.available.product.left.that.cannot.be.repurposed..remaining.product.waste.,"meal_period"=Meal.Period.Utilization.Recipe.Served,"meal_swipes"=Meal.swipes.per.period,"recipe_served"=Recipe.Name,"prod_vol_in_recipe"=Volume.of.Repurposed.Ingredient.Used.in.Recipe,"recipe_cost"=Cost.per.recipe,"recipe_servings"=Servings.per.recipe,"displaced_recipe"=Displaced.Recipe.Name..Optional..if.replacing.something.on.the.line.,"displaced_recipe_vol"=Volume.that.would.have.been.served,"displaced_recipe_cost"=Cost.per.recipe.1,"displaced_recipe_servings"=Servings.per.recipe.1) %>%
  mutate(site="san jose state university",
         .before="date")
```

- stanford university

``` r
su_data <- as_tibble(read.csv("/Users/kenjinchang/github/institutional-ingredient-repurposing/parent-data/csvs/stanford-university.csv",skip=1)) %>%
  filter(!row_number() %in% c(1)) %>%
  select(!X) %>%
  rename("date"=Site..Stanford,"prod_vol_purch"=Total.product.purchased,"prod_type"=Available.Product.Type..to.Prepare.for.Service.,"prod_vol_avail_upcyc"=Total.available.product.to.repurpose..after.primary.fabrication.or.preparation.,"prod_type_unavail_upcyc"=Product.Unable.to.Repurpose,"prod_vol_unavail_upcyc"=Total.available.product.left.that.cannot.be.repurposed..remaining.product.waste.,"meal_period"=Meal.Period.Utilization.Recipe.Served,"meal_swipes"=Meal.swipes.per.period,"recipe_served"=Recipe.Name,"prod_vol_in_recipe"=Volume.of.Repurposed.Ingredient.Used.in.Recipe,"recipe_cost"=Cost.per.recipe,"recipe_servings"=Servings.per.recipe,"displaced_recipe"=Displaced.Recipe.Name..Optional..if.replacing.something.on.the.line.,"displaced_recipe_vol"=Volume.that.would.have.been.served,"displaced_recipe_cost"=Cost.per.recipe.1,"displaced_recipe_servings"=Servings.per.recipe.1) %>%
  mutate(site="stanford university",
         .before="date")
```

- university of bristol

``` r
ub_data <- as_tibble(read.csv("/Users/kenjinchang/github/institutional-ingredient-repurposing/parent-data/csvs/university-of-bristol.csv",skip=1)) %>%
  filter(!row_number() %in% c(1)) %>%
  select(!X) %>%
  rename("date"=Site..Bristol,"prod_vol_purch"=Total.product.purchased,"prod_type"=Available.Product.Type..to.Prepare.for.Service.,"prod_vol_avail_upcyc"=Total.available.product.to.repurpose..after.primary.fabrication.or.preparation.,"prod_type_unavail_upcyc"=Product.Unable.to.Repurpose,"prod_vol_unavail_upcyc"=Total.available.product.left.that.cannot.be.repurposed..remaining.product.waste.,"meal_period"=Meal.Period.Utilization.Recipe.Served,"meal_swipes"=Meal.swipes.per.period,"recipe_served"=Recipe.Name,"prod_vol_in_recipe"=Volume.of.Repurposed.Ingredient.Used.in.Recipe,"recipe_cost"=Cost.per.recipe,"recipe_servings"=Servings.per.recipe,"displaced_recipe"=Displaced.Recipe.Name..Optional..if.replacing.something.on.the.line.,"displaced_recipe_vol"=Volume.that.would.have.been.served,"displaced_recipe_cost"=Cost.per.recipe.1,"displaced_recipe_servings"=Servings.per.recipe.1) %>%
  mutate(site="university of bristol",
         .before="date")
```

- university of california los angeles

``` r
ucla_data <- as_tibble(read.csv("/Users/kenjinchang/github/institutional-ingredient-repurposing/parent-data/csvs/university-of-california-los-angeles.csv",skip=1)) %>%
  filter(!row_number() %in% c(1)) %>%
  select(!X) %>%
  rename("date"=Site..UCLA,"prod_vol_purch"=Total.product.purchased,"prod_type"=Available.Product.Type..to.Prepare.for.Service.,"prod_vol_avail_upcyc"=Total.available.product.to.repurpose..after.primary.fabrication.or.preparation.,"prod_type_unavail_upcyc"=Product.Unable.to.Repurpose,"prod_vol_unavail_upcyc"=Total.available.product.left.that.cannot.be.repurposed..remaining.product.waste.,"meal_period"=Meal.Period.Utilization.Recipe.Served,"meal_swipes"=Meal.swipes.per.period,"recipe_served"=Recipe.Name,"prod_vol_in_recipe"=Volume.of.Repurposed.Ingredient.Used.in.Recipe,"recipe_cost"=Cost.per.recipe,"recipe_servings"=Servings.per.recipe,"displaced_recipe"=Displaced.Recipe.Name..Optional..if.replacing.something.on.the.line.,"displaced_recipe_vol"=Volume.that.would.have.been.served,"displaced_recipe_cost"=Cost.per.recipe.1,"displaced_recipe_servings"=Servings.per.recipe.1) %>%
  mutate(site="university of california los angeles",
         .before="date")
```

- university of michigan

``` r
um_data <- as_tibble(read.csv("/Users/kenjinchang/github/institutional-ingredient-repurposing/parent-data/csvs/university-of-michigan.csv",skip=1)) %>%
  filter(!row_number() %in% c(1)) %>%
  select(!X) %>%
  rename("date"=Site..University.of.Michigan,"prod_vol_purch"=Total.product.purchased,"prod_type"=Available.Product.Type..to.Prepare.for.Service.,"prod_vol_avail_upcyc"=Total.available.product.to.repurpose..after.primary.fabrication.or.preparation.,"prod_type_unavail_upcyc"=Product.Unable.to.Repurpose,"prod_vol_unavail_upcyc"=Total.available.product.left.that.cannot.be.repurposed..remaining.product.waste.,"meal_period"=Meal.Period.Utilization.Recipe.Served,"meal_swipes"=Meal.swipes.per.period,"recipe_served"=Recipe.Name,"prod_vol_in_recipe"=Volume.of.Repurposed.Ingredient.Used.in.Recipe,"recipe_cost"=Cost.per.recipe,"recipe_servings"=Servings.per.recipe,"displaced_recipe"=Displaced.Recipe.Name..Optional..if.replacing.something.on.the.line.,"displaced_recipe_vol"=Volume.that.would.have.been.served,"displaced_recipe_cost"=Cost.per.recipe.1,"displaced_recipe_servings"=Servings.per.recipe.1) %>%
  mutate(site="university of michigan",
         .before="date")
```

- university of north texas

``` r
unt_data <- as_tibble(read.csv("/Users/kenjinchang/github/institutional-ingredient-repurposing/parent-data/csvs/university-of-north-texas.csv",skip=1)) %>%
  filter(!row_number() %in% c(1)) %>%
  select(!X) %>%
  rename("date"=Site..UNT,"prod_vol_purch"=Total.product.purchased,"prod_type"=Available.Product.Type..to.Prepare.for.Service.,"prod_vol_avail_upcyc"=Total.available.product.to.repurpose..after.primary.fabrication.or.preparation.,"prod_type_unavail_upcyc"=Product.Unable.to.Repurpose,"prod_vol_unavail_upcyc"=Total.available.product.left.that.cannot.be.repurposed..remaining.product.waste.,"meal_period"=Meal.Period.Utilization.Recipe.Served,"meal_swipes"=Meal.swipes.per.period,"recipe_served"=Recipe.Name,"prod_vol_in_recipe"=Volume.of.Repurposed.Ingredient.Used.in.Recipe,"recipe_cost"=Cost.per.recipe,"recipe_servings"=Servings.per.recipe,"displaced_recipe"=Displaced.Recipe.Name..Optional..if.replacing.something.on.the.line.,"displaced_recipe_vol"=Volume.that.would.have.been.served,"displaced_recipe_cost"=Cost.per.recipe.1,"displaced_recipe_servings"=Servings.per.recipe.1) %>%
  mutate(site="university of north texas",
         .before="date")
```

- university of reading

``` r
ur_data <- as_tibble(read.csv("/Users/kenjinchang/github/institutional-ingredient-repurposing/parent-data/csvs/university-of-reading.csv",skip=1)) %>%
  filter(!row_number() %in% c(1)) %>%
  select(!X) %>%
  rename("date"=Site..University.of.Reading,"prod_vol_purch"=Total.product.purchased,"prod_type"=Available.Product.Type..to.Prepare.for.Service.,"prod_vol_avail_upcyc"=Total.available.product.to.repurpose..after.primary.fabrication.or.preparation.,"prod_type_unavail_upcyc"=Product.Unable.to.Repurpose,"prod_vol_unavail_upcyc"=Total.available.product.left.that.cannot.be.repurposed..remaining.product.waste.,"meal_period"=Meal.Period.Utilization.Recipe.Served,"meal_swipes"=Meal.swipes.per.period,"recipe_served"=Recipe.Name,"prod_vol_in_recipe"=Volume.of.Repurposed.Ingredient.Used.in.Recipe,"recipe_cost"=Cost.per.recipe,"recipe_servings"=Servings.per.recipe,"displaced_recipe"=Displaced.Recipe.Name..Optional..if.replacing.something.on.the.line.,"displaced_recipe_vol"=Volume.that.would.have.been.served,"displaced_recipe_cost"=Cost.per.recipe.1,"displaced_recipe_servings"=Servings.per.recipe.1) %>%
  mutate(site="university of reading",
         .before="date")
```

- vanderbilt university

``` r
vu_data <- as_tibble(read.csv("/Users/kenjinchang/github/institutional-ingredient-repurposing/parent-data/csvs/vanderbilt-university.csv",skip=1)) %>%
  filter(!row_number() %in% c(1)) %>%
  select(!X) %>%
  rename("date"=Site..Vanderbilt,"prod_vol_purch"=Total.product.purchased,"prod_type"=Available.Product.Type..to.Prepare.for.Service.,"prod_vol_avail_upcyc"=Total.available.product.to.repurpose..after.primary.fabrication.or.preparation.,"prod_type_unavail_upcyc"=Product.Unable.to.Repurpose,"prod_vol_unavail_upcyc"=Total.available.product.left.that.cannot.be.repurposed..remaining.product.waste.,"meal_period"=Meal.Period.Utilization.Recipe.Served,"meal_swipes"=Meal.swipes.per.period,"recipe_served"=Recipe.Name,"prod_vol_in_recipe"=Volume.of.Repurposed.Ingredient.Used.in.Recipe,"recipe_cost"=Cost.per.recipe,"recipe_servings"=Servings.per.recipe,"displaced_recipe"=Displaced.Recipe.Name..Optional..if.replacing.something.on.the.line.,"displaced_recipe_vol"=Volume.that.would.have.been.served,"displaced_recipe_cost"=Cost.per.recipe.1,"displaced_recipe_servings"=Servings.per.recipe.1) %>%
  mutate(site="vanderbilt university",
         .before="date")
```

now that we have loaded in all of the data from the participating sites,
we can begin the process of aggregating the individual site-level data

``` r
aggregated_data <- bind_rows(bu_data,cia_data,ru_data,sjsu_data,su_data,ub_data,ucla_data,um_data,unt_data,ur_data,vu_data)
aggregated_data
```

    ## # A tibble: 803 × 17
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
    ## # … with 793 more rows, 7 more variables: prod_vol_in_recipe <chr>,
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
our analytical approach and to map site-provided responses (i.e., what
respondents reported) with site-implemented practices (i.e., what sites
did in practice)

in addition, there are some other reporting discrepancies that require
standardization across sites (e.g., differences in the units reported
for cost and weight)

we trace, detail, and justify each of these modifications below:

- format alignment

because there are subtle differences in how respondents’ inputted their
site-level information, there are some formatting refinements that need
to be made in order accommodate some downstream functions

to identify these particularities, we consult the parent data in the
`xlsxs` folder

*san jose state university*

here, we see that the respondent appended unit identifiers within the
columns containing weight values. this will ultimately introduce `NA`
values by coercion in the subsequent step, where we convert columns
containing numeric data from character strings to double-class vectors

to subvert this issue, which will invariably result in a loss of data,
we will need to first remove these identifiers from the corresponding
strings

``` r
sjsu_data %>%
  mutate(prod_vol_purch=str_replace(prod_vol_purch,"lb","")) %>%
  mutate(prod_vol_purch=str_replace(prod_vol_purch,"oz",""))
```

    ## # A tibble: 69 × 17
    ##    site    date  prod_…¹ prod_…² prod_…³ prod_…⁴ prod_…⁵ meal_…⁶ meal_…⁷ recip…⁸
    ##    <chr>   <chr> <chr>   <chr>   <chr>   <chr>   <chr>   <chr>   <chr>   <chr>  
    ##  1 san jo… 23-O… ""      ""      ""      ""      ""      "Break… ""      ""     
    ##  2 san jo… 23-O… ""      ""      ""      ""      ""      "Lunch… ""      ""     
    ##  3 san jo… 23-O… ""      ""      ""      ""      ""      "Dinne… ""      ""     
    ##  4 san jo… 24-O… "50 "   "Yello… "5 lb"  "Stem"  "8 oz " "Break… "750"   ""     
    ##  5 san jo… 24-O… ""      ""      ""      "Slime" "8 oz " "Lunch… "1500"  "Pumpk…
    ##  6 san jo… 24-O… ""      ""      ""      ""      ""      "Dinne… "1500"  ""     
    ##  7 san jo… 25-O… "15 "   ""      ""      "Stem " "4oz"   "Break… ""      ""     
    ##  8 san jo… 25-O… ""      "Figle… "2 lb"  "Slime" "4oz"   "Lunch… "20 (G… "Chila…
    ##  9 san jo… 25-O… ""      ""      ""      ""      ""      "Dinne… ""      ""     
    ## 10 san jo… 26-O… "150 "  "Over … "138 l… "Rinds… "22 lb" "Break… ""      ""     
    ## # … with 59 more rows, 7 more variables: prod_vol_in_recipe <chr>,
    ## #   recipe_cost <chr>, recipe_servings <chr>, displaced_recipe <chr>,
    ## #   displaced_recipe_vol <chr>, displaced_recipe_cost <chr>,
    ## #   displaced_recipe_servings <chr>, and abbreviated variable names
    ## #   ¹​prod_vol_purch, ²​prod_type, ³​prod_vol_avail_upcyc,
    ## #   ⁴​prod_type_unavail_upcyc, ⁵​prod_vol_unavail_upcyc, ⁶​meal_period,
    ## #   ⁷​meal_swipes, ⁸​recipe_served

``` r
###need to disaggregate mint and cilantro###
```

furthermore, we will also need to make a note here to convert the values
that have been provided in ounces and gallons to pounds during the next
cleaning stages

because theseof each (need to separate mint and cilantro) - may need to
create item-specific variable set first

need to convert gallon to pounds (assume water)

- data structure changes

to address this initial concern, we will convert the columns containing
numeric data from character strings (i.e., `<chr>`) to double strings
(i.e., `<dbl>`)

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
intervals (i.e., some data that correspond to a single day and some data
that correspond to a single meal period), we need to align the
product-volume information such that they map onto each of the
individual cells that correspond to the day the product was purchased
and prepared

depending on the site, there are either three or four rows that
correspond to the same date for each site, and the product-volume
inputs—which are composed of the `prod_vol_purch`, `prod_type`,
`prod_vol_avail_upcyc`, `prod_type_unavail_upcyc`, and
`prod_vol_unavail_upcyc` variables—are frequently represented under just
one of those three or four sets of cells for the individual day the
specified ingredient was purchased and/or prepared

to correct this, we will need to consult the parent data tables located
in the `xlsxs` folder within `parent-datasets` and make the
corresponding adjustments in order to accommodate some of our downstream
mutations

this will require us to first generate a new variable describing the
number of ingredients that were purchased or prepared on a given day,
and separate the `prod_vol_purch`, `prod_type`, `prod_vol_avail_upcyc`,
`prod_type_unavail_upcyc`, and `prod_vol_unavail_upcyc` variables across
the maximum number of ingredients purchased in a single day (by creating
adjoining product-volume variables that are specific to each type of
ingredient purchased or prepared)

``` r
aggregated_data
```

    ## # A tibble: 803 × 18
    ##    site    date  prod_…¹ prod_…² prod_…³ prod_…⁴ prod_…⁵ meal_…⁶ meal_…⁷ recip…⁸
    ##    <chr>   <chr>   <dbl> <chr>     <dbl> <chr>     <dbl> <chr>     <dbl> <chr>  
    ##  1 boston… Oct …     190 pineap…   85.8  "tops,…    68.8 Breakf…     978 "pinea…
    ##  2 boston… Oct …      60 brocco…    7.25 "stem …     1   Lunch      2447 "pinea…
    ##  3 boston… Oct …      30 cilant…    3    "stem"      0.5 Dinner     1755 ""     
    ##  4 boston… Oct …     220 pineap…   99.3  "tops,…    79.7 Breakf…    1146 "pinea…
    ##  5 boston… Oct …      60 brocco…    7.7  "stem …     1.1 Lunch      2153 "pinea…
    ##  6 boston… Oct …      30 cilant…    3.4  "stem"      0.6 Dinner     1611 ""     
    ##  7 boston… Oct …     189 pineap…   85.3  "tops,…    68.4 Breakf…    1003 "pinea…
    ##  8 boston… Oct …      42 brocco…    5.4  "stem …     0.7 Lunch      2314 "pinea…
    ##  9 boston… Oct …      60 cilant…    6.7  "stem"      1.1 Dinner     1614 "pickl…
    ## 10 boston… Oct …      NA pineap…   NA    ""         NA   Breakf…    1085 "pinea…
    ## # … with 793 more rows, 8 more variables: prod_vol_in_recipe <dbl>,
    ## #   recipe_cost <dbl>, recipe_servings <dbl>, displaced_recipe <chr>,
    ## #   displaced_recipe_vol <dbl>, displaced_recipe_cost <dbl>,
    ## #   displaced_recipe_servings <dbl>, `case_when(...)` <lgl>, and abbreviated
    ## #   variable names ¹​prod_vol_purch, ²​prod_type, ³​prod_vol_avail_upcyc,
    ## #   ⁴​prod_type_unavail_upcyc, ⁵​prod_vol_unavail_upcyc, ⁶​meal_period,
    ## #   ⁷​meal_swipes, ⁸​recipe_served

\*\*\*\*\*\*note to reflect this in codebook\*\*\*\*\*

count_prod

this will require us to create a new variable-organization system that
maps the previously identified `prod_vol_purch`, `prod_type`,
`prod_vol_avail_upcyc`, `prod_type_unavail_upcyc`, and
`prod_vol_unavail_upcyc` variables onto each day for all ingredients
used. this should help with some downstream mutations, as it will allow
us to retain a record of all the originally input volume-of-product
information while also accommodating our time-interval data structure

said differently, we will construct

``` r
aggregated_data
```

    ## # A tibble: 803 × 18
    ##    site    date  prod_…¹ prod_…² prod_…³ prod_…⁴ prod_…⁵ meal_…⁶ meal_…⁷ recip…⁸
    ##    <chr>   <chr>   <dbl> <chr>     <dbl> <chr>     <dbl> <chr>     <dbl> <chr>  
    ##  1 boston… Oct …     190 pineap…   85.8  "tops,…    68.8 Breakf…     978 "pinea…
    ##  2 boston… Oct …      60 brocco…    7.25 "stem …     1   Lunch      2447 "pinea…
    ##  3 boston… Oct …      30 cilant…    3    "stem"      0.5 Dinner     1755 ""     
    ##  4 boston… Oct …     220 pineap…   99.3  "tops,…    79.7 Breakf…    1146 "pinea…
    ##  5 boston… Oct …      60 brocco…    7.7  "stem …     1.1 Lunch      2153 "pinea…
    ##  6 boston… Oct …      30 cilant…    3.4  "stem"      0.6 Dinner     1611 ""     
    ##  7 boston… Oct …     189 pineap…   85.3  "tops,…    68.4 Breakf…    1003 "pinea…
    ##  8 boston… Oct …      42 brocco…    5.4  "stem …     0.7 Lunch      2314 "pinea…
    ##  9 boston… Oct …      60 cilant…    6.7  "stem"      1.1 Dinner     1614 "pickl…
    ## 10 boston… Oct …      NA pineap…   NA    ""         NA   Breakf…    1085 "pinea…
    ## # … with 793 more rows, 8 more variables: prod_vol_in_recipe <dbl>,
    ## #   recipe_cost <dbl>, recipe_servings <dbl>, displaced_recipe <chr>,
    ## #   displaced_recipe_vol <dbl>, displaced_recipe_cost <dbl>,
    ## #   displaced_recipe_servings <dbl>, `case_when(...)` <lgl>, and abbreviated
    ## #   variable names ¹​prod_vol_purch, ²​prod_type, ³​prod_vol_avail_upcyc,
    ## #   ⁴​prod_type_unavail_upcyc, ⁵​prod_vol_unavail_upcyc, ⁶​meal_period,
    ## #   ⁷​meal_swipes, ⁸​recipe_served

- inputting supplemental survey data

- aligning date format

- mutating new variables

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
