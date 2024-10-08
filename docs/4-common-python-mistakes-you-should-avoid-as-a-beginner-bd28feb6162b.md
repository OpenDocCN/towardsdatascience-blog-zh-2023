# 4 个初学者应避免的常见 Python 错误

> 原文：[`towardsdatascience.com/4-common-python-mistakes-you-should-avoid-as-a-beginner-bd28feb6162b`](https://towardsdatascience.com/4-common-python-mistakes-you-should-avoid-as-a-beginner-bd28feb6162b)

## 以及如何在不小心破坏面试机会之前纠正自己。

[](https://murtaza5152-ali.medium.com/?source=post_page-----bd28feb6162b--------------------------------)![Murtaza Ali](https://murtaza5152-ali.medium.com/?source=post_page-----bd28feb6162b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bd28feb6162b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----bd28feb6162b--------------------------------) [Murtaza Ali](https://murtaza5152-ali.medium.com/?source=post_page-----bd28feb6162b--------------------------------)

·发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----bd28feb6162b--------------------------------) ·阅读时长 7 分钟·2023 年 1 月 13 日

--

![](img/6b371b75bddaaafc03c5b79151b4e80e.png)

照片由[David Pupaza](https://unsplash.com/@dav420?utm_source=medium&utm_medium=referral)提供，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Python 是一个很适合初学者的语言，但这并不意味着不会犯错误。特别是在学习编程的早期阶段，容易编写出技术上正确但风格上欠佳的代码。

如果你打算学习编程，那么学会如何编写高质量的代码至关重要。无论是在学术界还是工业界，代码的质量都很重要。它不仅影响到你自己，还影响到每一个将要阅读和使用你代码的人。更自私一点说，它还影响到你的招聘前景。

在本文中，我将讨论初学 Python 程序员常犯的四个错误。学习这些陷阱对我早期学习 Python 非常有帮助，希望对你也同样有用。

我们开始吧。

## 传统布尔条件

这是初学者程序员常犯的一个错误。这也是那些没有正式编程背景的程序员所犯的错误，因为他们只是将代码作为工具使用。我在说你们，数据科学家们。

Python 中的条件语句很有用，但并非总是必要的。特别是当你检查的条件已经包含布尔值（True 或 False）时，更是如此。

让我用一个简单的例子来说明。假设我们想编写代码来确定数据集是否已经被清理。幸运的是，代码库中包含一个名为`is_data_clean`的方便变量来跟踪这一点。我们只需检查它并返回正确的值即可。

作为第一次尝试，我们可能会写如下内容：

```py
def a_function():
    if is_data_clean == True:
        return True
    else:
        return False
```

这方法够用了，但不必要地复杂。你看到问题了吗？仔细看看。

变量`is_data_clean`已经是布尔值；因此，它已经包含了你需要返回的值！代码检查它是否为`True`，如果是，则返回`True`，如果不是`True`（即`False`），则代码返回`False`。这只是一些不必要的检查。

我们可以将函数中的代码简化为一行：

```py
def a_function():
    return is_data_clean
```

好得多。

## 手动计算总和、均值或其他内置操作

Python 具有比大多数人意识到的更多的内置功能。仍然使用循环手动计算总和的人数太多了。

如果我们在 Python 中有一组数字，我们绝对**不**应该这样计算总和：

```py
total = 0
for num in numbers_list:
    total += num
```

改用内置的`sum`函数：

```py
total = sum(numbers_list)
```

需要最小值还是最大值？宇宙不允许你这样写：

```py
import math
minimum = math.inf # start at highest possible value
for number in numbers_list:
    if number < minimum:
        minimum = number
```

这不是一门计算机科学原理入门课；这是现实世界。停止重新发明轮子，使用内置的`min`和`max`函数：

```py
minimum = min(numbers_list)
maximum = max(numbers_list)
```

要查看完整的内置函数列表，请参见[Python 文档](https://docs.python.org/3/library/functions.html) [1]。

**额外奖励：内置的函数虽然*从技术上讲* 不是内置的。**

有些函数较难找到，但这并不意味着你不应该找到它们。

例如，如果我们需要一组数字的均值（你可能会感受到这里的重复主题），我们*可以*使用下面的第一个代码片段，但我们*应该*使用第二个：

```py
# Snippet 1: Don't do this!
total = 0
for num in numbers_list:
    total += num
avg = total / len(numbers_list)

# Snippet 2: Do this!
import numpy as np
avg = np.mean(numbers_list)
```

通常，Python 提供了有用的函数，这些函数在模块中。找到我们需要的模块并导入函数可能需要一点额外的工作，但结果代码非常值得。

记住——Python 的核心是简单性和可读性。内置函数是你的朋友。与人类朋友不同，它们不会让你失望。

## 做点什么以便什么都不做

在我教授的某一门 Python 入门课程中，学生的第一个项目是编写一个简单的决策算法。这主要是一个条件判断的练习，要求学生定义一个问题和相关的评分系统，以确定某人是否符合该问题的条件。

例如，有人可能会问，“我应该成为数据科学家吗？”那么，算法可能会包含以下问题，所有这些问题都会根据答案对最终的输出分数进行加减：

+   我是否有兴趣使用数据来获取对世界的洞察？

+   我是否愿意学习 Python？

+   我是否喜欢与跨学科团队合作？

诸如此类。

在编写算法的过程中，许多学生意识到在某些情况下，他们只是希望对整体分数不做任何修改。例如，他们可能决定，如果有人愿意学习 Python，那会给整体分数加 10 分，但如果他们不愿意，则分数保持不变。

大多数学生用以下代码实现这一点：

```py
# willing_to_learn is some predefined variable based on user input
if willing_to_learn:
    score += 10
else:
    score += 0
```

这是一个经典的*做某事却什么也没做*的案例。让我们拆解 Python 在看到代码行`score += 0`时需要做的所有事情：

+   它需要查找变量`score`的值。

+   它需要在这个值上加上 0。这需要调用加法函数，传入两个参数（当前值和 0），并计算输出。

+   重新将`score`变量赋值为新值（显然，这个值是相同的）。

所有这些工作却做了……什么都没有。

当然，这对计算机来说并不是*大量*工作，也不会对代码的效率产生任何实际影响。不过，它是无用的且有些不干净，这在优秀的 Python 代码中是不常见的。

一个更好的解决方案是使用 Python 的`pass`关键字，它字面上告诉 Python 什么也不做，只是继续。它填补了一行代码，这一行代码本不需要存在，但如果完全空着会导致错误。我们甚至可以添加一个小注释以提供进一步的清晰度：

```py
if willing_to_learn:
    score += 10
else:
    pass # Leave score unchanged
```

更简洁、更清晰、更符合 Python 风格。

## 单个条件失控

条件语句可以说是标准编程中更强大和一致的构造之一。当第一次学习它时，很容易忽略一个重要的细微之处。

当我们需要检查两个或更多条件时，这个细微之处就会出现。例如，假设我们正在审查一个调查，调查结果有三种形式：“是”，“否”或“也许”。

早期的 Python 程序员通常会以两种方式之一来编写这个代码：

```py
# Possibility 1
if response == "Yes":
    # do something
if response == "No":
    # do something
if response == "Maybe":
    # do something

# Possibility 2
if response == "Yes":
    # do something
elif response == "No":
    # do something
else:
    # do something
```

在这种情况下，这两个代码片段实际上是一样的。它们的行为相同，理解起来并不特别困难，并且实现了预期的目标。问题在于，当人们错误地认为上述两个结构*总是*等同的时候。

这是错误的。上面的第二个代码片段是由多个部分组成的*单一*条件表达式，而第一个代码片段则包含了*三个独立*的条件表达式，尽管它们看起来是相互连接的。

为什么这很重要？因为每当 Python 遇到一个全新的`if`关键字（即，新的条件表达式开始时），它都会检查相关的条件。另一方面，Python 仅会进入`elif`或`else`条件，如果当前条件表达式中的所有先前条件都未被满足。

让我们看一个例子，看看这为什么重要。假设我们需要编写代码，根据学生在某个作业中的分数来分配字母等级。我们在 Python 文件中编写以下代码：

```py
score = 76

print("SNIPPET 1")
print()

if score < 100:
    print('A')
elif score < 90:
    print('B')
elif score < 80:
    print('C')
elif score < 70:
    print('D')
else:
    print('F')

print()
print("SNIPPET 2")
print()

if score < 100:
    print('A')
if score < 90:
    print('B')
if score < 80:
    print('C')
if score < 70:
    print('D')
if score < 60:
    print('F')
```

运行这段代码会输出以下内容：

```py
SNIPPET 1

A

SNIPPET 2

A
B
C
```

你看到区别了吗？在第二种情况下，我们得到了意外的输出。为什么？因为 Python 将每个`if`语句视为全新的条件，因此如果一个分数恰好小于多个数字检查，那么对应的字母等级会被输出所有这些检查的结果。

现在，有方法可以让多个`if`语句工作；例如，我们可以让条件检查一个范围，而不仅仅是一个上限。这个例子的重点不是争论一个示例优于另一个示例（虽然我个人倾向于使用`elif`和`else`以提高清晰度），而只是为了说明它们并不是*相同的*。

确保你理解这一点。

## 最后的思考与总结

这是你的 Python 初学者备忘单：

1.  当你可以直接返回布尔值时，不要为布尔值创建不必要的条件语句。

1.  内置函数是你最好的朋友。

1.  如果你需要告诉 Python 什么都不做，使用`pass`关键字。

1.  确保你正确构建条件表达式，理解`if`、`elif`和`else`关键字的含义。

你决定学习 Python 是很棒的——我向你保证，这门语言会对你很好。

只要记得回报它们。

**想在 Python 中表现出色？** [**点击这里获取独家、免费的简易指南**](https://witty-speaker-6901.ck.page/0977670a91)**。想在 Medium 上阅读无限故事？使用下面的推荐链接注册！**

[阅读 Murtaza Ali 的文章](https://murtaza5152-ali.medium.com/?source=post_page-----bd28feb6162b--------------------------------) [## Murtaza Ali - Medium

### 阅读 Murtaza Ali 在 Medium 上的文章。华盛顿大学的博士生。对人机交互感兴趣…

[murtaza5152-ali.medium.com](https://murtaza5152-ali.medium.com/?source=post_page-----bd28feb6162b--------------------------------)

## 参考资料

[1] [`docs.python.org/3/library/functions.html`](https://docs.python.org/3/library/functions.html)
