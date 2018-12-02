finalproject
================

``` r
business <- read_csv("./data/business.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   business_id = col_character(),
    ##   name = col_character(),
    ##   neighborhood = col_character(),
    ##   address = col_character(),
    ##   city = col_character(),
    ##   state = col_character(),
    ##   postal_code = col_integer(),
    ##   latitude = col_double(),
    ##   longitude = col_double(),
    ##   stars = col_double(),
    ##   review_count = col_integer(),
    ##   is_open = col_integer(),
    ##   categories = col_character()
    ## )

``` r
attributes <- read_csv("./data/attributes.csv") %>% 
  janitor::clean_names() %>% 
  select(business_id, alcohol)
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_character()
    ## )

    ## See spec(...) for full column specifications.
