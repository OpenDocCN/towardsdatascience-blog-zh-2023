# 5 个区分资深开发者和初级开发者的 Python 技巧

> 原文：[`towardsdatascience.com/5-python-tricks-that-distinguish-senior-developers-from-juniors-826d57ab3940`](https://towardsdatascience.com/5-python-tricks-that-distinguish-senior-developers-from-juniors-826d57ab3940)

## 通过对 Advent of Code 难题解决方法的差异进行说明

[](https://medium.com/@tomergabay?source=post_page-----826d57ab3940--------------------------------)![Tomer Gabay](https://medium.com/@tomergabay?source=post_page-----826d57ab3940--------------------------------)[](https://towardsdatascience.com/?source=post_page-----826d57ab3940--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----826d57ab3940--------------------------------) [Tomer Gabay](https://medium.com/@tomergabay?source=post_page-----826d57ab3940--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----826d57ab3940--------------------------------) ·阅读时间 6 分钟·2023 年 1 月 16 日

--

![](img/0d25891630ba9198b83532dc75587a05.png)

由[Afif Ramdhasuma](https://unsplash.com/@javaistan?utm_source=medium&utm_medium=referral)拍摄的照片，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

自 2015 年起，每年 12 月 1 日，[Advent of Code](https://adventofcode.com/)开始。正如他们网站上所描述的，Advent of Code（以下简称 AoC）是

> 一个[降临节日历](https://en.wikipedia.org/wiki/Advent_calendar)，包含各种技能水平的小编程难题，可以用[任何](https://github.com/search?q=advent+of+code)编程语言解决。人们将其用作[面试](https://y3l2n.com/2018/05/09/interview-prep-advent-of-code/) [准备](https://twitter.com/dznqbit/status/1037607793144938497)、[公司培训](https://twitter.com/pgoultiaev/status/950805811583963137)、[大学](https://gitlab.com/imhoffman/fa19b4-mat3006/wikis/home) [课程作业](https://gribblelab.org/teaching/scicomp2021/index.html)、[练习](https://twitter.com/mrdanielklein/status/936267621468483584) [问题](https://comp215.blogs.rice.edu/)、[速度竞赛](https://adventofcode.com/leaderboard)或[相互挑战](https://www.reddit.com/r/adventofcode/search?q=flair%3Aupping&restrict_sr=on)。

在这篇文章中，我们将探讨五种高级解决常见编码问题的方法，而不是初级方法。每个编码问题都来源于 AoC 难题，许多问题在 AoC 和其他编码挑战及评估中反复出现，比如在求职面试中。

为了阐明这些概念，我不会详细讲解完整的 AoC 难题解决方案，而是只专注于某个具体难题的一小部分，在这个难题中，资深开发者和初级开发者的差异非常明显。

# 1\. 有效地使用推导式和分割读取文件

在 [Day1](https://adventofcode.com/2022/day/1) 中，需要读取几个数字块。每个块之间由一个空行分隔（因此实际上是 `'\n’`）。

**输入和期望输出**

```py
# INPUT
10
20
30

50
60
70

# DESIRED OUTPUT
[[10, 20, 30], [50, 60 70]]
```

**初级开发者方法：** 使用 if-else 语句的循环

```py
numbers = []
with open("file.txt") as f:
  group = []
  for line in f:
    if line == "\n":
      numbers.append(group)
      group = []
    else:
      group.append(int(line.rstrip()))
  # append the last group because if line == "\n" will not be True for
  # the last group
  numbers.append(group)
```

**高级开发者方法：** 利用列表推导式和 `.split()`

```py
with open("file.txt") as f:
  # split input into groups based on empty lines
  groups = f.read().rstrip().split("\n\n")
  # convert all the values in the groups into integers
  nums = [list(map(int, (group.split()))) for group in groups]
```

使用列表推导式，我们可以将之前的 10 行代码压缩成两行，而不会显著损失（如果有的话）可理解性或可读性，并且性能有所提升（[列表推导式比常规循环更快](https://wiki.python.org/moin/PythonSpeed/PerformanceTips#Loops)）。对于那些未曾见过 `map` 的人，`map` 将一个函数（第一个参数）应用于第二个参数中的可迭代对象。在这种特定情况下，它将 `int()` 应用到列表中的每个值，使每个项目变成整数。有关 `map` 的更多信息，请点击 [这里](https://www.geeksforgeeks.org/python-map-function/)。

# 2\. 使用 Enum 代替 if-elif-else

在 [Day2](https://adventofcode.com/2022/day/1) 中，挑战围绕一个 *石头-剪刀-布* 游戏展开。不同选择的形状（石头、纸张或剪刀）会得到不同的点数：1 (*X)*，2 (*Y)* 和 3 (*Z)*。以下是解决此问题的两种方法。

**输入和期望输出**

```py
# INPUT
X
Y
Z

# DESIRED OUTPUT
1
2
3
```

**初级开发者方法：** if-elif-else

```py
def points_per_shape(shape: str) -> int:
  if shape == 'X':
    return 1
  elif shape == 'Y':
    return 2
  elif shape == 'Z':
    return 3
  else:
    raise ValueError('Invalid shape')
```

**高级开发者方法：** `Enum`

```py
from enum import Enum

class ShapePoints(Enum):
  X = 1
  Y = 2
  Z = 3

def points_per_shape(shape: str) -> int:
  return ShapePoints[shape].value
```

当然，在这个例子中，简单的方法并不是 *那* 糟糕，但使用 `[Enum](https://docs.python.org/3/library/enum.html)` 会导致更简洁和可读性更高的代码。特别是当选项更多时，简单的 *if-elif-else* 方法会变得越来越差，而使用 `Enum` 则相对容易保持概览。有关 `Enum` 的更多信息，请点击 [这里](https://docs.python.org/3/library/enum.html)。

# 3\. 使用查找表代替字典

在 [Day3](https://adventofcode.com/2022/day/3) 中，字母具有不同的值。小写字母 a-z 的值为 1 到 26，大写字母 A-Z 的值为 27 到 52。由于可能的值很多，像上述那样使用 `Enum` 会导致很多行代码。这里更实用的方法是使用 *查找表：*

```py
# INPUT
c
Z
a
...

# DESIRED OUPUT
3
52
1
...
```

**初级开发者方法：** 创建一个全局字典

```py
letters = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ'
letter_dict = dict()
for value, letter in enumerate(letters, start=1):
  letter_dict[letter] = value

def letter_value(ltr: str) -> int:
  return letter_dict[ltr]
```

**高级开发者方法：** 使用字符串作为查找表

```py
def letter_value(ltr: str) -> int
  return 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ'.index(ltr) + 1
```

使用字符串的 `.index()` 方法，我们可以获得索引，因此 `letters.index('c')+1` 将得到期望的值 3。没有必要将值存储在字典中，因为索引 *就是* 值。为了避免 `+1`，你可以在字符串的开头添加一个空格字符，使得 `a` 的索引从 1 开始。然而，这取决于你是否希望返回空格的值为 0 还是错误。

正如你现在可能想到的，是的，我们也可以使用查找表来解决 *石头、剪刀、布* 任务：

```py
def points_per_shape(shape: str) -> int:
  return 'XYZ'.index(shape) + 1
```

# 4\. 高级切片

在 [Day5](https://adventofcode.com/2022/day/5) 中，需要从行中读取字母（见下方输入）。每个字母在第四个索引上，从索引 1 开始。现在，几乎所有 Python 程序员都熟悉使用例如 `list_[10:20]` 的字符串和列表切片。但很多人不知道，你可以使用例如 `list_[10:20:2]` 来定义步长为 2。在 Day5（以及许多其他编码场景中），这可以节省你大量不必要的复杂代码：

```py
# INPUT
    [D]    
[N] [C]    
[Z] [M] [P]

# DESIRED OUTPUT
[' D ', 'NC', 'ZMP']
```

**初级开发者方法：** 使用 `range` 和索引的双重循环

```py
letters = []
with open('input.txt') as f:
  for line in f:
    row = ''
    for index in range(1, len(line), 4):
      row += line[index]
    letters.append(row)
```

**高级开发者方法：** 使用高级切片方法

```py
with open('input.txt') as f:
  letters = [line[1::4] for line in f]
```

# 5\. 使用类属性存储类实例

在 [Day11](https://adventofcode.com/2022/day/11) 中描述了一种猴子互相传递物品的情况。为了简化，我们假设它们只是互相传递香蕉。每只猴子可以被表示为一个 Python `class` 的实例，其 `id` 和香蕉数量作为实例属性。然而，有很多猴子，它们需要能够互相交互。存储所有猴子并让它们能够互相交互的一个技巧是定义一个包含所有 `Monkey` 实例的字典，将其作为 `Monkey` 类的类属性。使用 `Monkey.monkeys[id]`，你可以访问所有现有的猴子，而不需要 `Monkies` 类或外部字典：

```py
class Monkey:
  monkeys: dict = dict()

  def __init__(self, id: int):
      self.id = id
      self.bananas = 3
      Monkey.monkeys[id] = self

  def pass_banana(self, to_id: int):
      Monkey.monkeys[to_id].bananas += 1
      self.bananas -= 1

Monkey(1)
Monkey(2)
Monkey.monkeys[1].pass_banana(to_id=2)

print(Monkey.monkeys[1].bananas)
2

print(Monkey.monkeys[2].bananas)
4
```

# 6\. 自我文档化表达式（额外奖励）

这个技巧几乎在每次编写 Python 程序时都适用。与其在 f-string 中定义你正在打印的内容（例如

`print(f"x = {x}")` 你可以使用 `print(f"{x = }”)` 来打印值，并指定你正在打印的内容。

```py
# INPUT
x = 10 * 2
y = 3 * 7

max(x,y)

# DESIRED OUTPUT
x = 20
y = 21

max(x,y) = 21
```

**初级开发者方法：**

```py
print(f"x = {x}")
print(f"y = {y}")

print(f"max(x,y) = {max(x,y)}")
```

**高级开发者方法：**

```py
print(f"{x = }")
print(f"{y = }")

print(f"{max(x,y) = }")
```

# 总结

我们已经探讨了 5 个 Python 技巧，这些技巧区分了高级开发者和初级开发者。当然，单单应用这些技巧并不能让人瞬间晋升为高级开发者。然而，通过分析这两者之间风格和模式的差异，你可以学习高级开发者与初级开发者在处理编码问题时的方法差异，并开始内化这些方法，以便最终自己成为高级开发者！

如果你喜欢这篇文章并希望了解更多关于高级 Python 方法的信息，请务必阅读我关于如何找到内容以提升自己成为更高级开发者的另一篇文章！

[](/how-to-level-up-your-python-skills-by-learning-from-these-professionals-3e906b83f355?source=post_page-----826d57ab3940--------------------------------) ## 如何通过向这些专业人士学习提升你的 Python 技能

### 避免停留在 Python 编程的初级水平

towardsdatascience.com

# 资源

+   [列表推导](https://wiki.python.org/moin/PythonSpeed/PerformanceTips#Loops)

+   [映射解释](https://www.geeksforgeeks.org/python-map-function/)

+   [枚举文档](https://docs.python.org/3/library/enum.html)

+   [AoC Reddit](https://www.reddit.com/r/adventofcode/)（提示、解决方案和讨论）
