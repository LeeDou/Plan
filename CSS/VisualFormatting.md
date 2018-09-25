# Visual formatting model details(视觉格式化细节)

## Definition of "containing block" （包含块定义）
The position and size of an element's box(es) are sometimes calculated relative to a certain rectangle, called the containing block of the element. The containing block of an element is defined as follows: （包含块定义）
1. The containing block in which the root element lives is a rectangle called the initial containing block. For continuous media, it has the dimensions of the viewport and is anchored at the canvas origin; it is the page area for paged media. The 'direction' property of the initial containing block is the same as for the root element.
2. For other elements, if the element's position is 'relative' or 'static', the containing block is formed by the content edge of the nearest block container ancestor box.
3. If the element has 'position: fixed', the containing block is established by the viewport in the case of continuous media or the page area in the case of paged media.
4. If the element has 'position: absolute', the containing block is established by the nearest ancestor with a 'position' of 'absolute', 'relative' or 'fixed', in the following way:（对于absolute属性元素，包含块可以是absolute、relative、fixed)
- In the case that the ancestor is an inline element, the containing block is the bounding box around the padding boxes of the first and the last inline boxes generated for that element. In CSS 2.1, if the inline element is split across multiple lines, the containing block is undefined.
- Otherwise, the containing block is formed by the padding edge of the ancestor.（如果祖先元素是行内元素，则包含块的大小由包含元素的填充边界）
  If there is no such ancestor, the containing block is the initial containing block.（如果没有这样的祖先元素，则包含块是根包含块）
  
![包含块](/img/containing.png)

## content width: the'width' property
`'width'`
  Value:  	<length> | <percentage> | auto | inherit
  Initial:  	auto
  Applies to:  	all elements but non-replaced inline elements, table rows, and row groups
  Inherited:  	no
  Percentages:  	refer to width of containing block
  Media:  	visual
  Computed value:  	the percentage or 'auto' as specified or the absolute length
