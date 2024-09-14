# 两个强大的 Python 特性，以简化你的代码并提高可读性

> 原文：[`towardsdatascience.com/two-powerful-python-features-to-streamline-your-code-and-make-it-more-readable-51240f11d1a`](https://towardsdatascience.com/two-powerful-python-features-to-streamline-your-code-and-make-it-more-readable-51240f11d1a)

## 通过匹配语句和对象切片提升你的代码质量。

[](https://murtaza5152-ali.medium.com/?source=post_page-----51240f11d1a--------------------------------)![Murtaza Ali](https://murtaza5152-ali.medium.com/?source=post_page-----51240f11d1a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----51240f11d1a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----51240f11d1a--------------------------------) [Murtaza Ali](https://murtaza5152-ali.medium.com/?source=post_page-----51240f11d1a--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----51240f11d1a--------------------------------) ·8 分钟阅读·2023 年 9 月 29 日

--

![](img/4c2222582de9c851f5249d9cc8877197.png)

照片由 [Kevin Ku](https://unsplash.com/@ikukevk?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Python 在当前技术领域的流行程度如此广泛是有原因的。在现代编程语言中，它可能是对新手最为友好的。凭借这种可访问性，它也提供了强大的功能。网页开发、数据科学、科学计算——你可以用 Python 完成许多任务。

随着 Python 多年的发展，其开发者们付出了巨大的努力，以保持其可读性和简洁性。尽管许多特性可能需要额外的学习努力，但代码的清晰度和美观度是绝对值得的。

在这篇文章中，我们将深入探讨两个这样的特性：匹配语句和字符串/列表切片。我们将详细介绍每个特性的工作原理，并考虑一些示例，以加深对语法和语义的理解。

现在，让我们深入探讨一下吧。

## 匹配语句

匹配语句——从 Python 3.10 版本开始可用——是一种检查条件的相等性并根据条件执行某些操作的方法[1]。如果你来自其他语言如 C 或 JavaScript，你可能已经熟悉这种概念，它们被称为*switch* 语句。

原则上，匹配语句类似于条件语句，但它们确实提供了一些有用的优势。让我们首先通过与条件语句的比较来看其基本结构，然后再讨论这些优势。

你可能会写出以下条件语句来检查某人的银行账户名称：

```py
name = "Yen"

if name == "Yen":
    print("This is your account.")
elif name == "Ben":
    print("This is your sister's account.")
else:
    print("Fraud attempt.")
```

转换为匹配语句后，它将如下所示：

```py
name = "Yen"

match name:
    case "Yen":
        print("This is your account.")
    case "Ben":
        print("This is your sister's account.")
    case _:
        print("Fraud attempt.")
```

让我们逐行分析：

+   第一行是一样的——我们只是定义了 `name` 变量。

+   关键字 `match` 用于启动匹配语句。

+   然后，对于每个条件，我们使用 `case` 语句来有效地进行模式匹配，而不是明确地检查等式。因此，你可以把 `case "Yen"` 看作是在检查 `name`，即我们正在匹配的内容，是否等于 `"Yen"`。

+   最后，最后一个 case 是通配符 case。它由下划线（`_`）指定，实际上是 `else` 情况。

现在，你可能会问——为什么使用这个而不是传统的条件语句？我最初也有同样的问题，甚至对人们使用匹配语句而不是标准的 if-else 语句感到恼火。然而，确实有一些优势。

第一件事是，它是实现相同目标的一种更简洁的方法。这看起来可能是个托词，但实际上相当重要。Python 的整个精神在于编写干净、简洁的代码（如果你不相信我，试着在你的 Python 解释器中输入`import this`并按下回车）。

尤其是当条件数量较多时，解析长链的 `if` 和 `elif` 语句可能会很繁琐。使用匹配语句可以清理代码，并使同事程序员更容易阅读——这是任何 Python 程序员值得追求的成就。

除此之外，匹配语句还可以直接解构某些对象，从而不再需要手动使用条件语句。实际上，这意味着两件事：

+   你可以自动检查类型（省去了手动检查的需要）。

+   你可以在每个 `case` 中自动访问对象的属性。

让我们看一个例子。假设我们有以下代码，定义了两种不同类型的汽车类：

```py
# Online Python compiler (interpreter) to run Python online.
# Write Python 3 code in this online editor and run it.
class Honda:
    # See below for explanation of __match_args__
    __match_args__ = ("year", "model", "cost")
    def __init__(self, year, model, cost):
        self.year = year
        self.model = model
        self.cost = cost

class Subaru:
    __match_args__ = ("year", "model", "cost")
    def __init__(self, year, model, cost):
        self.year = year
        self.model = model
        self.cost = cost

car = Subaru(2021, "Outback", 18000) 
```

我们在上面定义了一个 `Subaru` 实例。现在，我们想编写代码来检查汽车的类型，并打印出其某些属性。使用传统的条件语句，我们可以这样做：

```py
if isinstance(car, Honda):
    print("Honda " + car.model)
elif isinstance(car, Subaru):
    print("Subaru " + car.model)
else:
    print("Failure :(")
```

对于我们上面的 `car` 变量，这将打印出 `"Subaru Outback"`。如果我们将其转换为匹配语句，将得到以下简化的代码：

```py
match car:
    case Honda(year, model, cost):
        print("Honda " + model)
    case Subaru(year, model, cost):
        print("Subaru " + model)
    case _:
        print("Failure")
```

匹配的模式匹配功能使 Python 能够在 `case` 语句中自动检查类型，并进一步使对象的属性可以直接访问。注意，这得益于在类定义中包含 `__match_args__` 属性，它为 Python 命名了位置参数。Python 文档中的推荐是使模式在为 `self` 分配属性时模拟 `__init__` 构造函数中使用的模式。

匹配版本的代码更容易阅读且编写起来不那么繁琐。这是一个相当小的例子，但随着情况变得更加复杂，[条件语句的字符串可能变得越来越复杂](https://peps.python.org/pep-0622/#rationale-and-goals) [2]。

说到这一点，请记住，这一功能仅在 Python 3.10 及其之后的版本中可用。因此，你需要确保你编写代码的系统、应用程序或项目不会在需要兼容旧版 Python 的代码库中存在。

只要满足那个条件，就考虑使用 match 语句。虽然这可能需要一点努力，但从长远来看，你的代码将会更好。

## 字符串和列表切片

你可能对这个功能有一些了解，但我敢打赌你还没有完全发挥它的潜力。让我们从快速回顾开始，然后深入了解一些更复杂的用法。

在最简单的形式中，切片指的是一种简洁的语法，让你可以在 Python [3] 中提取字符串或列表的一部分。这里有一个小例子：

```py
>>> my_str = "hello"
>>> my_str[1:3]
'el'
```

语法要求使用包含起始和结束索引的方括号，并且索引之间用冒号分隔。请记住，Python 使用的是 0 索引，所以这里`1`对应于`'e'`。此外，切片不包括右索引，因此它到达`3`但**不**包括它，因此输出是`'el'`而不是`'ell'`。

如果你只想从开始处开始或一直到字符串或列表的末尾，你可以将相应的索引留空：

```py
>>> my_lst = ['apple', 'orange', 'blackcurrant', 'mango', 'pineapple']
>>> my_lst[:3]
['apple', 'orange', 'blackcurrant']
>>> my_lst[2:]
['blackcurrant', 'mango', 'pineapple']
```

留下两个索引为空会得到整个对象的副本：

```py
>>> my_str[:]
'hello'
>>> my_lst[:]
['apple', 'orange', 'blackcurrant', 'mango', 'pineapple']
```

注意，在列表和字符串中，切片定义并返回一个全新的对象，这个对象与原始对象不同：

```py
>>> new_lst = my_lst[2:]
>>> new_lst
['blackcurrant', 'mango', 'pineapple']
>>> my_lst
['apple', 'orange', 'blackcurrant', 'mango', 'pineapple']
```

现在，让我们进入重点。通过切片，你还可以使用负数索引。如果你不熟悉负数索引，它基本上允许你从列表或字符串的末尾开始计数。最后一个字母对应`-1`，倒数第二个字母对应`-2`，依此类推。

这可以通过省去手动计算长度来简化代码。例如，要获取字符串的所有内容但不包括最后一个字母，你可以这样做：

```py
>>> my_str[:-1]
'hell'
```

最后，切片最被忽视的功能之一是你还可以指定第三个数字——这指定了一种“跳跃”。用一个例子来解释最简单：

```py
>>> my_long_lst = ['apple', 'orange', 'blackcurrant', 'mango', 'pineapple', 'grapes', 'kiwi', 'papaya', 'coconut']
>>> my_long_lst[1:-1:2]
['orange', 'mango', 'grapes', 'papaya']
```

让我们分解一下上面的内容：

+   为了清楚起见，我们定义了一个包含更多元素的列表，而不是我们之前的原始列表。

+   在列表切片中，前两个数字是`1`和`-1`。正如我们上面看到的，这会去掉被切片对象——在这种情况下是`my_long_list`的第一个和最后一个元素。

+   最后，我们在额外的冒号后面放一个`2`作为最终数字。这告诉 Python 我们希望从开始到结束索引切片，但只保留每隔一个的项。放一个`3`会给我们每隔第三个项，放一个`4`会给我们每隔第四个项，依此类推。

将上述两点结合起来，我们还可以对列表进行切片以获得反向元素：

```py
>>> my_long_lst[-1:1:-2]
['coconut', 'kiwi', 'pineapple', 'blackcurrant']

# To slice backwars successfully, the "jump" value must be negative
# Otherwise, we just get an empty list
>>> my_long_lst[-1:1:2]
[]
```

这就是了——关于列表切片的所有知识。当你对上述语法进行创意应用时，可以实现一些非常酷的行为。例如，以下是利用列表切片在 Python 中反转列表的最妙方式之一：

```py
>>> my_lst
['apple', 'orange', 'blackcurrant', 'mango', 'pineapple']
>>> my_lst[::-1]
['pineapple', 'mango', 'blackcurrant', 'orange', 'apple']
```

你看到它是如何工作的了吗？作为一个练习，你应该复习上述列表切片的每个特性，并尝试自己分解代码。**提示：看看当我们留空开始和结束索引时意味着什么。**

那么，让我们谈谈为什么你应该学习这些内容。

## 作为数据科学家，这有什么用？

一般来说，在用 Python 编写代码时，考虑代码的可读性和整洁性很重要。使用上述特性将大有帮助。如我们所讨论的，匹配语句在这方面比条件语句有几个显著的优势。至于列表切片，它比尝试使用复杂循环实现相同行为要整洁得多。

但超越这些广泛的好处，让我们专门谈谈数据科学。

从实际角度看，如果你作为数据科学家工作，你的正式培训很可能不是计算机科学，而是统计学、数学，或者如果你有幸找到这样的项目，可能是数据科学本身。在这些项目中，计算机科学通常作为工具来教授。

重点是以一种教你足够知识以处理数据、进行分析和大规模构建模型的方式来学习编程基本原理。因此，没有大量时间去学习像“有用的 Python 特定语法特性”这样的主题。实际上，这些主题在纯计算机科学课程中也常常被忽视。

然而，使用这些特性可以将你的代码提升到一个新水平，帮助你在才华横溢的同事中脱颖而出，并为客户提供更好的结果。匹配语句和对象切片是两个强大的例子，但 Python 还有很多其他的特性可以提供，我鼓励你去探索。

愿代码永远对你有利——下次见，朋友们。

**想要在 Python 中脱颖而出？** [**点击这里获取我简单易读的独家免费指南**](https://witty-speaker-6901.ck.page/0977670a91)**。想在 Medium 上阅读无限故事？使用下面的推荐链接注册吧！**

[](https://medium.com/@murtaza5152-ali/membership?source=post_page-----51240f11d1a--------------------------------) [## 使用我的推荐链接加入 Medium - Murtaza Ali

### 作为 Medium 会员，你的会员费用的一部分将用于支持你阅读的作者，并且你可以完全访问每个故事……

[medium.com](https://medium.com/@murtaza5152-ali/membership?source=post_page-----51240f11d1a--------------------------------)

## 参考文献

[1] [`docs.python.org/3.10/whatsnew/3.10.html#syntax-and-operations`](https://docs.python.org/3.10/whatsnew/3.10.html#syntax-and-operations)

[2] [`peps.python.org/pep-0622/#rationale-and-goals`](https://peps.python.org/pep-0622/#rationale-and-goals)

[3] [`docs.python.org/3/c-api/slice.html`](https://docs.python.org/3/c-api/slice.html)
