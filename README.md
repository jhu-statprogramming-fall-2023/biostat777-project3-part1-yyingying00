# 🗺 usmap

Original R package comes from: <https://github.com/pdil/usmap>, created by [Paolo Di Lorenzo](https://github.com/pdil).

Pkgdown website for this package: <https://jhu-statprogramming-fall-2023.github.io/biostat777-project3-part1-yyingying00>; the website and the Example Analysis were created by [Yingying Yu](https://github.com/yyingying00).

5 things customized in the `pkgdown` website:

-   Added a bootswatch theme `flatly`

-   Edited the sidebar to exclude `community`

-   Rearranged the navbar

-   Reordered the link on the `Articles`

-   Changed the footer to include my name

[![CRAN](http://www.r-pkg.org/badges/version/usmap?color=blue)](https://cran.r-project.org/package=usmap) [![Downloads](http://cranlogs.r-pkg.org/badges/grand-total/usmap)](https://cran.r-project.org/package=usmap) [![Build Status](https://img.shields.io/endpoint.svg?url=https%3A%2F%2Factions-badge.atrox.dev%2Fpdil%2Fusmap%2Fbadge%3Fref%3Dmaster&style=popout&label=build)](https://actions-badge.atrox.dev/pdil/usmap/goto?ref=master) [![codecov](https://codecov.io/gh/pdil/usmap/branch/master/graph/badge.svg)](https://app.codecov.io/gh/pdil/usmap)

<p align="center">

<img src="https://raw.githubusercontent.com/pdil/usmap/master/resources/example-plots.png"/>

</p>

View code used to generate these plots: [resources/examples.R](https://github.com/pdil/usmap/blob/master/resources/examples.R)

## Purpose

Typically in R it is difficult to create nice US [choropleths](https://en.wikipedia.org/wiki/Choropleth_map) that include Alaska and Hawaii. The functions presented here attempt to elegantly solve this problem by manually moving these states to a new location and providing a fortified data frame for mapping and visualization. This allows the user to easily add data to color the map.

## Shape Files

The shape files that we use to plot the maps in R are located in the [`usmapdata`](https://github.com/pdil/usmapdata) package. These are generated from the [US Census Bureau cartographic boundary files](https://www.census.gov/geographies/mapping-files/time-series/geo/cartographic-boundary.html). Maps at both the state and county levels are included for convenience.

#### Update History

| Date              | `usmap` version | Shape File Year |                                                  Link                                                  |
|------------------|:----------------:|:----------------:|:----------------:|
| February 27, 2022 |      0.6.0      |      2020       | [🔗](https://www.census.gov/geographies/mapping-files/time-series/geo/cartographic-boundary.2020.html) |
| June 3, 2018      |      0.3.0      |      2017       |  [🔗](https://www.census.gov/geographies/mapping-files/time-series/geo/carto-boundary-file.2017.html)  |
| January 29, 2017  |      0.1.0      |      2015       |  [🔗](https://www.census.gov/geographies/mapping-files/time-series/geo/carto-boundary-file.2015.html)  |

## Installation

To install from CRAN *(recommended)*, run the following code in an R console:

``` r
install.packages("usmap")
```

To install the package from this repository, run the following code in an R console:

``` r
# install.package("devtools")
devtools::install_github("pdil/usmap")
```

Installing using `devtools::install_github` will provide the most recent developer build of `usmap`.

⚠️ The developer build may be unstable and not function correctly, use with caution.

To begin using `usmap`, import the package using the `library` command:

``` r
library(usmap)
```

## Documentation

To read the package vignettes, which explain helpful uses of the package, use `vignette`:

``` r
vignette(package = "usmap")
vignette("introduction", package = "usmap")
vignette("mapping", package = "usmap")
vignette("advanced-mapping", package = "usmap")
```

For further help with this package, open an [issue](https://github.com/pdil/usmap/issues) or ask a question on Stackoverflow with the [usmap tag](https://stackoverflow.com/questions/tagged/usmap).

## Features

-   Obtain map with certain region breakdown

``` r
state_map <- us_map(regions = "states")
```

<details>

<summary><code>str(state_map)</code></summary>

``` r
  #> 'data.frame':  13696 obs. of  9 variables:
  #> $ x    : num  1093752 1093244 1093125 1092939 1092914 ...
  #> $ y    : num  -1378545 -1374233 -1360891 -1341458 -1338952 ...
  #> $ order: int  1 2 3 4 5 6 7 8 9 10 ...
  #> $ hole : logi  FALSE FALSE FALSE FALSE FALSE FALSE ...
  #> $ piece: int  1 1 1 1 1 1 1 1 1 1 ...
  #> $ group: chr  "01.1" "01.1" "01.1" "01.1" ...
  #> $ fips : chr  "01" "01" "01" "01" ...
  #> $ abbr : chr  "AL" "AL" "AL" "AL" ...
  #> $ full : chr  "Alabama" "Alabama" "Alabama" "Alabama" ...
```

</details>

<br>

``` r
county_map <- us_map(regions = "counties")
```

<details>

<summary><code>str(county_map)</code></summary>

``` r
  #> 'data.frame':  55097 obs. of  10 variables:
  #> $ x     : num  811200 829408 828835 855600 859265 ...
  #> $ y     : num  -821207 -819722 -814641 -811770 -846158 ...
  #> $ order : int  1 2 3 4 5 6 7 8 9 10 ...
  #> $ hole  : logi  FALSE FALSE FALSE FALSE FALSE FALSE ...
  #> $ piece : int  1 1 1 1 1 1 1 1 1 1 ...
  #> $ group : chr  "01001.1" "01001.1" "01001.1" "01001.1" ...
  #> $ fips  : chr  "01001" "01001" "01001" "01001" ...
  #> $ abbr  : chr  "AL" "AL" "AL" "AL" ...
  #> $ full  : chr  "Alabama" "Alabama" "Alabama" "Alabama" ...
  #> $ county: chr  "Autauga County" "Autauga County" "Autauga County" "Autauga County" ...
```

</details>

<br>

-   Look up FIPS codes for states and counties

``` r
fips("New Jersey")
#> "34"

fips(c("AZ", "CA", "New Hampshire"))
#> "04" "06" "33"

fips("NJ", county = "Mercer")
#> "34021"

fips("NJ", county = c("Bergen", "Hudson", "Mercer"))
#> "34003" "34017" "34021"
```

-   Retrieve states or counties with FIPS codes

``` r
fips_info(c("34", "35"))
#>         full abbr fips
#> 1 New Jersey   NJ   34 
#> 2 New Mexico   NM   35

fips_info(c("34021", "35021"))
#>         full abbr         county  fips
#> 1 New Jersey   NJ  Mercer County 34021
#> 2 New Mexico   NM Harding County 35021
```

-   Add FIPS codes to data frame

``` r
data <- data.frame(
  state = c("NJ", "NJ", "NJ", "PA"),
  county = c("Bergen", "Hudson", "Mercer", "Allegheny")
)

library(dplyr)
data %>% rowwise %>% mutate(fips = fips(state, county))

#>   state     county  fips
#> 1    NJ     Bergen 34003
#> 2    NJ     Hudson 34017
#> 3    NJ     Mercer 34021
#> 4    PA  Allegheny 42003
```

-   Plot US maps

``` r
plot_usmap("states")
plot_usmap("counties")
```

-   Display only certain states, counties, or regions

``` r
plot_usmap("states", include = .mountain, labels = TRUE)

plot_usmap("counties", data = countypov, values = "pct_pov_2014", include = "FL") +
    ggplot2::scale_fill_continuous(low = "green", high = "red", guide = FALSE)

plot_usmap("counties", data = countypop, values = "pop_2015", include = .new_england) + 
    ggplot2::scale_fill_continuous(low = "blue", high = "yellow", guide = FALSE)
```

<p align="center">

<img src="https://raw.githubusercontent.com/pdil/usmap/master/resources/example-usage.png"/>

</p>

## Additional Information

### Projection

`usmap` uses an [Albers equal-area conic projection](https://en.wikipedia.org/wiki/Albers_projection), with arguments as follows:

<details>

<summary><code>usmap::usmap_crs()</code></summary>

```         
  #> Coordinate Reference System:
  #> Deprecated Proj.4 representation:
  #>  +proj=laea +lat_0=45 +lon_0=-100 +x_0=0 +y_0=0 +ellps=sphere
  #> +units=m +no_defs 
  #> WKT2 2019 representation:
  #> PROJCRS["unknown",
  #>     BASEGEOGCRS["unknown",
  #>         DATUM["unknown",
  #>             ELLIPSOID["Normal Sphere (r=6370997)",6370997,0,
  #>                 LENGTHUNIT["metre",1,
  #>                     ID["EPSG",9001]]]],
  #>         PRIMEM["Greenwich",0,
  #>             ANGLEUNIT["degree",0.0174532925199433],
  #>             ID["EPSG",8901]]],
  #>     CONVERSION["unknown",
  #>         METHOD["Lambert Azimuthal Equal Area (Spherical)",
  #>             ID["EPSG",1027]],
  #>         PARAMETER["Latitude of natural origin",45,
  #>             ANGLEUNIT["degree",0.0174532925199433],
  #>             ID["EPSG",8801]],
  #>         PARAMETER["Longitude of natural origin",-100,
  #>             ANGLEUNIT["degree",0.0174532925199433],
  #>             ID["EPSG",8802]],
  #>         PARAMETER["False easting",0,
  #>             LENGTHUNIT["metre",1],
  #>             ID["EPSG",8806]],
  #>         PARAMETER["False northing",0,
  #>             LENGTHUNIT["metre",1],
  #>             ID["EPSG",8807]]],
  #>     CS[Cartesian,2],
  #>         AXIS["(E)",east,
  #>             ORDER[1],
  #>             LENGTHUNIT["metre",1,
  #>                 ID["EPSG",9001]]],
  #>         AXIS["(N)",north,
  #>             ORDER[2],
  #>             LENGTHUNIT["metre",1,
  #>                 ID["EPSG",9001]]]] 
```

</details>

This is the same projection used by the [US National Atlas](https://epsg.io/2163).

To obtain the projection used by `usmap`, use `usmap_crs()`.

Alternatively, the CRS ([coordinate reference system](https://www.nceas.ucsb.edu/sites/default/files/2020-04/OverviewCoordinateReferenceSystems.pdf)) can be created manually with the following command:

``` r
sp::CRS(paste("+proj=laea +lat_0=45 +lon_0=-100 +x_0=0 +y_0=0",
              "+a=6370997 +b=6370997 +units=m +no_defs"))
```

## Acknowledgments

The code used to generate the map files was based on this blog post by [Bob Rudis](https://github.com/hrbrmstr):\
[Moving The Earth (well, Alaska & Hawaii) With R](https://rud.is/b/2014/11/16/moving-the-earth-well-alaska-hawaii-with-r/)
