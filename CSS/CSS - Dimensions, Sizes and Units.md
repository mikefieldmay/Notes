## Dimensions, sizes and units

### Pixels, percentages and more

We want to be able to control the font sizes that we use, rather than the browser choosing how to zoom. We also want to be certain what we are giving percentage values too.

Units:
- Pixel: px
- Percentages: %
- Root em: rem (refers to the font size)
- em: em (refer to the fontt size like rem)
- viewport height: vh
- viewport width: vw

Where units matter:
- font-size
- padding
- border
- margin
- height
- width
- top
- bottom
- left
- right

### How is the size calculated

- Absolute lengths: These mostly ignore user settings. Px.
- Viewport lengths: Adjust the size of the element relative to the viewport: vh, vw, vmin, vmax
- Font-relative lengths: Adjust to the default font size
- Percentages: relate to both  viewport lengths and font-relative lengths.

## Percentage units:
3 rules to remember:
1. If the element is `position: fixed`. The reference point 
