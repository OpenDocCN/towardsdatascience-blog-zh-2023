# Python 中的并发与并行综合指南

> 原文：[`towardsdatascience.com/comprehensive-guide-to-concurrency-and-parallelism-in-python-415ee5fbec1a`](https://towardsdatascience.com/comprehensive-guide-to-concurrency-and-parallelism-in-python-415ee5fbec1a)

## 使用 multiprocessing、threading 和 asyncio

[](https://medium.com/@hrmnmichaels?source=post_page-----415ee5fbec1a--------------------------------)![Oliver S](https://medium.com/@hrmnmichaels?source=post_page-----415ee5fbec1a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----415ee5fbec1a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----415ee5fbec1a--------------------------------) [Oliver S](https://medium.com/@hrmnmichaels?source=post_page-----415ee5fbec1a--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----415ee5fbec1a--------------------------------) ·8 分钟阅读·2023 年 4 月 14 日

--

在这篇文章中，我们将详细介绍 Python 中的并发和并行。我们将介绍这些术语，并展示如何使用`multiprocessing`、`threading`和`asyncio`在 Python 中应用它们。我们将学习何时使用多个进程，何时使用多个线程，并为每种情况提供实际示例。

![](img/d46b0e529b898fbbd9fbb143c1f18a48.png)

图片来源于[Tomas Sobek](https://unsplash.com/@tomas_nz?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)的[Unsplash](https://unsplash.com/photos/plwud_FPvwU?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 概述

首先，从计算机科学的一般角度介绍标题中的两个术语：并发是指“同时”访问相同资源，例如 CPU 核心或磁盘——而并行则描述多个任务访问不同的资源，例如不同的 CPU 核心。

在 Python 中，通过`multiprocessing`包实现了并行——它允许创建多个独立的进程。并发可以通过`threading`包实现，允许创建不同的线程——或者通过`asyncio`，它遵循稍微不同的哲学。

它们的异同是什么？一个*进程*是由底层操作系统管理的独立进程。一个进程可以启动多个*线程*——线程可以被视为进程的一个子例程。默认情况下，进程是独立的实体，不共享内存或类似资源。它们的创建会产生开销，同时数据共享和传递也不是简单的，需要通过进程间通信（IPC）来管理。相比之下，线程是轻量级的，数据共享很容易，因为它们属于同一个进程和内存空间。

Python 与其他几种语言的不同之处在于，它使用了一个[全局解释器锁 (GIL)](https://betterprogramming.pub/pythons-gil-vs-c-with-mutexes-301b244e42)来管理对 Python 解释器的并发访问。这个锁在多线程应用程序中变得有效，确保一次只能有一个线程可以运行 Python 代码。因此，每个多线程 Python 应用程序实际上都是单核的！

有了这些，我们现在可以定义使用场景和建议：何时使用多处理，何时使用多线程：如果您的应用程序是 CPU 绑定的，意味着对 CPU 的访问是主要瓶颈，则使用多处理。只有这样，您的应用程序才能有效地同时使用多个核心，并加速可以在多个核心上并行的代码。缺点是创建进程的开销增加，以及如果数据需要共享则增加的复杂性。然而，除了 CPU 之外，还可能存在其他瓶颈，例如 I/O 绑定的应用程序。这些应用程序的特点是输入/输出操作的长时间等待，例如等待用户输入或等待 web 请求返回。如果是这种情况，多线程可能是更好的选择，因为我们可以避免多处理带来的开销，并原生共享数据。

接下来，我们将介绍这些概念，从多处理开始。然后，我们将使用多个线程实现相同的 CPU 绑定示例应用程序，并显示它确实一次只能限制一个核心。接着，我们给出一个使用 I/O 绑定应用程序的示例，并使用`threading`和`asyncio`来实现。

# **CPU 绑定应用程序**

为了在 Python 中实现并行，我们使用 `multiprocessing` 包。通过使用这个包，我们可以原生定义进程，通过 `Process` 类，然后简单地启动和停止它们。以下示例启动四个进程，每个进程都计算到 100000000。这意味着，该应用程序是 CPU 绑定的——CPU（核心）增量计数的速度越快，完成的速度就越快：

```py
from multiprocessing import Process
import time

MAX_COUNT = 100000000
NUM_PROCESSES = 4

def count(max_count: int) -> int:
    counter = 0
    for _ in range(max_count):
        counter += 1
    print("Finished")

if __name__ == "__main__":
    start_time = time.time()

    processes = [Process(target=count, args=(MAX_COUNT,)) for _ in range(NUM_PROCESSES)]

    for process in processes:
        process.start()

    for process in processes:
        process.join()

    print(f"Time elapsed: {time.time() - start_time}")
```

在我的笔记本电脑上，上面的程序执行大约需要 3 秒。

请注意，`__main__` 的检查在这种情况下很重要，因为新进程将基于相同的代码生成。如果没有检查，我们会遇到错误，因为这会触发无限循环。

## **多线程**

现在，让我们看看如何使用多线程来解决相同的问题。为此，我们可以简单地用 `threading.Thread` 替换 `multiprocessing.Process`，其他方面的代码基本上是类似的：

```py
import threading
import time

MAX_COUNT = 100000000
NUM_PROCESSES = 4

def count(max_count):
    counter = 0
    for _ in range(max_count):
        counter += 1
    print("Finished")

if __name__ == "__main__":
    start_time = time.time()

    threads = [
        threading.Thread(target=count, args=(MAX_COUNT,)) for _ in range(NUM_PROCESSES)
    ]

    for thread in threads:
        thread.start()

    for thread in threads:
        thread.join()

    print(f"Time elapsed: {time.time() - start_time}")
```

执行时，这段代码大约需要 10 秒才能完成——比使用不同进程时慢 3 倍。我们可以通过经验观察到 GIL 的作用，确实将并行代码执行限制在一个核心上，正如前面所述。

但现在，让我们稍微深入了解一下多处理，特别是使用一些更高层次的抽象来简化启动多个进程。

## **multiprocessing.Pool**

一种这样的抽象是 `multiprocessing.Pool`。这是一个方便的函数，用于生成一个工人/进程池，它会自动将给定的工作分配给彼此并执行：

```py
from multiprocessing import Pool

MAX_COUNT = 100000000
NUM_PROCESSES = 4

def count(max_count: int) -> int:
    counter = 0
    for _ in range(max_count):
        counter += 1
    print("Finished")
    return counter

if __name__ == "__main__":
    results = Pool(NUM_PROCESSES).map(count, [MAX_COUNT] * 5)
    print(results)
```

如我们所见，我们实例化一个 `Pool`，并设置要使用的工作者数量。建议将此数量设置为您拥有的 CPU 核心数（`os.cpu_count`）。然后我们使用 `map` 函数，将我们希望每个工作者运行的函数作为第一个参数传递，将输入数据作为第二个参数传递。在我们的案例中，这只是参数 `max_count`。由于我们传递了一个长度为 5 的数组，`count` 将运行 5 次。将池工作者数量设置为 4，结果是 2 个“周期”：在第一个周期中，工作者 0–3 处理前 4 个参数/数据集，在第二个周期中，完成得最早的工作者处理最后一个数据集。在这个示例中，`count` 还返回一个值，以展示 `map` 如何处理返回值。

**Pool.imap**

为了结束这一部分，让我们看看一个非常类似于 `map` 的函数，即 `imap`。`map` 和 `imap` 之间的区别在于任务处理的方式：`map` 将所有任务转换为一个列表，然后一次性将它们传递给工作者，将任务分解成块。另一方面，`imap` 逐个传递任务。这可能更节省内存，但也可能较慢。另一个区别是 `imap` 在每个任务完成后立即返回，而 `map` 会阻塞直到所有任务完成。我们可以这样使用 `imap`：

```py
results = Pool(NUM_PROCESSES).imap(count, [MAX_COUNT] * 5)
for result in results:
    print(result)
```

# I/O 绑定的应用程序

这总结了我们对并行性的介绍，它对于 CPU 绑定的应用程序非常有用并且推荐使用。现在，让我们谈谈那些不是 CPU 绑定的应用程序，特别是 I/O 绑定的应用程序。为此，我们使用并发。特别是，我们将通过一个示例来介绍这个概念，使用多线程，因为对于这种情况，使用多进程没有意义，因为：

+   这个应用程序不是 CPU 绑定的，因此多进程带来的额外开销不值得。

+   线程之间的通信会引入额外的开销和复杂性，当与多进程一起使用时尤为如此。

我们选择了以下示例来表示一个 I/O 绑定的应用程序：有一个发布者线程生成数据，在我们的示例中，这些数据来自用户。然后将有 N 个订阅者线程，它们等待数据中的特定条件，一旦这个条件激活，就开始执行一些涉及大量空闲操作的任务，即 CPU 不是瓶颈。

我们可以看到，对于这种情况，CPU 不是限制因素：大量时间将花费在等待输入上，并且随后的操作也不是 CPU 密集型的。然而，我们仍然希望/需要某种形式的并发——我们有不同的线程独立运行（生产者与订阅者），并且还希望执行后续任务时能够“并行”进行，而不是顺序执行。

因此，我们使用 `threading` 包，示例代码如下：

```py
import threading
import time

NUM_CONSUMERS = 2

condition_satisfied = False

lock = threading.Lock()

def producer() -> None:
    global condition_satisfied

    while True:
        user_input = input("Enter a comamnd:")
        if user_input == "Start":
            # Signal the producers to start
            lock.acquire()
            condition_satisfied = True
            lock.release()
            break
        else:
            print(f"Unknown command {user_input}")
        lock.release()
        time.sleep(1)

def consumer(consumer_idx: int) -> None:
    while True:
        lock.acquire()
        condition_satisfied_read = condition_satisfied
        lock.release()
        if condition_satisfied_read:
            for i in range(10):
                print(f"{i} from consumer {consumer_idx}")
                time.sleep(1)
            break
        time.sleep(1)

if __name__ == "__main__":
    threads = [threading.Thread(target=producer)] + [
        threading.Thread(target=consumer, args=(consumer_idx,))
        for consumer_idx in range(NUM_CONSUMERS)
    ]

    for thread in threads:
        thread.start()

    for thread in threads:
        thread.join()
```

生产者线程不断查询用户输入，一旦用户输入“开始”，一个标志会被切换，通知消费者开始。在消费者中，我们对此做出反应，并在收到信号后在 10 秒内计数到 10。

## asyncio

为了总结这篇文章，我们想快速介绍一下 `[asyncio](https://medium.com/towards-data-science/introduction-to-asyncio-57a5a1290ce0)` 以及如何在这种情况下使用它。对于完整的介绍，我建议参考链接中的文章。在这里，我们简单说一下 `asyncio` 可以被视为一种稍微不同的多线程代码编写/结构化范式——许多开发者在多种场景中更喜欢这种方式，主要是因为它的简洁性。`asyncio` 适用于 I/O 密集型应用，它采用了协程的概念：当一个协程在等待某个条件（如用户输入、网络请求完成等）时，它可以“让出”控制权，让另一个协程运行。

使用这个原则，上面的示例可以这样编写：

```py
import asyncio

NUM_CONSUMERS = 2

condition_satisfied = False
should_terminate = False

async def producer():
    global condition_satisfied

    while True:
        user_input = input("Enter a comamnd:")
        if user_input == "Start":
            # Signal the producers to start
            condition_satisfied = True
            break
        else:
            print(f"Unknown command {user_input}")
        await asyncio.sleep(1)

async def consumer(consumer_idx):
    global condition_satisfied

    while True:
        if condition_satisfied:
            for i in range(10):
                print(f"{i} from consumer {consumer_idx}")
                await asyncio.sleep(1)
            break
        await asyncio.sleep(1)

async def main():
    await asyncio.gather(producer(), consumer(0), consumer(1))

if __name__ == "__main__":
    asyncio.run(main())
```

请注意，在这种情况下不需要使用锁，因为我们决定何时让出执行并切换到不同的线程：我们仅在“等待”期间进行切换，这时没有共享变量被写入或访问。

# 结论

在这篇文章中，我们介绍了并行性和并发性概念，并描述了它们如何转换为 Python 中的多进程和多线程。

由于 GIL，Python 中的多线程应用实际上是单核的——因此，这种范式不适用于 CPU 密集型应用。对于这些应用，我们展示了如何管理多进程——然后通过将相同的示例重写为使用多个线程，经验性地证明了由于 GIL 导致的性能下降。

尽管如此，仍然有许多场景中多线程也是有益和必要的，特别是对于 I/O 密集型应用。对于这些应用，我们介绍了 Python 中的多线程，并通过将示例转换为 `asyncio` 这一稍微不同的多线程应用编写范式来结束。

这结束了本教程，希望对你有所帮助。下次见！
