## CSS Grid
CSS grid allows you to create highly customisable page layouts.

### What is CSS grid

A web page can be divided into a series of row and columns. These could be called the grid of the webpage. A grid will help position the elements easier. Positioning is one of the most difficult tasks when using CSS.
Most modern browsers support the grid well, but older internet explorer versions are not supported. 
Firefox offers the best debugging support for grid.

### Creating a grid

We can turn a container into a grid by setting the `display` value to `grid`. Only direct children are considered as part of the grid. All the child components are now part of the grid. 
`grid-template-columns` specifies the width of the columns in the grid
```
  grid-template-columns: 200px 150px 20% 1fr;
```
fr is a special unit we can use with grid. All regular units will be calculated first and then fr will work out the rest. 
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

It is possible to give names to columns and rows. Uoi pass the name in as so:
```
 .container {
      grid-template-rows: [row-1-start] 5rem [row-2-start] 4rem
  }
```

It is then possible to refer to the named rows from the child components.
      `grid-row-end: row-3-start; `
