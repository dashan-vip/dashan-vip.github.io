---
title: 问题总结
date: 2022-08-31 16:00:37
tags:
- 问题
categories: questions 
cover: https://w.wallhaven.cc/full/ne/wallhaven-ne78w4.jpg
---
#### 1. el-popover挤压页面问题
在使用element-plus中的el-popover弹出框组件的时候,如果这个弹框在浏览器显示边缘会造成页面挤压变形,解决方法是添加`teleported="false"`(是否将 popover 的下拉列表插入至 body 元素)



#### 2. Chrome浏览器不能打断点问题解决
在设置中取消选中 `Enable Javascript source maps`即可 

#### 3.文件夹下图片或文件后缀名批量操作
```
// 将所有.png后缀的图片改为.jpg
for /f "delims=" %%a in ('dir /a-d /s /b') do ( if "%%~xa" == ".png" ren "%%~fsa" "%%~na.jpg")
```
#### 4.画一个平行四边形

```html
<div class="link w100">
    <div class="fonts"></div>
</div>
<style>
.link{
    margin-bottom: 20px;
    text-align: center;
    border: 1px solid #125cb4;
    width: 20%;
    height: 34px;
    line-height: 34px;
    cursor: pointer;
    transform: skew(-20deg);
    &:hover{
        color:blue;
    }
    .fonts {
        font-size: 14px;
        font-weight: bold;
        transform: skew(20deg);
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
    }
}
</style>
```