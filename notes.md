# 3 - CSS Grid Fundamentals

Allows you to dice up whatever element you have into a grid and then put the items at any positions along the grid.

Magic: `.item{$}*10` -> spits out a div with a class item with one through ten inside of it.
More magic: `.item.item${$}*10` will give us 10 divs with class `item itemY`. Really useful

The grid template columns and rows are collectively named `Tracks`

Items inside cells are stretched out to the full grid size.

Possible functions to use to size columns:

`auto`
`repeat()`
`200px`

# 4 - CSS Grid DevTools

Tracks are numbered not by the column or row themselves but rather by the lines that border them. 

So if you have two columns vertically, you have three lines that are numbering them.

`Dashed` and `solid` lines in the firefox devtools represent the explicit grid. This means
that these rows and columns have been explicitly created by our specification of the grid. 

Compare this with the `dotted` lines which represent the `implicitly` created grid.

# 5 - CSS Grid Implicit vs Explicit

If something is not explicitly defined but the elements exist then this is called an `implicitly`
created element. 

To size `implicitly` created rows we use the property

  `grid-auto-rows`

By default implicit elements are created as rows rather than columns.

# 6 - CSS Grid Autoflow

Grid autoflow determines whether the extra elements are added to rows or columns. 

The default is `row`

Set as 
  `grid-auto-flow: column`
  
Then you can also specify the size of any implicitly created columns using
  `grid-auto-columns: 200px;`
  
# 7 - CSS Grid Sizing Tracks

Percentages are generally not a great idea to use to fit to the width. 
So don't use percentages when you want to add to 100%.

Instead use the `fr` unit to specify the percentage of things.

Fractional units specify 
  `the amount of space left after all the elements are laid out`
  
It specifies the remaining space left over after the explicit values are given. These values are
things like the specified grid gap, explicitly defined widths, etc. They are in proportion to how much free 
space is left.

The auto keyword automatically sizes things based on the widest element.

# 8 - CSS Grid Repeat 

`repeat(4, 1fr auto)`

This repeats the (1fr auto) set of columns four times. It can be combined in almost any kind of way.

As an example you could do the following
```
.item-y {
	grid-template-columns: repeat(4, 1fr 2fr);
} 
```

This will give us 8 columns altogether with alternating sizes of _1fr_ followed by _2fr_. 

This can be combined in all kind of ways. 

```
.item-z {
	grid-template-columns: 10px repeat(4, 1fr auto) 20px;
} 
``` 

So the above would give us a column of `10px` followed by eight columns of alternating `1fr` and `auto` and ending with a column of 20px

# 9 - CSS Grid Sizing Items 

`.item.item${$}*30` More magic

Spanning: Can specifically tell some items to be a specific across multiple grid positions. 
  `grid-column: span 2;`
  `grid-row: span 2;`
 
 The above sets that item to span across two positions
 
# 10 - CSS Grid Placing Items 

`grid-column` is equivalend to `grid-column-start: track-value` and `grid-column-end: track-value`

`grid-row` is equivalend to `grid-row-start: track-value` and `grid-row-end: track-value`

As an example consider the following css class

```
.item-y {
	grid-column: span 2 / 5;
} 
```

What the above means is that we want `item-y` to span across **2** columns but end at track **5**. So grid will 
automatically find the right place for this grid item to go. 

So we can tell it _where to start and where to stop_ or we can tell it _either where to start or where to stop_. 
And we can also tell it how big it should be. 

Note that negative numbers can also be used to refer to track items. This will only work for explicitly defined grid
rows and columns and not those which were implicit.

# 12 Auto-fit and Auto-fill

`auto-fill` - figure out how much content I have and how much space is available and fill the items as required.
`auto-fit` - very similar to the above except it makes the track smaller. This allows you to move elements around as required. 

The example showed used it as follows: 

```
.container {
	grid-template-columns: repeat(auto-fit, 150px);
} 
```

The above creates a grid of however many items and fits the elements, each which is 150px, into the grid.

# 13 - Using minmax()

Use as follows:
```
.container {
	grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
} 
```

Minmax specifies the minimum width and maximum width of some element. Particularly useful when combined with `auto-fit`.

Similarly there exists a command called `fit-content(100px)` which makes the element the maximum size specified. 

# 14 - Grid Template Areas

It is possible to define grids areas.

```
.container {
  display: grid;
  grid-gap: 20px;
  grid-template-columns: 1fr 10fr 1fr;
  grid-template-rows: 150px 150px 100px;
  grid-template-areas:
    "sidebar-1  content   sidebar-2"
    "sidebar-1  content   sidebar-2"
    "footer     footer    footer"
}
```

Then you can specify where items live as follows

```
.item1 {
  grid-area: sidebar-1;
}
```

The benefit of doing this is that layouts can easily be changed by using media queries

```
@media(max-width: 700px) {
  .container {
    grid-template-areas: 
      "content content content"
      "sidebar-1 sidebar-1 sidebar-2"
      "footer footer footer"
  } 
} 

```


Now the layout will change as soon as the width is reduced below 700px.


Another cool thing that can be done is specifying where columns start and end based on the area names. 

For example the following is valid. 

```
.item3 {
  grid-column: sidebar-1-start / sidebar-1-end;
} 
```

So this can be used instead of line numbers.

# 15 - Naming Lines in CSS Grid

It is possible to name the tracks rather than using line numbers. This can be done as follows


```
.container {
  display: grid;
  grid-gap: 20px;
  grid-template-columns: [sidebar-start site-left] 1fr [sidebar-end content-start] 500px [content-end] 1fr [site-right];
  grid-template-rows: [content-top] repeat(10, auto) [content-bottom];
}

.item3 {
  background: slateblue;
  grid-column: content-start;
  grid-row: content-top / content-bottom;
  /* grid-row: 1 / span 10; */
}
```


Now rather than specifying track one or track two we can use `sidebar-start` or `site-left` to specify the start of our track.

# 16 - Grid Auto Flow dense block fitting

When specifying the length and width of elements it is often the case that the natural grid filling won't be as expected. 
Essentially there will be certain spots where the grid will not put any element in because it just doesn't fit. 

To make grid fill in these positions with elements that are further along we can do the following 


```
.container {
    display: grid;
    grid-gap: 20px;
    grid-template-columns: repeat(10, 1fr);
    grid-auto-flow: dense;
}
```


The property we are interested in is `grid-auto-flow: dense;`. This fills in the grid empty spaces with any elements that it can find. 

# 17 - Grid Alignment + Centering

Six properties align items within a grid:

<hr>

`justify-items`
 
`align-items`

These two align the content within the element itself.

<hr>

`justify-content`

`align-content`

These two align all the elements within the grid to fit the grid as required. 

<hr>
 
`justify-self`

`align-self`

These overwrite on a case by case basis for any element.

<hr>

The `justify-*` set of properties works along the x-axis or row axis. (So it will move elements left or right) 

The `align-*` set of properties works along the y-axis or column axis. (So it will move elements up or down) 

# 18 - Reordering Grid Items

It is possible to reorder grid items with the `order: <number>;` property. 

It has a few issues when it comes to selecting elements. 

# 19 - Nesting Grid with Album Layouts

The way that things should naturally work is the way that they actually work.

Some notes: 

```
.albums {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(400px, 1fr));
    grid-gap: 10px;
}
```

`minmax` should be combined with fractional units rather than other units. 
Otherwise the other units will dominate and you will always get the maximum width instead of one which varies nicely.

There's a little trick to making images fit properly. Have a look at the following css


```
    .album {
        display: grid;
        grid-template-columns: 150px 1fr;
        align-items: center;
        color: white;
        font-weight: 100;
    }

    .album__details {
        display: grid;
        align-items: center;
    }

    /* This forces the image to fit into its container rather than overflowing its size*/
    .album__artwork {
        width: 100%;
    }
```

The gtr defines the image to be 150px yet the image from our url is 300px. Without the `width: 100%;` specification the image will actually overflow its container leading to bad visuals. As such this is an important property to shrink the image. 
