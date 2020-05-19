Creating and using timestamped pins
================

Here I show two workflows that may be useful for a data admin and for a
data analyst. The data admin adds timestamped versions of a dataset to
an Azure board for pins, and the data analyst uses the latest version.
Let me know if this won’t work, why, and hopefully an idea about how we
may adapt it to fit your needs.

## Setup

  - Setup your environmental variables: 01\_setup-azure-board.Rmd.
  - Use pins and register the Azure board (expect no output).

<!-- end list -->

``` r
library(pins)
board_register_azure()
```

## A data admin creates a timestamped pin.

Here I’m setting the few parameters that vary: the dataset itself and
the dataset name.

``` r
x <- mtcars
name <- "a_dataset"
description <- "A flat file from the database"
```

We now create a new timestamped name and use it to create a new pin of
the the dataset we want to share.

``` r
# Helper
timestamp_name <- function(name) {
  timestamp <- format(Sys.time(), "%F_%H-%M-%S")
  paste0(name, "_", timestamp)
}

(name_now <- timestamp_name(name))
#> [1] "a_dataset_2020-05-19_19-48-03"

pin(x, name = name_now, board = "azure", description = description)
#> No encoding supplied: defaulting to UTF-8.
```

Let’s see the last few timestamped versions we have on the Azure board.
I use `tail()` because I created many versions and but we only care
about the last few.

``` r
tail(pin_find(name), 2)
#> # A tibble: 2 x 4
#>   name                          description                   type  board
#>   <chr>                         <chr>                         <chr> <chr>
#> 1 a_dataset_2020-05-19_19-44-17 A flat file from the database table azure
#> 2 a_dataset_2020-05-19_19-48-03 A flat file from the database table azure
```

## Verify the timestamped pins are accumulating

Let’s confirm that (name\_now \<- timestamp\_name(name)I create a new
timestamped version of the) dataset every time I call `pin(name_now,
board = "azure")`. To make this clearer I first wait for a few seconds
so the timestamps are different by more than one second. I then pin yet
another timestamped version of the working dataset.

``` r
Sys.sleep(3)
(name_now <- timestamp_name(name))
#> [1] "a_dataset_2020-05-19_19-48-07"

pin(x, name = name_now, board = "azure", description = description)
#> No encoding supplied: defaulting to UTF-8.
```

And again I explore the last few timestamped versions I now have on the
Azure board.

``` r
tail(pin_find(name), 2)
#> # A tibble: 2 x 4
#>   name                          description                   type  board
#>   <chr>                         <chr>                         <chr> <chr>
#> 1 a_dataset_2020-05-19_19-48-03 A flat file from the database table azure
#> 2 a_dataset_2020-05-19_19-48-07 A flat file from the database table azure
```

## An analyst gets the latest timestamped dataset

An analyst may get any timestamped version; commonly the latest one.

``` r
# See pins setup: 01_setup-azure-board.Rmd
library(pins)
board_register_azure()

(latest <- dplyr::last(pin_find(name)$name))
#> [1] "a_dataset_2020-05-19_19-48-07"

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
