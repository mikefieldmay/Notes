## Working with Flexbox - The modern wat to change the way our elements are displayed
There are 3 main components of flexbox:
- The Flex-Container
- Main Axis vs Cross Axis
- The flex Items

### Why use Flexbox

Flex items can be a good way to position stuff rather than using inline-block. It also lets us more easily manipulate items without having to use hardcoded widths and values. It allows us to remove display: inline-block from our code. It makes our code leaner, more efficient and easier to read.

### Understanding Flexbox

Flexbox allows us to change the way our elements are displayed. The property that allows us to use flexbox is the `display: flex` property. This creates a flex container which is the parent of our flex items. When the parent has become a flex container. Properties that the flex container has access to are:
- flex-flow
- justify-conent
- align-content
- align-items

Flex item properties include:
- order
- flex 
- aign-self

### Flex container
When using a Flex Container, all child elements appear in the same row and take up the height of the highest child.
All the items inside the container become flex-items.

```
display: flex
```
`inline-flex` is a property that does not change the sizes of the parent and the internal elements. 
When you add display: flex, you also add two more properties by default `flex-direction: row;`
and `flex-wrap: no-wrap;`. 
`flex-wrap: wrap;` will wrap the contents of the div when the size of the viewport changes. The content will wrap itself when the width of the contents is wider than the view.
`flex-wrap: wrap-reverse;` will do the same, but the 1st child will be displayed last.
`flex-direction: column` causes the child elements to be displayed like block elements. When the width is reduced though the elements will shrink. `row-reverse` and `column-reverse` are also available. 

### Main Axis vs Cross Axis

The default value for the main-axis is the top left corner to the top right. The Ccross-axis goes from where the main-axis starts. From the top-left to the bottom-left. row-reverse just reverses the main axis and as such effects the cross-axis. Depending on the value we apply to the Axes we can effect the direction in which flexitems are displayed.
With flex-direction column, the main-axis now goes from top to bottom, and the cross axis from left to right. 
If flex direction is row the main-axis is a row, if it is a column, the main axis is the column.
The main and cross axes have effects on other properties that we use.
```
  flex-direction: row;
  flex-wrap: wrap;
  /* flex-flow: row wrap */ // flex flow is shorthand for both.

```

### Align Items and justify content

`align-items` has a default value of stretch. Other values we can use are `center` which centers the items along the cross-axis. `flex-start` aligns them to start of the cross axis `flex-end` aligns tthem to the opposite end of the cross-axis.
`justify-content` aligns items along the main-axis. You can use the same properties as the `align-items`.
`justify-content` aligns items along the main-axis, which in turn is decided by the `flex-direction`. `align-items` effects items along the `cross-axis`. 
`baseline` will align items along the baseline of the actual content. 

`align-content` allows us to align our items along the cross-axis. 

### Z index and flex box:
One exception from this behaviour is flexbox: Applying the z-index  to flex-items (so the elements inside of the flex-container) will change the order of these items even if no position  property was applied.

## Flex items:

### Order property
By applying flexbox code to flexbox items we can specifically target items with the styles we want to be applied to them. We are able to change the order of the elements by using the Order property. All items haeve a default `order` of 0. 1 postions an element at the end. -1 postions it at the front. The higher the number, the later the item will be positioned. It is also afected by row-reverse.

### Align-self
`align-self: flex-start` aligns an element to the cross axis. Align self will refer to a single element.

### Flex-grow
`flex-grow` has an default value of 0. A flex items width will decrease past it's width if it's container is decreasing and  `flex-wrap: wrap;` is applied. If we add flex grow and give it a higher priority than other items it will use the remaining space available in it's container. The extra space is spread across all elements witha  flex-grow greater than one. It divided it by the totoal flex-grow number and shares it across those elements. 

### Flex-shrink

`flex-shrink` by default shrinks the content at the point where the container needs the space. It has a default of 1. 0 will prevent an item from shrinking. 

### Flex-basis

The `flex-basis` defines the size of an element depending on the main axis (flex-direction). It is not the width or the height, but it can be both. Flex-basis is dependant on the main axis. Flex-basis will override the width of an element. flex-basis' default is auto. In row it will effect width, in column it will effect height. You can also use percentage values. This property can only apply to flex-items.

### flex
`flex: 0 1 auto` is shorthand for the defaults for grow, shrink and basis.

With flexbox we can easily change the way elements are displayed on a webpage. 