# font

## Font family
```js
'font-family'
Value:  	[[ <family-name> | <generic-family> ] [, <family-name>| <generic-family>]* ] | inherit
Initial:  	depends on user agent // 取决于用户平台
Applies to:  	all elements
Inherited:  	yes
Percentages:  	N/A
Media:  	visual
Computed value:  	as specified

<family-name>
The name of a font family of choice. In the last example, "Gill" and "Helvetica" are font families.
<generic-family>
In the example above, the last value is a generic family name. The following generic families are defined:
'serif' (e.g., Times)
'sans-serif' (e.g., Helvetica)
'cursive' (e.g., Zapf-Chancery)
'fantasy' (e.g., Western)
'monospace' (e.g., Courier)
```

## Font styling
```js
'font-style'
Value:  	normal | italic | oblique | inherit
Initial:  	normal
Applies to:  	all elements
Inherited:  	yes
Percentages:  	N/A
Media:  	visual
Computed value:  	as specified
```

## Font weight
```js
'font-weight'
Value:  	normal | bold | bolder | lighter | 100 | 200 | 300 | 400 | 500 | 600 | 700 | 800 | 900 | inherit
Initial:  	normal
Applies to:  	all elements
Inherited:  	yes
Percentages:  	N/A
Media:  	visual
Computed value:  	see text
```
__Values of 'bolder' and 'lighter' indicate values relative to the weight of the parent element.__

The meaning of 'bolder' and 'lighter'
|Inherited value|bolder|lighter|
|:---:|:---:|:---:|
|100	|400	|100  |
|200	|400	|100  |
|300	|400	|100  |
|400	|700	|100  |
|500	|700	|100  |
|600	|900	|400  |
|700	|900	|400  |
|800	|900	|700  |
|900	|900	|700  |


## Font size
```js
'font-size'
Value:  	<absolute-size> | <relative-size> | <length> | <percentage> | inherit
Initial:  	medium
Applies to:  	all elements
Inherited:  	yes
Percentages:  	refer to inherited font size
Media:  	visual
Computed value:  	absolute length
```