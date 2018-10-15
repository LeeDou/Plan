
# Event Loop
顾名思义，Event Loop就是事件循环的意思，js 事件就是要按照一定的规则排序执行。Event Loop在不同的环境下有着不同的方式。

## 浏览器下的Event Loop
![loop](/img/stack.jpg)   
- 当主线程运行的时候，JS会产生堆和栈（执行栈）
- 主线程中调用webapi所产生的异步操作（dom事件，ajax回调，定时器等）只要产生结果，就要把这个回调塞进“任务队列中等待执行”。
- 档主线程中的任务执行完毕，系统就会依次读取“任务队列”中的任务，将任务放进执行栈中执行。
- 执行任务时还可能产生行的异步操作，就产生新的循环，整个过程是循环不断的。

### 宏任务与微任务
任务队列中的所有任务都是会按照一定的规则执行，宏任务（macro-task）会按照事件队列先进先出的顺序执行，微任务（micro-task）会在主线程执行完成后执行，即在当前宏任务队列前执行。   
任务队列包含两个：宏任务，微任务。 微任务会在主线程执行完后执行，进入执行栈，当微任务队列没有任务时，宏任务才会进入执行栈。   
微任务包括： 原生Promise，Object.observe,MutationObserver,MessageChannel;   
宏任务包括： setTimeout,setInterval,setImmediate,I/O;

## Node 环境下的Event Loop
```
   ┌───────────────────────┐
┌─>│        timers         │
│  └──────────┬────────────┘
│  ┌──────────┴────────────┐
│  │     I/O callbacks     │
│  └──────────┬────────────┘
│  ┌──────────┴────────────┐
│  │     idle, prepare     │
│  └──────────┬────────────┘      ┌───────────────┐
│  ┌──────────┴────────────┐      │   incoming:   │
│  │         poll          │<─────┤  connections, │
│  └──────────┬────────────┘      │   data, etc.  │
│  ┌──────────┴────────────┐      └───────────────┘
│  │        check          │
│  └──────────┬────────────┘
│  ┌──────────┴────────────┐
└──┤    close callbacks    │
   └───────────────────────┘
```
node中的时间循环与浏览器中的不一样    
- timers 阶段： 这个阶段执行setTimeout（callback）and setInterval（callback）预定的callback；
- I/O callback阶段： 执行除了close事件的callback，被timers（定时器，setTimeout，setInterval等）设定的callbacks，setImmediate()设定的callback之外的callback
- idle，prepare 阶段： 仅node内部使用；
- poll阶段： 获取新的I/O事件，适当条件下node将阻塞在这里；
- check阶段： setImmediate()设定的callback；
- close callback 阶段： 比如socket.on('close', callback)的callback会在这个阶段执行。  

每一个阶段都会有一个装有callback的FIFO queue（队列），当event loop运行到一个指定阶段时，node将执行该阶段的FIFO queue，当callback执行完成，或执行callback的数量超过该阶段的上限时，eventloop就会转入下一个阶段。

### process.nextTick
process.nextTick方法不在上面的时间环中，我们可以把它理解为微任务，他执行的时机是当前“执行栈”的尾部----下一次Event Loop（主线程读取“任务队列”）之前触发回调函数。也就是说他指定的任务总是发生在所有的异步任务之前。setImmediate方法则是在当前任务队列的尾部添加新的事件，也就是说他指定的任务总是在下一次Event Loop时执行。  
```js
process.nextTick(function A() {
  console.log(1);
  process.nextTick(function B(){console.log(2);});
});

setTimeout(function timeout() {
  console.log('TIMEOUT FIRED');
}, 0)
// 1
// 2
// TIMEOUT FIRED

// 由此可以看到A在setTimeout前执行，B也在setTimeout前执行，多个process.nextTick无论是否嵌套，都会在当前
```

### setTimeout 和 setImmediate 
二者非常相似，但二者的区别取决于他们什么时候被调用
- setImmediate是在当前poll阶段完成执行  // setImmediate() is designed to execute a script once the current poll phase completes.
- setTimeout是在指定的时间到达后执行，且当前队列为空   //setTimeout() schedules a script to be run after a minimum threshold in ms has elapsed.

二者的调用顺序取决于当前event loop的上下文，如果他们在异步I/O callback之外调用，其执行先后顺序是不确定的。
```js
setTimeout(function timeout () {
  console.log('timeout');
},0);

setImmediate(function immediate () {
  console.log('immediate');
});

$ node timeout_vs_immediate.js
timeout
immediate

$ node timeout_vs_immediate.js
immediate
timeout
```
这是因为好一个事件进入的时候，事件环可能处于不同的阶段导致结果的不确定。当我们给了事件环确定的上下文，事件的先后顺序就能确定了。
```js
var fs = require('fs')

fs.readFile(__filename, () => {
  setTimeout(() => {
    console.log('timeout')
  }, 0)
  setImmediate(() => {
    console.log('immediate')
  })
})

$ node timeout_vs_immediate.js
immediate
timeout
```

[event loop 文档](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/)

[掘金](https://juejin.im/post/5aab2d896fb9a028b86dc2fd)


