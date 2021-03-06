
# DOM

Document 对象并非独立到的，他是一个巨大的API核心对象，叫做文档对象模型（Document Object Model， DOM），他代表和操作文档的内容。

## DOM架构：
- 查询或选取单独的元素
- 作为节点树遍历查找祖先、兄弟、和后代
- 设置和查找元素的属性
- 查询、设置和修改文档的内容
- 创建、插入、删除节点来修改文档结构
- 与HTML表单一起工作

## 选取文档的元素

### 通过ID选取元素
- 用指定的id属性选取一个元素
` var section1 = document.getElementById('section1') `
- 同过ID选取多个元素
```js
function getElements('/*ids*/) {
  var elements = {};
  for(var i=0;i<arguments.length;i++) {
    var id = arguments[i];
    var elt = document.getElementById(id);
    if(elt==null) {
      throw new Error('No element width id: ' + id);
    }
    elements[id] = elt;
  }
}
// 在低于IE8 的浏览器中，getElementById()对匹配元素的ID不区分大小写，而且也返回匹配name属性的元素
```

### 通过名字选取元素
name是分配给元素的属性，区别与id，name属性值不是必须唯一的，多个元素可以有相同的名字，基于name选取元素用getElementsByName()
` var radiobuttons = document.getElementsByName('buttom'); // 返回一个nodelist 类数组对象`
> 在IE中getElementsByName() 也返回id属性匹配定值的元素

### 通过标签名来选取元素
getElementsByTagName选取所有指定类型（标签）的HTML或XML元素。返回一个类数组
` var span = document.getElementsByTagName('span') `
> 不区分标签大小写  

> getElementsByName() 和getElementsByTagName() 都返回nodelist对象，而类似document.images 和 document.forms 的属性为HTMLCollection对象。 
他们都是只读的类数组对象。    

通过CSS类选取元素
getElementsByClassName() 返回一个实时的nodelist对象，包括文档或元素的所有匹配的后代节点，只需要一个字符串参数，但该该字符串可以由多个空格隔开
的标识符组成，只有当元素属性只包含所有的指定的标识符才能匹配。
` var warm = document.getElementsByClassName('warm'); `
