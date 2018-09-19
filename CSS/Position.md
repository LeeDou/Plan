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