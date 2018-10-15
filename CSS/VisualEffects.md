# Visual Effects

## overflow and clipping
```js
'overflow'
Value:  	visible | hidden | scroll | auto | inherit
Initial:  	visible
Applies to:  	block containers
Inherited:  	no
Percentages:  	N/A
Media:  	visual
Computed value:  	as specified
```

```js
'clip'
Value:  	<shape> | auto | inherit
Initial:  	auto
Applies to:  	absolutely positioned elements
Inherited:  	no
Percentages:  	N/A
Media:  	visual
Computed value:  	'auto' if specified as 'auto', otherwise a rectangle with four values, each of which is 'auto' if specified as 'auto' and the computed length otherwise
```
在CSS2.1中，唯一有效的shape值是： rect(`<top>,<right>,<bottom>,<left>`) `<top>和<bottom>`计算的基线是盒子顶部边框（top border）,`<right>和<left>`的计算基线是盒子的左边框（left border）。   
`<top>,<right>,<bottom>,<left>`其中任何一个的值可能是`<length>`value 或 auto，负的length值是被允许的。auto值意味着切割元素的边缘与生成元素的边缘相同。（在ie中，'auto'意味着`<left>`和`<top>`的值为0，bottom值为垂直方向上padding和border值的和，right值为水平方向padding和border值的和，最终auto值导致切割元素的大小与元素border相同）
> In CSS 2.1, the only valid `<shape>` value is: rect(`<top>, <right>, <bottom>, <left>`) where `<top>` and `<bottom>` specify offsets from the top border edge of the box, and `<right>`, and `<left>` specify offsets from the left border edge of the box. Authors should separate offset values with commas. User agents must support separation with commas, but may also support separation without commas (but not a combination), because a previous revision of this specification was ambiguous in this respect.      

### Visibility: the 'visibility'property
```js
'visibility'
Value:  	visible | hidden | collapse | inherit
Initial:  	visible
Applies to:  	all elements
Inherited:  	yes
Percentages:  	N/A
Media:  	visual
Computed value:  	as specified
```

- visible:
The generated box is visible.
- hidden:
The generated box is invisible (fully transparent, nothing is drawn), but still affects layout. Furthermore, descendants of the element will be visible if they have 'visibility: visible'.
- collapse:
Please consult the section on dynamic row and column effects in tables. If used on elements other than rows, row groups, columns, or column groups, 'collapse' has the same meaning as 'hidden'.