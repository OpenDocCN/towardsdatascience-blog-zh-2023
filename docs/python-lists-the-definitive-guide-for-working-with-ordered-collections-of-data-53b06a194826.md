# Python 列表：处理有序数据集合的终极指南

> 原文：[`towardsdatascience.com/python-lists-the-definitive-guide-for-working-with-ordered-collections-of-data-53b06a194826`](https://towardsdatascience.com/python-lists-the-definitive-guide-for-working-with-ordered-collections-of-data-53b06a194826)

## Python 列表的全面指南

[](https://federicotrotta.medium.com/?source=post_page-----53b06a194826--------------------------------)![Federico Trotta](https://federicotrotta.medium.com/?source=post_page-----53b06a194826--------------------------------)[](https://towardsdatascience.com/?source=post_page-----53b06a194826--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----53b06a194826--------------------------------) [Federico Trotta](https://federicotrotta.medium.com/?source=post_page-----53b06a194826--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----53b06a194826--------------------------------) ·10 分钟阅读·2023 年 7 月 19 日

--

![](img/f7fec4e1603355e89c91a3e935b6ae4c.png)

图片由 [Jill Wellington](https://pixabay.com/it/users/jillwellington-334088/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1902136) 提供，来源于 [Pixabay](https://pixabay.com/it//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1902136)

在编程时，我们总是需要处理数据结构。我的意思是，我们需要将信息存储在某个地方，以便以后可以重新使用。

Python 是一种非常灵活的编程语言，给我们提供了使用不同类型数据结构的可能性。

在这篇文章中，我们将分析 Python 列表。因此，如果你是 Python 初学者，并且正在寻找关于列表的全面指南，那么这篇文章绝对适合你。

在这里你将学到：

```py
Table of Contents:

What is a list in Python?
The top 9 features in Python lists, with examples
  How to create a list in Python
  Accessing list elements
  Modifying the elements of a list
  Adding elements to a list
  Removing elements from a list
  Concatenating lists
  Calculating the lenght of a list
  Sorting the elements of a list
  List comprehension
```

# 什么是 Python 列表？

在 Python 中，列表是一种内置的数据结构，允许我们以文本或数字的形式存储和操作数据。

列表以有序的方式存储数据，这意味着可以通过位置访问列表中的元素。

列表也是一种可修改的数据结构，与 元组 相对。

最后，列表还可以存储重复的值而不会引发错误。

# Python 列表的 9 大特性及示例

学习 Python 的最佳方式是亲自上手敲代码，并且尽可能地解决实际问题。

所以，现在我们将通过代码示例展示 Python 列表的 9 个主要特性，因为正如我们将看到的，理论在编程中意义不大：我们只需要编写代码并解决问题。

## 如何在 Python 中创建列表

要创建列表，我们需要使用方括号：

```py
# Create a simple list
numbers = [1,2,3,"dog","cat"]

# Show list
print(numbers)

>>>

[1, 2, 3, 'dog', 'cat']
```

创建列表的另一种方法是使用内置方法 `list`。例如，假设我们想创建一个包含从 0 到 9 的数字的列表。我们可以使用内置方法 `range` 来创建这个范围，然后将其作为参数传递给 `list` 方法来创建列表，如下所示：

```py
# Create a list in the range
list_range = list(range(10))

# Show list
print(range_list)

>>>

[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

我们还可以创建所谓的列表中的列表，即嵌套列表。例如，假设我们想存储与跑步的人测量时间（以秒为单位）相关的数据。如果我们需要这些数据作为列表，我们可以像这样创建一个列表的列表：

```py
# Create a list of lists
times = [["Jhon"], [23, 15, 18], ["Karen"], [17, 19, 15],
        ["Tom"], [21, 19, 25]]

# Print list
print(times)

>>>

[['Jhon'], [23, 15, 18], ['Karen'], [17, 19, 15], ['Tom'], [21, 19, 25]]
```

## 访问列表元素

列表中的元素可以通过其位置访问。我们需要记住的是，在 Python 中，我们从 0 开始计数。这意味着第一个元素通过 0 访问：

```py
# Create a list of elements
values = [1,2,3,"dog","cat"]

# Print elements by accessing them
print(f"The first element is: {values[0]}")
print(f"The fourth element is: {values[3]}")

>>>

The first element is: 1
The fourth element is: dog
```

因此，我们只需要注意正确计数。

访问列表中的列表稍微复杂一些，但并不难。我们首先需要访问与外部列表相关的位置，然后在内部列表中计数。

正如我们所说的，实践胜于理论。在 Python 中，我们用一个例子来展示这个概念：

```py
# Create a list of lists
times = [["Jhon"], [23, 15, 18], ["Karen"], [17, 19, 15],
        ["Tom"], [21, 19, 25]]

# Print
print(f"The first runner is:{times[0]}.\nHis first registered time is:{times[1][0]}\nThe min registered time is:{min(times[1])}")

>>>

The first runner is:['Jhon'].
His first registered time is:23
The min registered time is:15
```

Jhon 是第一个登记的跑步者，因此我们用 `times[0]` 访问它。

然后，我们想计算他第一次登记的时间。为此，我们需要输入 `times[1][0]`，因为：[1] 表示第二个位置，相对于外部列表。意思是我们访问了内部列表 `[23, 15, 18]`。最后，`[0]` 访问了内部列表中的第一个数字，确实是 23。

## 修改列表中的元素

正如我们所说，列表是可修改的，要修改列表中的元素，我们需要访问它。

那么，让我们做一个例子：

```py
# Create a list of numbers
numbers = [1, 2, 3, 4, 5]

# Modify the third element
numbers[2] = 10

# Print new list
print(numbers)

>>>

[1, 2, 10, 4, 5]
```

所以，在这种情况下，我们修改了第三个元素，将其从 3 改为 10。

我们还可以修改文本，特别是句子。让我们看一个例子：

```py
# Create a sentence in a list
sentence = list("Hello, World!")

# Substitute "world" with "Python"
sentence[7:] = list("Python!")

# Print sentence
print(''.join(sentence))

>>>

"Hello, Python!"
```

所以，在这里，我们用 `sentence[7:]` 替换了列表 "sentence" 中从第七个（从 0 开始计数，如前所述）元素到最后一个元素的所有字母。

然后，我们使用了 `''.join(sentence)` 方法来将句子作为一个整体打印。事实上，如果我们只是使用 `print()`，它会将字母逐个打印，如下所示：

```py
print(sentence)

>>>

['H', 'e', 'l', 'l', 'o', ',', ' ', 'P', 'y', 't', 'h', 'o', 'n', '!']
```

## 向列表中添加元素

由于列表是可变的，我们可以向其中添加新元素，如果需要，并且我们有几种方法可以做到这一点。

第一种方法是使用 `append()` 方法，这在我们只需要向列表中添加一个元素时特别适用。例如：

```py
# Create a list with fruits
fruits = ['apple', 'banana']

# Append the element "orange" to the list
fruits.append('orange')

# Print list
print(fruits)

>>>

['apple', 'banana', 'orange']
```

向现有列表中添加元素的另一种方法是使用 `extend()` 方法，这在需要一次添加多个元素时特别适用。例如，如下所示：

```py
# Create a list of numbers
numbers = [1, 2, 3, 4, 5]

# Extend the list with new numbers
numbers.extend([6, 7, 8])

# Print list
print(numbers)

>>>

[1, 2, 3, 4, 5, 6, 7, 8]
```

## 从列表中移除元素

由于可变性，我们可以向列表中添加元素，也可以删除元素。

在这里，我们有两种方法：我们可以使用切片功能，或者可以直接指定要删除的元素。

让我们通过 Python 示例来看看这些：

```py
# Create a list with fruits
fruits = ['apple', 'banana', 'orange']

# Remove the element banana
fruits.remove('banana')

# Print list
print(fruits)

>>>

['apple', 'orange']
```

所以，`remove()` 方法允许我们通过输入其值直接从列表中删除特定元素。

我们可以使用的另一种方法是通过以下方式访问我们想要删除的元素的位置：

```py
# Create a list of numbers
numbers = [1, 2, 3, 4, 5]

# Delete on element: slicing method
popped_element = numbers.pop(2)

# Print
print(numbers)  
print(f"The deleted element is:{popped_element}")  

>>>

[1, 2, 4, 5]
The deleted element is:3
```

因此，`pop()` 方法通过访问索引从列表中删除一个元素。

选择使用哪一个？这取决于情况。如果我们有一个非常长的列表，通常使用 `remove()` 方法是个好主意，这样我们可以直接写出我们实际上想要删除的元素，而不会在计算索引时出错。

## 合并列表

列表的可变性使我们能够执行许多任务，例如将多个列表合并成一个列表。

这个操作很简单，使用 `+` 来进行，如下所示：

```py
# Create a list
list1 = [1, 2, 3]

# Create a second list
list2 = [4, 5, 6]

# Concatenate list
combined_list = list1 + list2

# Print cncatenated lists
print(combined_list)

>>>

[1, 2, 3, 4, 5, 6]
```

当然，这个功能也可以在字符串上执行：

```py
# Create a list
hello = ["Hello"]

# Create another list
world = ["world"]

# Concatenate
single_list = hello + world

# Print concatenated
print(single_list)

>>>

['Hello', 'world']
```

另一种合并列表的方法是将嵌套列表展平。换句话说，我们可以从嵌套列表中创建一个单一的“直线”列表，如下所示：

```py
# Create a nested list
lists = [[1, 2], [3, 4], [5, 6]]

# Create a unique list
flattened_list = sum(lists, [])

# Print unique list
print(flattened_list)

>>>

[1, 2, 3, 4, 5, 6]
```

基本上，我们使用 `sum()` 方法来获取列表 `lists` 中的所有元素，并将它们附加到一个空列表 `[]` 中。

## 计算列表的长度

在前面的示例中，我们自己创建了列表，以演示如何操作列表的 Python 示例。

然而，当使用 Python 时，常常会从不同的来源检索数据，这意味着有人创建了一个我们实际上不知情的列表。

当我们面对一个未知列表时，我们最好先计算它的长度。我们可以这样做：

```py
# Create a list
fruits = ['apple', 'banana', 'orange']

# Print list lenght
print(f"In this list there are {len(fruits)} elements")

>>>

 In this list there are 3 elements
```

所以，`len()` 方法计算列表中有多少个元素，而不必担心它们的类型。这意味着元素可以是所有数字、所有字符串，或两者兼有：`len()` 方法会统计它们全部。

## 对列表元素进行排序

当我们不知道列表的内容时，另一个可能执行的操作是对其元素进行排序。

我们有不同的方法来实现这一点。

我们从 `sort()` 方法开始：

```py
# Creaye a list of numbers 
numbers = [5, 2, 1, 4, 3]

# Sort the numbers
numbers.sort()

# Print sorted list
print(numbers)

>>>

[1, 2, 3, 4, 5]
```

因此，我们可以直接将列表作为参数传递给 `sort()` 方法，它将对元素进行排序。

但如果我们想要排序一个包含字符串的列表呢？例如，假设我们想要按字母顺序对列表中的元素进行排序。我们可以这样做：

```py
# Create a list of strings
words = ['cat', 'apple', 'dog', 'banana']

# Sort in alphabeticla order
sorted_words = sorted(words, key=lambda x: x[0])

# Print sorted list
print(sorted_words)

>>>

['apple', 'banana', 'cat', 'dog']
```

因此，在这种情况下，我们使用 `sorted()` 方法，需要指定：

+   关于我们想要排序的列表的参数。在这种情况下，是 `words`。

+   `key`。这意味着我们需要指定一种方法。在这种情况下，我们使用了一个 lambda 函数，通过 `x[0]` 获取每个元素的第一个字母，遍历所有元素：这是我们选择每个单词第一个字母的方式。

对字符串进行排序的另一种方式是按每个元素的字符数进行排序。换句话说，假设我们想要将较短的单词放在列表的开头，而将最长的单词放在末尾。我们可以这样做：

```py
# Create a list of words
words = ['cat', 'apple', 'dog', 'banana']

# Sort words by lenght
words.sort(key=len)

# Print sorted list
print(words)

>>>

['cat', 'dog', 'apple', 'banana']
```

因此，即使使用`sort()`方法，我们也可以传递一个参数`key`。在这种情况下，我们选择了`len`，它计算每个单词的长度。因此，列表现在是按照从最短的单词到最长的单词的顺序排列的。

## 列表推导式

列表推导式是一种快速且简洁的方式，通过一行代码使用循环和语句的力量创建一个新列表。

让我们看一个例子。假设我们想取 1 到 6 的数字，并创建一个包含它们平方值的列表。我们可以这样做：

```py
# Create a list of squared numbers
squares = [x ** 2 for x in range(1, 6)]

# Print list
print(squares) 

>>>

[1, 4, 9, 16, 25]
```

现在，我们可以不使用列表推导式而达到相同的结果，但需要大量代码，如下所示：

```py
# Create empty list
squares = []

# Iterate over the numbers in the range
for squared in range(1, 6):
    # Calculare squares and append to empty list
    squares.append(squared ** 2)

# Print list    
print(squares)

>>>

[1, 4, 9, 16, 25]
```

因此，我们得到相同的结果，但列表推导式使我们只需一行代码即可实现。

我们还可以在列表推导式中使用`if`语句，这使得它比“标准方法”更加快捷和优雅，对于标准方法，我们需要使用`for`循环进行迭代，然后用`if`语句选择所需的值。

例如，假设我们想创建一个新的平方数列表，但只想要偶数。我们可以这样做：

```py
# Create a list with numbers in a range
numbers = list(range(1, 11))

# Get the even squared numbers and create a new list
squared_evens = [x ** 2 for x in numbers if x % 2 == 0]

# Print list with squared & even numbers
print(squared_evens)

>>>

[4, 16, 36, 64, 100]
```

因此，我们需要记住，为了取得偶数，我们可以利用它们能被 2 整除的事实。所以，`x % 2 == 0` 获取那些被 2 除时余数为 0 的数字。也就是说：它们是偶数。

# 结论

在本文中，我们展示了关于 Python 列表的全面指南。

列表是一种非常重要且有用的数据结构。它们不难学习，但对于每个 Python 程序员来说都是一个基本资产。

![](img/5079e3af9eda458328cb258c452fb935.png)

Federico Trotta

我是 Federico Trotta，我是一名自由技术写作员。

想与我合作吗？[联系我](https://bio.link/federicotrotta)。
