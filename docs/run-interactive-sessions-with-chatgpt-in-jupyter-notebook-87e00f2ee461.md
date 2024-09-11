# 在 Jupyter Notebook 中运行与 ChatGPT 的互动会话

> 原文：[https://towardsdatascience.com/run-interactive-sessions-with-chatgpt-in-jupyter-notebook-87e00f2ee461?source=collection_archive---------0-----------------------#2023-05-04](https://towardsdatascience.com/run-interactive-sessions-with-chatgpt-in-jupyter-notebook-87e00f2ee461?source=collection_archive---------0-----------------------#2023-05-04)

## 使用 LangChain 和 IPyWidgets 在 Jupyter Notebook 中与 ChatGPT 进行关于自定义文档的对话会话

[](https://konstantin-rink.medium.com/?source=post_page-----87e00f2ee461--------------------------------)[![Konstantin Rink](../Images/41bfc069d7382a0fd56f081eea7eb2d9.png)](https://konstantin-rink.medium.com/?source=post_page-----87e00f2ee461--------------------------------)[](https://towardsdatascience.com/?source=post_page-----87e00f2ee461--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----87e00f2ee461--------------------------------) [Konstantin Rink](https://konstantin-rink.medium.com/?source=post_page-----87e00f2ee461--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F337427fde9f0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frun-interactive-sessions-with-chatgpt-in-jupyter-notebook-87e00f2ee461&user=Konstantin+Rink&userId=337427fde9f0&source=post_page-337427fde9f0----87e00f2ee461---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----87e00f2ee461--------------------------------) ·6 分钟阅读·2023年5月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F87e00f2ee461&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frun-interactive-sessions-with-chatgpt-in-jupyter-notebook-87e00f2ee461&user=Konstantin+Rink&userId=337427fde9f0&source=-----87e00f2ee461---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F87e00f2ee461&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frun-interactive-sessions-with-chatgpt-in-jupyter-notebook-87e00f2ee461&source=-----87e00f2ee461---------------------bookmark_footer-----------)![](../Images/3e2944ef6748fadb370f0238766a8a0f.png)

原始照片由 [Charles Etoroma](https://unsplash.com/@charlesetoroma?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/de/fotos/ddPTOSMa-MI?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)。

2023年3月，OpenAI发布了其API，供开发者访问ChatGPT及其Whisper模型。从那时起，开发者可以通过API将这些服务和模型集成到他们的应用程序和产品中。许多优秀的教程随后发布，介绍如何使用他们的API以及Streamlit或Streamlit Chat创建自己的ChatGPT聊天网页应用。

本文提出了一种**更轻量级的方法**。无需运行或托管Streamlit服务器或使用Docker容器，**所有工作都在Jupyter Notebook中完成**。

在本文中，你将学习**如何在Jupyter Notebook中运行有关自定义文档的交互式会话**，**利用** OpenAI的LargeLanguageModel (LLM) **ChatGPT**，通过使用**LangChain**和**IPyWidgets**。

最终结果将如下所示：

![](../Images/43e468f126b37e2c69924176c1f375f1.png)

图1\. 最终结果的演示（图像由作者提供）。

接下来的章节将分别解释代码的每个部分。
