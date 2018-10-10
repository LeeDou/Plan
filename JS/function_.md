# function* 
> 这种声明方式会定义一个生成器函数（generator function），它返回一个Generator对象

```js
function* generator(i) {
  yield i;
  yield i + 10;
}

var gen = generator(10);

console.log(gen.next().value);
// expected output: 10

console.log(gen.next().value);
// expected output: 20
```

## 语法
> `function* name([param[,param[,...param]]]){ statements }`
__`name`__
  函数名  
__`param`__
  要传递给函数的一个参数的名称，一个函数最多可以有255个参数   
___`statments`__
  普通JS语句

### 也可以用构造函数`GeneratorFunction` 或 `function* expression` 定义生成器函数
- 构造函数
  `new GeneratorFunction([arg1[,arg2[],...argN]] functionBody)`
- 表达式内部定义
  ```
    var x = function* (y) {
      yield y * y;
    }
  ```

## 描述
**生成器函数**在执行时能暂停，后面又能从暂停处继续执行。   
调用一个生成器函数不会马上执行他里面的语句，而是返回这个生成器的迭代器（iterator）对象。当这个迭代器的`next()`方法被首次（后续）调用时，其内部语句会执行到第一个（后续）出现`yield`的位置为止，`yield`后紧跟迭代器要反回的值。或者用的是`yield*`（多了个*号），则表示将执行权移交给另一个生成器函数（当前生成器停止执行）。    
`next()`方法返回一个对象，这个对象包含两个属性： value和done，value属性表示本次yield表达式的返回值，done属性为布尔类型，表示生成器后续是否还有yield语句，即生成器是否已执行完毕并返回。   
调用next（）方法时，如果传入了参数，那么这个参数会作为上一条执行的yield语句的返回值，例如：
```js
function *gen(){
    yield 10;
    y=yield 'foo';
    yield y;
}

var gen_obj=gen();
console.log(gen_obj.next());// 执行 yield 10，返回 10
console.log(gen_obj.next());// 执行 yield 'foo'，返回 'foo'
console.log(gen_obj.next(10));// 将 10 赋给上一条 yield 'foo' 的左值，即执行 y=10，返回 10
console.log(gen_obj.next());// 执行完毕，value 为 undefined，done 为 true

```
当在生成器函数中显式 return 时，会导致生成器立即变为完成状态，即调用 next() 方法返回的对象的 done 为 true。如果 return 后面跟了一个值，那么这个值会作为当前调用 next() 方法返回的 value 值。

## 例子

- 使用迭代器遍历二位数组并转换成一维数组
```js
function* iterArr(arr) {            //迭代器返回一个迭代器对象
  if (Array.isArray(arr)) {         // 内节点
      for(let i=0; i < arr.length; i++) {
          yield* iterArr(arr[i]);   // (*)递归
      }
  } else {                          // 离开     
      yield arr;
  }
}
// 使用 for-of 遍历:
var arr = ['a', ['b', 'c'], ['d', 'e']];
for(var x of iterArr(arr)) {
  console.log(x);               // a  b  c  d  e
}

// 或者直接将迭代器展开:
var arr = [ 'a', ['b',[ 'c', ['d', 'e']]]];
var gen = iterArr(arr);
arr = [...gen];                        // ["a", "b", "c", "d", "e"]

```
- 传递参数
```js
function *createIterator() {
    let first = yield 1;
    let second = yield first + 2; // 4 + 2 
                                  // first =4 是next(4)将参数赋给上一条的
    yield second + 3;             // 5 + 3
}

let iterator = createIterator();

console.log(iterator.next());    // "{ value: 1, done: false }"
console.log(iterator.next(4));   // "{ value: 6, done: false }"
console.log(iterator.next(5));   // "{ value: 8, done: false }"
console.log(iterator.next());    // "{ value: undefined, done: true }"
```

> 生成器不能当构造函数用