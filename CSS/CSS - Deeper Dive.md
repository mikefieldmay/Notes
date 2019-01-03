# CSS Deeper Dive

## CSS Box Model

Every element in html is interpreted as a box in html. It is represented by the content itself, the padding (space between things internally), the border (Surrounds the padding) and the margin (not part of the core element, but specifies the distance between that element and the next). There are two types of element, block level and inline.

```
#product-overview {
    background: #ff1b68;
    padding: 20px;
    border: 5px black solid;
    margin: 20px;
}
```

The body has a default margin which comes from the browser settings. We can set the margin to 0 to alleviate that. h1 tags also have a default margin. 
If you have two elements next to each other, the margins between them are actually collapsed to one margin. This is known as margin collapsing. The bigger margin will always in. This is enforced by css to stop too big a margin from appearing. Too work around this use either margin-top, or margin-bottom.

### Shorthand Properties

You can use shorthand properties in order to cut down on the amount of css you write. If you later only want to specify a certain part of the property, you can use the slightly longer version in the selector.

```
border-width: 
border-style: 
border-color: 

border: width style colour

margin-top
margin-right
margin-bottom
margin-left

margin: top right bottom left

margin: top rightLeft bottom

margin: topBottom leftRight

```

## Height and width

Sections and divs are block level elements that take up 100% width by default. Inline elements don't. Width can be used by absolute values or percentages. We can also set the height. 
If you set height 100% it refers to the available height given by the parent container. If we give the container an explicit height, then set the childs height to 100%, it will fill that container.

When we set the height and the width, we actually set the width and height of the content. 
We can change this behaviour by setting the `box-sizing` propeerty. `box-sizing` by default has `content-box` set which only includes the content. It can be set to `border-box` which takes content + padding + border. It is often immediately changed from content-box to border-box as it's much more straight forward to think of the height and width using everything, not just the content. Margin is never included.

If you were to add it to the body it wouldn't work. This is because inheritance has the lowest level of specificity and the b.ock elements override the style: 
```
body {
    font-family: 'Montserrat', sans-serif;
    margin: 0;
    box-sizing: border-box;
}

* {
    box-sizing: border-box;
}

```
Instead use the wildcard selector. This is one of the few times wher you'd use it.

## Display property

The diplay property allows us to change the behaviour of an element from block to inline, or from inline to block, or to inline-block.
An example of inline elements are anchor tags. When you put two next to each other they are rendered inline (next to each other). Block elements(divs and sections) will render on top of each other. Margin top and bottom doesn't work that well for inline elements as they are not positioned in the flow of the css. We can change that behaviour with the display property. Display: none is removed from the visible document fow, but still exists on the DOM itself. Inline-block is the most useful. It mixes the behaviour of both inline and block elements. Like inline elements they can still go next to each other, but like block elements we can set paddings and top and bottom margins. 

### Display vs visibility

We had a look at display: none;  - this value removes the element to which you apply it from the document flow. This means that the element is not visible and it also doesn't "block its position". Other elements can (and will) take its place instead.

There is an alternative to that though.

If you only want to hide an element but you want to keep its place (i.e. other elements don't fill the empty spot), you can use visibility: hidden; 

Here's a visual example:
```
.box-1 {
    display: none;
}
 
.box-2 {
    display: inline-block;
}
Will render:

x  
```
where x  has the class box-2 . The first element just isn't displayed. It's still part of the DOM though, you can still access it via JavaScript for example.

Here's an example for visibility: hidden :
```
.box-1 {
    visibility: hidden;
}
 
.box-2 {
    display: inline-block;
}
Will render:

_x 

where _  simply is an empty spot and x  has the class box-2 .
```
The element is only invisible, it's not removed from the document flow and of course also not from the DOM.

## Block vs inline elements

It's not really a CSS topic, though it's related to it: The difference between block-level and inline elements.

You can read a more detailed article (which also includes a YouTube video about HTML at the top of the page) here: https://academind.com/learn/html/beginner-s-guide/diving-deeper-into-html#block-level-vs-inline-elements

Here's the executive summary:

Block-level elements are rendered as a block and hence take up all the available horizontal space. You can set margin-top and margin-bottom and two block-level elements will render in two different lines.

Some examples are: <div> , <section> , <article> , <nav>  but also <h1> , <h2>  etc and <p> .

Inline elements on the other hand only take up the space they require to fit their content in. Hence two inline-elements will fit into the same line (as long as the combined content doesn't take up the entire space in which case a line break would be added).

They also use the box-model you learned about but margin-top  and margin-bottom  have no effect on the element. padding-top  and padding-bottom  also have a different effect. They don't push the adjacent content away but they will do so with the element border. You can read more about that behavior in the following article: https://hacks.mozilla.org/2015/03/understanding-inline-box-model/

Additionally, setting a width  or height  on an inline element also has no effect. The width and height is auto to take as much space as required by the content.

Logically, this makes sense since you don't want your inline elements to destroy your multi-line text-layout. If you want to do so or need both block-level and inline behavior, you can set display: inline-block  to merge behaviors.

Some example elements are: <a> , <span> , <img>

Adding a class is marginally faster than adding a combinator, although it's likely you will never notice the difference

### Pseudo classes and pseudo elements

Pseudo classes define a style for a special state of a file. `:class name`
Pseudo elements define the style of a specific part of an element. `::element name`
It's about the state vs the part. A full list of pseudo classes are available on MDN. There are only a few that are actually used. They allow us to control different states or to style specific children. 

After and before pseudo elements allow you to render content through css. This should be content that adds to the design, not crucial page content.

### Properties worth remembering

- color:
- background-color:
- display:
- padding:
- margin:
- border:
- width:
- height:

### CSS Classes

We can add multiple classes to a single element.
When should you use a class selector or an id selector? The advantage of css classes is that they are re-usable and they are created for styling purposes only. They are strongly connected to css and should be the first pick. 
If you are using id's for a single object, you can use it to style an element, but it adds in some other effects that you may not be interested in. Use them, but don't add them in specifically for styling.

### !important

If you add !important to a css rule it overwrites specificity in all other selectors. You are overriding something that is baked into css so in general it's a very bad idea. Using specificity leads to better css code in the end. It makes your code hard to understand. It should only be used in edge cases such as a js related thing or a badly written third party library.
https://css-tricks.com/when-using-important-is-the-right-choice/

### Selecting with :not()
You can use :not() to specify a element or id of an element that you don't want the css rules applied to. 

### CSS and browser support

Not all features work across all browsers. MDN will tell you the if the feature is supported and which version across all browsers. You can also visit caniuse.com and let's you check if it works across all browsers and how much of the marke you will be reaching. 

### Outlines

Focus creates an outline for text input fields and buttons. Focus is normally set by the browser. It has an outline that is not part of the box model. You can style it with the outline property.

### Float

Float is great for positioning an image in text, but terrible for moving items around the page. It has essentially been replaced by f;exbox for positioning non image items. If you do use it you need to add an extra div with the clear both property