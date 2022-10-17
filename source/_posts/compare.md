---
title: webpack vite 对比
date: 2022-10-12 13:42:59
tags: 知识点
categories:
cover: https://w.wallhaven.cc/full/qz/wallhaven-qzd9dl.png
---

### webpack vite 对比

#### 二者用到的底层语言不通

`webpack`是基于`nodejs`构建的,js 是以毫秒计数的。
`vite`是基于`esbulid`预构建依赖，`esbulid`是采用`go`语言编写的，`go`语言是纳秒级别的。
总结：因为`js`是毫秒级别，`go`语言是纳秒级别。所以`vite`比`webpack`打包器快`10-100`倍。

#### 打包过程

`webpack`:分析各个模块间的依赖->然后进行编译打包->打包后的代码在本地服务器渲染。随着模块增多,打包的体积变大,造成热更新速度变慢。
`vite`:启动服务器->请求模块时按需动态编译显示。(`vite` 遵循的是 `ES Modules` 模块规范(在编译时就确定模块依赖关系即输入输出)来执行代码,不需要打包编译成 `es5` 模块即可在浏览器运行。)
总结:`vite` 启动时候不需要分析各个模块之间的依赖关系、不需要打包编译。`vite`可按需动态编译缩短时间。当项目复杂、模块越多的情况下,`vite`明显优于`webpack`。

#### 热更新

`webpack` :模块以及模块依赖的模块需重新编译
`vite` :浏览器重新请求改模块即可
总结:`vite`热更新效率高。
