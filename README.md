
# rhdx

<!-- README.md is generated from README.Rmd. Please edit that file -->

[![GitLab CI Build
Status](https://gitlab.com/dickoa/rhdx/badges/master/build.svg)](https://gitlab.com/dickoa/rhdx/pipelines)
<!-- [![AppVeyror Build status](https://ci.appveyor.com/api/projects/status/qytbcx7vjq0t9ao5/branch/master?svg=true)](https://ci.appveyor.com/project/dickoa/rhdx)  -->
[![Codecov Code
Coverage](https://codecov.io/gl/dickoa/rhdx/branch/master/graph/badge.svg)](https://codecov.io/gl/dickoa/rhdx)
<!-- [![](http://www.r-pkg.org/badges/version/rhdx)](http://www.r-pkg.org/pkg/rhdx) -->
<!-- [![CRAN RStudio mirror downloads](http://cranlogs.r-pkg.org/badges/rhdx)](http://www.r-pkg.org/pkg/rhdx) -->
[![License:
MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

`rhdx` is an R client for the Humanitarian Exchange Data platform.

## Installation

This package is not on CRAN and to install the development version, you
will also need the development version of the R package `crul`.

``` r
## install.packages("devtools")
devtools::install_github("ropensci/crul") ## need the dev versio > 0.5.0
devtools::install_git("https://gitlab.com/dickoa/rhdx")
```

![hello world example of
rhdx](https://gitlab.com/dickoa/rhdx/blob/5ec77127ad1a7322c6ff118f4b0a8fdcbba71788/inst/demo/demo.gif)

## rhdx tutorial

``` r
library("rhdx")
```

``` r
Configuration$create(hdx_site = "test")
Configuration$read()
## <HDX Configuration> 
##   HDX site: test
##   HDX site url: https://test-data.humdata.org/
##   HDX API key: 
```

``` r
dataset <- Dataset$read_from_hdx("acled-conflict-data-for-africa-realtime-2016")
dataset
## <HDX Dataset> 6f36a41c-f126-4b18-aaaf-6c2ddfbc5d4d 
##   Title: ACLED Conflict Data for Africa (Realtime - 2016)
##   Name: acled-conflict-data-for-africa-realtime-2016
##   Date: 12/03/2016
##   Tags (up to 5): conflict, political violence, protests, war
##   Locations (up to 5): dza, ago, ben, bwa, bfa
##   Resources (up to 5): ACLED-All-Africa-File_20160101-to-date.xlsx, ACLED-All-Africa-File_20160101-to-date_csv.zip

dataset$get_dataset_date()
## [1] "12/03/2016"
```

``` r
datasets <- Dataset$search_in_hdx("ACLED", rows = 1)
datasets
## [[1]]
## <HDX Dataset> ac9f19f0-132e-46e6-9a9c-7d29dca8a469 
##   Title: ACLED Conflict Data for Algeria
##   Name: acled-conflict-data-for-algeria
##   Date: 01/01/1997-12/31/2015
##   Tags (up to 5): conflict, political violence, protests, war
##   Locations (up to 5): dza
##   Resources (up to 5): Algeria.xlsx
```

``` r
resources <- datasets[[1]]$get_resources()
resources
## [[1]]
## <HDX Resource> 87ce2238-9049-4fb7-aa53-3c9d44b6183b 
##   Name: Algeria.xlsx
##   Description: 
##   Size: 
##   Format: XLSX
```

``` r
resources[[1]]$download()
## trying URL 'http://www.acleddata.com/wp-content/uploads/2016/01/Eritrea.xlsx'
## Content type 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet' length 69983 bytes (68 KB)
## ==================================================
## downloaded 68 KB
```

We can also read the resources directly in R

``` r
dplyr::glimpse(resources[[1]]$read_session())
## reading sheet:  Sheet1 
## Observations: 3,698
## Variables: 25
## $ GWNO             <dbl> 615, 615, 615, 615, 615, 615, 615, ...
## $ EVENT_ID_CNTY    <chr> "1ALG", "2ALG", "3ALG", "4ALG", "5A...
## $ EVENT_ID_NO_CNTY <dbl> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ...
## $ EVENT_DATE       <dttm> 1997-01-04, 1997-01-05, 1997-01-06...
## $ YEAR             <dbl> 1997, 1997, 1997, 1997, 1997, 1997,...
## $ TIME_PRECISION   <dbl> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,...
## $ EVENT_TYPE       <chr> "Violence against civilians", "Viol...
## $ ACTOR1           <chr> "GIA: Armed Islamic Group", "GIA: A...
## $ ALLY_ACTOR_1     <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA,...
## $ INTER1           <dbl> 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2,...
## $ ACTOR2           <chr> "Civilians (Algeria)", "Civilians (...
## $ ALLY_ACTOR_2     <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA,...
## $ INTER2           <dbl> 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7,...
## $ INTERACTION      <dbl> 27, 27, 27, 27, 27, 27, 27, 27, 27,...
## $ COUNTRY          <chr> "Algeria", "Algeria", "Algeria", "A...
## $ ADMIN1           <chr> "Blida", "Tipaza", "Tipaza", "Alger...
## $ ADMIN2           <chr> "Blida", "Douaouda", "Hadjout", "Bo...
## $ ADMIN3           <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA,...
## $ LOCATION         <chr> "Blida", "Douaouda", "Hadjout", "Al...
## $ LATITUDE         <dbl> 36.46860, 36.67250, 36.51390, 36.75...
## $ LONGITUDE        <dbl> 2.828900, 2.789400, 2.417800, 3.041...
## $ GEO_PRECISION    <dbl> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,...
## $ SOURCE           <chr> "www.algeria-watch.org", "www.alger...
## $ NOTES            <chr> "4 January: 16 citizens were murder...
## $ FATALITIES       <dbl> 16, 18, 23, 20, 5, 14, 43, 54, 30, ...
```

so far the following format are supported: `csv`, `xlsx`, `xls`, `zipped
shapefile`, `zipped kml` `kmz` `zipped geodatabase` and `zipped
geopackage`

## rhdx package API

  - Configuration - `create()` - `setup` - `read`
  - Dataset - `read_from_hdx` - `search_in_hdx`
  - Resource - `download` - `read_from_hdx` - `search_in_hdx`

## Future dev

  - Develop all `create_in_hdx` methods
  - Add tidy tools to easily get data
  - Shiny apps amd Rstudio add-in to browse and select data
    interactively

## Meta

  - Please [report any issues or
    bugs](https://gitlab.dickoa/rhdx/issues).
  - License: MIT
  - Please note that this project is released with a [Contributor Code
    of Conduct](CONDUCT.md). By participating in this project you agree
    to abide by its terms.
