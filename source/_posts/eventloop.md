---
title: eventloop(事件循环)
date: 2022-10-17 13:55:07
tags: js 事件循环
cover: https://w.wallhaven.cc/full/6d/wallhaven-6dqd17.png
---

##### 事件循环机制

`JavaScript` 是单线程的。所谓单线程意味着一次只能运行一个任务。

浏览器的事件循环分为**同步任务**和**异步任务**；所有同步任务都在主线程上执行，形成一个函数调用栈（执行栈），而异步则先放到**任务队列**（**task queue**）里，任务队列又分为**宏任务**（macro-task）与**微任务**（micro-task）。

执行顺序:**同步任务**->**微任务**->**宏任务**

##### 常见的微任务和宏任务

**宏任务**:
`setInterval()` `setTimeout()` `setImmediate(Node独有)` `requestAnimationFrame(浏览器独有)` `I/O UI rendering(浏览器独有)`

**微任务**:
`new Promise()` `new MutaionObserver()` `process.nextTick(Node独有)` `Object.observe`

##### 示例一

```js
async function async1() {
  console.log("a");
  const res = await async2();
  console.log("b");
}
async function async2() {
  console.log("c");
  return 2;
}
console.log("d");
setTimeout(() => {
  console.log("e");
}, 0);
async1().then((res) => {
  console.log("f");
});
new Promise((resolve) => {
  console.log("g");
  resolve();
}).then(() => {
  console.log("h");
});
console.log("i");
```

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/63bb4925a49b481987dbe8804ac2bfc8~tplv-k3u1fbpfcp-watermark.image?)

<a href="https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/63bb4925a49b481987dbe8804ac2bfc8~tplv-k3u1fbpfcp-watermark.image?" target="back">`图片详情`</a>

执行顺序如上图所示。最终得到的结果是 d、a、c、g、i、b、h、f、e。
