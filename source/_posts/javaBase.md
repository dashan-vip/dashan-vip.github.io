---
title: Java基础
date: 2022-09-07 09:18:04
tags: 基本数据类型 
categories: 
- Java
cover: https://w.wallhaven.cc/full/wq/wallhaven-wqpmdr.jpg
---
#### jdk环境变量配置
在电脑右键->属性->高级系统设置->环境变量->在系统变量中新建 变量名:JAVA_HOME 变量值:C:\Program File\Java\jdk1.8.0_60
#### 注释
* 单行注释: //
* 多行注释: /**/
* 文档注释: /*@autho @version~~~*/

1. `在java源文件中可以声明多个class,但是只能最多有一个类声明为public`

2. `要求声明为public的类类名必须与源文件名相同`
3. `程序入口是main()方法,格式是固定的`
4. `输出语句:System.out.println("Hello Java !");先输出数据然后换行
		System.out.print("hello")只输出数据`
5. 每一行执行语句都以";"结束
6. `编译的过程: 编译以后,会生成一个或多个字节码文件,字节码文件名与java源文件中的类名相同`
```JavaScript
public class Helloworld {
	public static void main(String[] args){//arguments:参数
		System.out.println("Hello Java !");//先输出数据然后换行
		System.out.print("hello");//只输出数据
	}
}
```
##### 1.JDK,JRE,JVM三者之间的关系,以及JDK,JRE包含的主要结构有哪些
JDK = JRE + Java的开发工具(javac.exe,java.exe,javadoc.exe)
JRE = JVM + java核心类库
##### 2.为什么要配置环境变量?如何配置
JAVA_HOME = bin的上层目录

path= %JAVA_HOME%\bin

配置环境变量是为了在任何文件夹下运行java
##### 3.常见的几个命令行操作都有哪些?
dir //列出当前目录下的文件以及文件夹

cd //到根目录

md //创建文件夹

rd //删除文件夹

del //删除文件

#### 变量的分类
对每一种数据都定义了明确的具体的数据类型(强类型语言),在内存中分配了不同大小的内存空间
|  基本数据类型(primitive type)   | 引用数据类型(reference type)  |
|  ----  | ----  |
| 数值型 (整数类型(byte,short,int,long)、浮点类型(float,double))  | 类(class) |
| 字符型 (char)  | 接口 (interface) | 
| 布尔型 (boolean)  | 数组 ([]) | 
#### 整数类型表示范围
* java各整数类型有固定的表数范围和字段长度,不受具体OS的影响,以保证java程序的可移植性.
* java的整型常量默认为int型,声明long型常量需后加"l"或"L"
* java程序中变量通常声明为int型,除非不足以表示较大的数,才使用long

| 类型 | 占用储存空间 | 表数范围 |
| -----| ---- | ---- |
| byte | 1字节=8bit | -128~127 |
| short | 2字节 | -2^15^~2^15^-1 |
| int | 4字节 | -2^31^~2^31^-1(约21亿) |
| long | 8字节 | -2^63^~2^63^-1 |

* 1MB = 1024KB 1KB = 1024B
* bit: 计算机中的最小储存单位
* byte: 计算机中基本储存单元
#### 浮点类型表示范围
* float:单精度,尾数可以精确到7位有效数字
* double: 双精度,精度是float的两倍,通常采用此类型
* java的浮点类型常量默认为double型,声明float型常量,需后加'f'或'F'

| 类型 | 占用储存空间 | 表数范围 |
| -----| ---- | ---- |
| 单精度 float | 4字节 | -3.403E387~3.403E38 |
| 双精度 (double) | 8字节 | -1.798E308~1.798E308 |
#### 字符型表示范围
* 定义char型变量,通常使用一对'',内部只能写一个字符

| 类型 | 占用储存空间 | 表数范围 |
| -----| ---- | ---- |
| 字符型 char | 2字节 |  |
```javascript
char a = '\n'//换行符
char b = '\t'//制表符
char c = '\r'//回车符
char d = '\"'//双引号
char e = '\''//单引号
char f = '\\'//反斜线
```