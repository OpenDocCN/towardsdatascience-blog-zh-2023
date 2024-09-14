# Python sorted() 函数解析

> 原文：[`towardsdatascience.com/python-sorted-function-explained-8e46bc002147`](https://towardsdatascience.com/python-sorted-function-explained-8e46bc002147)

## 本文将探讨如何使用 Python 的 **sorted()** 函数

[](https://pyshark.medium.com/?source=post_page-----8e46bc002147--------------------------------)![Misha Sv](https://pyshark.medium.com/?source=post_page-----8e46bc002147--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8e46bc002147--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8e46bc002147--------------------------------) [Misha Sv](https://pyshark.medium.com/?source=post_page-----8e46bc002147--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8e46bc002147--------------------------------) ·4 分钟阅读·2023 年 1 月 16 日

--

![](img/5fda325ced75dc2866b7414161b8a2b8.png)

[Andre Taissin](https://unsplash.com/fr/@andretaissin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 摄影，来自 [Unsplash](https://unsplash.com/photos/hOwcob_3dpc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

**目录**

+   介绍

+   基本排序使用 sorted()

+   使用 key 函数与 sorted()

+   使用`sorted()`对自定义对象进行排序

+   结论

# 介绍

Python **sorted()** 函数是用于排序可迭代对象的内置函数。

它使用 timsort 作为排序算法，该算法源自归并排序和插入排序。

Python **sorted()** 函数的语法是：

```py
sorted(iterable, key=None, reverse=False)
```

其中：

+   *iterable* — 可以是任何可迭代的 Python 对象，如字符串、元组、列表、集合、[字典](https://pyshark.com/python-dictionary-data-structure/) 等。

+   *key* — 可选参数，允许添加一个函数（例如 [lambda 函数](https://pyshark.com/python-lambda-functions/)）作为排序的关键字。默认为 *None*。

+   *reverse* — 可选参数，允许反转可迭代对象（按降序排序），如果设置为 *True*。默认为 *False*。

**sorted()** 函数的过程定义为：

```py
sorted(iterable) -> sorted list
```

# 基本排序使用 sorted()

**sorted()**函数有很多应用，下面我们来看几个基本的示例。

## 将数字列表按升序排序

最简单的例子是将一个 [列表](https://pyshark.com/python-list-data-structure/) 的数字按升序排序：

```py
#Create a list of numbers
nums = [3, 1, 9, 7, 5]

#Sort the list of numbers
s_nums = sorted(nums)

#Print sorted list
print(s_nums)
```

你应该得到：

```py
[1, 3, 5, 7, 9]
```

## 将数字列表按降序排序

类似于之前的示例，我们将排序一个数字列表，但现在按降序排序：

```py
#Create a list of numbers
nums = [3, 1, 9, 7, 5]

#Sort the list of numbers
s_nums = sorted(nums, reverse=True)

#Print sorted list
print(s_nums)
```

你应该得到：

```py
[9, 7, 5, 3, 1]
```

## 排序一个字符串列表

Python 的 **sorted()** 函数也可以排序包含字符串元素的列表。

排序数字的过程非常简单直观，也可以扩展到排序字符串。

Python **sorted()** 函数根据每个字符串的第一个字符对字符串进行排序（例如，*‘apple’* 排在 *‘orange’* 之前，因为 *‘a’* 在字母表中排在 *‘o’* 之前）。

让我们看一个例子：

```py
#Create a list of strings
fruit = ['banana', 'pineapple', 'orange', 'apple']

#Sort the list of strings
s_fruit = sorted(fruit)

#Print sorted list
print(s_fruit)
```

你应该得到：

```py
['apple', 'banana', 'orange', 'pineapple']
```

如你所见，字符串列表已经根据字符串的第一个字符按字母顺序（升序）排序了。

你还可以通过将可选的 **reverse** 参数设置为 *True* 来按降序对字符串列表进行排序。

**注意：** 你可以将上述功能扩展到其他可迭代对象，如 [元组](https://pyshark.com/python-tuple-data-structure/)、[集合](https://pyshark.com/everything-about-python-set-data-structure/)，以及其他对象。

# 使用带有 key 函数的 sorted()

对于更复杂的排序任务，我们可以在 **sorted()** 中使用 *key* 函数，这将作为排序的关键。

使用 key 函数有两种方式：

+   使用 lambda 函数作为 *key* 函数

+   使用自定义函数作为 *key* 函数

## 使用 lambda 函数与 sorted()

让我们创建一个包含单词的示例列表：

```py
['Python', 'programming', 'tutorial', 'code']
```

现在，在这个示例中，我们希望根据元素的长度对列表进行排序，这意味着单词将按从短到长的顺序排列。

如你所料，我们将不得不使用 **len()** 函数来计算每个元素的长度，使用 lambda 函数可以将其作为排序的 key 函数：

```py
#Create a list of words
words = ['Python', 'programming', 'tutorial', 'code']

#Sort the list of words based on length of each word
s_words = sorted(words, key=lambda x: len(x))

#Print sorted list
print(s_words)
```

你应该得到：

```py
['code', 'Python', 'tutorial', 'programming']
```

## 使用自定义函数与 sorted()

让我们重用前面示例中的相同单词列表：

```py
['Python', 'programming', 'tutorial', 'code']
```

现在，我们希望基于列表中每个元素的长度进行相同的排序，但使用自定义函数来计算每个单词的长度。

我们可以定义一个简单的函数来计算单词的长度，并将其作为 *key* 函数传递给 **sorted()**：

```py
#Create a list of words
words = ['Python', 'programming', 'tutorial', 'code']

#Define a function to calculate length of a word
def calc_len(word):
    len_w = len(word)
    return len_w

#Sort the list of words based on length of each word
s_words = sorted(words, key=calc_len)

#Print sorted list
print(s_words)
```

你应该得到：

```py
['code', 'Python', 'tutorial', 'programming']
```

这与我们使用 **len()** 和 lambda 函数作为 **sorted()** 的 *key* 函数时的结果是相同的。

# 使用 sorted() 对自定义对象进行排序

Python **sorted()** 函数的功能可以扩展到自定义对象（只要我们排序的是可迭代对象）。

例如，让我们创建一个具有两个属性 **name** 和 **age** 的自定义类 **Person**：

```py
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
    def __repr__(self):
        return repr((self.name, self.age))
```

这个类将创建一个包含每个人信息的元组列表：

```py
#Create a list of tuples
persons = [
    Person('Mike', 20),
    Person('John', 35),
    Person('David', 23),
]

#Print list of tuples
print(persons)
```

你应该得到：

```py
[('Mike', 20), ('John', 35), ('David', 23)]
```

如你所见，这现在是一个元组的列表，这是一个 Python 可迭代对象，可以使用 **sorted()** 函数进行排序。

在这个例子中，我们希望根据每个人的 **age** 属性对列表进行排序：

```py
#Sort the list of tuples based on age attribute
s_persons = sorted(persons, key=lambda person: person.age)

#Print sorted list
print(s_persons)
```

你应该得到：

```py
[('Mike', 20), ('David', 23), ('John', 35)]
```

# 结论

在本文中，我们探讨了如何使用 [Python sorted() 函数](https://docs.python.org/3/howto/sorting.html)。

现在你了解了基本功能，你可以在其他可迭代的 [数据结构](https://pyshark.com/category/data-structures/) 中练习使用它，以应对更复杂的用例。

如果你有任何问题或对某些修改有建议，请随时在下方留言，并查看更多我的[Python 函数](https://pyshark.com/category/python-functions/)教程。

*最初发表于* [*https://pyshark.com*](https://pyshark.com/python-sorted-function/) *于 2023 年 1 月 16 日。*
