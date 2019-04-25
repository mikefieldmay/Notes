## CSS Grid
CSS grid allows you to create highly customisable page layouts.
Elements that are not part of the document flow, are not part of the grid.

### What is CSS grid

A web page can be divided into a series of row and columns. These could be called the grid of the webpage. A grid will help position the elements easier. Positioning is one of the most difficult tasks when using CSS.
Most modern browsers support the grid well, but older internet explorer versions are not supported. 
Firefox offers the best debugging support for grid.

### Creating a grid

We can turn a container into a grid by setting the `display` value to `grid`. Only direct children are considered as part of the grid. All the child components are now part of the grid. 
`grid-template-columns` specifies the width and amount of the columns in the grid
```
  grid-template-columns: 200px 150px 20% 1fr;
```
fr is a special unit we can use with grid. All regular units will be calculated first and then fr will work out the rest. It works like a fraction.
`grid-template-rows: 5rem 10rem;` allows us to define the height of the rows we're using. 

### Positioning Child elements in a grid

We can override the default positioning of elements. `grid-column-start` allows us to choose where our element starts. THe grid automatically adds numbers to the lines in the grid. 
```
.el3 {
    background: rgba(0, 128, 0, 0.5);
    grid-column-start: 3; // start at column 3
    grid-column-end: 5; // end at column 5
    grid-row-start: 1; // start at row 1
    grid-row-end: 3; // end at row 3
}
```

### element-sizing, repeat and min-max

If you want to make sure that the final row to take up all remaining space, we should use the auto property:
```
.container {
    margin: 20px;
    display: grid;
    grid-template-columns: 1fr 1fr 1fr auto; // takes up as much space as is left
    grid-template-rows: 5rem auto; // takes as much space as the content requires
    // if height was set on container, it would take up the remaining height. 
}
```
`grid-template-columns: repeat(4, 25%)` // is shorthand for creating grids with the layout quickly.
`grid-template-rows: minmax(10px, 200px)` // min height 10, max height 200px.

### Advanced element positioning

There are more advanced positioning methods we can use
```
  .el3 {
      background: rgba(0, 128, 0, 0.5);
      grid-column-start: 3; // start at column 3
      grid-column-end: span 2; // end after it occupies 2 cells.
      grid-row-start: 1; // start at row 1
      grid-row-end: 3; // end at row 3
  }
```
```
    grid-column-start: 1;
    grid-column-end: -1; // this means it starts counting from the end of the grid which then moves back

```
It is possible for cells to overlap if we explicitly set the column adn row position. You have many different ways of targeting elements and rows, and where they start and end.

### Working with named lines

It is possible to give names to columns and rows. You pass the name in as so:
```
 .container {
      grid-template-rows: [row-1-start] 5rem [row-2-start] 4rem
  }
```

It is then possible to refer to the named rows from the child components.
      `grid-row-end: row-3-start; `

### Shorthands
There are some useful shorthands for css grid:
```
    grid-row-start: 1; // start at row 1
    grid-row-end: 3; // end at row 3
    // can be shortened to 
    grid-row: 1 / 3
```
The same thing can be done with column.

`grid-area` lets you wrap up multiple values into one in the following way `grid-area: row-start / column-start / row-end / column-end`. 
```
.el3 {
    grid-column-start: 3; 
    grid-column-end: span 2; 
    grid-row-start: 1; 
    grid-row-end: 3; 
    grid-area: 1 / 3 / 3 / span-2
}
```

### Working with gaps

`grid-row-gap` and `gird-column-gap` allow us to add a gutter between the cells. Elements that overlap multiple cells will flow over the gap.
`grid-gap: row-gap column-gap` is shorthand for defining both.

### Adding named template areas

It is possible to add named areas to your grid using `grid-template-areas`. You add strings to this in order to create your grid. The syntax is easy, but very different to css. In a 4 x 4 grid:
```
// In the parent
grid-template-areas: "header header header header"
                    "side side main main"
                    "footer footer footer footer"

// In the child
    grid-area: header
```
Grid areas are a powerful tool that lets us easily define areas. You have to add as many words as cells to the final grid though. 

### Automatically generate Grid Areas

```
.container {
    grid-template-columns: repeat(4, [col-start] 25% [col-end]); // we add column start and column end to each column
}

el1 {
    background: rgba(255, 154, 72, 1);
    grid-column: col-start 2 / col-end 4; 
    /* start at the second column staer and end at the 4th column end */
}
```
It is possible to add automatically add names to larger areas of the grid:
```
.container {
    grid-template-columns: [hd-start] repeat(4, [col-start] 25% [col-end]) [hd-end]; // adding hd start and end to all of our column area.
}
```

Manually laying out the grid is probably easier for other devs to understand, but it's useful taht certain things can be auto generated.


### fit-content

fit-content takes an argument of a size. It takes a default size if the content requires less space. If it needs more space it will take it.
```
    grid-template-rows: 3.5rem auto fit-content(8rem);
```

### Positioning elements in a grid

## Justify items
`justify-items` can be applied to a css grid. It positions the elements relative to the column in their grid areas. 
You can set it to `start`, `middle` and `end`. The default is stretch. Start and end are not left and right, if the read setting is right to left, `start` will appear on the right.

## Align Items
`align-items` is similar to `justify-items`, but for positioning in the row. You can set it to `start`, `middle` and `end`. The default is also stretch.

## Positioning the entire grid

You may have a grid that has a certain height or width which contains inside it a grid that is smaller than the containing div. `justify-content` changes the positioning across the x-axis. `align-content` changes the positioning across the y axis. 

## Positioning elements individually

All elements are set to stretch by default along the x and y axis. You can overide the specific position of grid items by using the `justify-self` for the x axis and `align-self` for the y axis.

### Understanding responsive grids

You may want to have different positioning for different rows and columns. You can do this by wrapping the container layout in a media query e.g
```
.container {
    margin: 20px;
    display: grid;
    grid-template-columns: [hd-start] repeat(4, [col-start] 25% [col-end]);
    grid-template-areas: "header header header header"
                        ". side main main"
                        "footer footer footer footer";
    grid-template-rows: [hd-start] 5rem [hd-end row-2-start] minmax(10px, 20rem) [row-2-end row-3-start] 100px [row-3-end];
    padding: 10px;
    background: grey;
}

@media (max-width: 40rem) {
    .container {
        grid-template-rows: [hd-start] 5rem [hd-end row-2-start] minmax(10px, 20rem) [row-2-end row-3-start] 150px [row-3-end row-4-start] 100px [row-4-end];
        grid-template-areas: "header header header header"
                                "main main main main"
                                "side side side side"
                                "footer footer footer footer";

    }
}
```
By using named areas, the element reposition themselves automatically when the screen switches breakpoints. This can be highly useful.

## Applying autoflow

The grid has some automatic features that make using it a lot easier. 
By default, if you add display grid, it will put all it's child elements into new rows. If we have a page where we don't know how many rows we need, we can rely on the grid automatically creating them. The default size of each cell is auto. We can override this default using the `grid-auto-rows` property. The default value for this is `auto`. We can use our own values if we like. `auto` fits it's content, so if we want to ensure the size of all rows, it's better to use a set value. 
New content is automatically put into a new row. We can switch this behaviour to allow the grid to append new columns. We can do this with `grid-auto-flow`. The default is set to row. When using this, you can also use     `grid-auto-columns` to customise the size of each column.
```
.container {
    margin: 20px;
    display: grid;
    grid-template-columns: repeat(auto-fill, 15rem);
    justify-content: center;
    grid-gap: 1.5rem;
    grid-auto-rows: 10rem;
    /* grid-auto-columns: 5rem; */
    grid-auto-flow: row; 
}
```

## Comparing the explicit and implicit grid

You can define the size of the rows manually. Even if you have multiple rows, you can specifically target rows in order to increase their size. Explicit is rows and columns that you have defined. Implicit is when the grid uses it's automatic features to build the rest of the grid. You don't always need to define the grid. You can rely on new rows being added automatically.

## Understanding auto-fill and auto-fit

if we want our content to be responsize on many different device sizes. There is a helper value we can use so we don;t need to explicitly know how many columns we need per row. `auto-fill` ensures that the current row is filled with as many items as possible. `grid-template-columns: repeat(auto-fill, 15rem);` There is an alternative value you can use called `auto-fit` as an alternative. This is used if it's likely you won't have enough content for an entire row.

## Creating a Dense Grid

If we want to have a specific child that is larger than the others and we use the `grid-column: span 2;` syntax without specifically declaring the columns we want to take, the space where this item is will not be filled in the grid. We can sort this out by setting the property `dense`  `grid-auto-flow: row dense;`. This will cause the grid to put in smaller child elements into the space if a smaller element appears later. It leaves the space at the end of the grid instead. This may not be optimal as it changes the visible dom order, but not the actual html so it may cause accessibility issues. 

## Comparing Grid and Flexbox

The key differences between the 2 are that the grid is about 2 dimensional positioning. Rows and Columns. Flexbox is one dimensional, rows or columns. Flexbox is always a decent choice if you just have a single row, or a single column. 
The grid is a great choice is if you have a more complex layout e.g if you want to have both column and row positioning. Flexbox is excellent fi items to be next to each other or to be displayed in a column.
1 Dimension = flexbox
Multiple dimensions = grid.


https://css-tricks.com/snippets/css/complete-guide-grid/