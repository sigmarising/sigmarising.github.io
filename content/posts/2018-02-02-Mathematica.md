---
title: "Mathematica Learning - 基础入门"
date: 2018-02-02T20:39:22+08:00
subtitle: "Mathematica 基础入门用法"  # can be deleted
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["数学", "CSDN备份"]
categories: ["数学"]
series: ["tech"]
slug: "20180202-mathematica"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

本文主要介绍一些Mathematica基础的入门用法，包括一些常用的函数和使用方法。

## 基本操作

### 常用快捷键

* 对于一个表达式**运行结果**，使用快捷键`Shift + Enter`。

* 对于一个函数**显示帮助**，快捷键是`F1`。

### 函数定义

一般的函数（带名）定义方法如下：

```c
 In[1]:= f[x_] := x + 1;
 In[2]:= f[2]
Out[2]:= 3

 In[1]:= g[x_, y_] := x + y;
 In[2]:= g[4, 4]
Out[2]:= 8
```

纯函数（匿名）定义方法为：

```c
(*例：对10求相反数*)
 In[1]:= (#*-1)&[10]
Out[1]:= -10
```

### 函数调用

* `a//f`$\Leftrightarrow$`f[a]`
* `f@a`$\Leftrightarrow$`f[a]`
* `f@@{a, b, c}`$\Leftrightarrow$`f[a, b, c]`

## 基本存储单元 List - 列表

**列表（List）**，是Mathematica的基本存储单元，就像矩阵（Mat）是Matlab的基本存储单元一样。

**创建列表**的方法很简单，例如：

```c
{1, 2, 3}					(*创建普通列表*)
{{1, 2}, {3, 4}}			(*矩阵，列表的列表*)
{{1,2,3}, 2, {1, {2}, 3}}	(*不规则列表*)
```

**Reverse方法：**

对列表求逆

```c
 In[1]:= 	Reverse[{1,2,3}]
Out[1]:=	{3, 2, 1}
```

## 三大常用函数

### 函数 - Range[]

`Range[]`常用于产生，一串步长一致的序列值：

```c
Range[imax]
	(* 生成列表{1, 2, ... ,imax} *)
	
Range[imin, imax]
	(* 生成列表，从imin到imax *)

Range[imin, imax, step]	
	(* 生成列表，从imin到imax，步长为step *)
```

### 函数 - Table[]

`Table[]`用于对某表达式中某个变量代入数值，产生一串序列：

```c
Table[expr, {i, imax}]
Table[expr, {i, imin, imax}]
Table[expr, {i, imin, imax, step}]

	(* 根据i的范围值，产生对应expr值的序列 *)
```

```c
(*例*)
 In[1]:= Table[x^2 + 1, {x, 0, 9, 3}]
Out[1]:= {1, 10, 37, 82}
```

### 函数 - Select

`Select[]`用于选择序列中某些符合要求的值，并生成新的序列：

```c
Select[list, crit]
	(* 选取list中使得crit[ei]为True的所有元素ei *)
	
Select[list, crit, n]
	(* 选取list中使得crit[ei]为True的前n个元素ei *)
```

```c
(*例*)
 In[1]:= Select[{1,2,3,4,5,6,7}, #>2&, 2]
Out[1]:= {3, 4}

 In[2]:= Select[{1,2,3,4,5,6,7}, EvenQ, 2]
Out[2]:= {2, 4}
```

#### 判断函数

Mathematica有很多内置的判断函数，都是以Q结尾的，例如:

* `EvenQ[]` - 判断偶数
* `OddQ[]` - 判断奇数

## 其他操作

### 函数 - With

替换变量

```c
With[{x=a, y=b}, expr]
	(* 将expr中的x、y使用a、b替换 *)
```

### 函数 - Flatten

将多维列表压为一维

```c
 In[1]:= Flatten[{{1, 2, 3}, 4, 5, {6, {7}}}]
Out[1]:= {1, 2, 3, 4, 5, 6, 7}
```

### 函数 - DeleteDuplicates

删除列表中的重复元素

```c
 In[1]:= DeleteDuplicates[{1,1,2}]
Out[1]:= {1,2}
```

### 函数 - Total

对列表中所有元素求和

```c
 In[1]:= Total[{1, 2}]
Out[1]:= 3
```

### 函数 - FactorInteger

用于分解质因数

```c
 In[1]:= FactorInteger[100]
Out[1]:= {{2, 2}, {5, 2}}
	(*  100 = 2*2*5*5  *)
```

### 函数 - Outer

外积

```c
 In[1]:= Outer[f, {a, b}, {x, y, z}]
Out[1]:= {{f[a, x], f[a, y], f[a, z]}, {f[b, x], f[b, y], f[b, z]}}

(*向量的外积*)
 In[2]:= Outer[Times, {1, 2, 3, 4}, {a, b, c}]
Out[2]:= {{a, b, c}, {2 a, 2 b, 2 c}, {3 a, 3 b, 3 c}, {4 a, 4 b, 4 c}}

(*矩阵的外积*)
 In[3]:= Outer[Times, {{1, 2}, {3, 4}}, {{a, b}, {c, d}}]
Out[3]:= {{{{a, b}, {c, d}}, {{2 a, 2 b}, {2 c, 2 d}}}, {{{3 a, 3 b}, {3 c, 3 d}}, {{4 a, 4 b}, {4 c, 4 d}}}}
```

---

## 官方文档

Mathematica官方文档极为详尽，应当养成随查的习惯，附链接如下:

> [Wolfram 语言与系统：参考资料中心](http://reference.wolfram.com/language/)
> 
> [Wolfram 语言：快速编程入门](http://www.wolfram.com/language/fast-introduction-for-programmers/zh/)

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
