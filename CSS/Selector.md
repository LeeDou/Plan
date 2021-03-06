## Selector
### contents
- [pattern matching](#chapter1)
- [selector syntax](#chapter2)
- [universal selector](#chapter3)
- [type selector](#chapter4)

## <div id='chapter1'>pattern matching</div>
> In CSS, pattern matching rules determine which style rules apply to elements in the document tree. These patterns, called selectors, may range from simple element names to rich contextual patterns. If all conditions in the pattern are true for a certain element, the selector matches the element.  

在CSS中模式匹配规则决定哪些样式适用文档树中的元素，这些模式称为选择器，从简单的节点到丰富的上下文，如果所有情况下这个模式适用于确定的元素，那么选择器匹配元素

![pattern](/img/pattern.jpg)
```
* match any
E match any E element
E F match any F element is E descendant
E>F match any F that is a child of an element E
E:first-child match E when E is the first child of its parent
E:link
E:visited match E a which target is not yet visited(:link) or already visited(:visited)
E:active
E:hover
E:focus match E during certain uer actions
E:lang(c) match type E if in langue c
E + F match F any F immediately preceded by a sibling element E //F立即被E领先
E(foo) match any E width th 'foo' attribute whenerver the attribute
E[foo="warning"] match E whose foo attribute id exactly equal to "warning"
E[foo~="warning"] match E whose foo is a list of space-seperated values,one of which is exactly equal to "warning"
E[lang|='en'] match any E whose "lang" has a hypen-separated list of values with "en"
DIV.warning  the same as DIV[class~=warning]
E#myid match E with ID equal to "myid"
```

## <div id='chapter2'> selector syntax </div>

A simple selector is either a type selector or universal selector followed immediately by zero or more attribute selectors, ID selectors, or pseudo-classes, in any order. The simple selector matches if all of its components match.