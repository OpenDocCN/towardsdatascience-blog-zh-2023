# Python enumerate() 函数解释

> 原文：[`towardsdatascience.com/python-enumerate-function-explained-7907837e171c`](https://towardsdatascience.com/python-enumerate-function-explained-7907837e171c)

## 在这篇文章中，我们将探索如何使用 Python **enumerate()**函数

[](https://pyshark.medium.com/?source=post_page-----7907837e171c--------------------------------)![Misha Sv](https://pyshark.medium.com/?source=post_page-----7907837e171c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7907837e171c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7907837e171c--------------------------------) [Misha Sv](https://pyshark.medium.com/?source=post_page-----7907837e171c--------------------------------)

·发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----7907837e171c--------------------------------) ·3 分钟阅读·2023 年 2 月 20 日

--

![](img/227b6430ee6fd418032c78c73fbda97f.png)

照片由[Nick Fewings](https://unsplash.com/ko/@jannerboy62?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供，来源于[Unsplash](https://unsplash.com/photos/6H8a6vNGDKg?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

**目录**

+   介绍

+   使用 enumerate()与列表

+   使用 enumerate()与字符串

+   使用 enumerate()与字典

+   使用 enumerate()与 zip()

+   结论

# 介绍

Python **enumerate()**函数是一个内置函数，可以在遍历 Python 可迭代对象时跟踪其索引。

Python **enumerate()**函数的语法是：

```py
enumerate(iterable, start=0)
```

在哪里：

+   *iterable* — 可以是任何可迭代的 Python 对象，如字符串、元组、列表、集合、[字典](https://pyshark.com/python-dictionary-data-structure/)等。

+   *start* — 指定起始索引的参数（可选）。默认为 0。

# 使用 enumerate()与列表

作为一个简单的例子，我们将创建一个[Python 列表](https://pyshark.com/python-list-data-structure/)，然后使用带有**enumerate()**函数的 for 循环打印列表的每个元素及其索引：

```py
my_list = ['Apple', 'Banana', 'Orange', 'Pineapple']

for i, elem in enumerate(my_list):
    print(i, elem)
```

你应该得到：

```py
0 Apple
1 Banana
2 Orange
3 Pineapple
```

# 使用 enumerate()与字符串

**enumerate()**函数也适用于 Python 字符串，因为它们是可迭代对象。

它的工作方式类似于与 Python 列表的示例。不同之处在于，对于字符串，我们将遍历字符串中的每个字符，并将其与在字符串中的索引（位置）一起打印出来：

```py
my_string = 'Apple'

for i, char in enumerate(my_string):
    print(i, char)
```

你应该得到：

```py
0 A
1 p
2 p
3 l
4 e
```

# 使用 enumerate()与字典

另一个有趣的例子是使用**enumerate()**函数与[Python 字典](https://pyshark.com/python-dictionary-data-structure/)。

虽然字典中的键值对是无序的且未索引，但使用**enumerate()**对你的代码来说是一个非常有用的选项。

输出的主要区别在于，当遍历字典时，你可以选择遍历字典的键、字典的值或字典的键值对。

## 遍历字典键

```py
my_dict = {
    'Apple': 3,
    'Banana': 1,
    'Orange': 2,
    'Pineapple': 5
    }

for i, key  in enumerate(my_dict.keys()):
    print(i, key)
```

你应该得到：

```py
0 Apple
1 Banana
2 Orange
3 Pineapple
```

## 遍历字典值

```py
my_dict = {
    'Apple': 3,
    'Banana': 1,
    'Orange': 2,
    'Pineapple': 5
    }

for i, value  in enumerate(my_dict.values()):
    print(i, value)
```

你应该得到：

```py
0 3
1 1
2 2
3 5
```

## 遍历字典键值对

当将键值对遍历与**enumerate()**功能结合使用时，输出将是每个索引和一个 [元组](https://pyshark.com/python-tuple-data-structure/) ，其中包含字典中每个条目的键值对：

```py
my_dict = {
    'Apple': 3,
    'Banana': 1,
    'Orange': 2,
    'Pineapple': 5
    }

for i, (key, value) in enumerate(my_dict.items()):
    print(i, (key, value))
```

你应该得到：

```py
0 ('Apple', 3)
1 ('Banana', 1)
2 ('Orange', 2)
3 ('Pineapple', 5)
```

# 使用 enumerate() 和 zip()

一个稍微高级的示例是将**enumerate()**与其他 Python 函数一起使用，例如。

两个函数组合的功能允许同时遍历多个列表，同时跟踪元素对的索引：

```py
fruits = ['Apple', 'Banana', 'Orange', 'Pineapple']
prices = [3, 1, 2, 5]

for i, (fruit, price) in enumerate(zip(fruits, prices)):
    print(i, fruit, price)
```

你应该得到：

```py
0 Apple 3
1 Banana 1
2 Orange 2
3 Pineapple 5
```

# 结论

在这篇文章中，我们探讨了 Python **enumerate()** 函数。

现在你知道了基本功能，你可以尝试将其与其他可迭代的 [数据结构](https://pyshark.com/category/data-structures/) 一起使用，以处理更复杂的用例。

如果你有任何问题或有编辑建议，请随时在下方留言，并查看更多我的 [Python Functions](https://pyshark.com/category/python-functions/) 教程。

*原文发布于* [*https://pyshark.com*](https://pyshark.com/python-enumerate-function/) *于 2023 年 2 月 20 日。*
