---
title: "Mathematica Learning - 常用Plot函数"
date: 2018-02-06T18:01:38+08:00
subtitle: "Mathematica 常用Plot函数"  # can be deleted
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["数学", "CSDN备份"]
categories: ["数学"]
series: ["tech"]
slug: "20180206-mathematica-plot"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

本文介绍MMA中一些常用的Plot函数。

## 数据可视化

可用于点、线、曲面、图、网络等绘图。

> [Wolfram 语言与系统 参考资料中心 - 数据可视化](http://reference.wolfram.com/language/guide/DataVisualization.html)

### ListPlot - 对列表绘制点

```c
ListPlot[{y1, y2}]
	(* 绘制点{1, y1}, {2, y2} *)
	
ListPlot[{ {x1, y1}, {x2, y2} }]
	(* 绘制点{x1, y1}, {x2, y2}} *)

ListPlot[{data1, data2}]
	(* 从所有datai中绘制数据 *)
```

> [ListPlot官方参考](http://reference.wolfram.com/language/ref/ListPlot.html)

### ListLinePlot - 对列表绘制线

```c
ListLinePlot[{y1, y2, y3}]
	(* 绘制穿过点{1,y1},{2,y2},{3,y3}的线 *)
	
ListLinePlot[{{x1,y1},{x2,y2}}]
	(* 绘制经过由指定的x和y确定的位置的曲线 *)

ListLinePlot[{data1, data2, data3}]
	(* 从所有datai中绘制数据 *)
```

**散点拟合：**

```c
ListLinePlot[{{1, 4, 2, 3, 10, 8, 2}, {2, 4, 5, 7, 9, 22}}, 
 InterpolationOrder -> 2, Mesh -> Full]
	 (* 插值至少为2，但是不宜过大 *)
```

> [ListLinePlot官方参考](http://reference.wolfram.com/language/ref/ListLinePlot.html)

### ListPlot3D - 对列表绘制面

```c
ListPlot3D[array]
	(* 产生一个表示高度值数组的三维曲面图 *)
	
ListPlot3D[{{x1,y1,z1},{x1,y1,z1}}]
	(* 产生一个曲面，在{xi,yi}的高度为zi *)
	
ListPlot3D[{data1,data2,data3}]
	(* 绘制对应每个datai的表面 *)
```

> [ListPlot3D官方参考](http://reference.wolfram.com/language/ref/ListPlot3D.html)

### ListPointPlot3D - 对列表绘制三维散点图

```c
ListPointPlot3D[{{x1,y1,z1},{x1,y1,z1}}]
	(* 对坐标{xi,yi,zi}的点产生一个三维散点图 *)
	
ListPointPlot3D[array]
	(* 用一个二维数组的高度值生成一个三维散点图 *)

ListPointPlot3D[{data1,data2,data3}]
	(* 绘制几个点集的曲线图，默认用不同颜色 *)
```

> [ListPointPlot3D官方参考](http://reference.wolfram.com/language/ref/ListPointPlot3D.html)

## 函数可视化

对函数进行二维或者三维可视化。

> [Wolfram 语言与系统 参考资料中心 - 函数可视化](http://reference.wolfram.com/language/guide/FunctionVisualization.html)

### Plot - 绘制函数二维图像

```c
Plot[f, {x, xmin, xmax}]
	(* 绘制f的图像，自变量x范围是xmin到xmax *)
	
Plot[{f1, f2}, {x, xmin, xmax}]
	(* 绘制多个函数fi *)
```

> [Plot官方参考](http://reference.wolfram.com/language/ref/Plot.html)

### Plot3D - 绘制函数三维图像

```c
Plot3D[f, {x, xmin, xmax}, {y, ymin, ymax}]
	(* 产生函数f在x和y上的三维图形 *)

Plot3D[{f1, f2}, {x, xmin, xmax}, {y, ymin, ymax}]
	(* 绘制多个函数 *)
```

> [Plot3D官方参考](http://reference.wolfram.com/language/ref/Plot3D.html)

### RegionPlot - 不等式二维区域

```c
RegionPlot[pred, {x, xmin, xmax}, {y, ymin, ymax}]
	(* 画图显示pred为True的区域 *)
```

> [RegionPlot官方参考](http://reference.wolfram.com/language/ref/RegionPlot.html)

### RegionPlot3D - 不等式三维区域

```c
RegionPlot3D[pred, {x, xmin, xmax}, {y, ymin, ymax}, {z, zmin, zmax}]
	(* 画图显示pred为True的区域 *)
```

> [RegionPlot3D官方参考](http://reference.wolfram.com/language/ref/RegionPlot3D.html)

### DiscretePlot - 绘制二维离散函数

```c
DiscretePlot[expr, {n, nmax}]
	(* 产生expr值的图形，n从1变化到nmax *)

DiscretePlot[expr, {n, nmin, nmax}]
	(* 产生expr值的图形，n从nmin变化到nmax *)

DiscretePlot[expr, {n, nmin, nmax, dn}]
	(* 使用步长dn *)

DiscretePlot[expr, {n, {n1, n2, n3}}]
	(* 使用连续值ni *)
	
DiscretePlot[{expr1, expr2}, ...]
	(* 绘制所有expri的值 *)
```

> [DiscretePlot官方参考](http://reference.wolfram.com/language/ref/DiscretePlot.html)

### DiscretePlot3D - 绘制三维离散函数

```c
DiscretePlot3D[expr, {i, imin, imax}, {j, jmin, jmax}]
	(* i从imin到imax，j从jmin到jmax *)

DiscretePlot3D[expr, {i, imin, imax, di}, {j, jmin, jmax, dj}]
	(* 使用步长di和dj *)
	
DiscretePlot3D[expr, {i, {i1,i2,i3}}, {j, {j1, j2, j3}}]
	(* i、j分别使用连续值 *)
```

> [DiscretePlot3D官方参考](http://reference.wolfram.com/language/ref/DiscretePlot3D.html)

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
