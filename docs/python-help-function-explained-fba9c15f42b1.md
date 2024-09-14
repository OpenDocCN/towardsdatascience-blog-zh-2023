# Python help() 函数解释

> 原文：[`towardsdatascience.com/python-help-function-explained-fba9c15f42b1`](https://towardsdatascience.com/python-help-function-explained-fba9c15f42b1)

## 在这篇文章中，我们将探讨如何使用 Python **help()** 函数

[](https://pyshark.medium.com/?source=post_page-----fba9c15f42b1--------------------------------)![Misha Sv](https://pyshark.medium.com/?source=post_page-----fba9c15f42b1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fba9c15f42b1--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----fba9c15f42b1--------------------------------) [Misha Sv](https://pyshark.medium.com/?source=post_page-----fba9c15f42b1--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fba9c15f42b1--------------------------------) ·4 分钟阅读·2023 年 1 月 13 日

--

![](img/478d5da12e5951b60853775b6e7feca4.png)

图片由 [Toa Heftiba](https://unsplash.com/@heftiba?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/_UIVmIBB3JU?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

**目录**

+   介绍

+   使用交互式帮助工具访问文档

+   使用 help() 访问对象文档

+   使用 help() 访问用户定义的函数文档

+   结论

# 介绍

在 Python 中，我们经常使用新的模块、函数、类或对象，这些模块、函数、类或对象我们以前没有使用过，且这些文档我们还没有阅读过。

我们可以使用 Python **help()** 函数来更快地获取这些信息，而不是浏览文档网站寻找特定的函数或类。

Python **help()** 函数用于显示指定模块、函数、类或对象的文档。

**help()** 函数的处理定义如下：

```py
help([object]) -> display documentation
```

# 使用交互式帮助工具访问文档

你可以在不带任何参数的情况下调用 Python **help()** 函数，它会启动一个交互提示符，你可以利用它来查找任何 Python 对象的文档。

让我们启动交互式帮助工具：

```py
#Start help utility
help()
```

你应该会看到一个帮助工具在终端中启动：

```py
Welcome to Python 3.7's help utility!

If this is your first time using Python, you should definitely check out
the tutorial on the Internet at https://docs.python.org/3.7/tutorial/.

Enter the name of any module, keyword, or topic to get help on writing
Python programs and using Python modules.  To quit this help utility and
return to the interpreter, just type "quit".

To get a list of available modules, keywords, symbols, or topics, type
"modules", "keywords", "symbols", or "topics".  Each module also comes
with a one-line summary of what it does; to list the modules whose name
or summary contain a given string such as "spam", type "modules spam".

help>
```

一旦帮助工具启动，我们可以利用它查找 Python 对象的文档。

例如，让我们尝试在帮助工具中运行 *map* 查找 [Python map() 函数](https://pyshark.com/python-map-function/) 的文档：

```py
help> map
```

你应该会得到函数文档：

```py
Help on class map in module builtins:

class map(object)
 |  map(func, *iterables) --> map object
 |  
 |  Make an iterator that computes the function using arguments from
 |  each of the iterables.  Stops when the shortest iterable is exhausted.
 |  
 |  Methods defined here:
 |  
 |  __getattribute__(self, name, /)
 |      Return getattr(self, name).
 |  
 |  __iter__(self, /)
 |      Implement iter(self).
 |  
 |  __next__(self, /)
 |      Implement next(self).
 |  
 |  __reduce__(...)
 |      Return state information for pickling.
 |  
 |  ----------------------------------------------------------------------
 |  Static methods defined here:
 |  
 |  __new__(*args, **kwargs) from builtins.type
 |      Create and return a new object.  See help(type) for accurate signature.
```

如你所见，文档包含函数描述、方法和文档字符串。

# 使用 help() 访问对象文档

你可以在不使用交互式帮助工具的情况下，一步访问 Python 对象文档。

只需以以下格式运行 Python **help()** 函数，并将 Python 对象作为参数传递进去：

```py
help([object])
```

让我们尝试使用这种方法访问[Python map() 函数](https://pyshark.com/python-map-function/)的文档：

```py
#Find documentation of Python map() function
help(map)
```

并且你应该会得到：

```py
Help on class map in module builtins:

class map(object)
 |  map(func, *iterables) --> map object
 |  
 |  Make an iterator that computes the function using arguments from
 |  each of the iterables.  Stops when the shortest iterable is exhausted.
 |  
 |  Methods defined here:
 |  
 |  __getattribute__(self, name, /)
 |      Return getattr(self, name).
 |  
 |  __iter__(self, /)
 |      Implement iter(self).
 |  
 |  __next__(self, /)
 |      Implement next(self).
 |  
 |  __reduce__(...)
 |      Return state information for pickling.
 |  
 |  ----------------------------------------------------------------------
 |  Static methods defined here:
 |  
 |  __new__(*args, **kwargs) from builtins.type
 |      Create and return a new object.  See help(type) for accurate signature.
```

如你所见，显示的文档与我们使用[交互式帮助工具](https://pyshark.com/python-help-function/#access-documentation-using-interactive-help-utility)找到的文档是一样的。

# 使用 help() 访问用户定义函数的文档

Python **help()** 函数也可以显示用户定义函数的信息。

在之前的示例中，我们访问了 Python 内置函数的文档，现在让我们创建一个自己的函数，并写一个简短的描述，然后尝试访问它的文档。

首先，创建一个空的 **main.py** 文件，然后创建一个简单的函数，该函数将两个数字相加并返回它们的和：

```py
#Define a function
def add(x, y):
    '''
    This function adds two given integer arguments

    Parameters:
    x : integer
    y : integer

    Output:
    val: integer
    '''

    val = x + y

    return val
```

现在我们已经定义了函数，在同一个 Python 文件中，我们可以调用 **help()** 函数，并将函数名称（**add**）作为参数传递：

```py
#Define a function
def add(x, y):
    '''
    This function adds two given integer arguments

    Parameters:
    x : integer
    y : integer

    Output:
    val: integer
    '''

    val = x + y

    return val

#Find documentation of user defined function add()
help(add)
```

并且你应该会得到：

```py
Help on function add in module __main__:

add(x, y)
    This function adds two given integer arguments

    Parameters:
    x : integer
    y : integer

    Output:
    val: integer
```

它显示了存储在 docstring 中的函数文档，包括其描述、输入参数和返回值。

# 结论

在本文中，我们探讨了如何使用 Python **help()** 函数，包括交互式帮助工具，访问内置函数以及用户定义函数的文档。

如果你有任何问题或有修改建议，请随时在下面留言，并查看更多我的[Python 函数](https://pyshark.com/category/python-functions/)教程。

*最初发布于* [*https://pyshark.com*](https://pyshark.com/python-help-function/) *2023 年 1 月 13 日。*
