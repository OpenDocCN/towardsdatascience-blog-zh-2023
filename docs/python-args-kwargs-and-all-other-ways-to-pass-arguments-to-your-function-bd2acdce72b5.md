# Python 中的 args、kwargs 以及传递参数给函数的所有其他方式

> 原文：[`towardsdatascience.com/python-args-kwargs-and-all-other-ways-to-pass-arguments-to-your-function-bd2acdce72b5`](https://towardsdatascience.com/python-args-kwargs-and-all-other-ways-to-pass-arguments-to-your-function-bd2acdce72b5)

## 在 6 个示例中巧妙设计你的函数参数

[](https://mikehuls.medium.com/?source=post_page-----bd2acdce72b5--------------------------------)![Mike Huls](https://mikehuls.medium.com/?source=post_page-----bd2acdce72b5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bd2acdce72b5--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----bd2acdce72b5--------------------------------) [Mike Huls](https://mikehuls.medium.com/?source=post_page-----bd2acdce72b5--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bd2acdce72b5--------------------------------) ·阅读时长 8 分钟·2023 年 3 月 7 日

--

![](img/232d65cc44f26ade0195ce94e644d795.png)

斜杠和星号的作用是什么？（作者提供的图片）

本文深入探讨了如何设计函数参数。我们将了解`*args`和`**kwargs`的作用，`/`和`*`的功能，以及如何以最佳方式设计函数参数。设计良好的函数参数更容易被其他开发者理解和使用。在本文中，我们探讨了**6 个问题**，这些问题展示了**你需要知道的一切**以**成为参数专家**。让我们开始编码吧！

# 准备工作：定义和传递参数

在这一部分，我们将快速了解术语和 Python 提供的所有处理参数传递的方法。

[](/six-levels-of-python-decorators-1f12c9067b23?source=post_page-----bd2acdce72b5--------------------------------) ## 理解 Python 装饰器：从初学者到专家的六个层次

### 装饰器如何工作，何时使用它们以及 6 个逐渐复杂的示例

towardsdatascience.com

## 参数和实参之间有什么区别？

许多人将这些术语互换使用，但它们之间是有区别的。参数是用参数提供的值初始化的：

+   **参数**是定义在函数定义中的名称

+   **实参**是传递给函数的值

![](img/9456d0abd2f46f9d3a8fb8e4818d04a2.png)

参数是红色的，实参是绿色的（作者提供的图片）

## 我可以通过哪两种方式传递参数？

你可以通过**位置**和**关键字**传递参数。在下面的示例中，我们将值`hello`作为位置参数传递。值`world`是通过关键字传递的；我们指定要将`world`传递给`thing`参数。

```py
def the_func(greeting, thing):
  print(greeting + ' ' + thing)

the_func('hello', thing='world')
```

位置参数和`kwargs`（关键字参数）之间的区别在于**顺序**。你传递位置参数的顺序很重要。如果你调用`the_func('world', 'hello')`，它会打印`world hello`。传递`kwargs`的顺序无关紧要：

```py
the_func('hello', 'world')                  # -> 'hello world'
the_func('world', 'hello')                  # -> 'world hello'
the_func(greeting='hello', thing='world')   # -> 'hello world'
the_func(thing='world', greeting='hello')   # -> 'hello world'
the_func('hello', thing='world')            # -> 'hello world'
```

还要注意（在最后一行），只要`kwargs`在位置参数之后，你可以混合使用位置参数和关键字参数。

## `args`的性能是否优于`kwargs`？

查看下面的文章！

[//args-vs-kwargs-which-is-the-fastest-way-to-call-a-function-in-python-afb2e817120?source=post_page-----bd2acdce72b5--------------------------------](https://towardsdatascience.com/args-vs-kwargs-which-is-the-fastest-way-to-call-a-function-in-python-afb2e817120?source=post_page-----bd2acdce72b5--------------------------------) ## Args vs kwargs：哪种是调用 Python 函数的最快方式？

### `timeit`模块的清晰演示

[towardsdatascience.com

# 设计函数参数

在这一部分，我们将回答 6 个问题，展示你可以如何设计函数参数。每个答案将附带示例和用例（如有需要）。

## 1\. 如何捕获所有未捕获的位置参数？

使用`*args`，你可以设计函数使其接受任意数量的参数。例如，查看下面的函数。

```py
def multiply(a, b, *args):
  result = a * b
  for arg in args:
    result = result * arg
  return result
```

在这个函数中，我们正常定义了前两个参数（`a`和`b`）。然后我们使用`*args`将所有剩余的参数打包成一个元组。把`*`看作是吞噬所有不匹配的参数并将它们推送到名为‘args’的元组变量中。让我们看看实际效果：

```py
multiply(1, 2)          # returns 2
multiply(1, 2, 3, 4)    # returns 24
```

最后的调用将值 1 分配给参数`a`，值 2 分配给`b`，`arg`变量填充为`(3, 4)`。由于这是一个元组，我们可以在函数中循环遍历它并使用这些值进行乘法运算！

[//why-and-how-custom-exceptions-lead-to-cleaner-better-code-2382216829fd?source=post_page-----bd2acdce72b5--------------------------------](https://towardsdatascience.com/why-and-how-custom-exceptions-lead-to-cleaner-better-code-2382216829fd?source=post_page-----bd2acdce72b5--------------------------------) ## 为什么及如何自定义异常能够使代码更干净、更好

### 通过创建自己的自定义异常来清理代码

[towardsdatascience.com

## 2\. 如何捕获所有未捕获的关键字参数？

我们在前一部分使用的相同技巧可以用于捕获所有剩余的不匹配的关键字参数：

```py
def introduce(firstname, lastname, **kwargs):
  introduction = f"I am {firstname} {lastname}"
  for key, value in kwargs.items():
    introduction += f" my {key} is {value} "
  return introduction
```

与`*args`类似，`**kwargs`关键字会吞噬所有不匹配的关键字参数，并将它们存储在名为`kwargs`的字典中。然后我们可以像在上面的函数中一样访问这个字典。

```py
print(introduce(firstname='mike', lastname='huls'))
# returns "I am mike huls"

print(introduce(firstname='mike', lastname='huls', age=33, website='mikehuls.com'))
# I am mike huls my age is 33  my website is mikehuls.com
```

使用`kwargs`，我们可以向`introduce`函数添加一些额外的参数。

towardsdatascience.com ## 永远不需要再写 SQL：SQLAlchemy 的 ORM 绝对初学者指南

### 使用这个 ORM，你可以创建表、插入、读取、删除和更新数据，而无需编写一行 SQL 代码

[towardsdatascience.com

## 3\. 我如何设计函数以只接受关键字参数？

当你真的不想混淆你的参数时，你可以强制你的函数只接受关键字参数。一个完美的使用案例可能是一个将钱从一个账户转到另一个账户的函数。你确实不想以位置方式传递账户号码，因为这样你有可能让开发者不小心交换账户号码：

```py
def transfer_money(*, from_account:str, to_account:str, amount:int):
  print(f'Transfering ${amount} FORM {from_account} to {to_account}')

transfer_money(from_account='1234', to_account='6578', amount=9999)
# won't work: TypeError: transfer_money() takes 0 positional arguments but 1 positional argument (and 2 keyword-only arguments) were given
transfer_money('1234', to_account='6578', amount=9999)
# won't work: TypeError: transfer_money() takes 0 positional arguments but 3 were given
transfer_money('1234', '6578', 9999)
```

在上面的函数中你再次看到`*`。我将星号视为吞噬所有不匹配的位置参数，但与`*args`将所有不匹配的位置参数存储在`args`元组中不同，裸`*`只是将这些参数作废。

towardsdatascience.com ## 了解 Python 上下文管理器：绝对初学者指南

### 使用光剑理解 WITH 语句

[towardsdatascience.com

## 4\. 我如何设计函数以只接受位置参数？

以下函数是只允许位置参数的函数示例：

```py
def the_func(arg1:str, arg2:str, /):
  print(f'provided {arg1=}, {arg2=}')

# These work:
the_func('num1', 'num2')
the_func('num2', 'num1')

# won't work: TypeError: the_func() got some positional-only arguments passed as keyword arguments: 'arg1, arg2'
the_func(arg1='num1', arg2='num2')
# won't work: TypeError: the_func() got some positional-only arguments passed as keyword arguments: 'arg2'
the_func('num1', arg2='num2') 
```

函数定义中的`/`强制所有在它之前的参数必须是位置参数。附带说明：这并不意味着所有在`/`之后的参数*必须*仅为关键字参数；这些参数可以是位置参数*和*关键字参数。

**我为什么需要这样做？这不会降低代码的可读性吗？** 好问题！一个例子可能是当你定义一个函数时，这个函数非常明确，以至于你不需要关键字参数来指定它的作用。例如：

```py
def exceeds_100_bytes(x, /) -> bool:
  return x.__sizeof__() > 100

exceeds_100_bytes('a')      
exceeds_100_bytes({'a'})
```

在这个例子中，很明显我们在检查`'a'`的内存大小是否超过 100 字节。我真的想不出一个更好的名字来给`x`参数，而且可以在不需要指定`x=’a’`的情况下调用这个函数。另一个例子是内置的`len`函数：调用`len(target_object=some_list)`会显得很尴尬。

作为额外说明，我们可以更改参数名，因为我们知道这样不会破坏对函数的调用：我们不允许使用关键字参数。此外，我们甚至可以在完全向后兼容的情况下扩展这个函数。下面的版本将检查任何提供的参数是否超过 100 字节。

```py
def exceeds_100_bytes(*args) -> bool:
  for a in args:
    if (a.__sizeof__() > 100):
      return True
  return False
```

我们可以用`*args`替换`x`，因为在之前的版本中，`/`确保函数仅以位置参数的形式调用。

[](/cython-for-absolute-beginners-30x-faster-code-in-two-simple-steps-bbb6c10d06ad?source=post_page-----bd2acdce72b5--------------------------------) ## Cython 对绝对初学者的指南：两步实现 30 倍更快的代码

### 快速应用的简单 Python 代码编译

towardsdatascience.com

## **5\. 混合与匹配 — 如何传递既是位置参数又是`kwargs`的参数？**

作为示例，我们将讨论之前提到的`len`函数。这个函数只允许位置参数。我们将扩展这个函数，允许开发者选择是否计算重复项。我们希望开发者通过`kwargs`传递这个关键字：

```py
def len_new(x, /, *, no_duplicates=False):
  if (no_duplicates):
    return len(list(set([a for a in x])))
  return len(x)
```

如你所见，我们希望计算`x`变量的`len`。由于`x`参数前面有`/`，我们只能以位置参数的方式传递它。`no_duplicates`参数必须以关键字的形式传递，因为它跟在`*`之后。让我们调用这个函数：

```py
print(len_new('aabbcc'))                                  # returns 6
print(len_new('aabbcc', no_duplicates=True))              # returns 3
print(len_new([1, 1, 2, 2, 3, 3], no_duplicates=False))   # returns 6
print(len_new([1, 1, 2, 2, 3, 3], no_duplicates=True))    # returns 3

# Won't work: TypeError: len_() got some positional-only arguments passed as keyword arguments: 'x'
print(len_new(x=[1, 1, 2, 2, 3, 3]))
# Won't work: TypeError: len_new() takes 1 positional argument but 2 were given
print(len_new([1, 1, 2, 2, 3, 3], True))
```

[](/image-analysis-for-beginners-destroying-duck-hunt-with-opencv-e19a27fd8b6?source=post_page-----bd2acdce72b5--------------------------------) ## 用 OpenCV 破坏《Duck Hunt》 — 初学者的图像分析

### 编写能够击败所有《Duck Hunt》高分的代码

towardsdatascience.com

## 6\. 混合与匹配 — 综合应用

下面的函数是如何将所有之前讨论的技术结合在一起的极端示例。首先，它强制前两个参数以位置参数的方式传递，接下来的两个参数可以以位置参数和关键字参数的方式传递，然后是两个仅限关键字的参数，最后我们用`**kwargs`捕捉其余未捕获的参数。

```py
def the_func(pos_only1, pos_only2, /, pos_or_kw1, pos_or_kw2, *, kw1, kw2, **extra_kw):
  # cannot be passed kwarg   <--   | --> can be passed 2 ways | --> can only be passed by kwarg
  print(f"{pos_only1=}, {pos_only2=}, {pos_or_kw1=}, {pos_or_kw2=}, {kw1=}, {kw2=}, {extra_kw=}")
```

你可以像这样传递这个函数：

```py
# works (pos_or_kw1 & pow_or_k2 can be passed positionally and by kwarg)
pos_only1='pos1', pos_only2='pos2', pos_or_kw1='pk1', pos_or_kw2='pk2', kw1='kw1', kw2='kw2', extra_kw={}
pos_only1='pos1', pos_only2='pos2', pos_or_kw1='pk1', pos_or_kw2='pk2', kw1='kw1', kw2='kw2', extra_kw={}
pos_only1='pos1', pos_only2='pos2', pos_or_kw1='pk1', pos_or_kw2='pk2', kw1='kw1', kw2='kw2', extra_kw={'kw_extra1': 'extra_kw1'}

# doesnt work, (pos1 and pos2 cannot be passed with kwarg)
# the_func(pos_only1='pos1', pos_only2='pos2', pos_or_kw1='pk1', pos_or_kw2='pk2', kw1='kw1', kw2='kw2')

# doesnt work, (kw1 and kw2 cannot be passed positionally)
# the_func('pos1', 'pos2', 'pk1', 'pk2', 'kw1', 'kw2')
```

[](/multi-tasking-in-python-speed-up-your-program-10x-by-executing-things-simultaneously-4b4fc7ee71e?source=post_page-----bd2acdce72b5--------------------------------) ## Python 中的多任务处理：通过同时执行操作使程序速度提高 10 倍

### 将线程和进程应用于加速代码的逐步指南

towardsdatascience.com

# 结论

在这篇文章中，我们讨论了设计函数参数的所有方法，并展示了如何混合和匹配这些参数，以便开发者能够以最佳方式使用你的函数。

我希望这篇文章如我所希望的那样清晰，如果不是这样，请告诉我我可以做些什么来进一步澄清。与此同时，看看我关于各种编程相关主题的[其他文章](https://mikehuls.com/articles?tags=python)，例如：

+   [Git 完全初学者指南：通过视频游戏理解 Git](https://mikehuls.medium.com/git-for-absolute-beginners-understanding-git-with-the-help-of-a-video-game-88826054459a)

+   [创建并发布你自己的 Python 包](https://mikehuls.medium.com/create-and-publish-your-own-python-package-ea45bee41cdc)

+   [使用 FastAPI 在 5 行代码中创建一个快速的自动文档、可维护和易用的 Python API](https://mikehuls.medium.com/create-a-fast-auto-documented-maintainable-and-easy-to-use-python-api-in-5-lines-of-code-with-4e574c00f70e)

编程愉快！

— 迈克

*附言：喜欢我在做的事情吗？* [*关注我！*](https://mikehuls.medium.com/membership)

[](https://mikehuls.medium.com/membership?source=post_page-----bd2acdce72b5--------------------------------) [## 使用我的推荐链接加入 Medium — 迈克·胡尔斯

### 阅读迈克·胡尔斯及其他数千位 Medium 作者的每一个故事。你的会员费直接支持迈克…

mikehuls.medium.com](https://mikehuls.medium.com/membership?source=post_page-----bd2acdce72b5--------------------------------)
