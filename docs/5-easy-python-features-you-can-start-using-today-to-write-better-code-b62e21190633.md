# 5 个简单的 Python 特性，你可以立即开始使用以编写更好的代码

> 原文：[`towardsdatascience.com/5-easy-python-features-you-can-start-using-today-to-write-better-code-b62e21190633`](https://towardsdatascience.com/5-easy-python-features-you-can-start-using-today-to-write-better-code-b62e21190633)

## 我使用 Python 已经超过 8 年了。以下是我喜欢的一些 Python 特性，它们能使你的代码焕然一新且高效。

[](https://thushv89.medium.com/?source=post_page-----b62e21190633--------------------------------)![Thushan Ganegedara](https://thushv89.medium.com/?source=post_page-----b62e21190633--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b62e21190633--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b62e21190633--------------------------------) [Thushan Ganegedara](https://thushv89.medium.com/?source=post_page-----b62e21190633--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b62e21190633--------------------------------) ·阅读时间 7 分钟·2023 年 7 月 19 日

--

![](img/41822c8a9c950dbeb77a5353e33dc731.png)

图片由 [Chris Ried](https://unsplash.com/@cdr6934?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，发布在 [Unsplash](https://unsplash.com/photos/ieic5Tq8YMk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

你必须承认，看到代码或拉取请求上出现类似“*这真是超级干净* 😎”或“*没想到可以这样做*”的评论，会让你感到非常愉悦。个人经验告诉我，拥抱良好的软件工程原则并充分利用现有的语言功能，是编写出别人会感激的好代码的秘诀。

作为一名 MLE，我每天都在使用 Python。由于其低门槛和庞大的科学工具生态系统，Python 是机器学习从业者的一个极佳选择。

> 这意味着，几乎没有软件工程知识的个人也可以快速开始使用 Python。

这最后一句话可以用两种不同的语气来表达：积极的或消极的（试试看！）。

起初这可能看起来是个福音，但从整体来看，缺乏软件工程原则（例如类型、对象）的约束使工程师（MLE）或科学家（DS/AS）不愿编写良好的代码（相信我，我们在软件工程师中已经有了不太好的声誉）。这不可避免地会导致大多数情况下的代码不可读、不可维护和无法测试。而更糟糕的是，有一天它会成为某个毫无防备的受害者最糟糕的噩梦，因为要重复使用这段恶劣的代码。你可能还会看到一种多米诺效应，即在糟糕代码之上构建的代码会导致……更多糟糕的代码。最终，这甚至可能会导致组织上的头痛。

总而言之，在 Python 中做事情很简单，但以正确的方式做事情却很困难。在与 Python 搅斗了 8 年之后，我仍在学习不同（和更好）的方式来改善我的代码。我很幸运有好的软件工程师会建设性地批评我的代码，当我以低效的方式做事时。如果你有同样的支持，那是你的幸运。在这里，我将分享一些可以提升你 Python 技能的方法。

# 1\. 数据类有助于清除杂乱

比如你想管理一个学生及其身高的列表。你可以使用元组的列表来做到这一点。

```py
students = [("Jack", 168), ("Zhou", 172), ("Emma", 165), ("Shan", 170)]
```

但如果你后来想添加其他属性，如体重、成绩和性别怎么办？如果不头痛并犯很多错误，你无法使用上述数据结构。你可以使用字典，但仍然显得笨重。更好的解决方案是使用 `dataclasses`。

```py
import dataclasses

@dataclasses.dataclass
class Student:
    name: str
    height: int
```

这样清爽多了！然后你只需实例化一堆 `Student` 对象。

```py
students = [
    Student(name=“Jack”, height=168), 
    Student(name=“Zhou”, height=172), 
    Student(name=“Emma”, height=165), 
    Student(name=“Shan”, height=170)
]
```

然后，你可以使用类似 `students[0].name` 的语法来访问属性。无需再依赖模糊的知识，例如名称在 `0th` 位置，或使用容易出错的字符串键（如果你使用字典的话）。你可以使用 `dataclasses` 做很多其他很酷的事情。例如，

+   使对象不可变（通过使用 `@dataclasses.dataclass(frozen=True)`）

+   定义 getter 和 setter

+   使用 `dataclasses.field` 为属性提供额外支持

+   将对象转换为字典（`.asdict()`）或元组（`.astuple()`）以进行序列化和兼容性处理。

你可以在 [这里](https://docs.python.org/3/library/dataclasses.html) 阅读更多关于 `dataclasses` 的内容。

# 2\. 使用风格在 Python 中进行比较

减少和排序是任何机器学习项目中一个重要的部分。你可能会在简单数据类型的列表（例如 `str`、`float` 等）上使用 `min`、`max` 或 `sorted` 函数。但你是否知道，有一个巧妙的技巧可以扩展这些基本函数所能解决的问题的范围？

你可以使用 `min`、`max` 和 `sorted` 创造性地解决问题，通过一个名为 `key` 的特殊参数。这个键允许你定义逻辑，从可迭代对象中的每个项目中提取一个“*比较键*”。

比如你想要排序以下的学生身高列表，

```py
students = [("Jack", 168), ("Zhou", 172), ("Emma", 165), ("Shan", 170)]
sorted_heights = sorted(students, key=lambda x: x[1])
```

更酷的是，假设你有一个 `dataclasses.dataclass` 而不是这个。

```py
sorted_heights = sorted(students, key=lambda x: x.height)
```

看起来很酷。也许你想找出身高最高的学生。

```py
tallest_student = max(students, key=lambda x: x.height).name
```

你知道吗，你甚至可以用纯 Python 模拟 `argmax` 操作？对于那些不知道的人，`argmax` 给出列表/数组中最大值的索引。这在很多算法中都是一种强制计算。

```py
a = [3, 4, 5, 2, 1]
max_idx = max(enumerate(a), key=lambda x: x[1])[0]
```

我曾多次遇到过这样的问题：写了很多行代码，而这些代码本来可以通过更关注关键点来完成。

# 3\. 让 defaultdict 成为你的默认选择

使用字典时，有一个方便的变体可能会让你的生活更轻松。假设你要管理 3 年内股票价格的变化。假设原始格式如下。

```py
stock_prices = [
    ("abc", 95), ("foo", 20), ("abc", 100), 
    ("abc", 110), ("foo", 18), ("foo", 25)
]
```

你还想把这个转换为字典。你可以这样做：

```py
stock_price_dict = {}
for code, price in stock_prices:
  if code not in stock_price_dict:
    stock_price_dict[code] = [price]
  else:
    stock_price_dict[code].append(price)
```

这段代码确实完成了任务。但这里有一个使用 `defaultdict` 的更优雅版本。

```py
from collections import defaultdict

stock_price_dict = defaultdict(list)
for code, price in stock_prices:
  stock_price_dict[code].append(price)
```

哇，代码这样会更简洁。无需再担心值是否已被实例化。这真的显示了你对 Python 和数据结构的了解。

# 4\. 对 itertools 说“我愿意”

`itertools` 是一个内置的 Python 库，用于轻松地对数据结构进行高级迭代。

你可能在生活中遇到过需要迭代多个列表以创建一个单一列表的情况。在 Python 中你可能会这样做：

```py
student_list = [["Jack", "Mary"], ["Zhou", "Shan"], ["Emma", "Deepti"]]
all_students = []
for students in student_list:
  all_students.extend(students)
```

你相信吗，用 `itertools` 这只需一行代码？

```py
import itertools
all_students = list(itertools.chain.from_iterables(student_list))
```

比如你想要移除身高低于 170cm 的学生。使用 `itertools` 这也是一行代码。

```py
students = [
    Student(name="Jack", height=168), 
    Student(name="Zhou", height=172), 
    Student(name="Emma", height=165), 
    Student(name="Shan", height=170)
]

above_170_students = list(itertools.dropwhile(lambda s: s.height<170, students))
```

还有许多其他有用的函数，比如 `accumulate`、`islice`、`starmap` 等。你可以在[这里](https://docs.python.org/3/library/itertools.html)查看更多。使用 `itertools`，而不是重新发明轮子，这样可以避免冗长和低效的代码。通过使用 `itertools`，你还能享受到速度上的优势，因为它在底层有高效的 CPython 实现。

# 5\. 打包/解包参数

打包和解包是通过星号 (`*`) 和双星号 (`**`) 操作符实现的。理解这一概念最简单的方法是使用函数。你可以定义一个带有打包参数或解包参数的函数。假设我们定义以下两个函数：

```py
# f1 unpacks arguments
def f1(a: str, b: str, c: str):
    return " ".join([a,b,c])

# f1 packs all arguments to args
def f2(*args):
    return " ".join(args)
```

在调用 `f1` 时你只能传递 3 个参数，而 `f2` 可以接受任意数量的参数，这些参数被打包成一个元组 `args`。所以你会这样调用：

```py
f1("I", "love", "Python")
f2("I", "love", "Python")
f2("I", "love", "Python", "and", "argument", "packing")
```

这些都可以工作。如果你想要一个字典，其中键是参数，值是传递的参数，你可以使用双星号操作符。

```py
def f3(**kwargs):
    return " ".join([f"{k}={v}" for k,v in kwargs.items()])
```

要看看这有多好，这里是写 `f2` 的替代方案：

```py
def f2(text_list: list[str]):
    return " ".join(text_list)

f2(("I", "love", "Python", "..."))
```

那双重括号已经让我不寒而栗了！使用 `*args` 甜美得多。

`zip()` 是一个实际的函数，它接受任意数量的可迭代对象。它通过取出每个可迭代对象的第一个项、第二个项，以此类推，创建几个新的列表。对于以下示例，`zip()` 让你可以在两种格式之间互换；

```py
[("a1", "b1"), ("a2", "b2"), ("a3", "b3")] # Format 1
<->
[("a1", "a2", "a3"), ("b1", "b2", "b3")] # Format 2
```

当你处理数据时，这是一种意外常见的需求。记得我们上面的学生示例吗？假设我们只需要排序后的学生姓名列表。我们可以简单地将元组 zip 成两个列表。

```py
students = [("Jack", 168), ("Zhou", 172), ("Emma": 165), ("Shan", 170)]
sorted_students, _ = zip(*sorted(students, key=lambda x: x[1]))
```

这就是将数据从格式 1 布局到格式 2，并丢弃第二个列表（因为它将包含所有身高数据）。

如果你正在开发一个需要处理任意数量参数的函数，可以使用参数打包。这比传递一个元组或字典要优雅得多。

# 结论

这些是你下次在 Python 中编写 ML 模型时可以做的 5 件不同的事。坚持良好的软件工程原则和语言标准为一组工程师和科学家提供了共同的基础，从而使他们能够协同工作并快速迭代。通过选择成为更好的软件工程师，你也将为你的同事照亮前进的道路，这将实现个人和团队之间无摩擦的合作。

如果你喜欢这个故事，欢迎[订阅](https://thushv89.medium.com/membership) Medium，你将获得来自我的新内容通知，并且解锁对其他作者成千上万精彩故事的完全访问。

[## 使用我的推荐链接加入 Medium - Thushan Ganegedara](https://thushv89.medium.com/membership?source=post_page-----b62e21190633--------------------------------)

### 作为 Medium 的会员，你的部分会员费将直接支持你阅读的作者，同时你也可以完全访问每一个故事……

[thushv89.medium.com](https://thushv89.medium.com/membership?source=post_page-----b62e21190633--------------------------------)
