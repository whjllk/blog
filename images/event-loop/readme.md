### JavaScript 事件循环

JavaScript 是单线程的编程语言，也就是说它在同一时间内只能执行一个任务，但是同时需要处理 dom 渲染、js 执行、网络请求、浏览器执行和各种 I/O 之类的任务 本文会梳理下， js 是如何将这些任务按照正确的方式组合起来并执行的。

事件机制必须要解决一下几个问题。

1. 单线程如何去执行这些任务
2. 单线程如何处理同步任务和异步任务

在弄清楚这些问题之前，来看一张图：
![](event-loop.png?=300x300)

上图中，包含一个调用栈，一个执行队列，一个 webApis。这三个模块形成一个完整的事件循环。
1. 脚本被调用会触发脚本任务执行。这些任务会被压入到调用栈中等待执行。
2. 如果在任务中遇到同步任务，任务会被直接压入栈中执行，执行结束后出栈。
3. 如果遇到 webApis(定时器，Http)之类的异步任务，以 setTimeout 为例，定时器执行时，会被压入栈，然后执行完成，然后出栈。这个过程中，会有一个 time 的变量产生。
4. 然后，定时器结束，time 会往执行队列中插入一个定时器的回调函数。
5. 当调用栈中任务为空的时候，会从执行队列中取出一个回调推到调用栈中。
6. 回调函数执行。

在 es6 之前，整个事件循环大概就是这个样子的。es6 中提出了一个 **任务队列(Job Queue)**。同时推出两个任务的概念 **宏任务** 和 **微任务**。
此时，回调任务不再是众生平等，有了优先级的差异了。简言之，在执行队列中的任务都是宏任务，而在任务队列中的任务就是微任务。
值得一说的是微任务是由 Promise 产生的，Promise 本身并不是微任务。
``` js
console.log(1);
new Promise(function callback1(resolve){
  console.log(2);
  resolve();
}).then(function callback2() {
  console.log(3);
})
console.log(4);
```
上面的打印结果应该是 1， 2， 4， 3。
从这里我们可以看出 Promise 的 callback1 是被直接压入栈中执行，然后会把 callback2 推入任务队列中等待执行。在调用栈内容为空时，回去执行宏任务，在这之前会去查看微任务队列是否为空，如果有则拿到调用栈中执行

来看个宏任务和微任务在一起的例子。

``` js
console.log(1);
setTimeout(function timeoutCallbac() {
  console.log(2)
})
new Promise(function callback1(resolve){
  console.log(3);
  resolve();
}).then(function callback2(resolve){
  for(let i = 0; i <9999; i++) {
  }
  console.log(4);
  return new Promise(resolve => {
    resolve();
  });
}).then(function callback3() {
  console.log(5)
})
console.log(6);
```
上面代码的输出顺序为 1, 3, 6, 4, 5, 2。
1. console.log(1) 被压栈执行。
2. setTimeout 执行，并在产生了一个 time 变量等待定时结束，同时 setTimeout 出栈。
3. Promise 压栈， callback1 压栈并执行，执行结束,callback1 出栈， Promise 出栈，并在微任务列表中添加一个 callback2 回调。
4. console.log(6) 压栈执行。
5. 此时调用栈为空，发现微任务列表中有任务，拿出来执行，callback2 有复杂运算，此时 js 被阻塞。执行完成后，又往微任务队列中推入 callback3。同时出栈。然后从执行微任务队列中的 callback3。
6. 最后调用栈和微任务队列都为空，会从 执行队列中拿出 timeoutCallback 压栈执行。

如此一个完整的事件循环结束。

这里回答文章开始的三个问题。
1. JS 使用调用栈对需要执行的任务进行依次编排执行
2. 宏任务和微任务概念的引入，我们在主线程上去执行我们的调用栈任务，同时浏览器会起一个副线程上处理一些事件注入，并加入到异步任务队列中，按照微任务优先于宏任务的原则依次执行。
此时，事件循环的应该是这个样子的。
![](full-event.png?=300x300)

> 总结一句任务优先级， 同步任务 > 微任务 > 宏任务

参考资料：
* [消息队列和事件循环](https://time.geekbang.org/column/article/132931)
* [Understanding Javascript Function Executions](https://medium.com/@gaurav.pandvia/understanding-javascript-function-executions-tasks-event-loop-call-stack-more-part-1-5683dea1f5ec)