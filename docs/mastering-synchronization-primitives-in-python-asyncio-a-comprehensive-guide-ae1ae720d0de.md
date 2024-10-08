# 精通 Python Asyncio 中的同步原语：全面指南

> 原文：[`towardsdatascience.com/mastering-synchronization-primitives-in-python-asyncio-a-comprehensive-guide-ae1ae720d0de`](https://towardsdatascience.com/mastering-synchronization-primitives-in-python-asyncio-a-comprehensive-guide-ae1ae720d0de)

## [PYTHON CONCURRENCY](https://medium.com/@qtalen/list/python-concurrency-2c979347da3b)

## asyncio.Lock、asyncio.Semaphore、asyncio.Event 和 asyncio.Condition 的最佳实践

[](https://qtalen.medium.com/?source=post_page-----ae1ae720d0de--------------------------------)![Peng Qian](https://qtalen.medium.com/?source=post_page-----ae1ae720d0de--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ae1ae720d0de--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ae1ae720d0de--------------------------------) [Peng Qian](https://qtalen.medium.com/?source=post_page-----ae1ae720d0de--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ae1ae720d0de--------------------------------) ·8 分钟阅读·2023 年 5 月 29 日

--

![](img/251106288dbd9d21a2c3fc7e21ad3d1d.png)

图片来源：由作者创建，[Canva](https://www.canva.com/)

这是我在 Python Concurrency 系列中的一篇文章，如果你觉得有用，可以从[这里](https://medium.com/@qtalen/list/python-concurrency-2c979347da3b)阅读其余部分。

# 介绍

在本文中，我将介绍为什么在 Python 的 asyncio 中需要同步原语，以及几种同步原语的最佳实践。在文章的最后部分，我将带你通过一个实际的同步原语示例进行讲解。

## 为什么你需要 asyncio 中的同步原语

任何使用过 Python 多线程的人都知道多个线程共享相同的内存块。因此，当多个线程同时对相同区域执行非原子操作时，会发生线程安全问题。

由于 asyncio 运行在单线程上，是否没有类似的线程安全问题？答案是否定的。

asyncio 中的并发任务是异步执行的，这意味着可能会有多个任务在时间上交替执行。当一个任务访问特定的内存区域并等待 IO 操作返回时，另一个任务也同时访问该内存区域，就会触发并发错误。

为了避免此类错误，Python asyncio 引入了一种类似于多线程的同步原语功能。

此外，为了避免过多任务同时访问资源，asyncio 的同步原语通过限制同时访问资源的任务数量来提供保护资源的能力。

接下来，让我们看看 asyncio 中有哪些同步原语可供使用。

# Python asyncio 的同步原语

## 锁

在介绍这个 API 之前，让我们看看一种情况：

假设我们有一个并发任务需要一份网站数据。它会首先检查缓存中是否有数据；如果有，它会从缓存中获取数据；如果没有，它会从网站中读取数据。

由于读取网站数据以返回并更新缓存需要一些时间，当多个并发任务同时执行时，它们都假设这些数据不在缓存中，并同时发起远程请求，如下代码所示：

![](img/cde3733f3dc4858fb66ff0d5981c8f61.png)

两个任务都认为缓存中没有数据，从而访问远程网站。作者提供的图片

这不符合我们最初的设计意图，因此`asyncio.Lock`派上了用场。

当并发任务需要首先获取锁时，我们可以检查缓存中是否有数据，其他未获取锁的任务将等待。直到获取锁的任务完成更新缓存并释放锁，其他任务才能继续执行。

整个流程图如下所示：

![](img/51c0871bb81b3888604eb3119ffbd307.png)

作者提供的图片

让我们看看如何编写代码：

![](img/dd1a36768aa16511094586a5e7cde0f0.png)

只有第一个任务需要更新缓存。作者提供的图片

问题解决了，是不是很简单？

## 信号量

有时，我们需要访问一个有限并发请求的资源。例如，一个特定的数据库只允许同时打开五个连接。或者根据你拥有的订阅类型，某个 Web API 仅支持一定数量的并发请求。

在这种情况下，你需要使用`asyncio.Semaphore`。`asyncio.Semaphore`使用一个内部计数器，每次获取 Semaphore 锁时计数器减少 1，直到计数器变为零。

![](img/63854ad79766fe71fbaac455157de246.png)

信号量将限制并发任务的数量。作者提供的图片

当`asyncio.Semaphore`的计数器为零时，其他需要锁的任务将会等待。在执行其他任务后调用释放方法时，计数器会增加 1，等待的任务可以继续执行。

代码示例如下：

这样，我们可以限制同时访问的连接数量。

## 有界信号量

有时，由于代码的限制，我们不能使用`async with`来管理获取和释放信号量锁，因此我们可能会在某处调用`acquire`，在另一处调用`release`。

如果我们不小心多次调用`asyncio.Semaphore`的`release`方法，会发生什么？

如代码所示，我们限制同时运行两个任务，但由于我们调用了多次释放，我们可以在下次同时运行三个任务。

为了解决这个问题，我们可以使用`asyncio.BoundedSemaphore`。

从源代码中我们可以知道，当调用`release`时，如果计数器的值大于初始化时设置的值，将抛出`ValueError`。

![](img/fced96224eb387b31f241903ccbe361c.png)

当我们多次调用释放方法时，会抛出 ValueError 异常。图片来源于作者

因此，问题正在得到解决。

## 事件

`Event`维护一个内部布尔变量作为标志。`asyncio.Event`有三个常用方法：`wait`、`set`和`clear`。

当任务运行到`event.wait()`时，任务处于等待状态。此时，可以调用`event.set()`将内部标记设置为 True，所有等待的任务可以继续执行。

当任务完成时，需要调用`event.clear()`方法将标记的值重置为 False，以将事件恢复到初始状态，并且下次可以继续使用事件。

我将展示如何使用`Event`在文章末尾实现一个事件总线，而不是示例代码。

## 条件

`asyncio.Condition`类似于`asyncio.Lock`和`asyncio.Event`的结合体。

首先，我们将使用`async with`确保获取条件锁，然后调用`condition.wait()`释放条件锁，使任务暂时等待。当`condition.wait()`返回时，我们重新获取条件锁，以确保只有一个任务同时执行。

当一个任务通过`condition.wait()`暂时释放锁并进入等待状态时，另一个任务可以通过`async with`获取条件锁，并通过`condition.notify_all()`方法通知所有等待的任务继续执行。

流程图如下：

![](img/6caa99a220d9de39f31f674777e80e82.png)

asyncio.Condition 的工作流程。图片来源于作者

我们可以通过一段代码演示`asyncio.Condition`的效果：

有时，我们需要`asyncio.Condition`等待特定事件发生后才能继续执行下一步。我们可以调用`condition.wait_for()`方法，并将一个方法作为参数传递。

每次调用`condition.notify_all`时，`condition.wait_for`会检查参数方法的执行结果，如果结果为 True，则结束等待；如果结果为 False，则继续等待。

我们可以通过一个示例来演示`wait_for`的效果。在以下代码中，我们将模拟一个数据库连接。在执行 SQL 语句之前，代码会检查数据库连接是否已经初始化，如果连接初始化完成，则执行查询；否则，等待直到连接初始化完成：

# 使用同步原语的一些提示

## 记得在需要时使用超时或取消。

在使用同步原语时，我们通常是在等待特定 IO 操作的完成。然而，由于网络波动或其他未知原因，任务的 IO 操作可能比其他任务花费更长时间。

在这种情况下，我们应该为操作设置一个超时，以便在执行时间过长时，能够释放锁并允许其他任务及时执行。

在另一个情况下，我们可能会循环执行一个任务。它可能会使一些任务在后台等待，阻止程序正确结束。这时，记得使用 cancel 来终止任务的循环执行。

## 避免使用同步原语或只锁定最少的资源

我们都知道 asyncio 的优势在于任务可以在等待 IO 返回时切换到另一个任务执行。

但一个 asyncio 任务通常同时包含 IO 绑定操作和 CPU 绑定操作。如果我们在任务上锁定了太多代码，它将无法及时切换到另一个任务，从而影响性能。

因此，如果没有必要，尽量不要使用同步原语或仅锁定最少的资源。

## 为了避免一些其他竞争锁定的情况

asyncio 中没有 RLock，因此在递归代码中不要使用锁。

与多线程一样，asyncio 也有死锁的可能性，因此尽量避免同时使用多个锁。

# 实践中的高级技术：基于 asyncio 的 Event Bus

在文章前面的介绍之后，我相信你对如何正确使用 asyncio 的同步原语有了清晰的理解。

接下来，我将通过带你实现一个事件总线，教你如何在实际项目中使用同步原语。

像往常一样，作为架构师的第一步是设计 EventBus API。

由于 `EventBus` 使用字符串进行通信，并且在内部，我打算使用 `asyncio.Event` 来实现与每个字符串对应的事件，我们将从实现一个 `_get_event` 方法开始：

`on` 方法将回调函数绑定到特定事件上：

`trigger` 方法可以手动触发事件并传入相应的数据：

最后，让我们编写一个 `main` 方法来测试 EventBus 的效果：

在主方法结束时，记得使用超时来防止程序一直执行，就像我之前警告的那样。

![](img/0b9ddc729f4589a233c171e72f65af9e.png)

代码按预期执行。图片来自作者

如你所见，代码按预期执行了。是不是很简单？

# 结论

本文首先介绍了为什么 Python asyncio 需要同步原语。

接着，我介绍了 Lock、Semaphore、Event 和 Condition 的最佳实践，并给出了一些正确使用它们的提示。

最后，我完成了一个关于 asyncio 同步原语的动手训练的小项目，希望能帮助你更好地在实际项目中使用同步原语。

随时评论、分享或与我讨论关于 asyncio 的话题。

你可以通过我的文章列表获取更多关于 Python 并发的知识：

![Peng Qian](img/fa6bd24b4781f623be8ea40c4e6bdb78.png)

[Peng Qian](https://qtalen.medium.com/?source=post_page-----ae1ae720d0de--------------------------------)

## Python 并发

[查看列表](https://qtalen.medium.com/list/python-concurrency-2c979347da3b?source=post_page-----ae1ae720d0de--------------------------------) 10 个故事！用 Aiomultiprocess 超级充实你的 Python Asyncio：全面指南![释放 Python Asyncio 队列的力量](img/aa5886c47ef891be14eb17f9a2ed3d0d.png)![](img/d5b38ed916e599eb0673eb311f95348d.png)[](https://qtalen.medium.com/membership?source=post_page-----ae1ae720d0de--------------------------------) [## 通过我的推荐链接加入 Medium - Peng Qian

### 阅读 Peng Qian（以及 Medium 上成千上万其他作者）的每一个故事。你的会员费直接支持 Peng…

qtalen.medium.com](https://qtalen.medium.com/membership?source=post_page-----ae1ae720d0de--------------------------------)

这篇文章最初发布于：[`www.dataleadsfuture.com/mastering-synchronization-primitives-in-python-asyncio-a-comprehensive-guide/`](https://www.dataleadsfuture.com/mastering-synchronization-primitives-in-python-asyncio-a-comprehensive-guide/)
