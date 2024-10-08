# Python 字符串数据类型解释

> 原文：[`towardsdatascience.com/python-string-data-type-explained-ff81a363fe08`](https://towardsdatascience.com/python-string-data-type-explained-ff81a363fe08)

## 在本文中，我们将探索 Python 字符串数据类型

[](https://pyshark.medium.com/?source=post_page-----ff81a363fe08--------------------------------)![Misha Sv](https://pyshark.medium.com/?source=post_page-----ff81a363fe08--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ff81a363fe08--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ff81a363fe08--------------------------------) [Misha Sv](https://pyshark.medium.com/?source=post_page-----ff81a363fe08--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ff81a363fe08--------------------------------) ·阅读时间 6 分钟·2023 年 1 月 30 日

--

![](img/251d45286cc239a7aea658321c440a0f.png)

照片由 [Gaelle Marcel](https://unsplash.com/@gaellemarcel?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄，来自 [Unsplash](https://unsplash.com/photos/S6hz7Y1FCTs?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在本文中，我们将探索 Python 字符串数据类型。

**目录**

+   介绍

+   在 Python 中创建字符串

+   在 Python 中访问字符串中的字符

+   在 Python 中查找字符串中的字符

+   在 Python 中切片字符串

+   在 Python 中迭代字符串

+   在 Python 中连接字符串

+   在 Python 中拆分字符串

+   结论

# 介绍

在 Python 中，字符串是不可变的字符序列，用于处理文本数据。

你应该了解关于字符串的关键点如下：

+   有序的

+   不可变的

+   可迭代的

学习每种编程语言中的数据类型对于理解代码和程序至关重要。

字符串数据类型在许多编程和机器学习解决方案中广泛使用，特别是在 Python 中用于存储一些格式化的文本数据。

# 在 Python 中创建字符串

在 Python 中，你可以通过 4 种不同的方式创建字符串：

+   通过用单引号括起字符

+   通过用双引号括起字符

+   通过用三重引号括起字符

+   通过使用 **str()** 构造函数

# 使用单引号创建字符串

这是在 Python 中创建字符串的最常见方式之一，非常简单：

```py
#Single quotes
my_string1 = 'Hello World!'

print(my_string1)
```

你应该得到：

```py
Hello World!
```

# 使用双引号创建字符串

这种创建字符串的方式与之前的方法相同，只是现在我们将使用双引号：

```py
#Double quotes
my_string2 = "Hello World!"

print(my_string2)
```

你应该得到：

```py
Hello World!
```

# 使用三重引号创建字符串

这种创建字符串的方式可能是最少见的，因为只有少数几种情况需要使用它。

用三重引号括起字符将产生与前两种方法相同的输出：

```py
#Double quotes
my_string3 = '''Hello World!'''

print(my_string3)
```

你应该得到：

```py
Hello World!
```

然而，使用三重引号的一个主要区别是，当你想创建一个多行字符串时，字符串的不同部分会在输出中显示在不同的行上。

例如：

```py
#Double quotes
my_string4 = '''Hello
World!'''

print(my_string4)
```

你应该得到：

```py
Hello
World!
```

# 使用 str() 构造函数创建一个字符串

在 Python 中，你也可以通过使用**str()**构造函数来创建字符串。

**str(***object***)** 构造函数接受任何对象并返回其字符串表示形式。

它返回：

+   如果*object*是内置的 Python 对象之一（[int()](https://pyshark.com/python-numeric-data-types/#int), [float()](https://pyshark.com/python-numeric-data-types/#float), [complex()](https://pyshark.com/python-numeric-data-types/#complex), [bool()](https://pyshark.com/python-boolean-data-type-explained/)，以及其他），其字符串表示形式如下。

+   如果*object*为空，则为空字符串

让我们来看几个使用**str()**的不同数据类型的示例：

```py
#String of int
str_int = str(5)

#String of float
str_float = str(1.5)

#String of complex
str_complex = str(1+3j)

#String of bool
str_bool = str(True)

#Print values
print(str_int)
print(str_float)
print(str_complex)
print(str_bool)
```

你应该得到：

```py
5
1.5
(1+3j)
True
```

# 在 Python 中访问字符串中的字符

Python 列表的一个重要且非常有用的属性是它是一个带索引的序列，这意味着对于一个包含**n**个元素的列表，第一个元素的索引 = 0，第二个元素的索引 = 1，一直到*n*-1。

字符串中的字符可以通过其索引访问，索引也可以反转，这意味着第一个元素的索引 = — *n*，第二个元素的索引 = — *n*+1，一直到 -1。

为了更容易展示，请看下面的可视化图：

![](img/f9219ee48fe2dfd698192b9da38a2f94.png)

图片由作者提供

我们可以看到字符串中的‘P’字符有两个索引：0 和 -6。

让我们在 Python 中创建这个字符串，并使用上述索引打印出它的第一个字符：

```py
#Create a string
my_string = 'Python'

#Print first character
print(my_string[0])
print(my_string[-6])
```

你应该得到：

```py
P
P
```

# 在 Python 中查找字符串中的字符

使用索引，我们还可以找到字符串中字符的位置。

让我们重用之前示例中的字符串：*‘Python’*，并尝试找到*‘y’*字符在字符串中的位置。

使用 Python 字符串的**.index()** 方法，我们可以通过将字符作为参数传递给它来找到字符的位置：

```py
#Create a string
my_string = 'Python'

#Find character
i = my_string.index('y')

#Print index
print(i)
```

你应该得到：

```py
1
```

# 在 Python 中切片字符串

在前一节中，我们探讨了如何通过其精确索引从 Python 字符串中访问一个字符。

在本节中，我们将探讨如何访问一系列字符，例如前两个或最后两个。

记住，若要使用索引从字符串中检索字符，我们将索引放在方括号**[]**中。

切片使用相同的方法，但我们传递的是一个范围，而不是单一的索引值。

Python 中的范围是使用以下语法传递的**[***from* **:** *to***]**。

使用范围我们可以切片字符串以访问多个字符：

```py
#Create a string
my_string = 'Python'

#First two characters
first_two = my_string[:2]

#Second to fourth characters
mid_chars = my_string[1:4]

#Last two characters
last_two = my_string[-2:]

#Print characters
print(first_two)
print(mid_chars)
print(last_two)
```

你应该得到：

```py
Py
on
yth
```

注意，指定的字符在*to*索引处不包括在内，因为在 Python 切片算法中，它会遍历字符直到指定的*to*索引，并包括所有到达该索引但不包括*to*索引下的字符。

# 在 Python 中迭代字符串

Python 字符串是一个可迭代对象，这意味着我们可以遍历字符串中的字符。

可以使用 **for()** 循环执行简单的迭代：

```py
#Create a string
my_string = 'Python'

#Iterate over a string
for char in my_string:
    print(char)
```

你应该得到：

```py
P
y
t
h
o
n
```

# 在 Python 中连接字符串

在 Python 中，我们也可以将多个字符串连接（组合）在一起以创建一个单一字符串。

在 Python 中连接字符串的两种最流行的方法是：

+   使用 ‘+’ 操作符

+   使用 .join() 方法

## 使用 ‘+’ 操作符

使用 ‘+’ 操作符是连接多个字符串的最常见方法之一。

让我们看一个例子：

```py
#Create strings
s1 = 'Python'
s2 = 'Tutorial'
sep = ' '

#Concatenate strings
new_string = s1 + sep + s2

#Pring new string
print(new_string)
```

你应该得到：

```py
Python Tutorial
```

## 使用 .join() 方法

Python 字符串 .join() 方法允许将一个字符串列表连接起来以创建一个新字符串。

Python 字符串 **.join()** 方法的语法是：

```py
separator.join([list of strings])
```

让我们看一个例子：

```py
#Create strings
s1 = 'Python'
s2 = 'Programming'
s3 = 'Tutorial'
sep = ' '

#Concatenate strings
new_string = sep.join([s1, s2, s3])

#Pring new string
print(new_string)
```

你应该得到：

```py
Python Programming Tutorial
```

# 在 Python 中拆分字符串

在 Python 中，正如我们可以连接多个字符串一样，我们也可以将一个字符串拆分成多个字符串。

有多种方法可以做到这一点，但最常用的方法是使用字符串的 .split() 方法，它根据分隔符（默认分隔符是：**‘ ’**）将字符串拆分成一个字符串列表。

Python 字符串 **.split()** 方法的语法是：

```py
string.split(separator)
```

让我们看一个例子：

```py
#Create a string
long_string = 'Apple Banana Orange Pineapple'

#Concatenate strings
new_strings = long_string.split()

#Pring new string
print(new_strings)
```

你应该得到：

```py
['Apple', 'Banana', 'Orange', 'Pineapple']
```

你还可以根据你想要拆分字符串的内容指定自定义分隔符。

例如：

```py
#Create a string
long_string = 'Apple, Banana, Orange, Pineapple'

#Concatenate strings
new_strings = long_string.split(', ')

#Pring new string
print(new_strings)
```

你应该得到：

```py
['Apple', 'Banana', 'Orange', 'Pineapple']
```

# 结论

在本文中，我们探讨了 Python 布尔数据类型，包括它在布尔表达式和控制结构中的使用。

作为学习 Python 的下一步，考虑阅读以下文章，了解 Python 数据类型和数据结构：

+   [Python 数值数据类型](https://pyshark.com/python-numeric-data-types/)

+   [Python 布尔数据类型](https://pyshark.com/python-boolean-data-type-explained/)

+   [Python 列表数据结构](https://pyshark.com/python-list-data-structure/)

+   [Python 字典数据结构](https://pyshark.com/python-dictionary-data-structure/)

+   [Python 集合数据结构](https://pyshark.com/everything-about-python-set-data-structure/)

*最初发表于* [*https://pyshark.com*](https://pyshark.com/python-string-data-type-explained/) *2023 年 1 月 30 日。*
