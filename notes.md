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

# 13 Using minmax()

Use as follows:
```
.container {
	grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
} 
```

Minmax specifies the minimum width and maximum width of some element. Particularly useful when combined with `auto-fit`.

Similarly there exists a command called `fit-content(100px)` which makes the element the maximum size specified. 
