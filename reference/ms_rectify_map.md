# Rectify one map to another

Rectify two previously scanned-in pdf or png maps with `RNiftyReg`, and
return the modifications in `map_modified` as spatial objects in sf
format.

## Usage

``` r
ms_rectify_map(
  map_original,
  map_modified,
  nitems = NULL,
  non_linear = 1,
  type = "hulls",
  downsample = 10,
  concavity = 0,
  length_threshold = 10,
  quiet = FALSE
)
```

## Arguments

- map_original:

  File name of the original map without anything drawn over it (either a
  `.pdf` or `.png`; extension will be ignored).

- map_modified:

  File name of the modified version with drawings (either a `.pdf` or
  `.png`; extension will be ignored).

- nitems:

  Optional parameter to explicitly specify number of distinct items to
  be extracted from map; if possible, specifying this parameter may
  improve results.

- non_linear:

  Integer value of 0, 1, or 2 representing degree of non-linearity in
  modified image - see Note.

- type:

  Currently either "points", "polygons", or "hulls", where "points"
  simply reduces each distinct object to a single, central point;
  "polygons" identifies individual groups and returns the polygon
  representing the outer boundary of each; and "hulls" constructs convex
  or concave polygons around each group.

- downsample:

  Factor by which to downsample `type = "polygons"`, noting that
  polygons initially include every outer pixel of image, so can
  generally be downsampled by at least an order or magnitude (`n = 10`).
  Higher values may be used for higher-resolution images; lower values
  will generally only be necessary for very low lower resolution images.

- concavity:

  For `type = "hulls"`, a value between 0 and 1, with 0 giving convex
  hulls and 1 giving highly concave hulls.

- length_threshold:

  For `type = "hulls"`, the minimal length of a segment to be made more
  convex. Low values will produce highly detailed hulls which may cause
  problems; if in doubt, or if odd results appear, increase this value.

- quiet:

  If `FALSE`, display progress information on screen

## Value

An sf object representing the drawn additions to map_modified.

## Note

The `non-linear` parameter should generally set according to how the
modified maps were digitised. A value of 0 will give fastest results,
and should be used for directly scanned or photocopied images. A value
of 1 (the default) still presumes modified images have been linearly
translated, and will apply affine transformations (rotations,
contractions, dilations). This value should be used when modified images
have been photographed (potentially from an oblique angle). A value of 2
should only be used when modified maps have somehow been non-linearly
distorted, for example through having been crumpled or screwed up.
Rectification with `non-linear = 2` will likely take considerably longer
than with lower values.

## Examples

``` r
f_orig <- system.file ("extdata", "omaha.png", package = "mapscanner")
f_mod <- system.file ("extdata", "omaha-polygons.png",
    package = "mapscanner"
)

# reduce file sizes to 1/4 to speed up these examples:
f_orig2 <- file.path (tempdir (), "omaha.png")
f_modified2 <- file.path (tempdir (), "omaha-polygons.png")
magick::image_read (f_orig) %>%
    magick::image_resize ("25%") %>%
    magick::image_write (f_orig2)
magick::image_read (f_mod) %>%
    magick::image_resize ("25%") %>%
    magick::image_write (f_modified2)

# then rectify those files:
if (FALSE) { # \dontrun{
xy_hull <- ms_rectify_map (f_orig2, f_modified2, type = "hull")
xy_poly <- ms_rectify_map (f_orig2, f_modified2, type = "polygons")
xy_pts <- ms_rectify_map (f_orig2, f_modified2, type = "points")
} # }
```
