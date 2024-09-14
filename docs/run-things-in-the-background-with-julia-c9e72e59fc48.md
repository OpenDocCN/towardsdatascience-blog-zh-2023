# 在 Julia 中后台运行任务

> 原文：[`towardsdatascience.com/run-things-in-the-background-with-julia-c9e72e59fc48`](https://towardsdatascience.com/run-things-in-the-background-with-julia-c9e72e59fc48)

## 停止等待，开始多线程

[](https://niczky12.medium.com/?source=post_page-----c9e72e59fc48--------------------------------)![本斯·科马尔尼茨基](https://niczky12.medium.com/?source=post_page-----c9e72e59fc48--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c9e72e59fc48--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c9e72e59fc48--------------------------------) [本斯·科马尔尼茨基](https://niczky12.medium.com/?source=post_page-----c9e72e59fc48--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c9e72e59fc48--------------------------------) ·阅读时长 4 分钟·2023 年 5 月 26 日

--

![](img/b6d8943bd405300d84e92529392e8559.png)

照片由 [Max Wolfs](https://unsplash.com/ko/@yesterdazed?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

即使 Julia 是现有的最快语言之一，有时执行任务也需要时间。如果你是一个使用 Julia 的数据科学家或分析师，也许你希望将计算任务发送到服务器，等待完成后再处理结果。

> 但等待是无聊的。

当你在工作中充满创意和热情，渴望交付有趣的内容时，你希望 **不断敲击键盘寻找其他内容**。

让我向你展示 Julia 中一个简单的技巧，如何 **将计算任务分配到另一个线程**，然后继续你的工作。

# 设置工作环境

正如我之前所说，Julia 很快。作为一种现代语言，它也 **考虑了多线程处理**。所以，如果你知道如何操作，使用计算机上的额外核心非常容易。

首先，我们必须确保以多个线程启动 Julia 实例：

```py
julia -t 4
```

这将使用 4 个线程启动 Julia。我们可以通过查询线程数来确认这一点：

```py
julia> using Base.Threads

julia> Threads.nthreads()
4
```

## 制作一个慢速函数

![](img/04421f6811cb6df54cb72c074f69ac46.png)

照片由 [Frederick Yang](https://unsplash.com/@fredericksail?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

现在我们有了更多的线程，是时候看看这些魔法的实际效果了。但我们需要一些东西运行一段时间才有意义。我假设你在阅读这篇文章时已经有了一些想法，但因为我喜欢在文章中提供完整的示例，我会在这里写一个小函数来娱乐一下自己。

> 这个“慢”的函数可能是构建 ML 模型的调用，运行一些类似 SQL 的数据库查询，或从云存储中获取一些数据。发挥你的想象力，尽情尝试吧！

```py
julia> function collatz(n, i=0)
           if n == 1
               i
           elseif iseven(n)
               collatz(n / 2, i + 1)
           else
               collatz(3n + 1, i + 1)
           end
       end
collatz (generic function with 2 methods)

julia> collatz(989345275647)
1348

julia> averageSteps(n) = sum(i -> collatz(i) / n, 1:n)
averageSteps (generic function with 1 method
```

如果你对上述内容感到好奇，以及为什么我选择了 989,345,275,647，那么[阅读这个维基页面](https://en.wikipedia.org/wiki/Collatz_conjecture)。

# 获取一些魔法

![](img/fe87378468a88af2ccaf2d451455f5df.png)

图片由 [K. Mitch Hodge](https://unsplash.com/@kmitchhodge?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上拍摄

由于我们在命名空间中有 `Threads`，我们可以使用 `**@spawn**` **宏将计算发送到另一个线程**。这意味着我们可以立即返回 REPL，并继续像以前一样工作。

```py
julia> res = @spawn averageSteps(1e7)
Task (runnable) @0x000000015d061f90

julia> 2⁵ + 12
44

julia> fetch(res)
155.2724831
```

*忽略我缺乏想象力，生成后我只是懒得想出更复杂的东西。*

基本上，这里发生的事情是 `@spawn` 返回一个 `Task`。**这个任务会自动分派到一个空闲的线程**，该线程可以在后台处理它，允许你在此期间编写更多代码和提出更多问题。一旦你需要结果，你可以用 `**fetch**` **收集任务的结果**，它会等待 `Task` 完成并返回结果。

# 证明这有效

一种展示这确实有效的方法是展示一些时间记录。

首先，我们将在当前线程上运行我们的函数并测量所需时间。然后我们将生成一个 `Task`，最后我们将生成并立即等待结果。

```py
julia> @time averageSteps(1e7)
 16.040698 seconds
155.2724831

julia> @time res = @spawn averageSteps(1e7)
  0.009290 seconds (31.72 k allocations: 1.988 MiB)
Task (runnable) @0x000000015d179f90

julia> @time fetch(@spawn averageSteps(1e7))
 16.358641 seconds (24.31 k allocations: 1.553 MiB, 0.06% compilation time)
155.2724831
```

正如你所见，我们的函数运行大约需要 16 秒。但如果我们调度任务，那么我们 **会立即返回一个任务**。这带来了一些开销，如你在最后一行所见，因为这比在主线程上运行计算稍微慢了 0.3 秒。

# 感谢阅读！

希望这个小技巧能让新手对 Julia 的现代多线程语言的强大功能有更多了解。如果你喜欢我关于这个话题的啰嗦，请给我一个 👏 或 👏 👏。
