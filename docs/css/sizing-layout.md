
# css sizing layout
[sizing layout in smashing magazine](https://www.smashingmagazine.com/2018/01/understanding-sizing-css-layout/)

# data-type

- font relative length unit
em rem ch ex 
- viewport relative length unit
vw vh vmin vmax
- absolute units
cm mm Q in pc pt px
- percentages


intrinsic and extrinsic sizing
content-based "intrinsic" sizes
context-based "extrinsic" sizes

[CSS Intrinsic and Extrinsic Sizing Module](https://drafts.csswg.org/css-sizing-3/)
> "This module extends the CSS sizing properties with keywords that represent content-based "intrinsic" sizes and context-based "extrinsic" sizes, allowing CSS to more easily describe boxes that fit their content or fit into a particular layout context."

manage situations where there is more space than needed to display items, or items need to fit into less space than they would take up if space was infinite.


Grid Layout track sizing
flex-basis sizing

leftover space in the grid container.

*Content-Based Sizing*
max-content
min-content
fit-content(<length-percentage>)
*Auto-Sized*
auto in flex basis

div
- hitting the edges of the containing block of edge

A div is a block level element, and so, if you don’t give it a width, it will stretch out to be as wide in the inline dimension as it can
Either until it reaches the edge of the viewport or the containing block.
If a string of text is longer than the space allowed, it will wrap inside the div, and the box will become taller to accommodate it

*minmax css function*




![offsetHeight vs clientHeight vs scollHeight](images/css*height.png)

- offsetHeight 

Height occupied by the element on document

- clientHeight:
only `VISIBLE content & padding`
CSS height + CSS padding - height of horizontal scrollbar


- scollHeihgt
`ENTIRE  content & padding (visible or not)`
Height of all content + paddings, despite of height of the element.

- Element.getBoundingClientRect()