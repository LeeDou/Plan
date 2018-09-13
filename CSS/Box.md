# box model

## box dimensions
Each box has a content area (e.g., text, an image, etc.) and optional surrounding padding, border, and margin areas; the size of each area is specified by properties defined below. The following diagram shows how these areas relate and the terminology used to refer to pieces of margin, border, and padding:
    ![boxdim](/img/boxdim.png)

### each box has four edges: 
- content edge o inner edge
    The content edge surrounds the rectangle given by the width and height of the box, which often depend on the element's rendered content. The four content edges define the box's content box.
- padding edge 
    The padding edge surrounds the box padding. If the padding has 0 width, the padding edge is the same as the content edge. The four padding edges define the box's padding box.
- border edge 
    The border edge surrounds the box's border. If the border has 0 width, the border edge is the same as the padding edge. The four border edges define the box's border box
- margin edge or outer edge
    The margin edge surrounds the box margin. If the margin has 0 width, the margin edge is the same as the border edge. The four margin edges define the box's margin box.

### margin property
margin-top,margin-botttom,margin-right,margin-left
margin:
  Value:  	<margin-width> | inherit
  Initial:  	0
  Applies to:  	all elements except elements with table display types other than table-caption, table and inline-table
  Inherited:  	no
  Percentages:  	refer to width of containing block
  Media:  	visual
  Computed value:  	the percentage as specified or the absolute length
```
  body { margin: 2em }         /* all margins set to 2em */
  body { margin: 1em 2em }     /* top & bottom = 1em, right & left = 2em */
  body { margin: 1em 2em 3em } /* top=1em, right=2em, bottom=3em, left=2em */
```

### Collapasing margins

note:
  - Margins between a floated box and any other box do not collapse (not even between a float and its in-flow children).
  - Margins of elements that establish new block formatting contexts (such as floats and elements with 'overflow' other than 'visible') do not collapse with their in-flow children.
  - Margins of absolutely positioned boxes do not collapse (not even with their in-flow children).
  - Margins of inline-block boxes do not collapse (not even with their in-flow children).
  - The bottom margin of an in-flow block-level element always collapses with the top margin of its next in-flow block-level sibling, unless that sibling has clearance.
  - The top margin of an in-flow block element collapses with its first in-flow block-level child's top margin if the element has no top border, no top padding, and the child has no clearance.
  - The bottom margin of an in-flow block box with a 'height' of 'auto' and a 'min-height' of zero collapses with its last in-flow block-level child's bottom margin if the box has no bottom padding and no bottom border and the child's bottom margin does not collapse with a top margin that has clearance.
  - A box's own margins collapse if the 'min-height' property is zero, and it has neither top or bottom borders nor top or bottom padding, and it has a 'height' of either 0 or 'auto', and it does not contain a line box, and all of its in-flow children's margins (if any) collapse.

> When two or more margins collapse, the resulting margin width is the maximum of the collapsing margins' widths. In the case of negative margins, the maximum of the absolute values of the negative adjoining margins is deducted from the maximum of the positive adjoining margins. If there are no positive margins, the maximum of the absolute values of the adjoining margins is deducted from zero.

If the top and bottom margins of a box are adjoining, then it is possible for margins to collapse through it. In this case, the position of the element depends on its relationship with the other elements whose margins are being collapsed.
  - If the element's margins are collapsed with its parent's top margin, the top border edge of the box is defined to be the same as the parent's.
  - Otherwise, either the element's parent is not taking part in the margin collapsing, or only the parent's bottom margin is involved. The position of the element's top border edge is the same as it would have been if the element had a non-zero bottom border.

### padding properties: padding-top,padding-right,padding-bottom,padding-left,padding

**'padding'**
  Value:  	<padding-width>{1,4} | inherit
  Initial:  	see individual properties
  Applies to:  	all elements except table-row-group, table-header-group, table-footer-group, table-row, table-column-group and table-column
  Inherited:  	no
  Percentages:  	refer to width of containing block
  Media:  	visual
  Computed value:  	see individual properties

### border property
  The border properties specify the width, color, and style of the border area of a box. These properties apply to all elements.

  __Border width__: 'border-top-width', 'border-right-width', 'border-bottom-width', 'border-left-width', and 'border-width'
  #### <boder-width> value type:
  __thin__ A thin boder
  __mediun__ A medium boder
  __thick__ A thick boder
  __<length>__ the border's thickness has an explicit value, Explicit border widths cannot be negative.
  > thin <= mediium <= thick

  __'border-width'__
      Value:  	<border-width>{1,4} | inherit
      Initial:  	see individual properties
      Applies to:  	all elements
      Inherited:  	no
      Percentages:  	N/A
      Media:  	visual
      Computed value:  	see individual properties

  __border-color__: 'border-top-color', 'border-right-color', 'border-bottom-color', 'border-left-color', and 'border-color'
  ```js
    Value:  	[ <color> | transparent ]{1,4} | inherit
    Initial:  	see individual properties
    Applies to:  	all elements
    Inherited:  	no
    Percentages:  	N/A
    Media:  	visual
    Computed value:  	see individual properties
  ```

  __boder-style__:  'border-top-style', 'border-right-style', 'border-bottom-style', 'border-left-style', and 'border-style'


    - none
      No border; the computed border width is zero.
    - hidden
      Same as 'none', except in terms of border conflict resolution for table elements.
    - dotted
      The border is a series of dots.
    - dashed
      The border is a series of short line segments.
    - solid
      The border is a single line segment.
    - double
      The border is two solid lines. The sum of the two lines and the space between them equals the value of 'border-width'.
    - groove
      The border looks as though it were carved into the canvas.
    - ridge
      The opposite of 'groove': the border looks as though it were coming out of the canvas.
    - inset
      The border makes the box look as though it were embedded in the canvas.
    - outset
      The opposite of 'inset': the border makes the box look as though it were coming out of the canvas.

### the box model for inline elements in bidirectional context
For each line box, UAs must take the inline boxes generated for each element and render the margins, borders and padding in visual order (not logical order).
