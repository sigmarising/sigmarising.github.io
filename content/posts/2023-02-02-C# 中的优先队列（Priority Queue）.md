---
title: "C# 中的优先队列（Priority Queue）"
date: 2023-02-02T20:25:08+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["开发", "CSDN备份"]
categories: ["开发"]
series: ["tech"]
slug: "20230202-tech-c-sharp-pq"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

在刷 LeetCode 等题库的时候，我们经常遇到使用堆（Heap）的情况，在 C++ 中可以直接使用 STL 的实现，在 Java 中可以使用 Priority Queue，但是**在 C# 10之前的版本并不直接提供这样的实现**。

**P.S. 在 C# 10 中，新增加了优先队列的官方实现，详见 [PriorityQueue<TElement,TPriority> 类 ](https://learn.microsoft.com/zh-cn/dotnet/api/system.collections.generic.priorityqueue-2?view=net-6.0)。**


## 一种可行的解决办法 （适用于 C# Version < 10）

C# 集合类的思想和 C++、Java 并不一致（提供尽可能多的集合类以满足需求），C# 只提供基础的通用集合类。

所以如果想要在 C# 中使用 Heap/Priority Queue，**要么使用第三方库实现，要么自己基于 SortedList、SortedSet、SortedDictionary 构造一个功能类似的**（或者直接去用 C++ 或者 Java 吧）。

> 至少在 C# 9.0 中还未提供，不确定将来的 C# 是否会提供 Priority Queue

下面附上一种基于 SortedList 构建的 Priority Queue：
```csharp
public class PriorityQueue<T> where T : IComparable<T> {
    private SortedList<T, int> list = new SortedList<T, int>();
    private int count = 0;

    public void Add(T item) {
        if (list.ContainsKey(item)) list[item]++;
        else list.Add(item, 1);

        count++;
    }

    public T PopFirst() {
        if (Size() == 0) return default(T);
        T result = list.Keys[0];
        if (--list[result] == 0)
            list.RemoveAt(0);

        count--;
        return result;
    }

    public T PopLast() {
        if (Size() == 0) return default(T);
        int index = list.Count - 1;
        T result = list.Keys[index];
        if (--list[result] == 0)
            list.RemoveAt(index);

        count--;
        return result;
    }

    public int Size() {
        return count;
    }

    public T PeekFirst() {
        if (Size() == 0) return default(T);
        return list.Keys[0];
    }

    public T PeekLast() {
        if (Size() == 0) return default(T);
        int index = list.Count - 1;
        return list.Keys[index];
    }
}
```

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
