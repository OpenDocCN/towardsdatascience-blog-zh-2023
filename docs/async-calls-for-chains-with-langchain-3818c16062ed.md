# LangChain 和 LLMs 的异步处理

> 原文：[https://towardsdatascience.com/async-calls-for-chains-with-langchain-3818c16062ed?source=collection_archive---------0-----------------------#2023-07-10](https://towardsdatascience.com/async-calls-for-chains-with-langchain-3818c16062ed?source=collection_archive---------0-----------------------#2023-07-10)

## 如何使 LangChain 链与异步调用的 LLMs 一起工作，从而加快运行顺序长链的时间

[](https://gabrielcassimiro17.medium.com/?source=post_page-----3818c16062ed--------------------------------)[![Gabriel Cassimiro](../Images/2cf8a09a706236059c46c7f0f20d4365.png)](https://gabrielcassimiro17.medium.com/?source=post_page-----3818c16062ed--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3818c16062ed--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3818c16062ed--------------------------------) [Gabriel Cassimiro](https://gabrielcassimiro17.medium.com/?source=post_page-----3818c16062ed--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3692fb93d7e5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fasync-calls-for-chains-with-langchain-3818c16062ed&user=Gabriel+Cassimiro&userId=3692fb93d7e5&source=post_page-3692fb93d7e5----3818c16062ed---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3818c16062ed--------------------------------) ·6分钟阅读·2023年7月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3818c16062ed&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fasync-calls-for-chains-with-langchain-3818c16062ed&user=Gabriel+Cassimiro&userId=3692fb93d7e5&source=-----3818c16062ed---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3818c16062ed&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fasync-calls-for-chains-with-langchain-3818c16062ed&source=-----3818c16062ed---------------------bookmark_footer-----------)![](../Images/3d661ccf3f4d03c45b31cfc182d4ecd0.png)

图片由[hp koch](https://unsplash.com/@iggii?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供，[Unsplash](https://unsplash.com/pt-br/fotografias/2OuTr9_VaUg?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在本文中，我将介绍如何使用异步调用 LLMs 来处理长工作流，使用 LangChain。我们将通过一个完整代码的示例进行讲解，并比较顺序执行与异步调用的差异。

这里是内容概览。如果你愿意，可以跳转到你感兴趣的部分：

1.  基础知识：什么是 LangChain

1.  如何使用 LangChain 运行同步链

1.  如何运行单个异步链与LangChain

1.  长工作流程中的实用技巧与异步链。

所以让我们开始吧！

# 基础知识：什么是LangChain

LangChain是一个基于语言模型开发应用程序的框架。这就是LangChain的官方定义。这个框架是最近创建的，已经被用作建立由LLM支持的工具的行业标准。

它是开源的，并且在非常快的时间框架内发布新功能。

官方文档可以在[这里](https://python.langchain.com/docs/get_started/introduction.html)找到，GitHub存储库可以在[这里](https://github.com/hwchase17/langchain)找到。

我们在这个库中遇到的一个缺点是，由于这些功能是新的，我们不能有效地使用Chat GPT来帮助构建新的代码。这意味着我们必须以“古老”的方式阅读文档、论坛和教程来工作。

LangChain的文档确实很好，但是关于某些特定事物的例子并不多。

我在异步长链方面遇到了这个问题。

这是我用来了解更多关于这个框架的主要资源：

1.  深度学习AI课程：[LangChain Chat with your data](https://www.deeplearning.ai/short-courses/langchain-chat-with-your-data/)；

1.  [官方文档](https://python.langchain.com/docs/get_started/introduction.html)；

1.  [YouTube频道](https://www.youtube.com/watch?v=_v_fgW2SkkQ&list=PLqZXAkvF1bPNQER9mLmDbntNfSpzdDIU5)。

（附注：它们都是免费的）

# 如何运行LangChain的同步链

所以让我说明我遇到的问题：我有一个包含大量行的数据框架，对于每一行，我需要运行多个提示（链）到一个LLM，并将结果返回到我的数据框架中。

当你有多个行时，比如1万行，每个行运行3个提示，并且每个响应（如果服务器没有超载）大约需要3-5秒，你最终会等待数天才能完成工作流程。

下面我将展示主要步骤和构建同步链的代码，并在数据子集上计时。

对于这个例子，我将使用数据集[Wine Reviews](https://www.kaggle.com/datasets/zynicide/wine-reviews)，[许可证](https://creativecommons.org/licenses/by-nc-sa/4.0/)。这里的目标是从书面评论中提取一些信息。

我想提取一份评论摘要、主要情感以及每种葡萄酒的前五个特征。

为此，我创建了两个链，一个用于摘要和情感，另一个使用摘要作为输入来提取特征。

这是运行它的代码：

运行时间（10个示例）：

> 摘要链（顺序执行）在22.59秒内执行完毕。
> 
> 特征链（顺序执行）在22.85秒内执行完毕。

如果你想更深入地了解我使用的组件，我真的推荐观看[深度学习AI课程](https://www.deeplearning.ai/short-courses/langchain-chat-with-your-data/)。

这段代码的主要收获是链的构建模块、如何以顺序方式运行它，以及完成此循环所需的时间。需要记住的是，对于 10 个示例，大约需要 45 秒，而完整的数据集包含 130K 行。因此，异步实现是以合理时间运行的“新希望”。

因此，既然问题已经设置并且基线已经确定，我们来看看如何优化这段代码以便更快地运行。

# 如何使用 LangChain 运行单个异步链

为此，我们将使用一种叫做异步调用的资源。为了说明这一点，我将首先简要解释代码的作用以及哪些地方的时间消耗过长。

在我们的示例中，我们遍历数据框的每一行，从行中提取一些信息，将它们添加到提示中，然后调用 GPT API 获取响应。收到响应后，我们解析它并将其重新添加到数据框中。

![](../Images/e446a0030466fef8a134fcceada0501a.png)

图片由作者提供

主要的瓶颈在于调用 GPT API，因为我们的计算机必须在等待该 API 响应时闲置（大约 3 秒）。其余的步骤都很快，可以继续优化，但这不是本文的重点。

那么，既然不需要闲等响应，我们为什么不同时发送所有 API 请求呢？这样我们只需等待一个响应，然后处理所有响应。这就是所谓的异步 API 调用。

![](../Images/e0b1422650d5840a248989d6ca91aedb.png)

图片由作者提供

这样，我们可以顺序地进行预处理和后处理，但对 API 的调用不必等到上一个响应回来后再发送下一个请求。

下面是异步链的代码：

在这段代码中，我们使用了 Python 的 async 和 await 语法。LangChain 也提供了运行异步链的代码，使用 arun() 函数。因此，开始时我们首先顺序处理每一行（可以优化），并创建多个“任务”来并行等待 API 响应，然后顺序处理响应以达到最终所需格式（也可以优化）。

运行时间（10 个示例）：

> 摘要链（异步）执行时间为 3.35 秒。
> 
> 特征链（异步）执行时间为 2.49 秒。

与顺序执行相比：

> 摘要链（顺序）执行时间为 22.59 秒。
> 
> 特征链（顺序）执行时间为 22.85 秒。

我们可以看到运行时间几乎提高了 10 倍。因此，对于大型工作负载，我强烈推荐使用这种方法。此外，我的代码中充满了可以进一步优化的 for 循环，以提升性能。

本教程的完整代码可以在这个 [Github Repo](https://github.com/gabrielcassimiro17/async-langchain) 中找到。

# 使用异步链的长工作流的实际建议。

当我运行这些代码时，遇到了一些限制和障碍，我想和你们分享一下。

## 笔记本电脑不支持异步操作

在Jupyter笔记本中运行异步调用时，你可能会遇到一些问题。然而，只需询问Chat GPT，它可能会帮助你解决这些问题。我编写的代码是为了在.py文件中运行大工作负载，所以在笔记本中运行时可能需要一些修改。

## 输出的键太多了

第一个问题是我的链有多个输出键，而当时的arun()只接受具有一个输出键的链。为了解决这个问题，我不得不将链拆分成两个独立的链。

## 不是所有链都可以异步处理

我在提示中使用了一个向量数据库进行示例和比较的逻辑，这需要将示例按顺序比较并添加到数据库中。这使得在整个链中使用异步处理变得不可行。

## 内容不足

针对这个特定的问题，我找到的最好内容是[官方异步文档](https://python.langchain.com/docs/modules/chains/how_to/async_chain)，并根据我的用例进行构建。所以如果你运行后发现新东西，记得与大家分享！

# 结论

LangChain是一个非常强大的工具，用于创建基于LLM的应用程序。我强烈推荐学习这个框架并参加上述提到的课程。

对于运行链的具体主题，对于高工作负载，我们看到异步调用的潜在改进，所以我的建议是花时间理解代码在做什么，并拥有一个模板类（如我提供的代码中），并以异步方式运行它！

对于小型工作负载或仅需要一次API调用的应用程序，通常不需要异步处理，但如果你有一个模板类，可以添加一个同步函数，以便于使用其中任何一个。

感谢阅读。

完整代码可以在[这里](https://github.com/gabrielcassimiro17/async-langchain)找到。

如果你喜欢这些内容并且想支持我，你可以请我喝咖啡：

[## Gabriel Cassimiro是一位分享免费内容的 数据科学家](https://www.buymeacoffee.com/cassimiro?source=post_page-----3818c16062ed--------------------------------)

### 嗨👋 我刚创建了一个页面。你现在可以请我喝咖啡了！

[在这里购买咖啡](https://www.buymeacoffee.com/cassimiro?source=post_page-----3818c16062ed--------------------------------)

这里有一些你可能感兴趣的其他文章：

[## 使用深度强化学习解决Unity环境问题](https://solving-unity-environment-with-deep-reinforcement-learning-836dc181ee3b?source=post_page-----3818c16062ed--------------------------------)

### 带有PyTorch实现深度强化学习代理的端到端项目代码。

[在这里购买咖啡](https://www.buymeacoffee.com/cassimiro?source=post_page-----3818c16062ed--------------------------------) [## 使用Tensorflow模型和OpenCV进行目标检测

### 使用训练好的模型来识别静态图像和实时视频中的对象

[towardsdatascience.com](/object-detection-with-tensorflow-model-and-opencv-d839f3e42849?source=post_page-----3818c16062ed--------------------------------) 
