## CSS - What is it?

CSS stands for cascading stylesheets. The core idea is taht it makes your webpage look good. HTML is the structure of the webpage. It is completely necessary. CSS is optional, but it allows you to style your boring HTML webpage. 
CSS 1 was first introduced in 1966. CSS 2 was released in 1998. CSS 3 is still in development. There will never be a CSS 4 as CSS 3 will continuously be in development. It is split up into independant modules. You will get different versions of the modules, but CSS 3 won't increase in version. It is under active development an will comntinue to evolve.

# Adding CSS to page

You can add css inline: 
```
<section style="background: #ff1b68">
    <h1>Get the freedom you deserve.</h1>
</section>
```

This is not reccomended as you may need to add many styles to a certain section and it qould be a nightmare to change the styles.

You can add it in a style tag in the head: 
```
<style>
    section {
        background: #ff1b68
    } 
</style>
```

One rule applies to all sections. This is a benefit over inline styles

The third method is to have a seperate stylesheet:
```
    <link rel="stylesheet" href="main.css">
```

The link is included in the head. This is the recommended method as it creates a clear seperation of concerns. It also allows the browser to cache the downloaded stylesheet and reduces the size of the html file

## Basic CSS

The fonts that are available to you are specified by your browser. Each browser has specific defaults:
```
h1 {
    color: white;
    font-family: sans-serif;
}
```
You can leave it blank which uses the default, sans-sarif, or monospace. You can rely on these keywords to use the browsers fonts.

To include a specific font, you can download the font from Google https://fonts.google.com/

There are different selectors we can use to specify rules to certain page sections: 
- HTML Elements (such as h1 or body). They set equal style for all the elements.
- Class selectors (set by using the class keyword). Set equal style for all elements within the same class. We add the class.
- Universal selectors (set * { } syntax in the style sheet). These rules apply to everything on the page. This is rarely used.
- ID selectors (set using the id keyword). Set style to only one specific element as IDs should be specific.
- Attributes (set using attributes such as disabled, hover etc). Set equal style to all elements with attributes. 
```
[disabled] {
    color: red;
}
```

When we add style rules to a sheet we sometimes overwrite them in certain areas. Multiple rules can effect the same element. 
Inline files have the highest specificity, followed by class and then element. Cascading means multiple rules camn be applied to teh same element. Specificity resolves conflicts arising from multiple rules

### Highest to lowest priority
- Inline styles
- #ID selectors
- .class, :pseudo-class and [attribute] selectors
- `<tag>` and ::pseudo- element selectors

These rules are good to keep in mind. 

It's better to apply styling to the body than to everything using the * wildcard.

Inheritence means that an element inherits some of it's styles from it's parent. It's specificity is very low, but all elements inherit styles from their parents. It is very useful for font-size and font-family.

### Combinators:

By adding the outer element/id/class on the outside, you can target children of the element. This is different to inheritence as it's not passing down a style., it's selecting an element that is the child of another element.

```
#product-overview h1 {
    color: white;
    font-family: 'Anton', sans-serif;
}
```

There are 4 important types of combinator:
- Adjacent sibling: 
Adjacent assigns it to all paragraphs that directly follow a h2 tag. It needs a direct siblingship. They must be on the same level.
```
// html
<div>
    <h2>Hi</h2>
    <p>Applied</p>
    <p>Not Applied</p>
    <h2>Hi</h2>
    <h2>Hi</h2>
    <h2>Hi</h2>
    <p>Applied</p>
</div>
// css
div + p {

}
```
- General Sibling:
For general sibling it is only important that they have a general sibling
```
// html
<div>
    <h2>Hi</h2>
    <p>Applied</p>
    <p>Applied</p>
    <h2>Hi</h2>
    <h2>Hi</h2>
    <h2>Hi</h2>
    <p>Applied</p>
</div>
// css
div ~ p {
    
}
```
- Child
Only applies it to direct children of the outer container. 
```
div > p {
    
}
```
- Decendant
Applies styles to all children of the outer container.
```
div p {

}
```
Direct selectors show a slightly better performance, but general combinators tend to be the most frequently used.

### Values:
Values are tightly coupled to css properties. There are (loosely) 4 types of vaklue:
- Pre-defined. These have a few specific options that you can choose from.
- Colors: let you work with names or hex codes
- Length sizes and numbers: pixels, percentages or just numbers
- Functions: Background: url(), transfrom: scale();