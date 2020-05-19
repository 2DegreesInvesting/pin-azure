Creating and using timestamped pins
================

This lesson shows workflows that seem common for data admins and data
analysts working in a team. It shows how to programatically add
timestamped versions of a dataset to an Azure board for pins, and how to
programatically get the latest version. This is how I imagine we could
work at 2Dii, but please do let me know if this won’t work, why, and
hopefully an idea about how we may adapt it.

## Setup

  - Setup your environmental variables: 01\_setup-azure-board.Rmd.
  - Use pins and register the Azure board (expect no output).

<!-- end list -->

``` r
library(pins)
board_register_azure()
```

## A data admin creates a timestamped pin.

Setup the two things that vary: the dataset itself and the dataset name.

``` r
x <- mtcars
name <- "a_dataset"
description <- "A flat file from the database"
```

  - Helper to create a timestamped file name.

<!-- end list -->

``` r
# Helper
timestamp_name <- function(filename) {
  timestamp <- format(Sys.time(), "%F_%H-%M")
  paste0(filename, "_", timestamp)
}
```

  - Pin a new timestamped version of the working dataset.

<!-- end list -->

``` r
(name_now <- timestamp_name(name))
#> [1] "a_dataset_2020-05-19_19-22"
pin(x, name_now, board = "azure", description = description)
#> No encoding supplied: defaulting to UTF-8.
```

  - Explore available timestamped versions on the Azure board.

<!-- end list -->

``` r
tail(pin_find(name), 2)
#> # A tibble: 2 x 4
#>   name                       description                   type  board
#>   <chr>                      <chr>                         <chr> <chr>
#> 1 a_dataset_2020-05-19_19-15 A flat file from the database table azure
#> 2 a_dataset_2020-05-19_19-22 A flat file from the database table azure
```

## Verify the timestamped pins are accumulating

Here I repeat to confirm we create a new timestamped file every time we
call `pin(timestamp_name(name), board = "azure")`.

  - To make this clearer I first wait for a few seconds.

<!-- end list -->

``` r
Sys.sleep(3)
```

  - I now pin a new timestamped version of the working dataset.

<!-- end list -->

``` r
(name_now <- timestamp_name(name))
#> [1] "a_dataset_2020-05-19_19-23"
pin(x, name_now, board = "azure", description = description)
#> No encoding supplied: defaulting to UTF-8.
```

  - Again I explore available timestamped versions on the Azure board.

<!-- end list -->

``` r
tail(pin_find(name), 2)
#> # A tibble: 2 x 4
#>   name                       description                   type  board
#>   <chr>                      <chr>                         <chr> <chr>
#> 1 a_dataset_2020-05-19_19-22 A flat file from the database table azure
#> 2 a_dataset_2020-05-19_19-23 A flat file from the database table azure
```

## An analyst gets the latest timestamped dataset

  - An analyst may get any timestamped version; commonly the latest one.

<!-- end list -->

``` r
# See pins setup: 01_setup-azure-board.Rmd
library(pins)
board_register_azure()

(latest <- dplyr::last(pin_find(name)$name))
#> [1] "a_dataset_2020-05-19_19-23"

pin_get(latest)
#> # A tibble: 32 x 11
#>      mpg   cyl  disp    hp  drat    wt  qsec    vs    am  gear  carb
#>    <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
#>  1  21       6  160    110  3.9   2.62  16.5     0     1     4     4
#>  2  21       6  160    110  3.9   2.88  17.0     0     1     4     4
#>  3  22.8     4  108     93  3.85  2.32  18.6     1     1     4     1
#>  4  21.4     6  258    110  3.08  3.22  19.4     1     0     3     1
#>  5  18.7     8  360    175  3.15  3.44  17.0     0     0     3     2
#>  6  18.1     6  225    105  2.76  3.46  20.2     1     0     3     1
#>  7  14.3     8  360    245  3.21  3.57  15.8     0     0     3     4
#>  8  24.4     4  147.    62  3.69  3.19  20       1     0     4     2
#>  9  22.8     4  141.    95  3.92  3.15  22.9     1     0     4     2
#> 10  19.2     6  168.   123  3.92  3.44  18.3     1     0     4     4
#> # … with 22 more rows
```
