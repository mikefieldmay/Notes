## CSS and Javascript

Sometimes you need to change a page after a page has loaded. 
Styles can be manipulated via javascript, and can also add and remove classes.

It is possible to write javascript inline with `<script>` tags. You can also use a separate javascript file and add it as a script src.

We can access elements in the dom and change their attributes.

We can access the dom using `document.querySelector('.Class')` or `document.querySelectorAll('.Class')`. The second returns an array of all items with the class, whilst the `querySelector` returns the first instance in the dom. 