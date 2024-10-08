# 解释 Python ord() 和 chr() 函数

> 原文：[`towardsdatascience.com/python-ord-and-chr-functions-explained-dcb39944c480`](https://towardsdatascience.com/python-ord-and-chr-functions-explained-dcb39944c480)

## 在这篇文章中，我们将探讨如何使用 Python **ord()** 和 **chr()** 函数。

[](https://pyshark.medium.com/?source=post_page-----dcb39944c480--------------------------------)![Misha Sv](https://pyshark.medium.com/?source=post_page-----dcb39944c480--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dcb39944c480--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----dcb39944c480--------------------------------) [Misha Sv](https://pyshark.medium.com/?source=post_page-----dcb39944c480--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dcb39944c480--------------------------------) ·阅读时长 3 分钟·2023 年 1 月 12 日

--

![](img/a8dd2b58383572c073c66cfb43419c4c.png)

由 [Brett Jordan](https://unsplash.com/fr/@brett_jordan?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供的照片，刊登在 [Unsplash](https://unsplash.com/photos/7PYqjNzvrc4?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

**目录**

+   介绍

+   使用 ord() 将字符转换为 Unicode 代码点

+   使用 ord() 将字符串转换为 Unicode 代码点

+   使用 chr() 将整数转换为 Unicode 字符

+   结论

# 介绍

Python **ord()** 函数是一个内置函数，它返回指定字符的 Unicode 代码点。

Unicode 代码点是一个整数，用于表示 [Unicode 标准](https://unicode.org/standard/standard.html) 中的字符。

**ord()** 函数的处理定义如下：

```py
ord(character) -> Unicode code
```

其中 *character* 是一个 [Unicode 字符](https://en.wikipedia.org/wiki/List_of_Unicode_characters)。

Python **chr()** 函数是一个内置函数，它返回指定字符的 Unicode 代码点。

**chr()** 函数的处理定义如下：

```py
chr(integer) -> Unicode character
```

# 使用 ord() 将字符转换为 Unicode 代码点

让我们尝试使用 **ord()** 函数来查找字母 A、B 和 C 的 Unicode 代码点：

```py
#UCP of letter A
a = ord('A')
#UCP of letter B
b = ord('B')
#UCP of letter C
c = ord('C')

#Print values
print(a)
print(b)
print(c)
```

你应该得到：

```py
65
66
67
```

每个整数代表一个 Unicode 字符。

你可以使用 **ord()** 函数查找其他字符的 Unicode 代码点，包括特殊字符。

# 使用 ord() 将字符串转换为 Unicode 代码点

注意 **ord()** 函数只能接受一个字符作为参数，如 [介绍](https://pyshark.com/python-ord-and-chr-functions/#introduction) 中提到的：

```py
ord(character) -> Unicode code
```

如果你尝试将其用于一个包含多个字符的字符串，你会得到一个 TypeError：

```py
#UCP of string
x = ord('Python')
```

你应该得到：

```py
TypeError: ord() expected a character, but string of length 6 found
```

那么我们如何将整个字符串转换为 Unicode 代码点呢？

我们需要逐个字符地处理它，有几种方法可以解决这个任务：

+   使用 Python 的[map()](https://pyshark.com/python-map-function/)函数

+   使用列表推导式

## 使用 ord() 和 map() 将字符串转换为 Unicode 代码点

使用[Python **map()** 函数](https://pyshark.com/python-map-function/)我们可以对字符串的每个元素应用 Python **ord()** 函数：

```py
#Define a string
py_str = 'Python'

#UCP of string
ucp_vals = list(map(ord, py_str)

#Print UCP values
print(ucp_vals)
```

你应该得到：

```py
[80, 121, 116, 104, 111, 110]
```

## 使用 ord() 和列表推导式将字符串转换为 Unicode 代码点

解决这个任务的另一种方法是使用 Python 中带有列表推导式的**ord()**函数：

```py
#Define a string
py_str = 'Python'

#UCP of string
ucp_vals = [ord(char) for char in py_str]

#Print UCP values
print(ucp_vals)
```

你应该得到：

```py
[80, 121, 116, 104, 111, 110]
```

# 使用 chr() 将整数转换为 Unicode 字符

你也可以通过使用**chr()**函数来逆转**ord()**函数的操作，它将一个 Unicode 代码点（以整数格式）转换为一个 Unicode 字符。

例如，让我们看看 97、98 和 99 的 Unicode 代码点代表了哪些字符：

```py
#UCP of letter A
c1 = chr(97)
#UCP of letter B
c2 = chr(98)
#UCP of letter C
c3 = chr(99)

#Print values
print(c1)
print(c2)
print(c3)
```

你应该得到：

```py
a
b
c
```

# 结论

在这篇文章中，我们探讨了如何使用 Python 的**ord()**和**chr()**函数。

现在你知道了基本功能，你可以通过与其他可迭代的[数据结构](https://pyshark.com/category/data-structures/)一起练习，以应对更复杂的用例。

如果你有任何问题或对某些编辑有建议，请随时在下方留言，并查看更多我的[Python 函数](https://pyshark.com/category/python-functions/)教程。

*原文发布于* [*https://pyshark.com*](https://pyshark.com/python-ord-and-chr-functions/) *于 2023 年 1 月 12 日。*
