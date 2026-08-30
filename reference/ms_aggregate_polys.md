# Aggregate disparate polygons

Planar partition from disparate polygon inputs. Overlaps aggregate to
`n`.

## Usage

``` r
ms_aggregate_polys(p)
```

## Arguments

- p:

  input (multi-)polygons (assumed to be overlapping)

## Value

Set of sf-format polygons with additional column, `n`, denoting number
of overlaps contributing to each of the resultant polygons.

## Details

Input is a single simple features polygon data frame. No attribute data
is considered.

## Examples

``` r
g <- sf::st_sfc (list (
    sf::st_point (cbind (0, 0)),
    sf::st_point (cbind (0, 1)),
    sf::st_point (cbind (1, 0))
))
pts <- sf::st_sf (a = 1:3, geometry = g)
overlapping_polys <- sf::st_buffer (pts, 0.75)

## decompose and count space-filling from overlapping polygons
x <- ms_aggregate_polys (overlapping_polys)
plot (x)

if (FALSE) { # \dontrun{
library (ggplot2)
ggplot (x) +
    geom_sf () +
    facet_wrap (~n)
} # }

library (sf)
#> Linking to GEOS 3.12.1, GDAL 3.8.4, PROJ 9.4.0; sf_use_s2() is TRUE
set.seed (6)
pts <- expand.grid (x = 1:8, y = 1:10) %>% st_as_sf (coords = c ("x", "y"))
xsf <- sf::st_buffer (pts, runif (nrow (pts), 0.2, 1.5))
if (FALSE) { # \dontrun{
out <- ms_aggregate_polys (xsf)
} # }
```
