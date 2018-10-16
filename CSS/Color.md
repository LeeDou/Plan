# Colors and Backgrounds

## Foreground color: the 'color'property
```js
'color'
Value:  	<color> | inherit
Initial:  	depends on user agent
Applies to:  	all elements
Inherited:  	yes
Percentages:  	N/A
Media:  	visual
Computed value:  	as specified
```

## The background 
就盒子模型而言，“background”是指的包含content、padding、border区域在内的背景。border color和style通过border 属性来设置，margin 总是透明。  background的属性是不能继承的，但是父元素的背景颜色总是会通过初始的background-color：“transparent” 透明值而发光。

> In terms of the box model, "background" refers to the background of the content, padding and border areas. Border colors and styles are set with the border properties. Margins are always transparent.

### background properties： 'background-color','background-image','background-repeat','background-attachment','background-position'and background
```js
'background-color'
Value:  	<color> | transparent | inherit
Initial:  	transparent
Applies to:  	all elements
Inherited:  	no
Percentages:  	N/A
Media:  	visual
Computed value:  	as specified

'background-image'
Value:  	<uri> | none | inherit
Initial:  	none
Applies to:  	all elements
Inherited:  	no
Percentages:  	N/A
Media:  	visual
Computed value:  	absolute URI or none


'background-repeat'
Value:  	repeat | repeat-x | repeat-y | no-repeat | inherit
Initial:  	repeat
Applies to:  	all elements
Inherited:  	no
Percentages:  	N/A
Media:  	visual
Computed value:  	as specified


'background-attachment'
Value:  	scroll | fixed | inherit
Initial:  	scroll
Applies to:  	all elements
Inherited:  	no
Percentages:  	N/A
Media:  	visual
Computed value:  	as specified

'background-position'
Value:  	[ [ <percentage> | <length> | left | center | right ] [ <percentage> | <length> | top | center | bottom ]? ] | [ [ left | center | right ] || [ top | center | bottom ] ] | inherit
Initial:  	0% 0%
Applies to:  	all elements
Inherited:  	no
Percentages:  	refer to the size of the box itself
Media:  	visual
Computed value:  	for <length> the absolute value, otherwise a percentage
```