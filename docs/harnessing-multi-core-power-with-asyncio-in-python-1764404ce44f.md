# 利用 Python 中的 Asyncio 发挥多核性能

> 原文：[`towardsdatascience.com/harnessing-multi-core-power-with-asyncio-in-python-1764404ce44f`](https://towardsdatascience.com/harnessing-multi-core-power-with-asyncio-in-python-1764404ce44f)

## [PYTHON 并发](https://medium.com/@qtalen/list/python-concurrency-2c979347da3b)

## 通过高效利用多个 CPU 核心，提升你的 Python 应用程序性能

[](https://qtalen.medium.com/?source=post_page-----1764404ce44f--------------------------------)![Peng Qian](https://qtalen.medium.com/?source=post_page-----1764404ce44f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1764404ce44f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1764404ce44f--------------------------------) [Peng Qian](https://qtalen.medium.com/?source=post_page-----1764404ce44f--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1764404ce44f--------------------------------) ·阅读时间 7 分钟·2023 年 5 月 9 日

--

![](img/229d20ad602218a5869ea3daecce8616.png)

图片来源：由作者创建，[Canva](https://www.canva.com/)

# 介绍

在这篇文章中，我将向你展示如何在多核 CPU 上执行 Python asyncio 代码，以解锁并发任务的*全部性能*。

## 我们的问题是什么？

asyncio 只使用一个核心。

在之前的文章中，我详细介绍了如何使用 Python asyncio。通过这些知识，你可以了解到 asyncio 允许 IO 绑定任务通过手动切换任务执行以绕过多线程任务切换中的 GIL 争用过程，从而以高速执行。

从理论上讲，IO 绑定任务的执行时间取决于从启动到 IO 操作响应的时间，而不依赖于你的 CPU 性能。因此，我们可以同时启动数万个 IO 任务，并迅速完成它们。

但最近，我在编写一个需要同时抓取数万个网页的程序时发现，尽管我的 asyncio 程序比使用迭代抓取网页的程序高效得多，但它仍然让我等待了很长时间。我是否应该充分利用我的计算机性能？于是我打开了任务管理器并检查了一下：

![](img/4a6532d75e085230100c06fe182e5996.png)

只有一个核心在负载中。图片来源：作者

我发现从一开始，我的代码只在一个 CPU 核心上运行，其他几个核心处于闲置状态。除了启动 IO 操作以获取网络数据外，一个任务在返回后还需要解包和格式化数据。虽然这部分操作并不消耗很多 CPU 性能，但随着任务的增加，这些 CPU 绑定操作会严重影响整体性能。

我希望让我的 asyncio 并发任务在多个核心上并行执行。这是否会挤压我计算机的性能？

# asyncio 的底层原理

要解决这个难题，我们必须从底层 asyncio 实现，即事件循环开始。

![](img/b758e8a4edaa316f667b8d276ffb5997.png)

事件循环如何工作。图片由作者提供

如图所示，asyncio 对程序的性能提升始于 IO 密集型任务。IO 密集型任务包括 HTTP 请求、读写文件、访问数据库等。这些任务的一个重要特点是，CPU 不会阻塞，并且在等待外部数据返回时花费大量时间计算，这与另一类同步任务完全不同，后者要求 CPU 始终占用以计算特定结果。

当我们生成一批 asyncio 任务时，代码会首先将这些任务放入队列中。这时，有一个名为事件循环的线程从队列中一个一个地取出任务并执行。当任务到达 await 语句并等待（通常是等待请求的返回）时，事件循环从队列中取出另一个任务并执行。直到之前等待的任务通过回调获得数据，事件循环才会返回到之前等待的任务并完成执行其余代码。

由于事件循环线程仅在一个核心上执行，当“其余代码”恰好占用 CPU 时间时，事件循环会被阻塞。当这种情况的任务数量很大时，每个小的阻塞段累加起来会使程序整体变慢。

# 我的解决方案是什么？

从中我们了解到，asyncio 程序变慢的原因是我们的 Python 代码仅在一个核心上执行事件循环，并且 IO 数据的处理导致程序变慢。有没有办法在每个 CPU 核心上启动一个事件循环以执行它呢？

众所周知，从 Python 3.7 开始，推荐使用`asyncio.run`方法来执行所有 asyncio 代码，这是一个高级抽象，它调用事件循环来执行代码，作为以下代码的替代：

```py
try:
    loop = asyncio.get_event_loop()

    loop.run_until_complete(task())
finally:
    loop.close()
```

从代码中可以看出，每次调用`asyncio.run`时，我们会得到（如果它已经存在）或创建一个新的事件循环。如果我们能够在每个核心上单独调用`asyncio.run`方法，是否可以实现同时在多个核心上执行 asyncio 任务的目标？

前一篇文章使用了一个实际示例来解释如何使用 asyncio 的`loop.run_in_executor`方法在进程池中并行化代码执行，同时从主进程中获取每个子进程的结果。如果你还没有阅读前一篇文章，你可以在这里查看：

[](/combining-multiprocessing-and-asyncio-in-python-for-performance-boosts-15496ffe96b?source=post_page-----1764404ce44f--------------------------------) ## 结合多进程和 asyncio 提升 Python 性能

### 使用真实世界的例子来演示一个 map-reduce 程序

[towardsdatascience.com

因此，我们的解决方案出现了：通过 `loop.run_in_executor` 方法将许多并发任务分发到多个子进程中， 然后在每个子进程上调用 `asyncio.run` 启动各自的事件循环并执行并发代码。下图展示了整个流程：

![](img/576f27087e45a3871343d5dfec459fc7.png)

代码的执行情况。图片由作者提供

绿色部分表示我们启动的子进程。黄色部分表示我们启动的并发任务。

# 启动前的准备

## 模拟任务的实现

在我们解决问题之前，我们需要做好准备。在这个示例中，我们不能编写实际的代码来抓取网络内容，因为这会对目标网站造成很大的困扰，所以我们将用代码模拟实际任务：

如代码所示，我们首先使用 `asyncio.sleep` 模拟 IO 任务在随机时间后返回，并进行迭代求和以模拟数据返回后的 CPU 处理。

## 传统代码的效果

接下来，我们采用传统的方法在主方法中启动 10,000 个并发任务，并观察这一批并发任务所耗费的时间：

如图所示，使用仅一个核心执行 asyncio 任务需要更长的时间。

![](img/9177895ae0985cf1c0e717bc37989c36.png)

在单个核心上耗时较长。图片由作者提供

# 代码实现

接下来，让我们按照流程图实现多核心 asyncio 代码，并查看性能是否有所提高。

## 设计代码的整体结构

首先，作为一个架构师，我们仍然需要首先定义整体脚本结构，需要哪些方法，以及每个方法需要完成什么任务：

## 每个方法的具体实现

然后，让我们一步步实现每个方法。

`query_concurrently` 方法会并发启动指定批次的任务，并通过 `asyncio.gather` 方法获取结果：

`run_batch_tasks` 方法不是一个异步方法，因为它直接在子进程中启动：

最后，这是我们的 `main` 方法。此方法将调用 `loop.run_in_executor` 方法，使 `run_batch_tasks` 方法在进程池中执行，并将子进程执行的结果合并到一个列表中：

由于我们正在编写一个多进程脚本，我们需要使用 `if __name__ == "__main__"` 来在主进程中启动主方法：

## 执行代码并查看结果

接下来，我们启动脚本并查看任务管理器中每个核心的负载：

![](img/a926d4be139a26f9625a2fcda8a0a5cd.png)

所有核心几乎都被利用。图片由作者提供

如你所见，所有 CPU 核心都被利用了。

最后，我们观察了代码执行时间，并确认多线程`asyncio`代码确实将代码执行速度提高了数倍！任务完成！

![](img/1e64a6dfd249ddaed28e499cfed5499f.png)

性能提升近三倍！作者图片

# 结论

在这篇文章中，我解释了为什么`asyncio`可以并发执行 IO 密集型任务，但在运行大量并发任务时仍然花费比预期更长的时间。

这是因为在`asyncio`代码的传统实现方案中，事件循环只能在一个核心上执行任务，其他核心处于空闲状态。

所以我为你实现了一个解决方案，可以在多个核心上分别调用每个事件循环，以并行执行并发任务。最终，它显著提高了代码性能。

由于我的能力有限，本文中的解决方案不可避免地存在一些不完善之处。我欢迎你的评论和讨论。我会积极为你解答。

使用`asyncio`在新代码中可以加速程序。但是在现实中，仍然有许多遗留系统。如何在这些遗留系统中集成`asyncio`并发代码将成为另一个重大挑战。如果你有兴趣了解更多，可以阅读我的下一篇文章：

[](/combining-traditional-thread-based-code-and-asyncio-in-python-dc162084756c?source=post_page-----1764404ce44f--------------------------------) ## 结合传统的基于线程的代码和 Python 中的 Asyncio

### 在 Python 中集成同步和异步编程的全面指南

[towardsdatascience.com

通过[加入 Medium](https://medium.com/@qtalen/membership)，你将可以无限制地访问我和其他成千上万作者的所有文章。只需一杯咖啡的价格，但对我来说是巨大的鼓励。

本文最初发布于：[`www.dataleadsfuture.com/harnessing-multi-core-power-with-asyncio-in-python/`](https://www.dataleadsfuture.com/harnessing-multi-core-power-with-asyncio-in-python/)
