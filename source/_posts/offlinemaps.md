---
title: offlinemaps
date: 2022-10-12 11:04:52
tags:
  - 地图
categories: config
cover: https://w.wallhaven.cc/full/p9/wallhaven-p93rpe.jpg
---

### vue3.0 中使用百度离线地图

#### 百度离线地图步骤

瓦片图下载器地址：下面会用到瓦片图和一些离线资源。下载链接: <a href="https://pan.baidu.com/s/1t6o9sig1H5KqxyllNryN0Q">`https://pan.baidu.com/s/1t6o9sig1H5KqxyllNryN0Q`</a> 提取码: `e7sn`。

从网盘下载资源解压后会有两个文件夹

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3dcfd9c26ef94dc1b73f303a2dc2d464~tplv-k3u1fbpfcp-watermark.image?)
项目文件目录如下

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5dc79420e74246b9aa5091581dbfd5c9~tplv-k3u1fbpfcp-watermark.image?)
在 public 下创建 static 文件夹,将网盘中`bmap_offline_demo`文件夹下的这些文件放到创建的 static 中

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/81bd5f11b810400598a1f8a2a4804272~tplv-k3u1fbpfcp-watermark.image?)
打开`map_load.js`进行一些相关的配置将下载的的瓦片图放到 static 下面,或者通过 nginx 代理

`注意:下载的瓦片图文件夹命名要与配置路径中的名称一致。瓦片图有些后缀是.jpg有些后缀是png,要统一转为.png或者.jpg`

图片后缀批量处理方法<a href="https://pan.baidu.com/s/1t6o9sig1H5KqxyllNryN0Q">`https://pan.baidu.com/s/1t6o9sig1H5KqxyllNryN0Q`</a>

```JavaScript
var bmapcfg = {
    imgext: '.png', //瓦片图的后缀 ------ 根据需要修改，一般是 .png .jpg
    tiles_dir: `/static/tiles`, //普通瓦片图的地址，为空默认在 offlinemap/tiles/ 目录
    tiles_hybrid: `/static/tiles_hybrid`, //卫星瓦片图的地址，为空默认在 offlinemap/tiles_hybrid/ 目录
    tiles_self: '', //自定义图层的地址，为空默认在 offlinemap/tiles_self/ 目录
};

//////////////////下面的保持不动///////////////////////////////////
var scripts = document.getElementsByTagName('script');
var JS__FILE__ = scripts[scripts.length - 1].getAttribute('src'); //获得当前js文件路径
bmapcfg.home = JS__FILE__.substr(0, JS__FILE__.lastIndexOf('/') + 1); //地图API主目录
(function () {
    window.BMap_loadScriptTime = new Date().getTime();
    //加载地图API主文件
    document.write('<script type="text/javascript" src="' + bmapcfg.home + 'bmap_offline_api_v3.0_min.js"></script>');
    //加载扩展函数
    document.write('<script type="text/javascript" src="' + bmapcfg.home + 'map_plus.js"></script>');
    //加载城市坐标
    document.write('<script type="text/javascript" src="' + bmapcfg.home + 'map_city.js"></script>');
})();
///////////////////////////////////////////////////////////////////

```

配置完成后将`map_load.js`引入到`index.html`中,vite 项目中引入如下:

1. 引入 vite-plugin-html 插件,配置插件

```JavaScript
//在vite中引入map_load.js
import { defineConfig, loadEnv } from 'vite';
import vue from '@vitejs/plugin-vue';
// html内容调整 插件
import { createHtmlPlugin } from 'vite-plugin-html';
export default ({ command, mode }) => {
    const plugins = [
        vue(),
           /**
         *  注入环境变量到html模板中
         *  如在  .env文件中有环境变量  VITE_APP_TITLE=admin
         *  则在 html模板中  可以这样获取  <%- VITE_APP_TITLE %>
         *  文档：  https://github.com/anncwb/vite-plugin-html
         */
        createHtmlPlugin({
            inject: {
                data: {
                    injectMapScript: `<script src="/static/map_load.js"></script>`,
                },
            },
            minify: true,
        }),

    ]
}
```

2. 在 index.html 中引入

```html
<head>
  <meta charset="utf-8" />
  <%- injectMapScript %>
</head>
```

然后就可以在项目中使用了

```JavaScript
<template>
    <div id="Map" class="w100 h100" />
</template>
<script setup >
    function initFn() {
        map = new BMap.Map('Map', { minZoom: 13, maxZoom: 19 });//1. 创建地图实例时，通过opts方式设置地图允许的最大最小级别
        map.centerAndZoom(new BMap.Point(120.06, 29.3), 14); // 创建中心点坐标 初始化地图，设置中心点坐标和地图级别
        map.setCurrentCity('金华'); // 设置地图显示的城市 此项是必须设置的
        map.enableScrollWheelZoom(); //开启鼠标滚轮缩放
        map.setMapType(BMAP_HYBRID_MAP); //设置默认显示卫星混合图
    }
    // 地图初始化
    onMounted(() => {
        initFn();
    });
</script>

```
