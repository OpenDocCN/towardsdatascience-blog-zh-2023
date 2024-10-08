# Python 怪癖：了解如何通过一个不返回任何东西的函数来修改变量

> 原文：[`towardsdatascience.com/python-quirks-understand-how-a-variable-can-be-modified-by-a-function-that-doesnt-return-anything-343a40cc6923`](https://towardsdatascience.com/python-quirks-understand-how-a-variable-can-be-modified-by-a-function-that-doesnt-return-anything-343a40cc6923)

## 深入了解 Python 如何传递参数和可变性，以防止意外错误

[](https://mikehuls.medium.com/?source=post_page-----343a40cc6923--------------------------------)![Mike Huls](https://mikehuls.medium.com/?source=post_page-----343a40cc6923--------------------------------)[](https://towardsdatascience.com/?source=post_page-----343a40cc6923--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----343a40cc6923--------------------------------) [Mike Huls](https://mikehuls.medium.com/?source=post_page-----343a40cc6923--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----343a40cc6923--------------------------------) ·8 分钟阅读·2023 年 4 月 13 日

--

![](img/66a1b3b46f16c0c485439cbce8fc83b8.png)

跟踪意外的错误（图片来自 [cottonbro studio](https://www.pexels.com/@cottonbro/) on [Pexels](https://www.pexels.com/photo/photo-of-person-taking-down-notes-7319070/))

在这篇文章中，我们将戴上侦探帽，解开一个“*Python 神秘*”。在这一集里，我们将了解**一个不返回值的函数如何改变一个变量**。（下面有示例）。不仅如此：**它只对某些类型的变量‘有效’**。此外，这种行为很容易让人陷入陷阱，因此了解其原因非常重要。

我们将重点理解神秘背后的机制。更好地理解 Python 不仅会让你成为**更优秀的开发者**，还会**节省你解决难以理解的错误**的沮丧。让我们开始编程吧！

# 神秘——一个例子

首先让我们更深入地分析一下我们的“*Python 神秘*”：假设我们有两个函数：

+   ***接受*** 一个变量

+   ***修改*** 该变量

+   ***不要返回*** 该变量

```py
def change_string(input_string:str) -> None:
    """ Notice that this functions doesn't return anything! """
    input_string += 'a'

def change_list(input_list:list) -> None:
    """ Notice that this functions doesn't return anything! """
    input_list.append('a')
```

对于这两个函数，我们定义一个变量，打印出来，调用函数并传递变量，然后再次打印出来

```py
my_str = 'hello'
print(my_str)                        # 'hello'
change_string(input_string=my_str)
print(my_str)                        # 'hello'

my_list = ['hello']
print(my_list)                       # ['hello'] 
change_list(input_list=my_list)
print(my_list)                       # ['hello', 'a'] !?
```

发生了什么？为什么`my_list`变量**改变**了，而`my_str`变量**没有**？尽管这些函数没有返回任何东西！我们有三个问题，将在三个相应的章节中解答：

1.  函数如何“访问”变量？

1.  为什么列表被修改而字符串没有改变？

1.  我们如何防止这种行为？

[](/thread-your-python-program-with-two-lines-of-code-3b474407dbb8?source=post_page-----343a40cc6923--------------------------------) ## 用两行代码线程化你的 Python 程序

### 通过同时做多件事来加快你的程序

towardsdatascience.com

# 1\. 函数如何访问变量

为了弄清楚这一点，我们需要理解变量是如何进入函数的：我们需要了解 **Python 是如何将变量传递给函数的**。有很多种方法可以做到这一点。为了理解 Python 如何将变量传递给函数，我们首先需要了解 **Python 如何在内存中存储值**。

# 1.1 Python 如何存储变量

你可能会认为当我们定义一个变量时，比如：`person = 'mike'`，内存中有一个名为 `‘person’` 的对象，其值为 `‘mike’`（参见下面的图片）。这只是部分正确。

![](img/cbe0ce130c0df00650699664d05a909d.png)

变量在 Python 和其他语言（例如 C）的内存存储方式（由作者专业绘制）

Python 使用 **引用**。它在内存中创建一个对象，然后创建一个名为 `‘person’` 的引用，指向内存中的对象，具体的内存地址和值是 `‘mike’`。可以把它看作是在对象上挂一个标签，这个标签上写着变量的名字。

如果我们做类似这样的操作：`person2 = person`，我们不会在内存中创建一个新对象，只是创建了一个名为‘person2’的新引用，指向已经存在的内存中的对象：

![](img/e31e37a5b5145078b38a97694c0e4e6e.png)

创建一个新的引用，指向相同的对象（图片由作者提供）

重新定义 `person2 = ‘bert'` 将导致 Python 在内存中创建一个新对象，并将名为“person2”的引用指向那里：

![](img/482ef44ae3de0b39f6d3a45256e209b8.png)

## 1.2 Python 是传递对象还是引用给函数？

理解一个关键点是，当我们调用 `somefunction(person)` 时 **我们并没有给函数一个内存中的对象，而只是该对象的引用**。

> Python 变量是 ***“按引用”*** 传递的，而不是 ***“按值”*** 传递的。

这是解决谜团的第一个答案：我们给函数提供了一个内存中值的引用，而不是给函数提供一个 **对象的副本**。这就是为什么我们可以修改值 **而不需要函数返回** 任何东西。

现在让我们来看解决方案的另一部分：为什么有些变量可以被修改而有些不能。

[](/args-vs-kwargs-which-is-the-fastest-way-to-call-a-function-in-python-afb2e817120?source=post_page-----343a40cc6923--------------------------------) ## 参数与关键字参数：哪种方式在 Python 中调用函数最快？

### timeit 模块的清晰演示

[towardsdatascience.com

# 2\. 为什么有些值可以被改变而有些不能？ — 可变性

可变性是对象在创建后改变其值的能力。让我们首先了解一下可变变量：

```py
IMMUTABLE                                  MUTABLE
int, float, decimal, complex (numbers)     list
bool                                       set
str                                        dict
tuple
frozenset
```

正如你所见，`str`是不可变的；这意味着它在初始化后不能改变。那么我们之前如何“修改”了我们的字符串（例如：`input_string += ‘a'`）。接下来的部分解释了当我们尝试**更改**和**覆盖**可变和不可变值时会发生什么。

[](/why-is-python-so-slow-and-how-to-speed-it-up-485b5a84154e?source=post_page-----343a40cc6923--------------------------------) ## 为什么 Python 这么慢以及如何加速

### 看一看背后的机制，了解 Python 的瓶颈所在

[towardsdatascience.com

## 2.1 当我们尝试更改不可变值时会发生什么？

我们创建了一个名为`my_str`的变量，值为`'a'`。接下来，我们使用`id`函数打印变量的内存地址。这是引用指向的内存位置。

重申一下：在下面的例子中，我们创建了一个*引用*，名为`my_str`，它指向一个*内存中的对象*，该对象的值为`'a'`，并位于*内存地址* `1988650365763`。

```py
my_str = 'a'
print(id(my_str))    # 1988650365763
my_str += 'b'
print(id(my_str))    # 1988650363313
```

接下来，在第 3 行，我们将`'b'`添加到`my_str`中，并再次打印内存位置。如你所见，通过内存位置的变化，`my_str`在添加了`'b'`后变得不同。这意味着**在内存中创建了一个新对象**。

看起来 Python 似乎在更改字符串，但实际上它只是创建了一个新的内存对象，并将名为`my_str`的引用指向那个新对象。值为`'a'`的旧对象将被移除。查看**这篇文章**了解更多关于为什么 Python 不直接覆盖内存中的对象以及旧值如何被移除的内容。

## 2.2 当我们尝试更改可变值时会发生什么？

让我们用一个可变变量做同样的实验：

```py
my_list= ['a']
print(id(my_list))    # 1988503659344
my_list.append('b')
print(id(my_list))    # 1988503659344
```

所以名为`my_list`的引用仍然指向内存中对象所在的同一位置。这证明了内存中的对象已经改变！还要注意，列表中的元素可以包含不可变类型。如果我们尝试更改这些变量，情况与之前所述相同。

## 2.3 当我们尝试覆盖变量时会发生什么？

正如我们在前面的部分所看到的，Python 不会覆盖内存中的对象。让我们看看实际效果：

```py
# Immutable var: string
my_str = 'a'
print(id(my_str))            # 1988650365936
my_str = 'b'
print(id(my_str))            # 1988650350704

# Mutable var: list
my_lst = ['a', 'list']
print(id(my_lst))            # 1988659494080
my_lst = ['other', 'list']
print(id(my_lst))            # 1988659420608
```

如你所见，所有内存位置都发生了变化，包括可变和不可变的变量。这是 Python 处理变量的默认方式。注意我们**并没有尝试改变**可变列表的内容：我们定义了一个新的列表；我们并不是在改变它，而是将完全新的数据分配给`my_lst`。

## 2.4 为什么有些值是可变的而有些不是？

可变性通常是设计选择；一些变量保证内容保持不变并且有序。

[](/getting-started-with-cython-how-to-perform-1-7-billion-calculations-per-second-in-python-b83374cfcf77?source=post_page-----343a40cc6923--------------------------------) ## 入门 Cython：如何在 Python 中每秒进行超过 1.7 亿次计算

### 将 Python 的简便性与 C 的速度结合

towardsdatascience.com

# 解决方案：按引用传递和可变性的实际操作

在这一部分，我们将运用新学到的知识来解决谜题。在下面的代码中，我们声明了一个（可变的）列表，并将其（通过引用）传递给一个函数。然后函数能够更改列表的内容。我们可以通过以下事实看到这一点：内存地址在第 3 行和最后一行是相同的，而内容已经改变：

```py
# 1\. Define list and check out the memory-address and content
my_list = ['a', 'list']
print(id(my_list), my_list)            # 2309673102336 ['a', 'list']

def change_list(input_list:list):
    """ Adds value to the list but don't return the list """
    print(id(input_list), input_list)  # 2309673102336 ['a', 'list']
    input_list.append('b')
    print(id(input_list))              # 2309673102336 ['a', 'list', 'b']

# 2\. Pass the list into our function (function doesn't return anything)
change_list(input_list=my_list)     

# 3\. Notice that the memory location is the same and the list has changed
print(id(my_list), my_list)            # 2309673102336 ['a', 'list', 'b']
```

## 这如何与不可变值一起工作？

好问题。让我们用一个不可变的元组来检查一下：

```py
# 1\. Define a tuple, check out memory address and content
my_tup = {'a', 'tup'}
print(id(immutable_string), my_tup)        # 2560317441984, {'a', 'tup'}

def change_tuple(input_tuple:tuple):
    """ 'overwrites' the tuple we received, don't return anything """
    print(id(input_tuple))                 # 2560317441984, {'a', 'tup'}
    input_tuple = ('other', 'tuple')
    print(id(input_tuple))                 # 2560317400064, {'other', 'tup'}

# 2\. Pass the list into our function (nothing is returned from function)
change_tuple(input_tuple=immutable_tuple) 

# 3\. Print out memory location and content again
print(id(my_tup), my_tup)                  # 2560317441984, {'a', 'tup'}
```

由于我们不能改变值，我们必须在`change_tuple`函数中“覆盖”`input_tuple`。这并不意味着内存中的对象被覆盖，而是创建了一个新的对象。

然后我们修改在`change_tuple`函数作用域内存在的引用`input_tuple`，使其现在指向这个新对象。当我们退出函数时，这个引用会被清理，在外部作用域中，`my_tup`引用仍然指向旧对象的内存地址。

简而言之：“新”元组仅存在于函数的作用域中。

[](/image-analysis-for-beginners-destroying-duck-hunt-with-opencv-e19a27fd8b6?source=post_page-----343a40cc6923--------------------------------) ## 使用 OpenCV 毁灭《鸭子猎人》——初学者的图像分析

### 编写能打破所有《鸭子猎人》高分的代码

towardsdatascience.com

# 3\. 如何防止不希望出现的行为

你可以通过给函数一个`my_list.copy()`来防止这种行为。这会先创建列表的副本，并将该副本的***引用***提供给函数，从而使所有更改都作用于副本而不是`my_list`：

```py
# 2\. Pass the list into our function (nothing is returned from function)
change_list(input_list=my_list.copy()) 
```

[](/a-complete-guide-to-using-environment-variables-and-files-with-docker-and-compose-4549c21dc6af?source=post_page-----343a40cc6923--------------------------------) ## 完整指南：使用 Docker 和 Compose 的环境变量和文件

### 通过这个简单的教程，让你的容器既安全又灵活。

towardsdatascience.com

# 结论

我们讨论了可变性以及 Python 如何将变量传递给函数；这两个概念在设计 Python 代码时非常重要。通过这篇文章，我希望你避免难以理解的错误和大量的调试时间。

我希望这篇文章能像我期望的那样清晰，如果不是这样，请告诉我我可以做些什么来进一步澄清。同时，查看我在[其他文章](https://mikehuls.com/articles?tags=python)中讨论的各种编程相关主题：

+   [Git 绝对初学者指南：通过视频游戏理解 Git](https://mikehuls.medium.com/git-for-absolute-beginners-understanding-git-with-the-help-of-a-video-game-88826054459a)

+   [创建并发布你自己的 Python 包](https://mikehuls.medium.com/create-and-publish-your-own-python-package-ea45bee41cdc)

+   [用 FastAPI 在 5 行代码中创建一个快速的自动文档、可维护且易于使用的 Python API](https://mikehuls.medium.com/create-a-fast-auto-documented-maintainable-and-easy-to-use-python-api-in-5-lines-of-code-with-4e574c00f70e)

编程愉快！

— Mike

*附注：喜欢我在做的事吗？* [*关注我!*](https://mikehuls.medium.com/membership)

[](https://mikehuls.medium.com/membership?source=post_page-----343a40cc6923--------------------------------) [## 通过我的推荐链接加入 Medium - Mike Huls

### 阅读 Mike Huls 的每个故事（以及 Medium 上的其他成千上万的作者）。你的会员费直接支持 Mike…

mikehuls.medium.com](https://mikehuls.medium.com/membership?source=post_page-----343a40cc6923--------------------------------)
