finalproject
================

#### Loading and tidying the Yelp business dataset

``` r
business <- read_csv("./data/business.csv") %>% 
  mutate(name = str_sub(name, 2, -2)) %>% 
  mutate(address = str_sub(address, 2, -2))
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
categories <- business %>% 
  select(business_id, categories) %>% 
  separate(categories, into = c("cat1", "cat2", "cat3", "cat4", "cat5", "cat6", "cat7", "cat8", "cat9", "cat10", "cat11", "cat12", "cat13", "cat14", "cat15", "cat16", "cat17", "cat18", "cat19", "cat20", "cat21", "cat22", "cat23", "cat24", "cat25"), sep  = ";") %>% 
  gather(key = "cat_id", value = "category", starts_with("cat")) %>% 
  select(-cat_id) 
```

    ## Warning: Expected 25 pieces. Additional pieces discarded in 1 rows [6851].

    ## Warning: Expected 25 pieces. Missing pieces filled with `NA` in 26774
    ## rows [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19,
    ## 20, ...].

``` r
restaurant_ids <- categories %>% 
  filter(category == "Restaurants" | category == "Food") %>% 
  distinct(business_id)

restaurants <- business %>% 
  left_join(restaurant_ids, by = "business_id") %>% 
  select(-categories)

rm(restaurant_ids)
rm(business)
```

#### Loading and tidying the attributes dataset

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

#### Joining the business and attributes datasets

``` r
restaurants <- restaurants %>% 
  left_join(attributes, by = "business_id")
```

#### Creating a plotly of restaurant locations
