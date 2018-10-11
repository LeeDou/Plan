# promise
## Promise 的含义
Promise 是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更合理和更强大。它由社区最早提出和实现，ES6 将其写进了语言标准，统一了用法，原生提供了Promise对象。

所谓Promise，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。

Promise对象有以下两个特点。

（1）对象的状态不受外界影响。Promise对象代表一个异步操作，有三种状态：pending（进行中）、fulfilled（已成功）和rejected（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是Promise这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变。

（2）一旦状态改变，就不会再变，任何时候都可以得到这个结果。Promise对象的状态改变，只有两种可能：从pending变为fulfilled和从pending变为rejected。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（已定型）。如果改变已经发生了，你再对Promise对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。

注意，为了行文方便，本章后面的resolved统一只指fulfilled状态，不包含rejected状态。

有了Promise对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。此外，Promise对象提供统一的接口，使得控制异步操作更加容易。

Promise也有一些缺点。首先，无法取消Promise，一旦新建它就会立即执行，无法中途取消。其次，如果不设置回调函数，Promise内部抛出的错误，不会反应到外部。第三，当处于pending状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。

如果某些事件不断地反复发生，一般来说，使用 Stream 模式是比部署Promise更好的选择。

### promise有什么
- promise有三种状态： pending，resolved，rejected：
  开始执行，即进入pending状态，根据执行的结果状态会变为resolved或rejected
- promise构造函数接受一个函数fn作为参数，这个函数包含两个参数，resolve，reject（这两个函数由JavaScript引擎提供）。resolve将promise对象状态由pending转变为resolved（由未完成转变为成功），并将异步操作的结果作为参数传递出去，reject将pending状态转化为rejected（由未完成变为失败），在异步操作失败时调用，并将失败的结果作为参数传递出去。（在函数中可以根据执行条件执行resolve或reject）
- promise的then方法： 在实例生成以后可以用then方法分别制定resolved状态和rejected状态的回调函数，rejected的回调函数可以忽略。then方法接受两个方法f1,f2作为参数,f1、f2接受promise对象传出的值作为参数，f1在promise状态变为resolved时候调用，f2在promise状态变为rejected时候调用。

#### promise实现Ajax例子
```js
const getJSON = function(url) {
  const promise = new Promise(function(resolve, reject){
    const handler = function() {
      if (this.readyState !== 4) {
        return;
      }
      if (this.status === 200) {
        resolve(this.response);
      } else {
        reject(new Error(this.statusText));
      }
    };
    const client = new XMLHttpRequest();
    client.open("GET", url);
    client.onreadystatechange = handler;
    client.responseType = "json";
    client.setRequestHeader("Accept", "application/json");
    client.send();

  });

  return promise;
};

getJSON("/posts.json").then(function(json) {
  console.log('Contents: ' + json);
}, function(error) {
  console.error('出错了', error);
});
``` 

## promise api
- Promise.prototype.then() Promise实例具有then()方法，也就是说then方法是定义在原型对象Promise.prototype上的。作用是为Promise实例添加状态改变时的回调函数。then方法第一个参数是resolve的回调函数，第二个参数（可选）是rejected状态的回调函数。
then方法两个处理函数返回值，若为Promise实例，就以promise状态进行处理，返回若为基本类型这直接传递给下一个then resolve处理函数，若没返回值，则下一个then方法处理函数参数为undefined

- Promise.prototype.catch() 方法是.then(null, rejection)的别名，用于指定发生错误时的回调函数。
```js
p.then((val)=> console.log('success:', val))
  .catch((err)=> console.log('rejected:', err))

// 等同于
p.then((val)=> console.log('success:', val))
  .then(null, (err)=> console.log('rejected:', err))
```
如果Promise状态已经变成resolved，再抛出错误是无效的。Promise对象的错误具有“冒泡”性质，会一直向后传递，直到被捕获为止。也就是说错误总是会被下一个catch语句捕获。一般来说，不要在then方法里面定义Reject状态的回调函数，总是使用catch方法。catch可以捕获then中方法执行的错误。  
promise抛出的错误不会传递到外层代码，即不会有任何反应。Promise的内部错误不会影响到promise的外部代码，通俗的说法就是“Promise会吃掉错误”
```js
const someAsyncThing = function() {
  return new Promise(function(resolve, reject) {
    // 下面一行会报错，因为x没有声明
    resolve(x + 2);
  });
};

someAsyncThing().then(function() {
  console.log('everything is great');
});

setTimeout(() => { console.log(123) }, 2000);
// Uncaught (in promise) ReferenceError: x is not defined
// 123

// 正常输出123
```
这个脚本放在服务器执行，退出码就是0（即表示执行成功）。不过，Node 有一个unhandledRejection事件，专门监听未捕获的reject错误，上面的脚本会触发这个事件的监听函数，可以在监听函数里面抛出错误。
```js
process.on('unhandledRejection', function (err, p) {
  throw err;
});
```
注意，Node 有计划在未来废除unhandledRejection事件。如果 Promise 内部有未捕获的错误，会直接终止进程，并且进程的退出码不为 0。catch方法还可以抛出错误。
- Promise.prototype.finally() finally方法用于指定不管Promise对象最后状态如何，都会执行的操作，该方法是ES2018引入的。   
finally方法的回调函数不接受任何参数，这意味着没有办法知道前面的Promise状态到底是fulfilled还是reject。finally方法里面的操作与状态无关，不依赖于Promise的执行结果。
finally的实现：
```js
Promise.prototype.finally = function (callback) {
  let P = this.constructor;
  return this.then(
    value => P.resolve(callback()).then(()=>value),
    reason => P.resolve(callback()).then(()=>{throw reson})
  );
}
```
- Promise.all()
Promise.all方法用于将多个Promise实例包装成一个新的Promise实例。
`const p = Promise.all([p1, p2, p3]);`
(Promise.all方法的参数可以不是数组，但必须具有Interator接口，且返回的每个成员都是Promise实例)    
p的状态由p1,p2,p3决定，分成两种情况。
  - 只有p1,p2,p3的状态都变成fulfilled，p的状态才会变成fulfilled，此时p1,p2,p3的返回值组成一个数组，传递给P的回调函数。
  - 只要p1,p2,p3的之中有一个被rejected，p的状态就变成rejected，此时第一个被rejected的实例的返回值会传递给p的回调函数。（若p1,p2,p3中rejected的自身有catch方法捕获，则不会抛出rejected到p,p的状态为resolved）

- Promise.race() 
Promise.race方法同样是将多个Promise实例，包装成一个新的Promise实例。
`const p = Promise([p1,p2,p3])`
只要p1,p2,p3之中的一个实例率先改变状态，p的状态就会跟着改变，那个率先改变的Promise实例就是返回值，就传递给p的函数调用。

- Promise.resolve()
有时需要将现有对象转化为Promise对象，Promise.resolve方法就是起到这个作用。
`const jsPromise = Promise.resolve($.ajax('/whatever.json'))`
Promise.resolve 等价于下面的写法。
```js
Promise.resolve('foo')
// 等价于
new Promise(resolve => resolve('foo'))
```

- Promise.reject()
Promise.reject(reason)方法也返回一个新的Promise实例，该实例的状态为rejected。
```js
const p = Promise.reject('出错了')
// 等同于
const p = new Promise((resolve, reject) => reject('出错了'))

p.then(null, function (s) {
  console.log(s)
})
// 出错了
```
上面代码生成一个 Promise 对象的实例p，状态为rejected，回调函数会立即执行。

注意，Promise.reject()方法的参数，会原封不动地作为reject的理由，变成后续方法的参数。这一点与Promise.resolve方法不一致。

- Promise.try()
在实际开发中，你不知道或不想知道函数f是同步函数还是异步函数，直接用Promise处理，这样处理的结果是如果f是同步函数他会在本轮事件循环的末尾执行。
让同步函数同步执行，异步函数异步执行：
方法一： 用async函数来写
```js
const f = () => console.log('now');
(async () => f())();
console.log('next');
// now
// next
// 如果f是异步的
const f = () => console.log('now');
(async () => f())()
  .then(()=>{})
  .catch(()=>{})
```
方法二：  使用new Promise()
```js
const f = () => console.log('now');
(
  () => new Promise(
    resolve => resolve(f())
  )
)();
console.log('next')
// now 
// next
```
现有提案用try 替代上面的写法
```js
const f = () => const.log('now');
Promise.try(f);
console.log('next')
// now 
// next
```