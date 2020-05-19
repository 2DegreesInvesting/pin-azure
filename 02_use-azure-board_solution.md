pins and Azure from a server
================

<https://bit.ly/dsi-pin-azure>

## Objectives

See headings outline.

## Use the pins package and register our Azure board

  - Use the pins package with `library(pins)`.
  - Register our Azure board with `board_register_azure()`

<!-- end list -->

``` r
library(pins)
board_register_azure()
```

## Find datasets in our Azure board

  - Find pins on our Azure board from the Connections tab.
  - Find pins on our Azure board with the Addin “Find pins”.
  - Find pins on our Azure board with `pin_find()`, by name or
    description.

<!-- end list -->

``` r
pin_find("mtcars", board = "azure")
```

    ## # A tibble: 1 x 4
    ##   name   description    type  board
    ##   <chr>  <chr>          <chr> <chr>
    ## 1 mtcars My mtcars data table azure

## Get a dataset from our Azure board

  - Get the `mtcars` dataset from our Azure board with `pin_get()`.
  - Assign the result to the object `mydata`.
  - Inspect mydata however you like. How many rows does it have?

<!-- end list -->

``` r
mydata <- pin_get("mtcars", board = "azure")
mydata
```

    ## # A tibble: 32 x 11
    ##      mpg   cyl  disp    hp  drat    wt  qsec    vs    am  gear  carb
    ##    <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
    ##  1  21       6  160    110  3.9   2.62  16.5     0     1     4     4
    ##  2  21       6  160    110  3.9   2.88  17.0     0     1     4     4
    ##  3  22.8     4  108     93  3.85  2.32  18.6     1     1     4     1
    ##  4  21.4     6  258    110  3.08  3.22  19.4     1     0     3     1
    ##  5  18.7     8  360    175  3.15  3.44  17.0     0     0     3     2
    ##  6  18.1     6  225    105  2.76  3.46  20.2     1     0     3     1
    ##  7  14.3     8  360    245  3.21  3.57  15.8     0     0     3     4
    ##  8  24.4     4  147.    62  3.69  3.19  20       1     0     4     2
    ##  9  22.8     4  141.    95  3.92  3.15  22.9     1     0     4     2
    ## 10  19.2     6  168.   123  3.92  3.44  18.3     1     0     4     4
    ## # … with 22 more rows

## Save processed data to the server’s cache

  - Get the `head()` of `mydata` and assign it to a new object
    `smalldata`.

<!-- end list -->

``` r
smalldata <- head(mydata)
smalldata
```

    ## # A tibble: 6 x 11
    ##     mpg   cyl  disp    hp  drat    wt  qsec    vs    am  gear  carb
    ##   <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
    ## 1  21       6   160   110  3.9   2.62  16.5     0     1     4     4
    ## 2  21       6   160   110  3.9   2.88  17.0     0     1     4     4
    ## 3  22.8     4   108    93  3.85  2.32  18.6     1     1     4     1
    ## 4  21.4     6   258   110  3.08  3.22  19.4     1     0     3     1
    ## 5  18.7     8   360   175  3.15  3.44  17.0     0     0     3     2
    ## 6  18.1     6   225   105  2.76  3.46  20.2     1     0     3     1

  - Store `smalldata` in you local (server) cache with `pin()`

<!-- end list -->

``` r
pin(smalldata)
```

  - Find “smalldata” in your local (server) cache, however you like.

<!-- end list -->

``` r
pin_find("smalldata")
```

    ## # A tibble: 1 x 4
    ##   name      description type  board
    ##   <chr>     <chr>       <chr> <chr>
    ## 1 smalldata ""          table local

  - Get “smalldata” from your local (server) cache.

<!-- end list -->

``` r
pin_get("smalldata", board = "local")
```

    ## # A tibble: 6 x 11
    ##     mpg   cyl  disp    hp  drat    wt  qsec    vs    am  gear  carb
    ##   <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
    ## 1  21       6   160   110  3.9   2.62  16.5     0     1     4     4
    ## 2  21       6   160   110  3.9   2.88  17.0     0     1     4     4
    ## 3  22.8     4   108    93  3.85  2.32  18.6     1     1     4     1
    ## 4  21.4     6   258   110  3.08  3.22  19.4     1     0     3     1
    ## 5  18.7     8   360   175  3.15  3.44  17.0     0     0     3     2
    ## 6  18.1     6   225   105  2.76  3.46  20.2     1     0     3     1

## Visualize the structure of the server’s cache.

  - Create a path to the local (server) cache that pins created for you.

<!-- end list -->

``` r
server_cache <- board_cache_path()
server_cache
```

    ## [1] "/home/rstudio-user/.cache/pins"

  - Explore the structure of the local (server) cache with
    fs::dir\_tree().

<!-- end list -->

``` r
fs::dir_tree(server_cache)
```

    ## /home/rstudio-user/.cache/pins
    ## ├── azure
    ## │   ├── data.txt
    ## │   ├── data.txt.lock
    ## │   ├── iris
    ## │   │   ├── data.rds
    ## │   │   └── data.txt
    ## │   └── mtcars
    ## │       ├── data.rds
    ## │       └── data.txt
    ## └── local
    ##     ├── data.txt
    ##     ├── data.txt.lock
    ##     └── smalldata
    ##         ├── data.csv
    ##         ├── data.rds
    ##         └── data.txt

## Takeaways

  - Go to the collaborative document and write your takeaways.

\<bit.ly/dsi-pin-azure\>
