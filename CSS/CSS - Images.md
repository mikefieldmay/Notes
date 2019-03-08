### CSS - Understanding background images and images

## Understanding background-size:

```
#product-overview {
    background: url("freedom.jpg");
}
```
background is just shorthand. You can use other properties such as:
- background-image: usually passed a url to an image
- background-color: Can define a block color that does not override image
- background-size: takes multiple arguments. Only one argument and it will take the width e.g `background-size: 100px`. This will set the width and cause the image to no-repeat(by default). The second value it takes will be the height. If it's not set the height is auto based on the aspect ratio.
you can also set it as cover. Cover automatically finds out which value to set to 100% based on teh aspect ratio. Cover will ensure your image always fills your entire container. Contain will ensure that the full image is visible in the container. This will not neccessarily fill the entire container though. 
- background-repeat: you can set various properties to this. It can be set to repeatx (only on the x), repeaty (only on the y) or no repeat.

When you declare a background image the image is automatically cropped. We can also define how we want the image to be cropped. 

## Understanding background-position
background-position can be used to position a background image that covers or contains the background. the first value passed to it defines the x axis, whilst the second defines the y. You can use pixel vaues or percentage values. With percentage values you define how much of the excess space is positioned in the image e.g `background-position: 0% 10%;` meanse that 10% of our excess image should go over the top. The default is 50% which means that of the parts of the image that do not fit. 50% will be cropped at the top, so 50% is cropped at the bottom. 100% would crop all the content at the top, and none at the bottom. `background-position: center` is shorthand for 50% 50%. `background-position: left top` is essentially align to left and to top. 

## background shorthand
background is a shorthand for many different properties:
- background-image: allows us to set one or multiple background images
- background-color: allows us to set a solid background color
- background-position: set initial position relative to background layer (position layer is defined by background-origin and only applies to background-image)
- background-size: sets size of background image.
- background-repeat: defines how an image is repeated
- background-origin: sets a background positioning area. 
- background-clip: allows us to define whether a background extends underneath a potential border. If it ends before a border starts.
- background-attachment: defines the scrolling of an image.

## Applying background-clip/origin/attachment

`background-origin` can define what the actual container is for the background property. You can set it to border-box (like divs), content-box (content without border or padding). The default is padding-box which is the content with padding. `background-origin` doesn't clip an image. For this we use `background-clip`. It defines where the content should be clipped where necessary. `border-box` will set it to fit within the border.

`background-attachment` allows us to set whether the image set as the backround would scroll. It is rare to use this.

## Using the background shorthand

`background: url("freedom.jpg") left 10% bottom 20%/cover no-repeat;`
is equivalent to
```
background-image: url("freedom.jpg");
background-position: left 10% bottom 20%;
background-size: cover;
background-repeat: no-repeat;
```

## Styling images

If you use an image tag that points to an image, the default height and width of the image will be used to display that image. If you want to change it's size you need to add classes onto the image directly. Height 100% will get the image to use it's own height, not it's container. Percentage values do not respect it's container unless the container is inline-block.

img vs backgroud img: if you need to do a lot of img manipulation it is better to use background img. background img is not part of your usual document flow which makes it worse for accessibility. Background images should really just be used as backgrounds. 

## Linear gradients

Linear gradients are treated as images. `background-image: linear-gradient(to left bottom, red, blue);` the first argument the linear gradient function takes is a direction. You can use `to` to define which direction the gradient moves too. You can also specify it in degrees. You can add as any colors as you want (hex codes and other color functions are also fine). Transparent is also a viable option. Besides specifying multiple colors you can define where these colors start or stop

## Radial Gradients

Radial gradients start witha  set shape and transitions outwards. The first argument it takes is the shape. The shape will always be in the middle unless you specify it's start position.
`background-image: radial-gradient(circle at 20% 50%, red, blue);`first postion is x ais, second is y axis.

## Stacking Multiple backgrounds

We can set multiple backgrounds for a single element. The multiple backgrounds would need to be transparent. Only one solid colour can be used and it is always at the bottom. We seperate the different layers with commas:
`background: linear-gradient(), url("images/freedom.jpg"), left 10% bottom 20%/cover no-repeat, #ff1b68;`
It is only ever really used for a fall back, or if your higher image is transparent.

## Filters

Filters allow us to change the visual appearance of an element by changing the element they are placed on. They are a quick and easy way of changing the visuals of something.

## Styling SVGs
Two important SVG properties are fill and stroke and stroke width.

### Links: 
The background  Property: https://developer.mozilla.org/en-US/docs/Web/CSS/background
Styling Images: https://www.w3schools.com/css/css3_images.asp
Filters: https://developer.mozilla.org/en-US/docs/Web/CSS/filter
Styling SVG: https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/SVG_and_CSS