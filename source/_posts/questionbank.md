---
title: 知识点、面试题整理
date: 2022-09-01 16:24:48
tags: 
- 面试题
- Vue
- Js
- Axios
- 方法
categories: 
- questions 
- 方法
cover: https://w.wallhaven.cc/full/50/wallhaven-501w34.png
aplayer: true
---
{% meting "1320544645" "netease" "song" "theme:#555" "mutex:true" "listmaxheight:340px" "preload:auto" %}
#### $nextTick
Vue生命周期的created()钩子函数进行的DOM操作一定要放在Vue.nextTick()的回调函数中，原因是在created()钩子函数执行的时候DOM 其实并未进行任何渲染，而此时进行DOM操作无异于徒劳，所以此处一定要将DOM操作的js代码放进Vue.nextTick()的回调函数中。与之对应的就是mounted钩子函数，因为该钩子函数执行时所有的DOM挂载已完成。nextTick 就是设置一个回调，用于异步执行。
就是把你设置的回调放在 setTimeout 中执行，这样就算异步了，等待当时同步代码执行完毕再执行。
```javascript
  created(){
    let that=this;
    that.$nextTick(function(){  //不使用this.$nextTick()方法会报错
        that.$refs.aa.innerHTML="created中更改了按钮内容";  //写入到DOM元素
    });
  },
```
#### 跨域

 * ==iframe:== iframe一般用来包含别的页面，例如我们可以在我们自己的网站页面加载别人网站的内容，为了更好的效果，可能需要使iframe透明效果，那么就需要了解更多的iframe属性。 
 
 * postMessage
 
 * proxyTable(vue中的proxy代理)
 
 * nodejs 中间件(设置请求头)
 ```javascript
 app.all("*", function(req, res, next) {
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-control-Allow-Headers", "xCors");    //允许请求头中携带 xCors
    next();
})
```
* cors
```javascript
var express = require('express');
var app = express();
var allowCrossDomain = function (req, res, next) {
  res.header('Access-Control-Allow-Origin', 'http://localhost:3001');
  res.header('Access-Control-Allow-Methods', 'GET,PUT,POST,DELETE');
  res.header('Access-Control-Allow-Headers', 'Content-Type');
  next();
}
app.use(allowCrossDomain);
```
* websocket
```javascript
// Create WebSocket connection.
const socket = new WebSocket('ws://localhost:8080');

// Connection opened
socket.addEventListener('open', function (event) {
    socket.send('Hello Server!');
});

// Listen for messages
socket.addEventListener('message', function (event) {
    console.log('Message from server ', event.data);
});
```
* jsonp

基本原理： 主要就是利用了 script 标签的src没有跨域限制来完成的(只能进行GET请求)。
```javascript
<script type='text/javascript'>
    window.jsonpCallback = function (res) {
        console.log(res)
    }
</script>
<script src='http://localhost:8080/api/jsonp?id=1&cb=jsonpCallback' type='text/javascript'></script>
```

* nginx反向代理

 正向代理代理客户端，反向代理代理服务器。
 
 #### 闭包
 1. 什么是闭包,闭包的优缺点?
 
闭包就是函数作用域链保留了一段本来应该被销毁的作用域.

优点:有点是全局变量私有化
 
缺点:内存溢出.(一般执行完以后让变量变成null)

闭包的使用场景节流和防抖

函数防抖是某一段时间内只执行一次，而函数节流是间隔时间执行。

登录、input输入时候的联想菜单请求发短信等按钮避免用户点击太快，以致于发送了多次请求，需要防抖
```javascript
//防抖
function handle() {
    let timer;
    return function () {
        clearTimeout(timer);
        timer = setTimeout(() => {
            console.log(11);
        }, 300);
    };
}
```
```javascript
//节流，滚动
function throttle() {
  let timeout

  return function() {
    if (timeout) return 
      timeout = setTimeout(() => {
        timeout = null
        console.log(11);
      }, 1000)
  }
}
window.onscroll = throttle()
```
#### 普通函数和箭头函数的区别
1.箭头函数没有原型 原型是undefined

2.箭头函数this指向全局对象 而函数指向引用对象

3.call，apply，bind方法改变不了箭头函数的指向

#### jquery和vue区别
1. vue数据驱动
2. jq控制dom元素
3. vue渲染优雅，代码易维护
#### js基本数据类型

string、boolean、number、null、undefined、symbol、object

#### this指向

this指向调用函数的主体对象,谁点他调用this就指向谁

==call apply bind==区别:call() 与apply()只有一个区别，就是 call() 方法接受的是一个参数列表，而 apply() 方法接受的是一个包含多个参数的数组。bind()  方法创建一个新的函数，在 bind() 被调用时，这个新函数的 this 被指定为 bind() 的第一个参数，而其余参数将作为新函数的参数，供调用时使用。
* call的实现
```javascript
 Function.prototype.myCall = function(target, ...args) {
  // this 指向调用 myCall函数的对象
  if (typeof this !== "function") {
    throw new TypeError("not a function")
  }
  target = target || window
  //相当于给obj对象中创建了fn属性并赋值为foo函数 === obj.fn = foo()
  target.fn = this // 隐式绑定，改变构造函数的调用者间接改变 this 指向foo,foo.myCall调用了
  let result = target.fn(...args)//obj.fn() === foo()
  return result
};
// 测试
let obj = { name: 123 }
function foo(...args) {
  console.log(this.name, args)
}
let s = foo.myCall(obj, '111', '222')
```
* apply实现
```javascript
Function.prototype.myApply = function(target,arg){
     //判断传进来的arg是不是一个数组
     let state = Object.prototype.toString.call(arg).slice(8,-1)
     //判断当前this是不是一个函数
    if (typeof this !== "function") {
        throw new TypeError("not a function")
    }
    //判断当前传入的参数是不是一个数组
    if(state !== 'Array'){
        throw new TypeError("not a Array")
    }
    target = target || window
    target.fn = this
    let result = target.fn(arg)
    return result
 }
 const obj = { name: 123 };
function foo(args) {
  console.log(this.name, args);
}
const s1 = [1, 2, 3, 4, 5];
const s = foo.myApply(obj,s1);
```
* bind实现
```

```
#### es6的知识点
* 声明和表达式：let const 解构赋值 Symbol
* 内置对象：Map和Set  proxy和reflect
* 字符串模板
* 函数：参数扩展 箭头函数 迭代器 for of
* class类
* export和import 模块
* promise async await和generator
#### js事件循环机制
js是事件驱动单线程执行,在执行任务队列的时候会依次执行,首先会执行主线程中的代码,然后执行异步任务,在执行异步任务时:

(1)存在微任务的话，那么就执行所有的微任务

(2)微任务都执行完之后，执行下一个宏任务

微任务队列追加在process.nextTick队列的后面。也属于本轮循环。所以，下面的代码总是先输出
```javascript
process.nextTick(()=>console.log(3))
promise.resolve().then(()=>console.log(4))
//输出为
//3
//4
```
* 宏任务
```javascript
setInterval() ；
setTimeout()；
setImmediate(Node独有)；
requestAnimationFrame(浏览器独有)
I/O
UI rendering(浏览器独有)
```

* 微任务
```javascript
new Promise() ；
new MutaionObserver()；
process.nextTick(Node独有)
Object.observe
```


#### ajax和axios的区别
==ajax:== 传统Ajax 指的是new一个 XMLHttpRequest（XHR）代理，最早出现的向后端发送请求的技术，隶属于原始 js 中， 核心使用 XMLHttpRequest 对象，多个请求之间如果有先后关系的话，就会出现 ==回调地狱==

==axios:== axios 是一个基于 Promise 的 http请求库，可以用在浏览器和 node.js 中，本质上也是对原生XHR的封装，只不过它是Promise 的实现版本，符合最新的ES规则。
```javascript
//请求拦截器
instance.interceptors.request.use(
  config => {
    const token = sessionStorage.getItem('token')
    if (token ) { // 判断是否存在token，如果存在的话，则每个http header都加上token
      config.headers.authorization = token  //请求头加上token
    }
    return config
  },
  err => {
    return Promise.reject(err)
  })
```
```javascript
// http response 响应拦截器
instance.interceptors.response.use(
  response => {
    //拦截响应，做统一处理 
    if (response.data.code) {
      switch (response.data.code) {
        case 1002:
          store.state.isLogin = false
          router.replace({
            path: 'login',
            query: {
              redirect: router.currentRoute.fullPath
            }
          })
      }
    }
    return response
  },
  //接口错误状态处理，也就是说无响应时的处理
  error => {
    return Promise.reject(error.response.status) // 返回接口返回的错误信息
  })
```
#### Promise优缺点,为什么使用promise
promise是为了解决多条请求相互依赖时,如果上一条请求返回的值需要在外部用到就需要一个回调函数抛出,依次就会形成回调地狱.
* 优点: 让异步操作像同步一样表现出来,避免了层层嵌套的回调函数.
* 缺点: 无法取消promise,一旦建成就会立即执行,如果不设置回调函数,promise内部返回的错误无法在外部表现出来,当处于pedding状态时,无法得知进行到了哪一步
* Promise.all方法
```javascript
//Promise.all获得的成功结果的数组里面的数据顺序和Promise.all接收到的数组顺序是一致的，即p1的结果在前，即便p1的结果获取的比p2要晚。这带来了一个绝大的好处：在前端开发请求数据的过程中，偶尔会遇到发送多个请求并根据请求顺序获取和使用数据的场景，使用Promise.all毫无疑问可以解决这个问题。
let p1 = new Promise((resolve, reject) => {
  resolve('成功了')
})

let p2 = new Promise((resolve, reject) => {
  resolve('success')
})

let p3 = Promse.reject('失败')

Promise.all([p1, p2]).then((result) => {
  console.log(result)               //['成功了', 'success']
}).catch((error) => {
  console.log(error)
})

Promise.all([p1,p3,p2]).then((result) => {
  console.log(result)
}).catch((error) => {
  console.log(error)      // 失败了，打出 '失败'
})
```
* 手写一个promise.all
```javascript
Promise.myAll = (arr) =>{
    return new Promise((rs,rj) =>{
        //数组长度
        let len = arr.length
        //计数器,当count === len 时结束
        let count = 0
        //存放返回的值
        let result = []
        if (len === 0) {
            return rs([])
        }
        arr.forEach((p,i) => {
            Promise.resolve(p).then((res) =>{
                count++
                result[i] = res
                if (count === len) {
                    rs(result)
                }
            }).catch(rj)
        });
    })
 } 
 let p1 = Promise.resolve('11')
 let p2 = Promise.reject('22')
 let p3 = Promise.resolve('33')
Promise.myAll([p1,p2,p3]).then((res) =>{
    console.log(res);
})

```
* Promise.race方法
```javascript
//Promse.race就是赛跑的意思，意思就是说，Promise.race([p1, p2, p3])里面哪个结果获得的快，就返回那个结果，不管结果本身是成功状态还是失败状态。
let p1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('success')
  },1000)
})

let p2 = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject('failed')
  }, 500)
})

Promise.race([p1, p2]).then((result) => {
  console.log(result)
}).catch((error) => {
  console.log(error)  // 打开的是 'failed'
})
```
#### v-if和v-show的区别
* v-if切换开销比较大(是通过控制dom节点的存在与否来控制元素的显隐)
* v-show 初始开销比较大(是通过设置DOM元素的display样式，block为显示，none为隐藏)
#### v-for的key的作用
key 的作用主要是 为了实现高效的更新虚拟 DOM，提高性能。确定唯一的标识,diff算法更高效
```
//如果在ABCDE中的BC之间插入F变成ABFCDE,如果没有key,即把C更新成F，D更新成C，E更新成D，最后再插入E，是不是很没有效率？ 更新了3次，之后做了一次创建插入的操作
```
#### []==false 和 ![]==false
* 第一个 [] == false 转为数字 0==0
* 第二个 ![] ==false 转为布尔 false == false
* ! 的定义：除了取反也可将变量转换成 boolean 类型。null 、 undefined 、 NaN 以及空字符串 ('') 取反都为 true ，其余都为 false 。所以 ![] 运算后的结果就是 false
* 可以思考一下显性转换和隐性转换
#### 路由的两种方式 路由守卫
* hash:比如这个 URL： http://www.abc.com/#/hello ，hash 的值为 #/hello。它的特点在于：hash 虽然出现在 URL 中，但不会被包括在 HTTP 请求中，对后端完全没有影响，因此改变 hash 不会重新加载页面。
* history:push跳转或者replace跳转是不需要请求服务器的！

 #### 数组对象去重
 ```javascript
 let log = console.log.bind(console);
let person = [
     {id: 0, name: "小明"},
     {id: 1, name: "小张"},
     {id: 2, name: "小李"},
     {id: 3, name: "小孙"},
     {id: 1, name: "小周"},
     {id: 10, name: "小周"},
     {id: 2, name: "小陈"},   
];

let obj = {};

person = person.reduce((cur,next) => {
    //
    obj[next.id] ? "" : obj[next.id] = true && cur.push(next);
    return cur;
},[]) //设置cur默认类型为数组，并且初始值为空的数组
log(person);
 ```
 #### var,let,const的区别
* let声明的变量只在他所在的代码块内有效,只要块级块级作用域内存在let命令,它所声明的变量就“绑定”（binding）这个区域，不再受外部的影响。
* let不允许在相同作用域内，重复声明同一个变量。
* var命令会发生变量提升现象,即变量可以在声明之前使用,值为undefined
* const声明一个只读的常量。一旦声明，常量的值就不能改变。不可重复声明
 ```javascript
 //第一种场景
 if (true) {
  // TDZ开始
  tmp = 'abc'; // ReferenceError
  console.log(tmp); // ReferenceError

  let tmp; // TDZ结束
  console.log(tmp); // undefined

  tmp = 123;
  console.log(tmp); // 123
}
//第二种场景(内部tmp覆盖了外部的tmp)
  var tmp = new Date();

function f() {
  console.log(tmp);
  if (false) {
    var tmp = 'hello world';
  }
}

f(); // undefined
//第三种场景(i 泄露为全局变量)
  var s = 'hello';

for (var i = 0; i < s.length; i++) {
  console.log(s[i]);
}

console.log(i); // 5
 ```
 #### class类
 类不存在变量提升,必须保证子类在父类之后.
 ```javascript
 var F = function () {}
Object.prototype.a = function () {}
Function.prototype.b = function () {}

var f = new F()
// 请问f有方法a  方法b吗
f的__proto__指向F.prototype.F.prototype.__proto__指向Object.prototype,所以f可以取到a方法,由于f的原型链没有经过Function.prototype,所以取不到b方法
 ```
 ```javascript
 function Person(){}

let p1 = new Person()
let p2 = new Person()
let obj = {}
 写出 p1  p2  Person  Function   obj   Object等的原型链。
 　p1:      __proto__ :  Person.prototype       

　　p2:      __proto__ :  Person.prototype 

　　Person  :         __proto__： Function.prototype，    prototype： Person.prototype

　　Person.prototype ：         __proto__ ： Object.prototype ，  constructor： Person

　　Function：       __proto__ ： Function.prototype，   prototype： Function.prototype

　　Function.Prototype：     __proto__ ：  Object.prototype ，   constructor：  Function

　　obj：    __proto__ ： Object.prototype

　　Object：   __proto__ ： Function.prototype  ，   prototype：  Object.prototype

　　Object.prototype：    __proto__ ：  null  ，   constructor  ：  Object
　　1. Function.__proto__    ===     Function.prototype
　　Function.prototype是引擎创造出来的对象，一开始就有了，又因为其他的构造函数都可以通过原型链找到Function.prototype，Function本身也是一个构造函数，为了不产生混乱，就将这两个联系到一起了。
　　2.Object.__proto__  === Function.prototype
    Object是对象的构造函数，那么它也是一个函数，当然它的__proto__也是指向Function.prototype
 ```
 #### js为什么是单线程
 js主要实现的是用户与浏览器之间的交互以及操作dom,这决定了它只能是单线程,如果js被设计成多线程,在这个线程中要修改一个dom,而在另一个线程又要删除这个dom,此时浏览器就会不知道如何处理.
 
 #### h5的worker
这是h5的一个api也是一个类new worker新增一个线程 但是这个线程有限制不能操作dom元素而且受主线程管理
#### Object.prototype.toString、typeof与instanceof的区别
* typeof会返回一个变量的基本类型，instanceof返回的是一个布尔值
* instanceof 可以准确地判断复杂引用数据类型，但是不能正确判断基础数据类型
* 而 typeof 也存在弊端，它虽然可以判断基础数据类型（null 除外）typeof(null)='object'
* Object.prototype.toString，调用该方法，统一返回格式“[object Xxx]” 的字符串
#### 一个浏览器是如何工作的？
1. 浏览器首先使用 HTTP 协议或者 HTTPS 协议，向服务端请求页面；
2. 把请求回来的 HTML 代码经过解析，构建成 DOM 树；
3. 计算 DOM 树上的 CSS 属性；
4. 最后根据 CSS 属性对元素逐个进行渲染，得到内存中的位图；
5. 一个可选的步骤是对位图进行合成，这会极大地增加后续绘制的速度；
6. 合成之后，再绘制到界面上。
##### 一个页面从输入 URL 到页面加载显示完成，这个过程中都发生了什么？
* DNS解析,将域名地址解析为IP地址
（什么是dns:DNS 即域名系统，所以 DNS 的查询过程，说白了，就是去向这些 DNS 服务器询问，你知道这个主机名的 IP 是多少吗，不知道？那你知道去哪台 DNS 服务器上可以查到吗？直到查到我想要的 IP 为止。）
* 和服务器建立TCP连接(TCP三次握手协议)第一次由浏览器发起,第二次由服务器发起,第三次由浏览器发起
* 发送请求 请求报文(HTTP协议的通信内容)
* 浏览器接收响应资源
* 浏览器对加载到的资源进行语法解析,生成dom树和cssom树,执行js代码
* 将dom树和css树合并生成渲染树
* 根据渲染树来计算布局,计算每个节点的几何信息(布局)
* 就是渲染节点元素,完成页面整体渲染
* 断开连接,TCP四次挥手
##### 回流和重绘
* ==回流一定会触发重绘,重绘不一定会回流==
* ==回流==: 就是节点的几何信息改变,从而导致整体的布局变化,需要重新计算称之为回流.
* ==重绘==:只是节点的颜色,背景颜色等不会造成集合信息改变的变化,只需要重新绘制该节点称之为重绘.
* ==display:none==;会发生回流.不为被隐藏的对象保留其物理空间(看不见也摸不着)
* ==visibility:hidden==;只会触发重.(会保留元素的空间,仅为视觉上的完全透明(看不见,摸的着))
##### 数组去重
```javascript
//reduce去重
let arr = [1,2,3,4,4,1] 
let newArr = arr.reduce((pre,cur)=>{ 
    if(!pre.includes(cur)){ 
        return pre.concat(cur)
    }
    return pre 
},[])
console.log(newArr)//[1,2,3,4]


//new Set()去重
let arr = new Set([1,1,2,3,4,4,5,6,7,7])


//for循环去重
let arr = [1,1,2,3,4,4,5,6,7,7]
for(let i=0;i<arr.length;i++){
   for(let j=i+1;j<arr.length;j++){
       if(arr[i] === arr[j]){
           arr.splice(j,1)
           
       }
   } 
}


//indexof去重
let arr = [1,1,2,3,4,4,5,6,7,7]
      function unlink(arr){
        let newArr = []
        for(let i of arr){
          if(newArr.indexOf(i) === -1){
            newArr.push(i)
          }
          
        }
        return newArr
      }
      
      
//includes去重
let arr = [1,1,2,3,4,4,5,6,7,7]
      function unlink(arr){
        let newArr = []
        for (let i of arr) {
          if(!newArr.includes(i) ){
            newArr.push(i)
          }         
        }
        return newArr
      }
      
      
//filter过滤
let arr = [1,1,2,3,4,4,5,6,7,7,8,9]
      arr.filter((item,index,arr) =>{
      //当前元素,在原始数组中的第一个索引值===当前索引值,否则返回当前元素
        return arr.indexOf(item,0) === index
      })
```
* 计算数组中元素出现的次数
```javascript
let arr = ["a","a","b","c","b","k","l"]
let times = arr.reduce((newArr,item) =>{
    if(item in newArr){
       newArr[item]++ 
    }else{
        newArr[item] = 1
    }
    return newArr
},{})
//{a: 2, b: 2, c: 1, k: 1, l: 1}
```
* 数组对象去重
```javascript
//通过数组对象中的某一个值进行去重
let arr = [
    {key:1,name:'第一'},
    {key:2,name:'第二'},
    {key:3,name:'第三'},
    {key:3,name:'第四'},
    {key:4,name:'第五'},
]
let obj = {}
arr.reduce((cur,item)=>{
    obj[item.key] ? "" : obj[item.key] = true && cur.push(item)
    return cur
},[])
```
#### 前端性能优化
<font color="red">gzip</font> 压缩效率非常高，通常可以达到 70% 的压缩率，也就是说，如果你的网页有 30K，压缩之后就变成了 9K 左右。
* gzip压缩
```javascript
//npm i -D compression-webpack-plugin
// 如果这个值是一个对象，则会通过 webpack-merge 合并到最终的配置中。
  // 如果这个值是一个函数，则会接收被解析的配置作为参数。该函数及可以修改配置并不返回任何东西，也可以返回一个被克隆或合并过的配置版本。Type: Object | Function
  configureWebpack: c => {
    return {
      name: '中钢富全智慧矿山平台',
      output: { // 输出重构  打包编译后的 文件名称
        filename: `js/[name].${Timestamp}.js`,
        chunkFilename: `js/[name].${Timestamp}.js`
      },
    }
  },

```
* ui按需引入
* 代码层面优化
1. v-if 和 v-show 区分使用场景
* computed 和 watch  区分使用场景
1. computed： 是计算属性，依赖其它属性值，并且 computed 的值有缓存，只有它依赖的属性值发生改变，下一次获取 computed 的值时才会重新计算 computed  的值；
2. watch： 更多的是「观察」的作用，类似于某些数据的监听回调 ，每当监听的数据变化时都会执行回调进行后续操作；
* v-for 遍历必须为 item 添加 key，且避免同时使用 v-if
* 事件的销毁
```javascript
created(){
    this.$emit("message",val)
}
beforeUnmount(){
    this.off('message')
}
```
* 图片资源懒加载
1. 对于图片过多的页面，为了加速页面加载速度，所以很多时候我们需要将页面内未出现在可视区域内的图片先不做加载， 等到滚动到可视区域后再去加载。这样对于页面加载性能上会有很大的提升，也提高了用户体验。我们在项目中使用 Vue 的 vue-lazyload 插件
* 路由懒加载：提高首屏加载速度
* Webpack 对图片进行压缩

设置limit的值,大于limit的原样输出,小于limit的图片打包成base64
#### 常见状态码
* 200 - 成功。
* 301 - 永久重定向（配合 location，浏览器自动处理）。
* 302 - 临时重定向（配合 location，浏览器自动处理）。
* 304 - 资源未被修改。
* 401 - 认证失败，无法访问系统资源
* 403 - 没权限。
* 404 - 资源未找到。
* 500 - 服务器错误。
* 504 - 网关超时。
#### 进制转换
```javascript
const num = 10;

num.toString(2);
// 输出: "1010"
num.toString(16);
// 输出: "a"
num.toString(8);
// 输出: "12"
```
#### 精度丢失问题
```javascript
/**
 * 解决两个数相加精度丢失问题
 * @param a
 * @param b
 * @returns {Number}
 */
export function floatAdd(a, b) {
    var c, d, e;
    if (undefined == a || null == a || '' == a || isNaN(a)) {
        a = 0;
    }
    if (undefined == b || null == b || '' == b || isNaN(b)) {
        b = 0;
    }
    try {
        c = a.toString().split('.')[1].length;
    } catch (f) {
        c = 0;
    }
    try {
        d = b.toString().split('.')[1].length;
    } catch (f) {
        d = 0;
    }
    e = Math.pow(10, Math.max(c, d));
    return (floatMul(a, e) + floatMul(b, e)) / e;
}
/**
 * 解决两个数相减精度丢失问题
 * @param a
 * @param b
 * @returns {Number}
 */
export function floatSub(a, b) {
    return (floatMul(a, e) - floatMul(b, e)) / e;
}
/**
 * 解决两个数相乘精度丢失问题
 * @param a
 * @param b
 * @returns {Number}
 */
export function floatMul(a, b) {
    var c = 0,
        d = a.toString(),
        e = b.toString();
    try {
        c += d.split('.')[1].length;
    } catch (f) {}
    try {
        c += e.split('.')[1].length;
    } catch (f) {}
    return (Number(d.replace('.', '')) * Number(e.replace('.', ''))) / Math.pow(10, c);
}
/**
 * 解决两个数相除精度丢失问题
 * @param a
 * @param b
 * @returns
 */
export function floatDiv(a, b) {
    var c,
        d,
        e = 0,
        f = 0;
    try {
        e = a.toString().split('.')[1].length;
    } catch (g) {}
    try {
        f = b.toString().split('.')[1].length;
    } catch (g) {}
    return (c = Number(a.toString().replace('.', ''))), (d = Number(b.toString().replace('.', ''))), floatMul(c / d, Math.pow(10, f - e));
}

/**
 * 保留有效数字并且4舍5入
 * @param {*} v 表示要转换的值 表示要保留的位数
 * @param {*} e 表示要保留的位数
 * @returns
 */
export function round(v, e) {
    var t = 1;
    for (; e > 0; t *= 10, e--);
    for (; e < 0; t /= 10, e++);
    return Math.round(v * t) / t;
}


```
#### Axios封装
```javascript
import axios from 'axios';

//遇到传递给后台参数格式为 ?ids=1&ids=2&ids=3这种键名相同形式的数据，需要用到paramsSerializer序列化
import qs from 'qs';
const Axios = axios.create({
    herader:{
          'Content-Type': 'application/json;charset=UTF-8',
        // ie浏览器get请求兼容问题
        'Cache-Control': 'no-cache',
        'If-Modified-Since': '0',
    },
    timeout:0,//设置请求超时
    withCredentials: false,//是否带上验证信息， 默认：true
     maxContentLength: -1,//限制最大发送内容长度，默认：-1 不限制
      baseURL: import.meta.env.VITE_APP_BASE_API,//vite当前运行环境中的变量 import.meta.env而在webpack中获取当前运行的环境变量则用process.argv可以获取命令行的参数，其值无法在项目文件中使用
})
export default function (options) {
     // 处理默认参数，传参和默认参数合并
    let config = Object.assign({ method: 'get' }, options || {});
     // 必须要传入url
    if (!config.url) {
        throw new Error('ajax url is required!');
    }
      let { url, method, data } = config;
    //如果截取的最后一项是page,那么默认第一页
    if (config.url.split('/').pop() === 'page') {
        data = Object.assign({ needCount: 1 }, data || {});
    }
       if (config.url.split('/').pop() === 'export') {
        config.responseType = 'blob';
    }
     const http = ['get', 'head', 'delete'].includes(method)
        ? service[method](url, {
              ...config,
              params: data,
          })
        : service[method](url, data, config);
    return http;
}
//请求拦截
service.interceptors.request.use(
    (config) => {
        // 是否需要设置 token
        const isToken = (config.headers || {}).isToken === false,
            { token } = user();
        if (token && !isToken) {
            config.headers['Authorization'] = 'Bearer ' + token; // 让每个请求携带自定义token 请根据实际情况自行修改
        }
        /**
         * 请求未完成时保存取消的cancelToken
         */
        config.cancelToken = new axios.CancelToken((cancel) => {
            // 判断是否重复
            const key = createKey(config);
            if (axiosPromiseArr.has(key)) {
                cancel();
            } else {
                axiosPromiseArr.set(key, cancel);
            }
        });
        if (config.method === 'get') {
            config.paramsSerializer = function (params) {
                return qs.stringify(params, { arrayFormat: 'comma' });
            };
        }
        return config;
    },
    (error) => {
        ElMessage.error({
            message: '加载超时',
        });
        return Promise.reject(error);
    }
);
```
#### axios 之cancelToken原理以及使用
在真实项目中，当路由已经跳转，而上一页的请求还在pending状态，如果数据量小还好，数据量大时，跳到新页面，旧的请求依旧没有停止，这将会十分损耗性能，这时我们应该先取消掉之前还没有获得相应的请求，再跳转页面。这里axios给我们提供了一个方法：
```

```
#### 数组扁平化
```javascript
 const arr = [1, [2, [3, [4, 5]]], 6]
 //方法1 arr.flat(Infinity)
  //方法2 arr.toString().split(',').map(item => Number(item))
```
#### 扁平数组Tree化
```javascript
let arr = [
    {id: 1, name: '部门1', pid: 0},
    {id: 2, name: '部门2', pid: 0},
    {id: 3, name: '部门3', pid: 0},
    {id: 4, name: '部门4', pid: 2},
    {id: 5, name: '部门5', pid: 2},
]
function nest(pid,arr){
    return arr.filter(item =>item.pid === pid).map(item=>({...item,children:nest(item.id,arr)}))
}
nest(0,arr)
```
#### vue3实现v-model实现
```javascript
<!--vue3中就实现了这个功能，v-model绑定的不再是value，而是modelValue，接收的方法也不再是input，而是update:modelValue-->
<!--父组件-->
<wm-tinymce
  ref="editor"
  v-model="data.introduction"
  :min-height="400"
  name="职能管理"
  placeholder="请编辑职能大类"
/>

<!--子组件-->
<div class="tinymce-container">
    <editor
      v-model="tinymceData"
      :api-key="key"
      :init="tinymceOptions"
      :name="name"
      :readonly="tinymceReadOnly"
    />
</div>
<script>
  import { ref, watch, watchEffect } from 'vue'
  import Editor from '@tinymce/tinymce-vue'
  import { key, plugins, toolbar, setting } from './config'
  export default {
    name: 'WmTinymce',
    components: { Editor },
    props: {
      modelValue: {
        type: String,
        default: '',
      },
      readOnly: {
        type: Boolean,
        default: false,
      },
      options: {
        type: Object,
        default() {
          return { plugins, toolbar }
        },
      },
      name: {
        type: String,
        default: '',
      },
    },
    emits: ['update:modelValue'],
    setup(props, { emit }) {
      const tinymceData = ref(props.modelValue) // 编辑器数据
      watch(
        () => tinymceData.value,
        (data) => emit('update:modelValue', data)
      ) // 监听富文本输入值变动
      return {
        tinymceData,
      }
    },
  }
</script>

```
#### 节流函数和防抖函数

#### requestAnimationFrame 

requestAnimationFrame比起setTimeout、setInterval的优势主要有两点:
1、requestAnimationFrame会把每一帧中的所有DOM操作集中起来,在一次重绘或回流中就完成,并且重绘或回流的时间间隔紧紧跟随浏览器的刷新频率,一般来说,这个频率为每秒60帧.
2、在隐藏或不可见的元素中,requestAnimationFrame将不会进行重绘或回流，这当然就意味着更少的的cpu，gpu和内存使用量。

