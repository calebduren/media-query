# SCSS Media Query Mixin

A flexible SCSS mixin for writing media queries

## Media Query Values

| Value | Required? | Default Value | Description |
| --- | --- | --- | --- |
| `min-width` | Yes | None | Enter an scss variable that represents a width or an explicit pixel value (e.g. 768px) |
| `max-width` | No | `null` | Enter an scss variable or explicit width, but **it is important to note** that this value has 1px subtracted from it in the mixin, which makes using variables much easier (see example 2) |
| `media` | No | `screen` | The other valid value is `print` for a media query that responds to print |
| `orientation` | No | `null` | The valid values are `orientation` (meaning the width of the screen is > the height) and `portrait` (the width is < than the height) |

## Basic usage:

```
@mixin respond($min, $max, $media, $orientation) {
  @content;
}
```

The only required value is a `min-width`, based on a mobile-first approach.

## Examples

### Example 1 – Min-Width Only

Because this is the most common media query I use, it is the simplest to write. Since the other values have defaults, only passing 1 value will result in a `min-width` media query for screens, for example:

```
@mixin respond(768px) {
  @content;
}
```

Compiles to:

```
@media screen and (min-width: 768px) {
  @content;
}
```

### Example 2 – Min-Width and Max-Width

To create rules that are applied between to given breakpoints, using a comma to seperate 2 values or variables in the mixin will result in a media query with min and max widths, such as:

```
@mixin respond(768px, 1024px) {
  @content;
}
```

Compiles to:

```
@media screen and (min-width: 768px) and (max-width: 1024px) {
  @content;
}
```

### Example 3 – Separate Min-Width and Max-Width Media Queries

A common use case for media queries is to have a selector respond both ≤ a certain width and > a certain width without overriding any styles.

To accomplish this, use 2 mixins, one to establish the rules that will be applied above the `min-width` value, and one to establish the rules that will be applied below the `max-width` value, consider the following code for example:

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
@media screen and (min-width: 768px) {
  .selector {
    /* greater than medium */
  }
}

@media screen and (max-width: 767px) {
  .selector {
    /* less than medium */
  }
}
```

### Example 3 – Print

Because the default value for `@media` is `screen`, the `print` value needs to be passed as the third value; if you do not want to use a min or max width, you must pass `null` values.

> Don't forget to comma separate the values

Here is the simplest print media query:

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

If you need to target screens with a specific orientation, consider the following structure:

```
$small: 480px;
$large: 1024px;

@mixin respond($small, $large, screen, portrait) {
  @content;
}
```

Compiles to:

```
@media screen and (min-width: 768px) and (max-width: 1023px) and (orientation: portrait) {
  @content;
}
```
