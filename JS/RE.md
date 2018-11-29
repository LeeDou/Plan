# JavaScript正则表达式

正则表达式（英语：Regular Expression，在代码中常简写为regex、regexp或RE）使用单个字符串来描述、匹配一系列符合某个句法规则的字符串搜索模式。

搜索模式可用于文本搜索和文本替换。

## 语法

```js
/正则表达式主体/修饰符（可选）

// 实例：
var patt = /runoob/i
```

### 使用字符串方法
- search() 方法，用于检索字符串中指定的子字符串，返回子串的起始位置
- replace() 方法，用于在一些字符串中用一些字符串替换另一些字符串，或替换一个与正则表达匹配的子串。
- match() 方法，找到一个或多个正则表达式的匹配
- split() 方法，将字符串分割为数组
```js
// 实例一: search方法使用正则表达式 
var str = "visit Runoob!";
var n = str.search(/Runoob/i); // n:6

// 实例二： search 方法使用字符串
var str = "visit Runoob";
var n = str.search("Runoob")

// 实例三： replace方法使用正则表达式
var str = "this is Visit microsoft";
var txt = str.replace(/microsoft/i, "runoob"); // txt: this is visit runoob

// 实例四： replace使用字符串
var str = "this is Visit microsoft";
var txt = str.replace("microsoft", "runoob"); // txt: this is visit runoob

// 实例五： match
var arr = "the best things in life."
var arr1 = arr.match(/[e]/g)  // [e, e]
```

[正则](Other/Reg.md)

### 使用 RegExp对象
在JavaScript中，Regexp对象十一预定义了属性和方法的正则表达式对象。
- test
test() 是正则表达式方法，用于检测一个字符串是否匹配某个模式，如果字符串含有匹配的文本，则返回true ，否则返回false
```js
var patt = /e/;
patt.test("the best things in left.");  // true
```

- exec
exec() 方法用于检测正则表达式中的匹配，返回一个数组存放匹配的结果，肉未找到匹配就返回null
```js
/e/.exec("the best things in life.") // e
```

- toString
返回正则表达式的字符串