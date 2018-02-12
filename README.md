rhdx
====

[![GitLab CI Build
Status](https://gitlab.com/dickoa/rhdx/badges/master/build.svg)](https://gitlab.com/dickoa/rhdx/pipelines)
[![AppVeyror Build
status](https://ci.appveyor.com/api/projects/status/qytbcx7vjq0t9ao5/branch/master?svg=true)](https://ci.appveyor.com/project/dickoa/rhdx)
[![Codecov Code
Coverage](https://codecov.io/gl/dickoa/rhdx/branch/master/graph/badge.svg)](https://codecov.io/gl/dickoa/rhdx)
[![](http://www.r-pkg.org/badges/version/rhdx)](http://www.r-pkg.org/pkg/rhdx)
[![CRAN RStudio mirror
downloads](http://cranlogs.r-pkg.org/badges/rhdx)](http://www.r-pkg.org/pkg/rhdx)
[![License:
MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

`rhdx` is an R client for the Humanitarian Exchange Data platform.

Installation
------------

Development version

    install.packages("devtools")
    devtools::install_git("https://gitlab.com/dickoa/rhdx")

rhdx tutorial
-------------

    library("rhdx")

    Configuration$create(hdx_site = "test")
    Configuration$read()
    ## <HDX Configuration> 
    ##   HDX site: test
    ##   HDX site url: https://test-data.humdata.org/
    ##   HDX API key: 

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

    resources <- datasets[[1]]$get_resources()
    resources
    ## [[1]]
    ## <HDX Resource> 87ce2238-9049-4fb7-aa53-3c9d44b6183b 
    ##   Name: Algeria.xlsx
    ##   Description: 
    ##   Size: 
    ##   Format: XLSX

    resources[[1]]$download()
    ## trying URL 'http://www.acleddata.com/wp-content/uploads/2016/01/Eritrea.xlsx'
    ## Content type 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet' length 69983 bytes (68 KB)
    ## ==================================================
    ## downloaded 68 KB

rhdx package API
----------------

-   Configuration - `create()` - `setup` - `read`
-   Dataset - `read_from_hdx` - `search_in_hdx`
-   Resource - `download` - `read_from_hdx` - `search_in_hdx`

Future dev
----------

### Develop all `create_in_hdx` methods

### Add tidy tools to easily get data

### Rstudio Add-in to browse data interactively

Meta
----

-   Please [report any issues or
    bugs](https://gitlab.dickoa/rhdx/issues).
-   License: MIT
-   Please note that this project is released with a [Contributor Code
    of Conduct](CONDUCT.md). By participating in this project you agree
    to abide by its terms.
