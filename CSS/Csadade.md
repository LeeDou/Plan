## Assgining property values,cascading, and inheritance

> [文档链接](https://www.w3.org/TR/CSS2/cascade.html)
> Once a user agent has parsed a document and constructed a document tree, it must assign, for every element in the tree, a value to every property that applies to the target media type.

The final value of a property is the result of a four-step calculation: the value is determined through specification (the "specified value"), then resolved into a value that is used for inheritance (the "computed value"), then converted into an absolute value if necessary (the "used value"), and finally transformed according to the limitations of the local environment (the "actual value").

### specified values (规定值)
指定元素规定值顺序：
  1. If the cascade results in a value, use it
  2. Otherwise, if the property is inherited and the element is not the root of the document tree,use the computed value of the parent element.
  3. Othewise use the property's initial value, the initial of each property is indicated in the property's definition.

### computed values (计算值)
  Specified values are resolved to computed values during the cascade; for example URIs are made absolute and 'em' and 'ex' units are computed to pixel or absolute lengths. Computing a value never requires the user agent to render the document.

### Used values （使用值）
  The used value is the result of taking the computed value and resolving any remaining dependencies into an absolute value.

### Actual value (真实值)
  The actual value is the used value after any approximations have been applied.

### inheritance （继承）
  ```
  body {
    color: black !important; 
    background: white !important;
  }
                                           // 由于body优先级高于* 所以body color、background样式会以自己声明样式，* 统配所有的元素，故其他元素均以*中样式展示
  * { 
    color: inherit !important; 
    background: transparent !important;
  }
  ```
### the @import rule
  ```
  @import "mystyle.css";
  @import url("mystyle.css");
  @import url("fineprint.css") print;
  @import url("bluish.css") projection, tv;   // 指定媒体类型
  ```
### the cascade
style sheet may have three diffent origins: author, user, and user agent
```
  !important // all user and author rulers have more weight than rules in the UA's default style sheet 
```

### Csacading oder
To find the value for an element/property combination, user agents must apply the following sorting order:

 1. Find all declarations that apply to the element and property in question, for the target media type. Declarations apply if the associated selector matches the element in question and the target medium matches the media list on all @media rules containing the declaration and on all links on the path through which the style sheet was reached.
 2. Sort according to importance (normal or important) and origin (author, user, or user agent). In ascending order of precedence:
    1. user agent declarations
    2. user normal declarations
    3. author normal declarations
    4. author important declarations
    5. user important declarations
 3. Sort rules with the same importance and origin by specificity of selector: more specific selectors will override more general ones. Pseudo-elements and pseudo-classes are counted as normal elements and classes, respectively.
 4. Finally, sort by order specified: if two declarations have the same weight, origin and specificity, the latter specified wins. Declarations in imported style sheets are considered to be before any declarations in the style sheet itself.


Apart from the "!important" setting on individual declarations, this strategy gives author's style sheets higher weight than those of the reader. User agents must give the user the ability to turn off the influence of specific author style sheets, e.g., through a pull-down menu. Conformance to UAAG 1.0 checkpoint 4.14 satisfies this condition [UAAG10].

### !important rules
> CSS attempts to create a balance of power between author and user style sheets. By default, rules in an author's style sheet override those in a user's style sheet (see cascade rule 3).

### Calculating a selector's specificity
  ```
  *             {}  /* a=0 b=0 c=0 d=0 -> specificity = 0,0,0,0 */
  li            {}  /* a=0 b=0 c=0 d=1 -> specificity = 0,0,0,1 */
  li:first-line {}  /* a=0 b=0 c=0 d=2 -> specificity = 0,0,0,2 */
  ul li         {}  /* a=0 b=0 c=0 d=2 -> specificity = 0,0,0,2 */
  ul ol+li      {}  /* a=0 b=0 c=0 d=3 -> specificity = 0,0,0,3 */
  h1 + *[rel=up]{}  /* a=0 b=0 c=1 d=1 -> specificity = 0,0,1,1 */
  ul ol li.red  {}  /* a=0 b=0 c=1 d=3 -> specificity = 0,0,1,3 */
  li.red.level  {}  /* a=0 b=0 c=2 d=1 -> specificity = 0,0,2,1 */
  #x34y         {}  /* a=0 b=1 c=0 d=0 -> specificity = 0,1,0,0 */
  style=""          /* a=1 b=0 c=0 d=0 -> specificity = 1,0,0,0 */
  ```

# Prencedence of non-CSS presentational hints