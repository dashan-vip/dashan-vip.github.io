---
title: display grid 属性
date: 2022-10-13 15:08:43
tags: 知识点
categories: Css
cover: https://w.wallhaven.cc/full/l8/wallhaven-l8q9pr.jpg
---

#### grid 属性

`Grid` 是二维网格系统。它可以用来构建复杂的布局以及较小的界面。

##### 属性:`grid-template-columns`

定义列，按照你希望它们在网格中出现的顺序，把 grid -template-columns 属性设置为列大小

```css
.grid {
  display: grid;
  grid-template-columns: 100px 100px 100px; // 定义三个宽度为100px的单元格
}
```

##### 属性: grid-template-rows

grid-template-rows 用于定义网格中行的数量和大小。

```css
.grid {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px 100px; // 高度 与‘columns’类似
}
```

##### 属性: grid-template

grid 是 grid-template-rows、grid-template-clumns 和 grid-template-areas 三个属性的简写。

```css
.grid {
  display: grid;
  grid-template:
    "header header  header" 80px
    "nav    article article" 600px
    / 100px 1fr;
}
```

##### 属性: fr

fr 是为 css 网格布局创建的新单位。 fr 使你不需要计算百分比就能创建灵活的网格， 1fr 表示可用空间的一等份。

```css
.grid {
  display: grid;
  grid-template-columns: 3fr 4fr 3fr; // 宽度分为3+4+3=10个等份，4个单元格分别占3、4、3份
  grid-template-columns: 3fr 4fr 3fr 2fr; // 宽度分为3+4+3+2=12个等份，4个单元格分别占3、4、3、2份
}
```

##### 属性: grid-auto-flow

grid-auto-flow 属性控制 网格单元 如何流入网格，其默认值为 row。网格中的“网格单元”将会被一一填充，直到没有剩余的项目为止。

```css
.grid {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-auto-flow: column;
}
```

##### 函数：repeat()

有时候，重复写同样的值非常麻烦，尤其网格很多时。这时，可以使用 repeat()函数，简化重复的值。

repeat() 函数有助于使 网格轨道 列表变得不是那么多余，并为其添加了语义层。

repeat()接受两个参数，第一个参数是重复的次数（上例是 3），第二个参数是所要重复的值。

```css
.container {
  display: grid;
  grid-template-columns: repeat(3, 33.33%);
  grid-template-rows: repeat(3, 33.33%);
}
.grid {
  display: grid;
  grid-template-columns: repeat(
    3,
    1fr 2fr
  ); // 重复3次的 1fr 2fr。 还可以是：grid-template-columns:2fr repeat(5，1fr) 4fr;
}
```

##### auto-fill 关键字

有时，单元格的大小是固定的，但是容器的大小不确定。如果希望每一行（或每一列）容纳尽可能多的单元格，这时可以使用`auto-fill`关键字表示自动填充。

```css
.container {
  display: grid;
  grid-template-columns: repeat(auto-fill, 100px);
}
//上面代码表示每列宽度100px，然后自动填充，直到容器不能放置更多的列。
```

##### minmax()

minmax()函数产生一个长度范围，表示长度就在这个范围之中。它接受两个参数，分别为最小值和最大值。

```css
grid-template-columns: 1fr 1fr minmax(100px, 1fr);
//上面代码中，minmax(100px, 1fr)表示列宽不小于100px，不大于1fr。
```

##### grid-row-gap 属性，grid-column-gap 属性，grid-gap 属性

`grid-row-gap`属性设置行与行的间隔（行间距），`grid-column-gap`属性设置列与列的间隔（列间距）。

```css
.container {
  grid-row-gap: 20px;
  grid-column-gap: 20px;
}
```

根据最新标准，上面三个属性名的`grid-`前缀已经删除，`grid-column-gap`和`grid-row-gap`写成`column-gap`和`row-gap`，`grid-gap`写成`gap`。

##### grid-template-areas 属性

网格布局允许指定"区域"（area），一个区域由单个或多个单元格组成。`grid-template-areas`属性用于定义区域。

##### grid-auto-flow 属性

划分网格以后，容器的子元素会按照顺序，自动放置在每一个网格。默认的放置顺序是"先行后列"，即先填满第一行，再开始放入第二行，即下图数字的顺序。

这个顺序由`grid-auto-flow`属性决定，默认值是`row`，即"先行后列"。也可以将它设成`column`，变成"先列后行"。

##### place-items 属性

`place-items`属性是`align-items`属性和`justify-items`属性的合并简写形式。

##### grid-column-start 属性，grid-column-end 属性，grid-row-start 属性，grid-row-end 属性

- grid-column-start 属性：左边框所在的垂直网格线
- grid-column-end 属性：右边框所在的垂直网格线
- grid-row-start 属性：上边框所在的水平网格线
- grid-row-end 属性：下边框所在的水平网格线
  简写

```css
.item {
  grid-column: <start-line> / <end-line>;
  grid-row: <start-line> / <end-line>;
}
.item-1 {
  grid-column: 1 / 3;
  grid-row: 1 / 2;
}
/* 等同于 */
.item-1 {
  grid-column-start: 1;
  grid-column-end: 3;
  grid-row-start: 1;
  grid-row-end: 2;
}
```

```css
.item-1 {
  grid-column-start: 2;
  grid-column-end: 4;
}
.item-1 {
  grid-column-end: span 2;
}
//这四个属性的值还可以使用span关键字，表示"跨越"，即左右边框（上下边框）之间跨越多少个网格。
```

关于 Grid(网格) 布局,就看大神阮一峰的<a href="https://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html">Grid 网格布局教程</a>
