# 如何使用 Llama2 和 LangChain 构建本地聊天机器人

> 原文：[`towardsdatascience.com/can-a-llama-2-powered-chatbot-be-trained-on-a-cpu-ce9ec6ebe3c2`](https://towardsdatascience.com/can-a-llama-2-powered-chatbot-be-trained-on-a-cpu-ce9ec6ebe3c2)

## 使用 Python 的概述和实现

[](https://medium.com/@aashishnair?source=post_page-----ce9ec6ebe3c2--------------------------------)![Aashish Nair](https://medium.com/@aashishnair?source=post_page-----ce9ec6ebe3c2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ce9ec6ebe3c2--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ce9ec6ebe3c2--------------------------------) [Aashish Nair](https://medium.com/@aashishnair?source=post_page-----ce9ec6ebe3c2--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ce9ec6ebe3c2--------------------------------) ·6 分钟阅读·2023 年 10 月 12 日

--

![](img/ded2ef377ff10804f982df906fed5cbe.png)

图片来源：[Adi Goldstein](https://unsplash.com/@adigold1?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 目录

∘ 介绍

∘ 案例研究

∘ 第 1 步 — 创建向量存储

∘ 第 2 步—创建 QA 链

∘ 第 3 步 — 创建用户界面

∘ 评估聊天机器人

∘ 结果

∘ 最终判决

∘ 参考文献

## 介绍

本地模型的出现受到希望建立自己定制 LLM 应用程序的企业的欢迎。这些模型使开发者能够构建可以离线运行并符合隐私和安全要求的解决方案。

这些大规模语言模型（LLMs）最初非常庞大，主要服务于那些拥有资金和资源来配置 GPU 和在大量数据上训练模型的企业。

然而，本地 LLM 现在有了更小的尺寸，这引发了一个问题：普通 CPU 用户是否可以利用这些相同的工具和技术？

这个问题值得考虑，因为用户可以从构建自己的个人本地聊天机器人中获得很多好处，这些机器人可以离线执行任务。

在这里，我们通过在 CPU 上使用 Meta 的 Llama2 构建一个封闭源代码的聊天机器人来探讨这种可能性，并评估其作为个人可靠工具的表现。

## **案例研究**

为了测试在个人电脑上离线运行本地聊天机器人的可行性，让我们进行一个案例研究。

目标是使用量化版本的 Meta 的 Llama2（7B 参数）构建一个聊天机器人。该模型将用于构建一个 LangChain 应用程序，促进响应生成，并可以通过用户界面访问，让人们与应用程序进行互动。

![](img/4e0460af3009b86e062b0bbd6677d1ee.png)

聊天机器人图示（由作者创建）

该聊天机器人将使用两个 PDF 文档进行训练（两个文档均可通过 arXiv API 访问）：

1.  [计算机视觉在体育中的全面综述：开放问题、未来趋势和研究方向](https://browse.arxiv.org/pdf/2203.02281.pdf)

1.  [深度学习在体育应用中的调研：感知、理解和决策](https://browse.arxiv.org/pdf/2307.03353.pdf)

为了提供背景，这个机器人将在具有以下规格的计算机上进行训练：

+   操作系统：Windows 10

+   处理器：Intel i7

+   RAM: 8GB

> 注：跟随本案例研究需要具备 LangChain 框架和 Streamlit 库的基础知识

## 第 1 步 — 创建向量存储

首先，我们创建向量存储，它将存储来自文档的嵌入数据，并促进与用户查询相关文档的检索。

![](img/2f5d8138129b691dd2d8d3963a7a7789.png)

创建向量存储（由作者创建）

为此，数据必须转换为文本块。这通过使用 PyPDFLoader 加载 PDF 文档，并将文本分割成 500 个字符的块来完成。

接下来，使用来自 HuggingFace 的句子变换器将文本块转换为嵌入。参数中指定设备为“cpu”是很重要的。

在创建了文本块并加载了嵌入模型后，我们可以创建向量存储。对于这个案例研究，我们使用 Facebook AI 相似性搜索（FAISS）。

这个向量存储将被保存在本地，以备将来使用。

## 第 2 步—**创建 QA 链**

接下来，我们需要加载检索 QA 链，它从向量存储中检索相关文档，并使用它们来回答用户的查询。

![](img/fb286d6019a13a7ece66c82aaf70f62b.png)

QA 链（由作者创建）

QA 链需要三个组件：量化的 Llama 2 模型、FAISS 向量存储和一个提示模板。

首先，我们下载量化的 Llama2 模型，该模型可在[HuggingFace 仓库](https://huggingface.co/TheBloke/Llama-2-7B-Chat-GGML/tree/main)中找到。对于这个案例研究，模型通过名为“[llama-2-7b-chat.ggmlv3.q2_K.bin](https://huggingface.co/TheBloke/Llama-2-7B-Chat-GGML/blob/main/llama-2-7b-chat.ggmlv3.q2_K.bin)”的文件进行下载，占用内存 2.87 GB。

然后使用 CTransformers 加载模型，这是一个用于绑定在 C 中实现的变换器模型的 Python 库。由于我们想要一个生成响应的目标型聊天机器人，因此将`temperature`设置为 0。

接下来，我们加载先前创建的向量存储。

之后，我们定义提示模板。此步骤是可选的，但由于我们希望与研究论文进行互动，我们需要优先考虑准确性，这可以通过提示中的指令来实现。幻想回答是非常不受欢迎的，因此主要指令是对无法通过提供的 PDF 文档回答的问题回应“我不知道”。

有了这些元素，我们可以创建 QA 链，该链将使用加载的量化 Llama2 模型、向量存储和提示模板生成对用户查询的响应。

最后，我们创建执行响应生成的函数。

## 第 3 步——创建用户界面

LangChain 应用所需的核心元素已经构建完成，因此我们可以转向为聊天机器人构建用户界面。

Streamlit 库适合这个任务，因为它包含了针对聊天机器人应用的特性。

以下代码将先前构建的函数整合到用户界面中（要查看完整的源代码，请访问 GitHub [仓库](https://github.com/anair123/Llama2-Powered-QA-Chatbot-For-Research-Papers)）。

Streamlit 应用使用如下：

```py
streamlit run app.py
```

完成了！我们的个人闭源聊天机器人已成功运行！

![](img/5eb1eda91c8dff86597b8a988f2c9a8a.png)

聊天机器人用户界面（由作者创建）

## 评估聊天机器人

我们有了聊天机器人，让我们用 3 个不同的问题来评估其表现：

1.  **计算机视觉在运动分析中的好处是什么？**

![](img/c796ce889e5faa6928d3ed29082b0146.png)

聊天机器人响应（由作者创建）

**2\. 给我一个包含计算机视觉的运动项目列表。**

![](img/085b38ed3cfe22a1f9edce81dfdad268.png)

聊天机器人响应（由作者创建）

**3\. 用什么算法来追踪运动员？**

![](img/a33fd4016343cfd0a9ac8a7b81cf95e2.png)

聊天机器人响应（由作者创建）

## 结果

总的来说，机器人似乎能够返回令人满意的响应，而不包含未经请求的信息。

然而，从对“用什么算法来追踪运动员？”这个问题的回答中，可以明显看出一个限制，即答案在句子中途被截断。这可以归因于量化版 Llama2 模型的有限上下文窗口（即令牌数量）。聊天机器人无法正确回答需要大量令牌的问题。

此外，一个在响应中未传达的限制是时间。虽然机器人对问题的回答适当，但在这台 CPU 上生成响应平均需要超过一分钟。由于运行时间如此长，使用此工具作为替代手动搜索文档集合中的内容的理由并不充分。

最后，这整个过程消耗了我计算机大量内存，使其他应用程序无法使用。

## 最终裁决

![](img/34038f008137bb643850a7239b2b6497.png)

图片由[Arek Socha](https://pixabay.com/users/qimono-1962238/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2492011)提供，来自[Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2492011)

既然案例研究已经结束，让我们重新审视最初的问题：我们能否在 CPU 上构建 LLM 驱动的应用程序？

答案是：是的……但我们可能不应该。

本案例研究的积极之处在于量化后的 Llama2 模型容易下载并融入应用中，并且聊天机器人能够生成高质量的回应。

然而，有限的令牌、较长的运行时间和高内存使用的组合使得在 CPU 上训练闭源聊天机器人的前景变得不可行。

当然，这一结论是基于进行案例研究所用设备的限制，因此，具有更高处理能力和存储空间的计算机可能会产生更有希望的结果。此外，未来会有更小的 LLM 与更大的上下文窗口公开，使得闭源聊天机器人在 CPU 上构建和使用变得更加容易。

如果你有兴趣了解这个应用在你的设备上表现如何，随时可以查看以下代码库中的代码：

[anair123/Llama2-Powered-QA-Chatbot-For-Research-Papers (github.com)](https://github.com/anair123/Llama2-Powered-QA-Chatbot-For-Research-Papers)

感谢阅读！

## 参考文献

1.  Naika, B. T., Hashmi, M. F., & Bokde, N. D. (n.d.). *《计算机视觉在体育中的综合评述：开放问题、未来趋势和研究方向》*。Arxiv. [`arxiv.org/pdf/2203.02281`](https://arxiv.org/pdf/2203.02281)

1.  Zhao, Z., Chai, W., Hao, S., Hu, W., Wang, G., Cao, S., Song, M., Hwang, J.-N., & Wang, G. (n.d.). *《深度学习在体育应用中的调查：感知、理解与决策》*。Arxiv. [`arxiv.org/pdf/2307.03353.pdf`](https://arxiv.org/pdf/2307.03353.pdf)
