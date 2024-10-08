# Args 与 kwargs：在 Python 中调用函数的最快方式是什么？

> 原文：[`towardsdatascience.com/args-vs-kwargs-which-is-the-fastest-way-to-call-a-function-in-python-afb2e817120`](https://towardsdatascience.com/args-vs-kwargs-which-is-the-fastest-way-to-call-a-function-in-python-afb2e817120)

## `timeit`模块的清晰演示

[](https://mikehuls.medium.com/?source=post_page-----afb2e817120--------------------------------)![Mike Huls](https://mikehuls.medium.com/?source=post_page-----afb2e817120--------------------------------)[](https://towardsdatascience.com/?source=post_page-----afb2e817120--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----afb2e817120--------------------------------) [Mike Huls](https://mikehuls.medium.com/?source=post_page-----afb2e817120--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----afb2e817120--------------------------------) ·阅读时间 3 分钟·2023 年 2 月 6 日

--

![](img/f6dccb8745eef8fdb949b1245807f318.png)

决战时刻（作者提供的图片）

你是否曾经想过使用关键字调用函数是否比不使用关键字更慢？换句话说：**位置参数**（`myfunc('mike', 33)`）和**kwargs**（`myfunc(name='mike', age=33)`）哪个更快？

在这篇简短的文章中，我们将探讨牺牲传递**关键字参数**的可读性与按**位置**传递参数是否值得。我们使用`timeit`模块设置基准测试并比较结果。没有太复杂的内容；让我们开始编码吧！

# 该函数

为了基准测试函数调用的性能，我们首先需要一个可以调用的函数：

```py
def the_func(arg1, arg2):
    pass
```

这个函数仅包含一个`pass`语句，意味着这个函数本身什么都不做。这确保了我们可以单独分析和比较调用函数的方式（即按位置调用还是使用 kwargs）。

[](/cython-for-absolute-beginners-30x-faster-code-in-two-simple-steps-bbb6c10d06ad?source=post_page-----afb2e817120--------------------------------) ## Cython 入门：两步实现 30 倍速度提升的代码

### 轻松的 Python 代码编译，实现飞快的应用程序

towardsdatascience.com

# 基准测试脚本

接下来，我们需要一些代码来多次调用函数并记录函数执行所需的时间：

```py
import timeit

number = 25_000_000
repeat = 10

times_pos: [float] = timeit.repeat(stmt="func('hello', 'world')", globals={'func': the_func}, number=number, repeat=repeat)
times_kwarg: [float] = timeit.repeat(stmt="func(arg1='hello', arg2='world')", globals={'func': the_func}, number=number, repeat=repeat)
```

这就是`timeit`模块的作用。我们使用`timeit.repeat`来测量运行函数若干次的时间。我们在`stmt`中定义函数调用，并使用`globals`字典将`func`映射到我们之前定义的函数。然后我们使用`repeat`参数多次重复实验。最终我们得到一个包含 10 个浮点数的数组：每个数值表示运行函数调用 2500 万次的结果。

[如何使用 OpenCV 进行图像分析 - 初学者创建运动检测器](https://towardsdatascience.com/image-analysis-for-beginners-creating-a-motion-detector-with-opencv-4ca6faba4b42?source=post_page-----afb2e817120--------------------------------) [## 使用 OpenCV 检测运动 - 图像分析初学者指南

### 如何使用 OpenCV 检测和分析移动对象

[如何使用 OpenCV 进行图像分析 - 初学者创建运动检测器](https://towardsdatascience.com/image-analysis-for-beginners-creating-a-motion-detector-with-opencv-4ca6faba4b42?source=post_page-----afb2e817120--------------------------------)

# 查看结果

使用下面的代码，我们获取`timeit`提供的浮点数列表，并显示最小值、最大值和平均执行时间。

```py
print("\t\t\t min (s) \t max (s) \t avg (s)")
print(f"pos: \t\t {min(times_pos):.5f} \t {max(times_pos):.5f} \t {sum(times_pos) / len(times_pos):.5f}")
print(f"arg only: \t {min(times_arg_only):.5f} \t {max(times_arg_only):.5f} \t {sum(times_arg_only) / len(times_arg_only):.5f}")
```

完成了！让我们进行一些基准测试！

[如何在 Python 中建立数据库连接 - 绝对初学者](https://towardsdatascience.com/how-to-make-a-database-connection-in-python-for-absolute-beginners-e2bfd8e4e52?source=post_page-----afb2e817120--------------------------------) [## 如何在 Python 中建立数据库连接 - 绝对初学者

### 3 个步骤（加上示例）连接 MS SQL Server、MySQL、Oracle 和其他许多数据库

[如何在 Python 中建立数据库连接 - 绝对初学者](https://towardsdatascience.com/how-to-make-a-database-connection-in-python-for-absolute-beginners-e2bfd8e4e52?source=post_page-----afb2e817120--------------------------------)

# 结果

结果如下：

```py
 min (s)   max (s)   avg (s)
pos:        1.38941   1.72278   1.58808
kwarg:      1.72834   1.76344   1.75132

          min (s)   max (s)   avg (s)
pos:      2.07883   2.77485   2.35694
kwarg:    2.05186   3.05402   2.74669
```

看起来，平均而言，位置参数比**0.39 秒**快，或者稍微超过**16.5%**。不过请注意，位置参数在调用函数时快**0.39 秒**，这是基于函数调用了***2500 万次***的情况。

这意味着选择位置参数而非关键字参数可以节省约**16 纳秒**的时间；这是光线穿越约 4.8 米（16 英尺）距离所需的时间。

[为什么 Python 这么慢以及如何加快速度](https://towardsdatascience.com/why-is-python-so-slow-and-how-to-speed-it-up-485b5a84154e?source=post_page-----afb2e817120--------------------------------) [## 为什么 Python 这么慢以及如何加快速度

### 看一看背后的瓶颈在哪里

[为什么 Python 这么慢以及如何加快速度](https://towardsdatascience.com/why-is-python-so-slow-and-how-to-speed-it-up-485b5a84154e?source=post_page-----afb2e817120--------------------------------)

# 结论

让我们从**主要结论开始：不要因为性能问题而停止使用关键字参数**！我个人喜欢使用关键字参数，因为它使代码更具可读性，并减少了混淆参数的可能性。在这篇文章中，我们看到使用关键字参数带来了微不足道的性能提升。为了可读性而接受几纳秒的速度损失，还是值得的。

我希望这篇文章的内容能够达到我的期望，但如果没有，请告诉我如何进一步澄清。同时，查看我关于各种编程相关话题的[其他文章](https://mikehuls.com/articles?tags=python)，例如：

+   [Git 初学者指南：通过视频游戏理解 Git](https://mikehuls.medium.com/git-for-absolute-beginners-understanding-git-with-the-help-of-a-video-game-88826054459a)

+   [创建并发布你自己的 Python 包](https://mikehuls.medium.com/create-and-publish-your-own-python-package-ea45bee41cdc)

+   [使用 FastAPI 在 5 行代码中创建一个快速的自动文档、可维护、易用的 Python API](https://mikehuls.medium.com/create-a-fast-auto-documented-maintainable-and-easy-to-use-python-api-in-5-lines-of-code-with-4e574c00f70e)

编程愉快！

— Mike

*附言：喜欢我做的事情吗？* [*关注我！*](https://mikehuls.medium.com/membership)

[](https://mikehuls.medium.com/membership?source=post_page-----afb2e817120--------------------------------) [## 通过我的推荐链接加入 Medium — Mike Huls

### 阅读 Mike Huls 和数千名其他 Medium 作者的所有故事。你的会员费直接支持 Mike…

mikehuls.medium.com](https://mikehuls.medium.com/membership?source=post_page-----afb2e817120--------------------------------)
