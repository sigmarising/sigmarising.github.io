---
title: "关于 Python 中的 global/nonlocal 关键字"
date: 2019-06-03T10:55:21+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["Python", "CSDN备份"]
categories: ["Python"]
series: ["tech"]
slug: "20190603-tech-python-global-nonlocal"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

## 1. Python 变量作用域

Python 的变量作用域可以分下面四种：
* Local：局部作用域
* Enclosing：闭包函数外的函数作用域
* Global：全局作用域
* Built-in：内建作用域

查找变量会逐级别查找（L、E、G、B）。这里值得注意的是，Python **不**具备**块级作用域**，而是一种类似于**函数作用域**的形式。

## 2. Python 变量的定义

Python 无需声明便直接定义变量，但是如下例：
```python
a = 1
b = 99

def print_two(x, y):
    print(x)
    print(y)

def function_test():
    a = 2
    print_two(a, b)

function_test()
print_two(a, b)
```
运行时输出为：
```shell
2
99
1
99
```
可知，在**内部**作用域中，**可以直接引用**全局作用域中变量的值，但是**无法直接进行赋值修改**操作。

## 3. global 关键字

如例子所示：

```python
a = 1
b = 99

def print_two(x, y):
    print(x)
    print(y)

def function_test():
    global a
    a = 2
    print_two(a, b)

function_test()
print_two(a, b)
```

运行结果：

```shell
2
99
2
99
```

**global 关键字的作用是，在局部作用域中，使用全局变量。若不涉及全局变量的修改（只是单纯引用），也可以不使用 global 关键字。**

## 4. nonlocal 关键字

示例代码：
```python
a = 1

def test():
    a = 2
    def test1():
        a = 3
        def test2():
            nonlocal a
            a = 4
            print(a)
        test2()
        print(a)
    test1()
    print(a)

test()
print(a)
```
运行结果：
```shell
4
4
2
1
```

**即 nonlocal 关键字，用来在局部作用域中使用外层（非全局）变量。**

## 5. 关于全局变量

在 Python 中，另一种使用全局变量的方式，就是使用单独的 `global.py` 文件，存储所有的全局变量，可以进行引用和修改。

## 参考链接

* [初识 Python： global 关键字](https://linux.cn/article-9561-1.html)
* [python变量总结: 全局变量、局部变量、类变量、实例变量以及global和nonlocal关键字的使用示例](https://blog.csdn.net/helloxiaozhe/article/details/79139256)
* [python基础 - global关键字及全局变量的用法](https://blog.csdn.net/diaoxuesong/article/details/42552943)
* [python语法32\[global与nonlocal比较\]](https://www.cnblogs.com/itech/archive/2011/12/31/2308640.html)

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
