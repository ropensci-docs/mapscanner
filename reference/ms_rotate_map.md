# Rotate maps

Display original and modified maps to determine necessary rotation

## Usage

``` r
ms_rotate_map(map_original, map_modified, rotation = 0, apply_rotation = FALSE)
```

## Arguments

- map_original:

  File name of the original map without anything drawn over it (either a
  `.pdf` or `.png`; extension will be ignored).

- map_modified:

  File name of the modified version with drawings (either a `.pdf` or
  `.png`; extension will be ignored).

- rotation:

  Rotation value to be applied, generally +/- 90

- apply_rotation:

  If `FALSE`, display results of rotation without actually applying it;
  otherwise transform the specified `map_modified` image according to
  the specified rotation.

## Value

No return value. Function either modifies files on disk by rotating
images by the specified amount (if `apply_rotation = TRUE`), or displays
a rotated version of `map_original` (if `apply_rotation = FALSE`).

## Note

If a call to
[ms_rectify_map](https://docs.ropensci.org/mapscanner/reference/ms_rectify_map.md)
detects potential image rotation, that function will stop and suggest
that rotation be applied using this function in order to determine the
required degree of image rotation. Values for `rotation` can be trialled
in order to determine the correct value, following which that value can
be entered with `apply_rotation = TRUE` in order to actually apply that
rotation to the modified image.
