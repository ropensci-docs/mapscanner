# Generate maps for 'mapscanner' use

Generate a map image for a specified area or bounding box, by
downloading tiles from <https://www.mapbox.com/>. Map is automatically
saved in both `.pdf` and `.png` formats, by default in current working
directory, or alternative location when `mapname` includes the full
path.

## Usage

``` r
ms_generate_map(
  bbox,
  max_tiles = 16L,
  mapname = NULL,
  bw = TRUE,
  style = "light",
  raster_brick = NULL
)
```

## Arguments

- bbox:

  Either a string specifying the location, or a numeric bounding box as
  a single vector of (xmin, ymin, xmax, ymax), or a 2-by-2 matrix with
  columns of (min, max) and rows of (x, y), respectively.

- max_tiles:

  Maximum number of tiles to use to create map

- mapname:

  Name of map to be produced, optionally including full path. Extension
  will be ignored.

- bw:

  If `FALSE`, print maps in colour, otherwise black-and-white. Note that
  the default `style = "light"` is monochrome, and that this parameter
  only has effect for `style` values of `"streets"` or `"outdoors"`.

- style:

  The style of the map to be generated; one of 'light', 'streets', or
  'outdoors', rendered in black and white. See
  <https://docs.mapbox.com/api/maps/#styles/> for examples.

- raster_brick:

  Instead of automatically downloading tiles within a given `bbox`, a
  pre-downloaded `raster::rasterBrick` object may be submitted and used
  to generate the `.pdf` and `.png` equivalents.

## Value

Invisibly returns a `rasterBrick` object from the raster package
containing all data used to generate the map.

## Examples

``` r
if (FALSE) { # \dontrun{
# code used to generate internal files for a portion of Omaha:
bb <- osmdata::getbb ("omaha nebraska")
shrink <- 0.3 # shrink that bb to 30% size
bb <- t (apply (bb, 1, function (i) {
    mean (i) + c (-shrink, shrink) * diff (i) / 2
}))
ms_generate_map (bb, max_tiles = 16L, mapname = "omaha")
} # }
```
