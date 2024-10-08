# 如何在 10 分钟内利用 LlamaIndex 构建定制的 GPT 聊天机器人

> 原文：[`towardsdatascience.com/how-i-built-a-custom-gpt-based-chatbot-in-under-10-minutes-with-llamaindex-2102f0173420`](https://towardsdatascience.com/how-i-built-a-custom-gpt-based-chatbot-in-under-10-minutes-with-llamaindex-2102f0173420)

## Python 的概述和实现

[](https://medium.com/@aashishnair?source=post_page-----2102f0173420--------------------------------)![Aashish Nair](https://medium.com/@aashishnair?source=post_page-----2102f0173420--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2102f0173420--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2102f0173420--------------------------------) [Aashish Nair](https://medium.com/@aashishnair?source=post_page-----2102f0173420--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2102f0173420--------------------------------) ·6 分钟阅读·2023 年 4 月 12 日

--

![](img/1ae789d4b6cc2e0d3b71df8d0b8fd123.png)

照片来源：Miguel Á. Padriñán: [`www.pexels.com/photo/two-white-message-balloons-1111368/`](https://www.pexels.com/photo/two-white-message-balloons-1111368/)

不久前，我读到了一篇由 Jerry Liu 撰写的文章，介绍了 LlamaIndex，这是一种利用 GPT 来综合响应用户提供信息的接口。

[](https://medium.com/@jerryjliu98/how-unstructured-and-llamaindex-can-help-bring-the-power-of-llms-to-your-own-data-3657d063e30d?source=post_page-----2102f0173420--------------------------------) [## 如何利用非结构化数据和 LlamaIndex 将 LLM 的力量带入你的数据

### （合著者：LlamaIndex 的创始人 Jerry Liu 和 Unstructured 的 CEO Brian Raymond）

[medium.com](https://medium.com/@jerryjliu98/how-unstructured-and-llamaindex-can-help-bring-the-power-of-llms-to-your-own-data-3657d063e30d?source=post_page-----2102f0173420--------------------------------)

它立刻引起了我的注意，我知道我必须亲自尝试一下。

毕竟，许多在世界上引起轰动的大型语言模型（LLM）由于没有用用户可用的数据进行训练，因此用途有限。

因此，仍然需要可定制的 LLM，以满足用户的需求。简单来说，企业需要能够处理文本总结、问答（Q&A）以及使用他们自己信息生成文本的 LLM。

为了看看 LlamaIndex 是否有潜力满足这一需求，我尝试了它的功能，并对我能够做到的事情感到真正惊讶。我甚至在大约 10 分钟内构建了一个 Q&A 聊天机器人！

在这里，我将带你深入了解我的旅程。

## LlamaIndex 的功能是什么？

LlamaIndex 通过将 LLM 连接到用户提供的信息来生成查询响应。

如[文档](https://gpt-index.readthedocs.io/en/latest/guides/primer/usage_pattern.html)中详细描述的，使用 LlamaIndex 涉及以下步骤：

1.  加载文档

1.  将文档解析成节点（可选）

1.  构建索引

1.  在构建的索引上构建索引（可选）

1.  查询索引

实质上，LlamaIndex 将数据加载到文档对象中，然后将其转换为索引。当索引接收到查询时，它将查询传递给 GPT 提示，以合成响应。默认情况下，这使用的是 OpenAI 的 `text-davinci-003` 模型。

虽然这个过程听起来相当复杂，但实际上可以用很少的代码来执行，正如你将要发现的那样。

## 设置

为了测试 LlamaIndex 的多样性，我最终构建了 3 个不同的聊天机器人，每个机器人都使用不同的数据源。为了简洁起见，之前提到的可选步骤（即第 2 步和第 4 步）将被省略。

首先，让我们处理好前提条件。

可以使用以下命令通过 pip 安装 LlamaIndex 和 OpenAI：

```py
pip install llama-index
pip install openai
```

用户还需要一个来自 OpenAI 的 API 密钥：

```py
import os

os.environ['OPENAI_API_KEY'] = 'API_KEY' 
```

项目还需要以下导入：

## 加载数据

数据可以手动加载，也可以通过数据加载器加载。在这个案例中，我加载了 3 种数据：

+   一个本地 .txt 文件，其中写有我最喜欢的水果（在[GitHub 仓库](https://github.com/anair123/Creating-A-Custom-GPT-Based-Chatbot-With-LlamaIndex)中可以访问）

+   一个[关于苹果的 Wikipedia 页面](https://en.wikipedia.org/wiki/Apple)

+   一个[展示香草蛋糕食谱的 YouTube 视频](https://www.youtube.com/watch?v=EYXQmbZNhy8)

第一个索引将使用本地 .txt 文件创建，该文件位于名为 `data` 的文件夹中。这些数据将手动加载。

第二个索引将使用来自 Wikipedia 页面关于苹果的数据创建。它可以通过 LlamaIndex 的数据加载器之一进行加载。

第三个索引将使用展示如何制作香草蛋糕的 YouTube 视频构建。这些数据也将通过数据加载器加载。

## 构建索引

所有数据加载到文档对象中后，我们可以为每个聊天机器人构建索引。

每个索引可以通过一行代码从文档对象构建。

吃惊吗？从加载数据到创建索引，使用 LlamaIndex 只需要几行代码！

## 查询索引

现在，构建的索引可以为任何给定的查询生成响应。再次强调，这一步也可以用一行代码完成。

1.  **查询使用 .txt 文件构建的索引**

![](img/b9aec3f5b43cd6e2602026412bf1c93b.png)

代码输出（作者创建）

如果你在想，这就是正确的答案。

**2\. 查询使用 Wikipedia 页面（主题：苹果）构建的索引**

![](img/5c79e0a389fbefd562be76d1df02fe7e.png)

代码输出（作者创建）

**3\. 查询使用 YouTube 视频（主题：香草蛋糕食谱）构建的索引**

![](img/31d4364682152a02c7cc5146a3d3b6c8.png)

代码输出（由作者创建）

最后，值得注意的是，索引仅在包含所需上下文时才会提供查询的答案。

这是如何使用 YouTube 视频数据创建的相同索引对完全不相关的查询的响应。

![](img/b1f8e568abcffff112491b39387d811b.png)

代码输出（由作者创建）

幸运的是，LlamaIndex 似乎已采取措施防止幻觉（即模型自信地给出未由数据支持的答案）。

## 使用 Web 应用部署聊天机器人

最后，我们可以创建一个 Web 应用，以便与最终用户分享构建的索引。

为此，我们需要首先使用`save_to_disk`方法保存索引。

这些索引将用于 Streamlit 应用。整个应用的底层代码如下：

在应用中，用户可以选择他们希望根据其提问的数据源，并在提供的框中输入他们的查询。

我们可以看到在运行应用后，索引的表现如何：

```py
streamlit run app.py
```

查询使用.txt 文件（我最喜欢的水果）构建的索引：

![](img/b53cafcf295ec1349aa5c732b523f78b.png)

Streamlit 应用（由作者创建）

查询使用维基百科页面（苹果）构建的索引：

![](img/d51be9f3f497e4558712958fd44aebf2.png)

Streamlit 应用（由作者创建）

查询使用 YouTube 视频（香草蛋糕食谱）构建的索引：

![](img/858391c3c5d591dfa7ee2893c1f489d2.png)

Streamlit 应用（由作者创建）

很酷，对吧？我们在短短 10 分钟内构建了一个功能完整的 Web 应用！

## 结论

![](img/e955c0ba062ac53f3af4c5eef0332371.png)

图片由[Prateek Katyal](https://unsplash.com/@prateekkatyal?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

到目前为止，我仅实现了 LlamaIndex 接口的基本功能。这个项目中有很多领域尚未探索，例如自定义 LLMs 和使用非默认设置。

欲了解更多信息，请访问[文档](https://gpt-index.readthedocs.io/en/latest/index.html)。

如果你打算自己尝试这个工具，请注意使用 OpenAI 的 API 所产生的费用。这个项目只花了我$1，但这归功于我处理的小文档（定价基于你使用的令牌数量）。如果你过于投入，可能会收到一张令人不快的账单。

最后，创建 Q&A 聊天机器人所用的所有源代码可以在这个 Github 库中访问：

[](https://github.com/anair123/Creating-A-Custom-GPT-Based-Chatbot-With-LlamaIndex?source=post_page-----2102f0173420--------------------------------) [## GitHub - anair123 的创建自定义 GPT 聊天机器人]

### 你现在无法执行该操作。你在另一个标签页或窗口中登录了。你在另一个标签页中已退出…

[github.com](https://github.com/anair123/Creating-A-Custom-GPT-Based-Chatbot-With-LlamaIndex?source=post_page-----2102f0173420--------------------------------)

感谢阅读！
