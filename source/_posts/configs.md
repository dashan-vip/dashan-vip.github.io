---
title: _config.yml配置
date: 2022-09-01 14:57:16
tags: 
- 配置
categories: config
cover: https://w.wallhaven.cc/full/0q/wallhaven-0q2mq0.jpg
---

### Hexo 官方文档
#### 中文地址: https://hexo.io/zh-cn/docs/configuration.html

### 网站
|  参数    |  描述  |
|  ----   |  ---- |
| title:  | 网站标题 |
| subtitle  | 网站副标题 |
| description  | 网站描述 |
| keywords  | 网站的关键词。支持多个关键词。 |
| author  | 您的名字 |


### 主题: https://hexo.io/themes/
##### 注意: theme: "name" 这个name必须与安装主题的文件夹(themes/flex-block)同名
```javascript
theme: flex-block
```



### 部署上线配置
1. 首先下载插件 `hexo-deployer-git`
```javascript
npm i hexo-deployer-git -S
```
2. 然后在_config.yml中找到deploy进行配置
```javascript
deploy:
  type: git //git 
  repo: https://github.com/dashan-vip/dashan-vip.github.io.git //github地址
  branch: master //部署分支
```
3. 登录github创建仓库,` 仓库名必须是github登录名加.github.io.git `比如你的登录名是jerry,那么你的仓库名就应该是` jerry.github.io.git `
4. 在项目终端运行,看是否有报错
```javascript
hexo s
```
5. 如果项目运行正常无报错,然后进行下一步生成静态文件。
```javascript
hexo generate
//简写
hexo g
```
6. 最后文件生成后立即部署网站
```javascript
hexo deploy
//简写
hexo d
```
随即我们自己的博客就成功部署的github上啦!

