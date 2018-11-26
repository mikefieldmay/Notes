# Positioning

The document flow that the page takes already has the position property set to static by default. We may want to change the position of elements on the page by adding a new value to postition property. Some properties are: 
- static: The default property of any webpage
- fixed: The position is fixed in place relative to the viewport. Good for headers.
- absolute: If there are no ancestors that contain the position property, it will place itself relative to to the html. If there are, it will place itself on the closest ancestore where the position is applied. 
- relative: Relative allows us to change the position of an element. With relative the positioning context is the element itself. You push the element away from it's current position.
- sticky. Sticky is a hybrid of relative and fixed. We can specify a distance from the viewport to become fixed, but it stops being fixed as soon as reaches the end of it's parent element. 

When we specify we want to change the position, we also need to specify how. We tell the element where to move to by using thde properties top, bottom, left and right. These properties refer to the original postions in the document flow. 
There is another important concept. We need to know the Positioning context of the thing we're moving.
By removing something from the document flow, the rest of the elements no longer care about them.

Position works for both block and inline elements.

If we reposition an element within an outer element using the relative value, we can then choose how the object is further displayed with teh overflow property. Overflow has a few different values that can be applied to it: 
- hidden: Anything outside the parent element it becomes invisible.

You cannot use overflow hidden on the body itself. Body passes the overflow property to the html element instead. This is the default css behaviour. You can add overflow hidden to both the body and the html.

### Stacking context

Stacking context is applied to fixed elements. When these elements are taken out of the flow, the elements fourther down the html will appear on top of the other elements. The stacking context only appies tot he elements that are within it. e.g the child of an element with a high z-index will not appear higher than the sibling of the parent element. 
