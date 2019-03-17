# 两个数组的并集、交集和差集

### ES5方法
ES5利用filter方法和indexOf进行数学集操作，但是indexOf方法中NAN永远返回-1
所以需要进行兼容性处理。

- 不考虑NAN
```js
var a = [1,2,3,4];
var b = [2,3,5,6];
// 并集
var union = a.concat(b.filter(function(v) {
    return a.indexOf(v) === -1; 
}))
// 1,2,3,4,5,6

// 交集
var intersection = a.filter(function(v) {
    retrun b.indexOf(v) > -1;
})
// 2,3

// 差集
var difference = a.concat(b).filter(function(v) {
    return a.indexOf(v) === -1 || b.indexOf(v) === -1;
})
// 1,4,5,6
```
- 考虑NaN
```js
var a = [1,2,NaN];
var b = [2,4,5];
var aHasNaN = a.some(function(v) {
    return isNaN(v);
})
var bHasNan = b.some(function(v) {
    return isNaN(v);
})
// 并集
var union = a.concat(b.filter(function(v) {
    return a.indexOf(v) === -1 && !isNaN(v);
})).concat(!aHasNaN && bHasNaN ? [NaN] : [])
// 1,2,4,5.NaN

// 交集
var intersection = a.filter(function(v) {
    return b.indexOf(v) > -1;
}).concat(aHasNaN && bHasNaN ? [NaN] : []);
// 2

// 差集
var difference = a.concat(b).filter(function(v) {
    return b.indexOf(v) === -1 && a.indexOf(v) === -1 && !isNaN(v);
}).concat((aHasNaN && !bHasNaN) || (!aHasNaN && bHasNaN) ? [NaN] : []);
// 1,4,5,NaN
```

### ES6
通过ES6中的set结构的方法来实现
```js
let a = [1,2,3];
let b = [2,3,4];

let m = new Set(a);
let n = new Set(b);

// 并集
let union = Array.from(new Set(a.concat(b)));
// 1,2,3,4

// 交集
let intersection = Array.from(new Set(a.filter(v=> n.has(v))));
// 2,3

// 差集
let difference = Array.form(new Set(a.concat(b).filter( v=> {
    (m.has(v) && !n.has(v)) || ( !m.has(v) && n.has(v));
})))
```

### ES7
利用ES7中includes 方法
```js
let a = [1,2,3];
let b = [2,3,4];

// 并集
let union = a.concat(b.filter(v => !a.includes(v)));
// 1,2,3,4

// 交集
let intersection = a.filter(v => b.includes(v));
// 2,3

// 差集
let difference = a.concat(b).filter(v => (a.includes(v) && !b.includes(v)) || (!a.includes(v) && b.includes(v)));
// 1,4
```