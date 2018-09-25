# position schemes

## three position shemes:
  - Noraml flow
  - Floats
  - Absolute positioning

## position property
```js
  'position'
    value:  	static | relative | absolute | fixed | inherit
    Initial:  	static
    Applies to:  	all elements
    Inherited:  	no
    Percentages:  	N/A
    Media:  	visual
    Computed value:  	as specified
```
- static: The box is a normal box, laid out according to the normal flow. The 'top', 'right', 'bottom', and 'left' properties do not apply.
- relative: The box's position is calculated according to the normal flow (this is called the position in normal flow). Then the box is offset relative to its normal position.
- absolute: The box's position (and possibly size) is specified with the 'top', 'right', 'bottom', and 'left' properties.
- fixed: The box's position is calculated according to the 'absolute' model, but in addition, the box is fixed with respect to some reference. As with the 'absolute' model, the box's margins do not collapse with any other margins. 

## box offsets: top,right,bottom,left
```js
  'top'|bottom|left|right
    Value:  	<length> | <percentage> | auto | inherit
    Initial:  	auto
    Applies to:  	positioned elements
    Inherited:  	no
    Percentages:  	refer to height of containing block
    Media:  	visual
    Computed value:  	if specified as a length, the corresponding absolute length; if specified as a percentage, the specified value; otherwise, 'auto'.
```

## Normal flow
> Boxes in the normal flow belong to a formatting context, which may be block or inline, but not both simultaneously. Block-level boxes participate in a block formatting context. Inline-level boxes participate in an inline formatting context.

### block formatting contexts (块级格式化上下文)
浮动、绝对定位、块容器（例如 inline-blocks,table-cells,table-captions）不是块盒子，块盒子的溢出超出视图为他的内容创建新的盒模式不是块盒子

### inline formating context （行内格式化上下文）
In an inline formatting context, boxes are laid out horizontally, one after the other, beginning at the top of a containing block. Horizontal margins, borders, and padding are respected between these boxes. The boxes may be aligned vertically in different ways: their bottoms or tops may be aligned, or the baselines of text within them may be aligned. The rectangular area that contains the boxes that form a line is called a line box.
> 在行内格式化上下文中，元素都是一个接一个水平排布，水平margin、border、padding在每个元素之间，每个盒子可能有不同的垂直对齐方式，

## ralative positioning (相对定位)
Once a box has been laid out according to the normal flow or floated, it may be shifted relative to this position. This is called relative positioning. 
相对定位元素会保持正常的大小，left,right改变元素的水平位置，盒子不会因left或right而撕裂或拉伸，所以有value `left = -right`
> If both 'left' and 'right' are 'auto' (their initial values), the used values are '0' (i.e., the boxes stay in their original position).
If 'left' is 'auto', its used value is minus the value of 'right' (i.e., the boxes move to the left by the value of 'right').
If 'right' is specified as 'auto', its used value is minus the value of 'left'.
If neither 'left' nor 'right' is 'auto', the position is over-constrained, and one of them has to be ignored. If the 'direction' property of the containing block is 'ltr', the value of 'left' wins and 'right' becomes -'left'. If 'direction' of the containing block is 'rtl', 'right' wins and 'left' is ignored.

## floats (浮动)
A float is a box that is shifted to the left or right on the current line. 
浮动盒子就是在当前行内移动到左或右

- Positioning the float: the__'float'__ property
  ```js
  float
    value: left|right|none|inherit
    initial: none
    applies to: all
    perv=centages: N/A
    media: visual
    computed value: as specified
  ```
- controlling flow next to floats: the 'clear' property
  ```js
    clear
      value: none|left|right|both|inherit
      initial: none
      applies to: block-leval elements
      inheited: no
      percentages: N/A
      media: visual
      computed: as sepcified
  ```

## absolute positionig
绝对定位元素，会显示地在正常文档流中移除，绝对定位盒子会建立一个新的包含正常文档流的文档块，但不包含fixed元素。绝对定位文档流或许会覆盖正常文档流也可能被正常文档流覆盖，这取决于他的层叠样式层级（z-index）

### fixed positioninig 
fixed positionig 是 absolute positioning的子集，他们唯一的区别是fixed 定位的包含区块是建立在视图（viewport）,对于连续的媒体，当文件（document）滑动时绝对定位元素不会移动。

## relationships between 'display', 'position', and 'float'
1. If 'display' has the value 'none', then 'position' and 'float' do not apply. In this case, the element generates no box.
2. Otherwise, if 'position' has the value 'absolute' or 'fixed', the box is absolutely positioned, the computed value of 'float' is 'none', and display is set according to the table below. The position of the box will be determined by the 'top', 'right', 'bottom' and 'left' properties and the box's containing block.
3. Otherwise, if 'float' has a value other than 'none', the box is floated and 'display' is set according to the table below.
4. Otherwise, if the element is the root element, 'display' is set according to the table below, except that it is undefined in CSS 2.1 whether a specified value of 'list-item' becomes a computed value of 'block' or 'list-item'.
5. Otherwise, the remaining 'display' property values apply as specified.
| specified value | computed value |
| :---: | :---: |
| inline-table | table |
| inline,table-row-group,table-column-group,table-column,table-header-group,table-row,table-cell,table-caption,inline-block | block |
| others | same as specified |

## layered presentation 

###  Specifying the stack level: the 'z-index' property
```js
'z-index'
  Value:  	auto | <integer> | inherit
  Initial:  	auto
  Applies to:  	positioned elements // 添加position属性的元素
  Inherited:  	no
  Percentages:  	N/A
  Media:  	visual
  Computed value:  	as specified
```
In CSS 2.1, each box has a position in three dimensions. In addition to their horizontal and vertical positions, boxes lie along a "z-axis" and are formatted one on top of the other. Z-axis positions are particularly relevant when boxes overlap visually. This section discusses how boxes may be positioned along the z-axis.
在CSS2.1中每个包含position属性的元素，会根据"Z-zais"进行绘制

Within each stacking context, the following layers are painted in back-to-front order:(堆叠元素的绘制次序)
  1. the background and borders of the element forming the stacking context.
  2. the child stacking contexts with negative stack levels (most negative first).
  3. the in-flow, non-inline-level, non-positioned descendants.
  4. the non-positioned floats.
  5. the in-flow, inline-level, non-positioned descendants, including inline tables and inline blocks.
  6. the child stacking contexts with stack level 0 and the positioned descendants with stack level 0.
  7. the child stacking contexts with positive stack levels (least positive first).

## Text direction: the 'direction' and 'unicode-bidi' properties
```js
'direction'
  Value:  	ltr | rtl | inherit
  Initial:  	ltr
  Applies to:  	all elements, but see prose
  Inherited:  	yes
  Percentages:  	N/A
  Media:  	visual
  Computed value:  	as specified
```
- __ltr__
Left-to-right direction.
- **rtl**
Right-to-left direction.

```js
'unicode-bidi'
  Value:  	normal | embed | bidi-override | inherit
  Initial:  	normal
  Applies to:  	all elements, but see prose
  Inherited:  	no
  Percentages:  	N/A
  Media:  	visual
  Computed value:  	as specified
```
- **normal**
The element does not open an additional level of embedding with respect to the bidirectional algorithm. For inline elements, implicit reordering works across element boundaries.
- **embed**
If the element is inline, this value opens an additional level of embedding with respect to the bidirectional algorithm. The direction of this embedding level is given by the 'direction' property. Inside the element, reordering is done implicitly. This corresponds to adding a LRE (U+202A; for 'direction: ltr') or RLE (U+202B; for 'direction: rtl') at the start of the element and a PDF (U+202C) at the end of the element.
- **bidi-override**
For inline elements this creates an override. For block container elements this creates an override for inline-level descendants not within another block container element. This means that inside the element, reordering is strictly in sequence according to the 'direction' property; the implicit part of the bidirectional algorithm is ignored. This corresponds to adding a LRO (U+202D; for 'direction: ltr') or RLO (U+202E; for 'direction: rtl') at the start of the element or at the start of each anonymous child block box, if any, and a PDF (U+202C) at the end of the element.
