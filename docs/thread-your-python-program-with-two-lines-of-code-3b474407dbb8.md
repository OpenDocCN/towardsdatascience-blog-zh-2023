# 用两行代码将你的 Python 程序多线程

> 原文：[`towardsdatascience.com/thread-your-python-program-with-two-lines-of-code-3b474407dbb8`](https://towardsdatascience.com/thread-your-python-program-with-two-lines-of-code-3b474407dbb8)

## 通过同时做多件事来加速你的程序

[Mike Huls](https://mikehuls.medium.com/?source=post_page-----3b474407dbb8--------------------------------) ![Mike Huls](https://mikehuls.medium.com/?source=post_page-----3b474407dbb8--------------------------------) ![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3b474407dbb8--------------------------------) [Mike Huls](https://mikehuls.medium.com/?source=post_page-----3b474407dbb8--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----3b474407dbb8--------------------------------) ·阅读时间 8 分钟·2023 年 1 月 10 日

--

![](img/a45a0f3dc3164d2dedb79a814dca0124.png)

更好地组织我们的线程（图片来自[Karen Penroz](https://unsplash.com/@penrosekaren)在[Unsplash](https://unsplash.com/photos/06ZTGDcAQFs)）

当你的程序有很多涉及等待的任务时，你可以通过同时执行这些任务来加速程序，而不是一个一个地处理。比如在做早餐时，你不会等咖啡机完成后再煮鸡蛋。相反，你会启动咖啡机，同时给自己倒一杯橙汁，并加热平底锅以煎炒鸡蛋。

这篇文章向你展示如何做到这一点。最后你将能够**安全地在两行**代码中**应用线程**，并在程序中实现**巨大的加速**。让我们开始编程吧！

# 但首先……

本文将详细介绍如何通过对整列表参数应用相同的函数来应用线程。然后我们将查看如何以线程方式应用不同的函数。

[## Cython 入门：两步实现 30 倍速度提升

### 轻松的 Python 代码编译，打造飞快的应用程序

[前往数据科学](https://towardsdatascience.com/?source=post_page-----3b474407dbb8--------------------------------)的文章链接

## 线程能解决我的问题吗？理解并发

确实在许多情况下，通过“同时做多件事”可以加速你的程序，但盲目地在各处应用线程并不是一个明智的解决方案。在 Python 中有两种多任务处理的方法：多进程和线程：

+   **线程**以**并发**方式运行代码：我们有**一个活跃的 CPU**，它在多个线程之间快速切换

+   **多进程**以**并行**的方式运行代码：我们有**多个活动的 CPU** 每个都运行自己的代码（请查看下面的文章）

[## Applying Python multiprocessing in 2 lines of code](https://towardsdatascience.com/applying-python-multiprocessing-in-2-lines-of-code-3ced521bac8f?source=post_page-----3b474407dbb8--------------------------------)

### 何时以及如何使用多个核心以更快地执行多次任务

[## Applying Python multiprocessing in 2 lines of code](https://towardsdatascience.com/applying-python-multiprocessing-in-2-lines-of-code-3ced521bac8f?source=post_page-----3b474407dbb8--------------------------------)

在线程处理时，你有一个执行所有任务的角色，通过在任务之间切换来同时完成它们。在早餐示例的上下文中：有一个角色（你）在咖啡机、锅和橙汁杯之间切换。

在多进程中，你激活多个任务，每个任务都被分配一个任务。在早餐的类比中，这就像把自己克隆两次，并给每个克隆分配一个单独的任务。虽然它比一个一个地运行任务要快得多，但多进程有更多的开销；克隆自己是一项很大的工作，只是为了让克隆在等待锅加热！

简而言之：**多进程**在需要**计算**大量数据时是最佳解决方案，**线程**则更适合需要**等待**的情况。

在这篇文章中，我们将专注于线程处理；如果你对多进程感兴趣，请查看下面的文章：

[## Multi-tasking in Python: Speed up your program 10x by executing things simultaneously](https://towardsdatascience.com/multi-tasking-in-python-speed-up-your-program-10x-by-executing-things-simultaneously-4b4fc7ee71e?source=post_page-----3b474407dbb8--------------------------------)

### 逐步指南，以应用线程和进程来加速你的代码

[## Multi-tasking in Python: Speed up your program 10x by executing things simultaneously](https://towardsdatascience.com/multi-tasking-in-python-speed-up-your-program-10x-by-executing-things-simultaneously-4b4fc7ee71e?source=post_page-----3b474407dbb8--------------------------------)

# 设置

对于这篇文章，我们假设一个旅行程序接收到一个需要验证的大型电子邮件地址列表。假设我们设置了一个 API，可以发送一个电子邮件地址，并根据电子邮件地址的有效性返回真/假。

最重要的是，我们需要发送请求并等待 API 响应。这是一个典型的可以多线程处理的任务：我们不需要额外的核心来加速计算；我们只需要一些额外的线程来同时发送多个电子邮件地址。

对于这篇文章，我们将使用这个电子邮件地址列表：

```py
email_addresses = [
  'mikehuls42@gmail.com',
  'mike@mikehuls.com',
  'johndoe@some_email.com',
  'obviously_wrong@address',
  'otheraddress.com',
  'thisis@@wrong.too',
  'thisone_is@valid.com'
]
```

这将是我们的函数，它模拟将电子邮件地址发送到验证 API：

```py
def send_email_address_to_validation_api(email_address:str):
  # We'll simulate the request to the validation API by just sleeping between 1 and 2 seconds
  sleep_time = random.random() + 1
  time.sleep(sleep_time)
  # Randomly return a true / false depending on the sleep_time
  return sleep_time > 1.5
```

[## Docker for absolute beginners: the difference between an image and a container](https://towardsdatascience.com/docker-for-absolute-beginners-the-difference-between-an-image-and-a-container-7e07d4c0c01d?source=post_page-----3b474407dbb8--------------------------------)

### 了解 Docker 镜像和容器的区别 + 实用代码示例

towardsdatascience.com](/docker-for-absolute-beginners-the-difference-between-an-image-and-a-container-7e07d4c0c01d?source=post_page-----3b474407dbb8--------------------------------)

# A. 非线程

首先，我们来看一下在不使用线程的情况下如何使用这个函数。

## 遍历电子邮件地址

我们将遍历我们的 7 个电子邮件地址列表，将每个值发送到 API；简单明了：

```py
for email_address in email_addresses:
  is_valid = send_email_address_to_validation_api(email_address=email_address)
  # do other stuff with the email address and validity
```

这很容易理解，但它快吗？（剧透：不快）。由于我们依次验证每个 7 个电子邮件地址，每个地址需要 1 到 2 秒，因此总共需要 7 到 14 秒。我测得的时间是**11.772 秒**。

[](/image-analysis-for-beginners-destroying-duck-hunt-with-opencv-e19a27fd8b6?source=post_page-----3b474407dbb8--------------------------------) ## 用 OpenCV 摧毁《鸭子猎人》 — 初学者的图像分析

### 编写代码以打破每一个《鸭子猎人》的高分

towardsdatascience.com

## 使用映射函数

为了更好地理解下一部分，我们将使用 Python 的`map`函数重写上面的代码：

```py
results: [bool] = map(send_email_address_to_validation_api, email_addresses)
```

上面的代码完全一样；它将函数映射到地址列表，这意味着它对`email_addresses`列表中的每个值执行该函数。

让我们将时间添加到基准测试中：

```py
NON THREADED           11.772 secs
```

[](/why-is-python-so-slow-and-how-to-speed-it-up-485b5a84154e?source=post_page-----3b474407dbb8--------------------------------) ## 为什么 Python 如此慢以及如何加快速度

### 检查一下 Python 的瓶颈在哪里

towardsdatascience.com

# B. 使用线程

在这一部分，我们检查 3 种不同的将线程应用到我们函数的方法。所有这些方法都使用线程池，可以通过以下方式导入：

```py
from multiprocessing.pool import ThreadPool
```

将线程池视为等待任务的线程集合。线程池具有一个`map`函数，我们可以像在上面的非线程示例中一样使用它。一旦线程完成任务，它会返回到线程池中，等待另一个任务。

线程池允许我们通过提供对线程池中线程数量的限制，轻松且安全地使用线程

[](/python-to-sql-upsert-safely-easily-and-fast-17a854d4ec5a?source=post_page-----3b474407dbb8--------------------------------) ## Python 到 SQL — 安全、简单且快速的 UPSERT

### 使用 Python 进行闪电般快速的插入和/或更新

towardsdatascience.com

## 1\. 线程池映射

我们将首先切换到线程池提供的`map`函数。

```py
with ThreadPool(processes=10) as t_pool:
  results = t_pool.map(send_email_address_to_validation_api, email_addresses)
```

我们定义了一个最大进程数为 10 的线程池。由于这个原因，`map` 函数会同时启动所有对函数的调用。一旦所有工作线程完成，我们就可以评估结果，在这种情况下是**1.901 秒**。

```py
NON THREADED           11.772 secs
THREADED MAP            1.901 secs
```

[](/create-and-publish-your-own-python-package-ea45bee41cdc?source=post_page-----3b474407dbb8--------------------------------) ## 创建并发布自己的 Python 包

### 关于如何使用 pip 安装你自制包的简短而简单的指南

towardsdatascience.com

## 2\. 线程池 imap

在之前的示例中，我们不得不等待所有函数调用完成。如果我们使用 `imap` 而不是 `map`，情况就不同了。`imap` 函数返回一个迭代器，我们可以在结果可用时立即访问：

```py
strt_time_t_imap = time.perf_counter()
with ThreadPool(processes=10) as t_pool:
  for res in t_pool.imap(send_email_address_to_validation_api, email_addresses):
    print(time.perf_counter() - strt_time_t_imap, 'seconds')
```

上述代码几乎完全相同。唯一的区别是添加了一些计时代码。此外，我们显然在第 3 行的 `t_pool` 上使用了 `imap` 函数。

如果我们查看打印结果，我们会看到：

```py
1.4051628 seconds
1.4051628 seconds
1.7985222 seconds
1.7985749 seconds
1.7985749 seconds
1.7985957 seconds
1.7986305 seconds
```

`imap` 函数返回一个迭代器，我们可以在结果完成后立即访问。不过，这些结果是***按顺序***返回的。这意味着例如第二个电子邮件地址必须等待第一个；如果第二个电子邮件地址在 1.3 秒内完成，第一个电子邮件地址在 1.4 秒内完成；两者都在 1.4 秒后返回（正如你在上面的打印输出中看到的）。

尽管验证完整的电子邮件地址列表所需的时间与之前的示例大致相同，但我们可以更快地获取结果！第一个结果在**1.4 秒**后即可访问！

```py
NON THREADED           11.772 secs
THREADED MAP            1.901 secs
THREADED IMAP           1.901 secs(first result accessible after 1.4  secs)
```

[](https://mikehuls.medium.com/virtual-environments-for-absolute-beginners-what-is-it-and-how-to-create-one-examples-a48da8982d4b?source=post_page-----3b474407dbb8--------------------------------) [## 针对绝对初学者的虚拟环境——什么是它以及如何创建一个（+ 示例）

### 深入了解 Python 虚拟环境、pip 和避免复杂的依赖关系

mikehuls.medium.com](https://mikehuls.medium.com/virtual-environments-for-absolute-beginners-what-is-it-and-how-to-create-one-examples-a48da8982d4b?source=post_page-----3b474407dbb8--------------------------------)

## 3\. 线程池 imap_unordered

另一个改进：我们将返回无序的迭代器，而不是按顺序返回：

```py
strt_time_t_imap = time.perf_counter()
with ThreadPool(processes=10) as t_pool:
  for res in t_pool.imap_unordered(send_email_address_to_validation_api, email_addresses):
    print(time.perf_counter() - strt_time_t_imap, res)
```

使用上述代码，我们可以在结果可用时立即访问。你也可以在打印输出中看到这一点：

```py
1.0979514 seconds
1.2382307 seconds
1.3781070 seconds
1.4730333 seconds
1.7439070 seconds
1.7909826 seconds
1.9953354 seconds
```

最后的电子邮件地址可能在**1.09 秒**内完成并最先返回。这非常方便。

```py
NON THREADED           11.772 secs
THREADED MAP            1.901 secs
THREADED IMAP           1.901 secs(first result accessible after 1.4  secs)
THREADED IMAP_UNORDERED 1.901 secs(first result accessible after 1.09 secs)
```

[](/create-a-fast-auto-documented-maintainable-and-easy-to-use-python-api-in-5-lines-of-code-with-4e574c00f70e?source=post_page-----3b474407dbb8--------------------------------) ## 创建一个快速自动文档生成、可维护且易于使用的 Python API，只需 5 行代码

### 非经验丰富的开发者只需一个完整、有效、快速且安全的 API，这非常适合他们。

[towardsdatascience.com

## 4\. 不同的函数

在前面的例子中，我们探讨了如何以线程方式应用相同的函数，但如果我们有多个函数呢？在下面的示例中，我们模拟加载一个网页。我们有不同的函数用于加载横幅、广告、帖子以及当然，还有吸引点击的内容：

```py
def load_ad():
    time.sleep(1)
    return "ad loaded"
```

```py
def load_clickbait():
    time.sleep(1.5)
    return "clickbait loaded"def load_banner():
    time.sleep(2)
    return "banner loaded"def load_posts():
    time.sleep(3)
    return "posts loaded"
```

如果我们顺序运行这些函数，我们的程序将需要大约 **7.5 秒**。我们可以通过稍微调整来使用线程池及其 `map`、`imap` 和 `imap_unordered` 函数。请参见下面的 `imap_unordered` 示例：

```py
with ThreadPool(processes=4) as t_pool:  # limit to 4 processes as we only need to execute 
  results = t_pool.imap_unordered(lambda x: x(), [load_ad, load_posts, load_banner, load_clickbait])
```

如你所见，我们将函数列表映射到一个 lambda 函数。函数列表由 lambda 函数执行（`x` 是每个函数的占位符，`x()` 将执行它）。以这种方式执行，渲染网页仅需 **3.013 秒**。

[](/git-for-absolute-beginners-understanding-git-with-the-help-of-a-video-game-88826054459a?source=post_page-----3b474407dbb8--------------------------------) ## Git 入门：通过视频游戏了解 Git

### 对如何使用 git 的经典 RPG 类比获得直观的理解

[towardsdatascience.com

# 结论

使用线程池进行多线程处理是安全且易于应用的。总结：multiprocessing 库的 Pool 对象提供了三个函数。`map` 是 Python 内置 `map` 的并发版本。`imap` 函数返回一个有序迭代器，访问结果是阻塞的。`imap_unordered` 函数返回一个无序迭代器，使得可以在每个结果完成后立即访问，而无需等待另一个函数先完成。

我希望这篇文章的解释清晰明了，但如果不是，请告诉我我可以做什么进一步澄清。同时，查看我 [其他文章](https://mikehuls.com/articles?tags=python)，涉及各种编程相关主题：

+   [Git 入门：通过视频游戏了解 Git](https://mikehuls.medium.com/git-for-absolute-beginners-understanding-git-with-the-help-of-a-video-game-88826054459a)

+   [创建并发布你自己的 Python 包](https://mikehuls.medium.com/create-and-publish-your-own-python-package-ea45bee41cdc)

+   [使用 FastAPI 在 5 行代码中创建一个快速的自动文档、可维护且易于使用的 Python API](https://mikehuls.medium.com/create-a-fast-auto-documented-maintainable-and-easy-to-use-python-api-in-5-lines-of-code-with-4e574c00f70e)

编程愉快！

— Mike

*附言：喜欢我做的事吗？* [*关注我！*](https://mikehuls.medium.com/membership)

[](https://mikehuls.medium.com/membership?source=post_page-----3b474407dbb8--------------------------------) [## 使用我的推荐链接加入 Medium — Mike Huls

### 阅读 Mike Huls 的每一个故事（以及 Medium 上其他成千上万位作家的故事）。你的会员费用直接支持 Mike…

[mikehuls.medium.com](https://mikehuls.medium.com/membership?source=post_page-----3b474407dbb8--------------------------------)
