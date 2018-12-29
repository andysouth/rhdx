
# rhdx

<!-- README.md is generated from README.Rmd. Please edit that file -->

[![GitLab CI Build
Status](https://gitlab.com/dickoa/rhdx/badges/master/build.svg)](https://gitlab.com/dickoa/rhdx/pipelines)
[![Codecov Code
Coverage](https://codecov.io/gl/dickoa/rhdx/branch/master/graph/badge.svg)](https://codecov.io/gl/dickoa/rhdx)
[![License:
MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

`rhdx` is an R client for the Humanitarian Exchange Data
platform.

![](https://gitlab.com/dickoa/rhdx/raw/5ec77127ad1a7322c6ff118f4b0a8fdcbba71788/inst/demo/demo.gif)

## Installation

This package is not on yet on CRAN and to install it, you will need the
[`devtools`](https://github.com/r-lib/devtools) package. You can
download it from Github or Gitlab

``` r
## install.packages("remotes") 
remotes::install_github("dickoa/rhdx")
remotes::install_gitlab("dickoa/rhdx")
```

## rhdx tutorial

``` r
library("rhdx")
```

The first step is to connect to an HDX server, we can use the `demo`
server, we can use the `rhdx_config` function

``` r
set_rhdx_config(hdx_site = "prod")
get_rhdx_config()
## <HDX Configuration> 
##   HDX site: prod
##   HDX site url: https://data.humdata.org/
##   HDX API key: 
```

We can `search_datasets`, `get_resources` and `read_session` to get data
from HDX to R directly

``` r
library(tidyverse)
search_datasets("ACLED", rows = 2) %>% ## search dataset in HDX
  first() %>% ## select the first dataset
  get_resources() %>% ## get all resources in the datasets
  first() %>% ## select the first resource
  read_session() ## read it into R
## reading sheet:  Sheet1 
## # A tibble: 3,698 x 25
##     GWNO EVENT_ID_CNTY EVENT_ID_NO_CNTY EVENT_DATE         
##    <dbl> <chr>                    <dbl> <dttm>             
##  1  615. 1ALG                        1. 1997-01-04 00:00:00
##  2  615. 2ALG                        2. 1997-01-05 00:00:00
##  3  615. 3ALG                        3. 1997-01-06 00:00:00
##  4  615. 4ALG                        4. 1997-01-07 00:00:00
##  5  615. 5ALG                        5. 1997-01-11 00:00:00
##  6  615. 6ALG                        6. 1997-01-12 00:00:00
##  7  615. 7ALG                        7. 1997-01-17 00:00:00
##  8  615. 8ALG                        8. 1997-01-19 00:00:00
##  9  615. 9ALG                        9. 1997-01-21 00:00:00
## 10  615. 11ALG                      10. 1997-01-22 00:00:00
## # ... with 3,688 more rows, and 21 more variables: YEAR <dbl>,
## #   TIME_PRECISION <dbl>, EVENT_TYPE <chr>, ACTOR1 <chr>,
## #   ALLY_ACTOR_1 <chr>, INTER1 <dbl>, ACTOR2 <chr>,
## #   ALLY_ACTOR_2 <chr>, INTER2 <dbl>, INTERACTION <dbl>,
## #   COUNTRY <chr>, ADMIN1 <chr>, ADMIN2 <chr>, ADMIN3 <lgl>,
## #   LOCATION <chr>, LATITUDE <dbl>, LONGITUDE <dbl>,
## #   GEO_PRECISION <dbl>, SOURCE <chr>, NOTES <chr>,
## #   FATALITIES <dbl>
```

`read_session` will not work for all data in HDX, so far the following
format are supported: `csv`, `xlsx`, `xls`, `json`, `geojson`, `zipped
shapefile`, `kmz`, `zipped geodatabase` and `zipped geopackage`

## Getting data in R a short tutorial

### Connect to a server

In order to connect to HDX, we can use the `set_rhdx_config` function

``` r
set_rhdx_config(hdx_site = "prod")
```

### Search datasets

Once a server is chosen, we can now search from dataset using the
`search_datasets` In this case we will limit just to two results (`rows`
parameter).

``` r
list_of_ds <- search_datasets("displaced Nigeria", rows = 2)
list_of_ds
## [[1]]
## <HDX Dataset> 4fbc627d-ff64-4bf6-8a49-59904eae15bb 
##   Title: Nigeria - Internally displaced persons - IDPs
##   Name: idmc-idp-data-for-nigeria
##   Date: 01/01/2009-12/31/2016
##   Tags (up to 5): displacement, idmc, population
##   Locations (up to 5): nga
##   Resources (up to 5): displacement_data, conflict_data, disaster_data

## [[2]]
## <HDX Dataset> 4adf7874-ae01-46fd-a442-5fc6b3c9dff1 
##   Title: Nigeria Baseline Assessment Data [IOM DTM]
##   Name: nigeria-baseline-data-iom-dtm
##   Date: 01/31/2018
##   Tags (up to 5): adamawa, assessment, baseline-data, baseline-dtm, bauchi
##   Locations (up to 5): nga
##   Resources (up to 5): DTM Nigeria Baseline Assessment Round 21, DTM Nigeria Baseline Assessment Round 20, DTM Nigeria Baseline Assessment Round 19, DTM Nigeria Baseline Assessment Round 18, DTM Nigeria Baseline Assessment Round 17
```

### Choose the dataset you want to manipulate in R, in this case we will take the first one.

The result of `search_datasets` is a list of HDX datasets, you can
manipulate this list like any other `list` in R. We can use
`dplyr::first` to select the first element of our list.

``` r
ds <- first(list_of_ds)
ds
## <HDX Dataset> 4fbc627d-ff64-4bf6-8a49-59904eae15bb 
##   Title: Nigeria - Internally displaced persons - IDPs
##   Name: idmc-idp-data-for-nigeria
##   Date: 01/01/2009-12/31/2016
##   Tags (up to 5): displacement, idmc, population
##   Locations (up to 5): nga
##   Resources (up to 5): displacement_data, conflict_data, disaster_data
```

### List all resources in the dataset

With our dataset, the next step is to list all the resources. If you are
not familiar with CKAN terminology, `resources` refer to the actual
files shared in a dataset page. Each dataset page contains one or more
resources.

``` r
list_of_rs <- get_resources(ds)
list_of_rs
## [[1]]
## <HDX Resource> f57be018-116e-4dd9-a7ab-8002e7627f36 
##   Name: displacement_data
##   Description: Internally displaced persons - IDPs (new displacement associated with conflict and violence)
##   Size: 
##   Format: JSON

## [[2]]
## <HDX Resource> 6261856c-afb9-4746-b340-9cf531cbd38f 
##   Name: conflict_data
##   Description: Internally displaced persons - IDPs (people displaced by conflict and violence)
##   Size: 
##   Format: JSON

## [[3]]
## <HDX Resource> b8ff1f4b-105c-4a6c-bf54-a543a486ab7e 
##   Name: disaster_data
##   Description: Internally displaced persons - IDPs (new displacement associated with disasters)
##   Size: 
##   Format: JSON
```

### Choose a resource we need to download/read

For this example, we are looking for the displacement data and it’s the
first resource in our list `list_of_rs`. The selected resource can be
then downloaded and store or directly read into your R session using the
`read_session` function. The resource is a `json` file and it can be
read directly with some options like `simplify_json` (get a `vector` or
a `data.frame` when possible instead of a `list`)

``` r
idp_nga_rs <- first(list_of_rs)
idp_nga_df <- read_session(idp_nga_rs, simplify_json = TRUE, folder = tempdir())
idp_nga_df
## $results
##   iso iso3 geo_name year conflict_new_displacements
## 1  NG  NGA  Nigeria 2009                       5000
## 2  NG  NGA  Nigeria 2010                       5000
## 3  NG  NGA  Nigeria 2011                      65000
## 4  NG  NGA  Nigeria 2012                      63000
## 5  NG  NGA  Nigeria 2013                     471000
## 6  NG  NGA  Nigeria 2014                     975000
## 7  NG  NGA  Nigeria 2015                     737000
## 8  NG  NGA  Nigeria 2016                     501000
##   disaster_new_displacements conflict_stock_displacement
## 1                     140000                          NA
## 2                     560000                          NA
## 3                       6300                          NA
## 4                    6112000                          NA
## 5                     117000                     3300000
## 6                       3000                     1075000
## 7                     100000                     2096000
## 8                      78000                     1955000

## $lookups
## named list()

## $errors
## list()

## $success
## [1] TRUE

## $params
## [1] "/api/displacement_data?iso3=NGA&ci=HDX00AKEYJUl17"

## $total
## [1] 8

## $limit
## [1] 0

## $offset
## [1] 0
```

### Using `magrittr` pipes

All these operations can be chained using pipes `%>%` and allow for a
powerful grammar to easily get your data in R

``` r
library(tidyverse)

set_rhdx_config(hdx_site = "prod")

search_datasets("displaced Nigeria", rows = 2) %>%
 first() %>%
 get_resources() %>%
 first() %>%
 read_session(simplify_json = TRUE, folder = tempdir()) -> idp_nga_df

idp_nga_df
## $results
##   iso iso3 geo_name year conflict_new_displacements
## 1  NG  NGA  Nigeria 2009                       5000
## 2  NG  NGA  Nigeria 2010                       5000
## 3  NG  NGA  Nigeria 2011                      65000
## 4  NG  NGA  Nigeria 2012                      63000
## 5  NG  NGA  Nigeria 2013                     471000
## 6  NG  NGA  Nigeria 2014                     975000
## 7  NG  NGA  Nigeria 2015                     737000
## 8  NG  NGA  Nigeria 2016                     501000
##   disaster_new_displacements conflict_stock_displacement
## 1                     140000                          NA
## 2                     560000                          NA
## 3                       6300                          NA
## 4                    6112000                          NA
## 5                     117000                     3300000
## 6                       3000                     1075000
## 7                     100000                     2096000
## 8                      78000                     1955000

## $lookups
## named list()

## $errors
## list()

## $success
## [1] TRUE

## $params
## [1] "/api/displacement_data?iso3=NGA&ci=HDX00AKEYJUl17"

## $total
## [1] 8

## $limit
## [1] 0

## $offset
## [1] 0
```

## Read data with HXL tags

The `rhxl` package is used to manipulate HXLated dataset

## Create a dataset in HDX

### Connect to a server

### Get an API key

## Meta

  - Please [report any issues or
    bugs](https://gitlab.dickoa/rhdx/issues).
  - License: MIT
  - Please note that this project is released with a [Contributor Code
    of Conduct](CONDUCT.md). By participating in this project you agree
    to abide by its terms.
