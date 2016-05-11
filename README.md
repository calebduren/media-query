# SCSS Media Query

SCSS mixin for media queries

## Values

* `min-width` (required): enter an scss variable that represents a width or an explicit pixel value (e.g. 768px)
* `max-width` (optional, defaults to `null`): enter an scss variable or explicit width, but **it is important to note** that this value has 1px subtracted from it in the mixin, which makes using variables much easier (see example 2)
* `media` (optional, defaults to `screen`): the other valid value is `print` for a media query that responds to print
* `orientation` (optional, defaults to `null`): the valid values are `orientation` (meaning the width of the screen is > the height) and `portrait` (the width is < than the height)

Basic usage:

```
@mixin respond($min, $max: null, $media: "screen", $orientation: null) {
  @content;
}
```

The only required value is a `min-width`, based on mobile-first principles.

## Examples

### Example 1 – Min-Width Only

```
@mixin respond(768px) {
  @content;
}
```

Compiles to:

```
@media screen and ( min-width: 768px ) {
  @content;
}
```

### Example 2 – Min-Width and Max-Width

```
@mixin respond(768px, 1024px) {
  @content;
}
```

Compiles to:

```
@media screen and ( min-width: 768px ) and (max-width: 1024px) {
  @content;
}
```

### Example 3 – Separate Min-Width and Max-Width Media Queries

A common use case for media queries is to have a selector respond both ≤ a certain width and > a certain width without overriding any styles

```
$medium: 768px;

.selector {

  @mixin respond($medium) {
    // greater than medium
  }

  @mixin respond(null, $medium) {
    // less than medium
  }
}
```

Compiles to:

```
@media screen and ( min-width: 768px ) {
  .selector {
    // greater than medium
  }
}

@media screen and ( max-width: 767px ) {
  .selector {
    // less than medium
  }
}
```

### Example 3 – Print

Because the default value for `@media` is `screen`, the `print` value needs to be passed as the third value; if you do not want to use a min or max width, you must pass `null` values as follows:

```
@mixin respond(null, null, print) {
  @content;
}
```

Compiles to:

```
@media print {
  @content;
}
```

### Example 4 – Orientation

Using orientation:

```
$small: 480px;
$large: 1024px;

@mixin respond($small, $large, screen, portrait) {
  @content;
}
```

Compiles to:

```
@media screen and ( min-width: 768px ) and (max-width: 1023px) and ( orientation: portrait ){
  @content;
}
```
