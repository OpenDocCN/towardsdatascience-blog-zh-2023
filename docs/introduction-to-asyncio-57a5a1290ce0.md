# asyncio 介绍

> 原文：[`towardsdatascience.com/introduction-to-asyncio-57a5a1290ce0`](https://towardsdatascience.com/introduction-to-asyncio-57a5a1290ce0)

## 用 Python 管理 I/O 绑定的并发

[](https://medium.com/@hrmnmichaels?source=post_page-----57a5a1290ce0--------------------------------)![Oliver S](https://medium.com/@hrmnmichaels?source=post_page-----57a5a1290ce0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----57a5a1290ce0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----57a5a1290ce0--------------------------------) [Oliver S](https://medium.com/@hrmnmichaels?source=post_page-----57a5a1290ce0--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----57a5a1290ce0--------------------------------) ·阅读时间 6 分钟·2023 年 3 月 24 日

--

并发和并行指的是程序或计算机同时运行多个操作的能力。通常，我们区分多处理（并行）和多线程（并发）——前者描述运行多个进程，而后者指的是在同一个进程内生成多个线程。在 Python 中，由于[全局解释器锁](https://realpython.com/python-gil/)（GIL），一次只能执行一个线程，导致任何多线程应用实际上都是单核的。不过，例如对于 I/O 绑定的程序（即更多时间花在等待输入上——这才是瓶颈而非计算），在 Python 中使用多线程仍然是有意义的。在这篇文章中，我们将介绍[asyncio](https://docs.python.org/3/library/asyncio.html)，这是一种优雅且简便的方法来实现这一点。

![](img/56706dacea79f62fb82537889418d6a7.png)

[Gabriel Gusmao](https://unsplash.com/@gcsgpp?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在[Unsplash](https://unsplash.com/photos/pMmw3ynuXHw?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄的照片

# 为什么选择 asyncio？

我们将指定一篇未来的文章来讨论多处理和多线程的区别，以及 asyncio 在其中的作用。不过我们这里想要激励大家使用它。如上所述，多线程特别适用于 I/O 绑定的，即非 CPU 绑定的应用。而 asyncio 是实现这一点的一种方式，许多开发者在某些用例中更倾向于使用它。

asyncio 对于 I/O 绑定的程序非常出色，我们希望编写易于理解、结构清晰、抗错误的代码。asyncio 实际上只使用一个线程，并且由用户决定当前执行的代码块何时“让出”执行以等待外部输入，并允许执行不同的代码块。这通常使代码更易于阅读，同时也给予用户更多控制，减少死锁等风险。

# 激励示例

让我们从一个入门示例开始。考虑以下代码：

```py
import time

def sleepy_function():
    print("Before sleeping.")
    time.sleep(1)
    print("After sleeping.")

def main():
    for _ in range(3):
        sleepy_function()

main()
```

在其中，我们调用了三次 `sleepy_function()`，它简单地打印两件事，然后睡眠一秒。总的来说，这个程序需要 3 秒钟来执行。但当然，让 CPU 等待 1 秒而不做其他工作是相当浪费的。因此，进行并行化是很自然的 —— 这正是 I/O 绑定程序的定义。

使用 asyncio，以下代码可以并行化如下：

```py
import asyncio

async def sleepy_function():
    print("Before sleeping.")
    await asyncio.sleep(1)
    print("After sleeping.")

async def main():
    await asyncio.gather(*[sleepy_function() for _ in range(3)])

asyncio.run(main())
```

请注意，asyncio 仅在 Python 3.4 及以上版本中可用，并且在这些版本中包含在标准库中。

接下来，我们将更详细地查看所使用的语法和 asyncio 一般情况。

# 使用 asyncio

asyncio 的核心概念是协程和任务。协程基本上是函数，它们可以将执行权交给控制 asyncio 线程（例如，等待输入），而不会丢失其状态（有点类似于 [生成器表达式](https://medium.com/@hrmnmichaels/generators-and-generator-expressions-in-python-a8d2e700945e)）。因此，在这种形式的并发中，用户完全控制线程之间（协程/任务）的上下文切换发生的时机，而不是像经典的多线程那样由操作系统决定。由于这一点，以及 asyncio 只使用一个线程 —— asyncio 代码更不易出错，并且我们（通常）不需要使用任何锁定机制，如锁或互斥量。

协程使用关键字 async 定义：

```py
async def sample_coroutine():
    print("Inside sample_coroutine")
```

现在仅仅像平常一样调用 `sample_coroutine()` 不够 —— 这将不起作用。相反，我们必须指示 asyncio 执行此操作。我们可以通过以下方式实现：

```py
asyncio.run(sample_coroutine())
```

下一个重要的关键字是 `await`。这向 asyncio 发出信号，表示我们想要等待某个事件，并且它可以继续执行代码的另一部分。我们可以 `await` 的任何东西都被称为 `Awaitable`，协程和任务就是两个例子。

```py
import asyncio

async def sample_coroutine_2():
    print("Inside sample_coroutine_2")

async def sample_coroutine():
    print("Inside sample_coroutine")
    await asyncio.sleep(1)
    await sample_coroutine_2()

asyncio.run(sample_coroutine())
```

在上述示例中，我们首先使用 `asyncio.sleep` 等待 1 秒 —— 然后等待 `sample_coroutine_2`。注意这个程序的执行时间仍然是 1 秒，并没有更少 —— 但缩短执行时间不是这个简单示例的重点。

## 任务

如果我们现在想要并行执行不同的代码块，我们可以使用任务。通过 `asyncio.create_task` 我们可以从协程创建一个任务，然后通过 `await` 执行它。这样，我们可以并行运行多个协程：

```py
import asyncio

async def sleepy_function():
    print("Before sleeping.")
    await asyncio.sleep(1)
    print("After sleeping.")

async def main():
    task1 = asyncio.create_task(sleepy_function())
    task2 = asyncio.create_task(sleepy_function())
    task3 = asyncio.create_task(sleepy_function())

    await task1
    await task2
    await task3

asyncio.run(main())
```

## gather

然而，我通常发现使用以下涉及`asyncio.gather`的语法更加方便和直观，它可以接受任意多个`Awaitables`作为输入：

```py
import asyncio

async def sleepy_function():
    print("Before sleeping.")
    await asyncio.sleep(1)
    print("After sleeping.")

async def main():
    await asyncio.gather(
        sleepy_function(), sleepy_function(), sleepy_function()
    )

asyncio.run(main())
```

我们可以通过使用星号（*）运算符来缩短上述代码，该运算符将给定的可迭代对象解包为单个参数（从而缩小了与介绍示例之间的差距）：

```py
import asyncio

async def sleepy_function():
    print("Before sleeping.")
    await asyncio.sleep(1)
    print("After sleeping.")

async def main():
    await asyncio.gather(*[sleepy_function() for _ in range(3)])

asyncio.run(main())
```

## Queue

让我们通过介绍`asyncio.Queue`来结束这篇文章，它是一个适用于“多线程” asyncio 应用程序的安全队列，是许多实际应用场景中的有用工具。我们将用它来进行一个稍微复杂一点的示例，希望通过一个更真实的“实际”示例来结束这篇文章。

我们的示例由一个生产者线程和 N 个消费者线程组成。生产者尝试猜测[ISBN 号码](https://en.wikipedia.org/wiki/ISBN)，并将其放入队列中。消费者从队列中移除这些号码，并对返回关于该 ISBN 书籍信息的公共端点发起[GET 请求](https://www.w3schools.com/tags/ref_httpmethods.asp)。如果消费者猜对了 ISBN，我们会打印返回的书籍信息。

为了以异步模式执行请求，我们使用[aiohttp](https://docs.aiohttp.org/en/stable/)。Web 请求通常是 asyncio 的常见主题和用例：通常，一个网络服务器将并发运行不同的请求，渲染/查询不同的内容，然后返回结果网页。由于这不是 CPU 密集型的，大多数时间（可能）都花在等待请求结果上，因此 asyncio 非常适合这里。

让我们看看代码：

```py
import asyncio
import threading
from random import randint

import aiohttp

NUM_CONSUMERS = 8

async def producer(queue: asyncio.Queue) -> None:
    while True:
        random_isbn = "".join(["{}".format(randint(0, 9)) for _ in range(0, 10)])
        queue.put_nowait(random_isbn)
        await asyncio.sleep(0.05)

async def consumer(consumer_idx: int, queue: asyncio.Queue) -> None:
    while True:
        isbn = await queue.get()
        async with aiohttp.ClientSession() as session:
            async with session.get(
                "https://openlibrary.org/api/books?bibkeys=ISBN:"
                + isbn
                + "&format=json"
            ) as resp:
                book_descriptor = await resp.json()
                if book_descriptor != {}:
                    print(
                        f"Consumer {consumer_idx} found valid ISBN. Current queue size: {queue.qsize()}. Discovered book: {book_descriptor}"
                    )
        queue.task_done()

async def main():
    queue = asyncio.Queue()
    await asyncio.gather(
        *([producer(queue)] + [consumer(idx, queue) for idx in range(NUM_CONSUMERS)])
    )

if __name__ == "__main__":
    asyncio.run(main())
```

除了上述内容外，值得指出的是如何使用异步队列。正如我们所见，我们通过`put_nowait()`将项目放入队列。`_nowait`后缀仅意味着当队列已满时不等待——这对我们来说不重要，因为我们没有对队列大小设限。

项目通过`get()`从队列中弹出——但需要通过相应的`tasks_done()`来标记完成！

请注意，我调整了消费者的数量和生成 ISBN 之间的休眠时间，使得在我的笔记本电脑上队列大小保持相对恒定。

**这标志着我们对 asyncio 教程的结束。希望这次阅读对你有趣，谢谢你的光临！**

PS：如果你偶然发现了使用上一个示例程序的任何有趣阅读，请告诉我！
