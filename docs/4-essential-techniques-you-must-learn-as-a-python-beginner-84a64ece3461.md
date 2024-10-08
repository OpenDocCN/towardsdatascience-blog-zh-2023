# 作为 Python 初学者，你必须掌握的 4 个关键技巧

> 原文：[`towardsdatascience.com/4-essential-techniques-you-must-learn-as-a-python-beginner-84a64ece3461`](https://towardsdatascience.com/4-essential-techniques-you-must-learn-as-a-python-beginner-84a64ece3461)

## 以及如何使用它们，以便你能轻松应对下一个面试

[](https://murtaza5152-ali.medium.com/?source=post_page-----84a64ece3461--------------------------------)![Murtaza Ali](https://murtaza5152-ali.medium.com/?source=post_page-----84a64ece3461--------------------------------)[](https://towardsdatascience.com/?source=post_page-----84a64ece3461--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----84a64ece3461--------------------------------) [Murtaza Ali](https://murtaza5152-ali.medium.com/?source=post_page-----84a64ece3461--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----84a64ece3461--------------------------------) ·阅读时间 8 分钟·2023 年 2 月 9 日

--

![](img/384232c5c565694cf27955acb6a266aa.png)

照片由[Carl Heyerdahl](https://unsplash.com/@carlheyerdahl?utm_source=medium&utm_medium=referral)提供，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

## Lambda 函数

假设你在 Jupyter notebook 中处理一些数据，只是进行一些快速探索和分析。你仍处于数据清理和处理的早期阶段，离任何生产就绪的模型、可视化或应用程序还有很远。但你有一个截止日期要赶，所以你快速高效地进行探索，充分发挥你出色的 Python 技能。

在你冒险的过程中，你遇到了需要转换的数据列。你只需要对该列中的数字进行平方。这并不复杂，但不幸的是，它也是一种奇怪的需求，简单到可以快速完成，但复杂到没有内置函数。

所以，你决定使用[Pandas’s](https://pandas.pydata.org/docs/reference/api/pandas.Series.apply.html) `[apply](https://pandas.pydata.org/docs/reference/api/pandas.Series.apply.html)` [函数来转换数据](https://pandas.pydata.org/docs/reference/api/pandas.Series.apply.html) 列，使用你自己的自定义函数[1]。为此，你需要编写一个平方数字的函数，你以唯一的方式知道如何做到这一点：

```py
def square(num):
    return num * num
```

这样可以完成任务，但有点烦人且杂乱，尤其是在 Jupyter notebook 中。它与大多数 Pandas 操作的单行结构不太匹配，因此在同事审阅你的 notebook 时，可能看起来不是很好。

但不要灰心，我的朋友，因为 lambda 函数来拯救你了。**Lambda 函数**——或更一般地说，匿名函数——提供了一种在 Python 中定义函数的替代方法。而最棒的是，你可以用一行代码来编写它们！这一点通过示例最为明显：

```py
square = lambda num: num * num
```

上面的代码与我们之前定义的这个函数是相同的。这里有几点你应该知道的：

+   `lambda` 关键字类似于 `def` 关键字，让 Python 知道我们想要定义一个函数。

+   lambda 函数的参数在冒号的左边，返回值在冒号的右边（实际上不使用 `return` 关键字）。

+   我们不需要给函数起名字，但如果我们愿意，可以通过变量赋值来做到这一点。

最后一点是关键。使用 lambda 函数让我们可以定义一个函数，并调用它或将其作为参数传递给另一个函数，而无需给它一个名称。让我们通过回到之前的例子并使其具体化来说明这一点。

让我们想象一下，我们有如下的 DataFrame `my_df`，其中包含三个人的薪资：

```py
 Name  Salary
0   John   45000
1   Mary   60000
2  Julie  100000
```

在这个极度理想化的世界里，雇主刚刚宣布大家的薪水将被*平方*。我们可以通过使用 lambda 函数在一行中更新我们的 DataFrame：

```py
>>> my_df['Salary'] = my_df['Salary'].apply(lambda num: num * num)
>>> my_df
    Name       Salary
0   John   2025000000
1   Mary   3600000000
2  Julie  10000000000
```

于是，财富无尽！也许有点戏剧化，但希望你现在能记住什么是 lambda 函数以及如何使用它们。如果你想更详细地讨论它们的细微差别，我建议查看我详细讨论 lambda 函数的两部分文章系列。

在这篇文章中，我们要进入下一个话题。

## 列表推导式

如果你在学习 Python 的早期阶段，可能还没听说过列表推导式，但我敢说你可能听说过 for 循环。你知道的，那种让你遍历序列并按自己的意愿操作的东西：

```py
my_lst = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
my_new_lst = list() # Make an empty list
for item in my_lst:
    if item % 2 == 0: # checking if even
        my_new_lst.append(item)

# This will give us a new list with only the even numbers
```

在上面的例子中，我们取了一组数字，只保留了偶数。代码足够简单——但因为这是 Python，我们喜欢简单——我们可以让它变得更加优雅。

如何做？进入列表推导式。**列表推导式**是一种 Python 语法，它有效地让你在一行中运行一个 for 循环，并将所有生成的元素放入一个新构建的列表中。这在抽象的术语中有点混乱，但通过示例会更容易理解。以下是使用列表推导式实现与上面相同的效果的方法：

```py
my_lst = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
my_new_lst = [item for item in my_lst if item % 2 == 0]
```

这段代码基本上只是将一个 for 循环压缩到了一行。你仍然有 `for` 关键字告诉 Python 遍历原始列表，开头的 `item` 只是告诉 Python 我们想要包含在最终列表中的内容。在这种情况下，我们只想包含原始项，但*只有在*它通过偶数筛选器时。

更重要的是，你还可以在过滤后应用额外的转换。下面的代码还会在将结果放入最终列表之前对其进行平方操作：

```py
my_lst = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
my_new_lst = [(item * item) for item in my_lst if item % 2 == 0]

# Final list will be [4, 16, 36, 64, 200]
```

你可以看到列表推导式如何派上用场。如果你选择使用它们，这里有一些重要的点需要记住：

+   过滤器总是会在映射转换之前应用。在上面的代码中，过滤器检查偶数，而映射转换会对原始项进行平方操作。

+   像任何 for 循环一样，列表推导式可以用于迭代任何可迭代序列，而不仅仅是列表。那它为什么叫做列表推导式呢？因为它总是*生成*一个列表作为最终结果。

+   最后，请记住，虽然列表推导式通常可以使你的代码更简洁，但并不是总是必要的。一个好的经验法则是，只有在它们确实使你的代码更具可读性时才使用它们。记住，可读性不一定是长度的函数——在传统的 for 循环中容易理解的代码在列表推导式中可能会变得杂乱和混乱。

要了解更多关于列表推导式的信息，你可以查看我关于它们的两部分系列文章。

## 继续和中断

Python 编程语言（以及其他编程语言）中有两个有用但偶尔被忽视的特性就是`continue`和`break`关键字。

让我们先定义每个关键字的作用，然后看几个具体的例子，并讨论你可能想使用它们的原因。

`continue`和`break`都是可以在循环内部执行的操作。它们的效果类似，但本质上不同。**Continue** 可以立即停止*当前*的循环运行并跳到*下一*次运行，而**break** 用于*完全退出循环*。

和大多数编程相关概念一样，这个概念最好通过例子来理解。仔细研究下面的代码片段。

```py
# Continue example
>>> for i in range(10):
...     if i == 5:
...         continue
...     print(i)
...
0
1
2
3
4
6
7
8
9

-------------------------------------------------------------------------------
# Break example 

>>> for i in range(10):
...     if i == 5:
...         break
...     print(i)
...
0
1
2
3
4
```

你看出区别了吗？当我们使用`continue`时，我们只是跳过了`5`，因为 Python 立即进入循环的下一次迭代，因此永远不会看到`5`的`print`语句。而当我们使用`break`时，Python 完全退出循环，所以在我们遇到`4`之后不会再打印任何东西。

此外，值得注意的是，虽然我们在上面的例子中使用了 for 循环，但`continue`和`break`在 while 循环中同样有效。

下面是几个你可能需要使用这些迷人小关键字的情况：

+   如果你需要无限循环直到满足某些条件，通常做法是将`break`与`while True:`结合使用。通常，`while True:`会导致无限循环，但将其与`break`配合使用可以绕过这一点。

+   假设你正在循环处理实时传输的数据（如果要技术一点，可以称之为**流**）。如果你想忽略一些数据但继续循环处理其余的数据，那么`continue`将是一个非常有用的关键字。

那么，让我们`继续`吧。

## 获取用户输入

有人可能会争辩说这不是 Python 中的一个必备技术，但在编程中确实是。但由于 Python 是编程的一部分，而它也是我假设你学习的语言，因为你已经读到了我的文章的这一部分，我认为这个话题是合理的。

如果你对这一点不熟悉，收集**用户输入**只是一个花哨的编程术语，用于让使用你的程序或应用程序的人通过（通常是）输入他们自己的字符来与之互动。在 Python 中，这一功能通过`input`函数访问。你可以在实时解释器会话中尝试一下：

```py
>>> input()
Hi I am a person
'Hi I am a person'
```

这里发生了三件事：

+   首先，我只是调用了`input`函数

+   这导致光标移动到下一行。此时，我能够输入`Hi I am a person`。我们知道 Python 期待在这一行有用户输入，因为即使这不是有效的 Python 代码，也没有出错。我们还可以看到解释器提示符（`>>>`）在这一行没有出现。

+   最后一行只是将用户输入的内容打印回终端。通常，这不会发生，因为你可能会希望将用户输入保存到一个变量中，以便以后使用。

我知道上面的例子有点模糊，所以这里有一个实际程序中如何使用这个功能的代码示例。我们通常也可以将字符串传递给`input`函数，这将用于提示用户输入一些内容。

```py
# Writing code that delivers a survey

user_response = input("Did you find this product useful? Please answer yes or no")
if user_response == "yes":
    print("Excellent! We are happy we could be of service.")
elif user_response == "no":
    print("Ah, we're sorry to hear that. We'll do better.")
```

就这样！现在你知道如何直接与使用你精彩程序的人进行互动了。还有一件事我应该提到：*所有*通过用户输入读取的内容都被视为字符串。因此，请记得显式地使用`int`或`float`转换数字，以免遇到一些非常恼人的错误！

## 最后的想法和总结

这是你的 Python 初学者备忘单（[第二部分](https://medium.com/towards-data-science/4-common-python-mistakes-you-should-avoid-as-a-beginner-bd28feb6162b)）：

1.  通过使用一行的**lambda 函数**来编写更好的 Pandas 代码（或者说代码的一般，但尤其是 Pandas 代码）。

1.  认识一下 for 循环的“表亲”——**列表推导式**。这是一个有用的、常被忽视的特性，可以优化你的代码质量。

1.  利用`**break**`和`**continue**`最大化你的循环使用。

1.  大量程序将要求你使用**用户输入**，所以你最好对它感到熟悉。

Python 是一种丰富且充满特性的语言，所有这些特性和宝藏共同旨在使*你*成为更好的程序员。

所以学好它，掌握它。

**想在 Python 中出类拔萃？** [**在这里获取我简单易读的指南的独家免费访问权限**](https://witty-speaker-6901.ck.page/0977670a91)**。想在 Medium 上阅读无限制的故事？使用下面我的推荐链接注册吧！**

[](https://murtaza5152-ali.medium.com/?source=post_page-----84a64ece3461--------------------------------) [## Murtaza Ali - Medium

### 阅读来自 Murtaza Ali 在 Medium 上的文章。他是华盛顿大学的博士生，关注人机交互…

[murtaza5152-ali.medium.com](https://murtaza5152-ali.medium.com/?source=post_page-----84a64ece3461--------------------------------)

## 参考资料

[1] [`pandas.pydata.org/docs/reference/api/pandas.Series.apply.html`](https://pandas.pydata.org/docs/reference/api/pandas.Series.apply.html)
