# 入门LangChain：构建LLM驱动应用程序的初学者指南

> 原文：[https://towardsdatascience.com/getting-started-with-langchain-a-beginners-guide-to-building-llm-powered-applications-95fc8898732c?source=collection_archive---------1-----------------------#2023-04-25](https://towardsdatascience.com/getting-started-with-langchain-a-beginners-guide-to-building-llm-powered-applications-95fc8898732c?source=collection_archive---------1-----------------------#2023-04-25)

## 一个LangChain教程，用于用Python构建任何大型语言模型

[](https://medium.com/@iamleonie?source=post_page-----95fc8898732c--------------------------------)[![Leonie Monigatti](../Images/4044b1685ada53a30160b03dc78f9626.png)](https://medium.com/@iamleonie?source=post_page-----95fc8898732c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----95fc8898732c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----95fc8898732c--------------------------------) [Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----95fc8898732c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3a38da70d8dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgetting-started-with-langchain-a-beginners-guide-to-building-llm-powered-applications-95fc8898732c&user=Leonie+Monigatti&userId=3a38da70d8dc&source=post_page-3a38da70d8dc----95fc8898732c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----95fc8898732c--------------------------------) · 11分钟阅读 · 2023年4月25日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F95fc8898732c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgetting-started-with-langchain-a-beginners-guide-to-building-llm-powered-applications-95fc8898732c&user=Leonie+Monigatti&userId=3a38da70d8dc&source=-----95fc8898732c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F95fc8898732c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgetting-started-with-langchain-a-beginners-guide-to-building-llm-powered-applications-95fc8898732c&source=-----95fc8898732c---------------------bookmark_footer-----------)![](../Images/ba0a0432f474352f0aac6ee6c0e9eeea.png)

“随机鹦鹉对另一只说了什么？”（图像由作者绘制）

自从ChatGPT发布以来，大型语言模型（LLMs）获得了极大的关注。尽管你可能没有足够的资金和计算资源在地下室从零开始训练一个LLM，但你仍然可以使用预训练的LLM来构建一些酷炫的应用，比如：

+   [个人助理](https://python.langchain.com/en/latest/use_cases/personal_assistants.html)可以基于你的数据与外部世界互动。

+   [聊天机器人](https://python.langchain.com/en/latest/use_cases/chatbots.html)根据你的需求进行定制。

+   对你的文档或 [代码](https://python.langchain.com/en/latest/use_cases/code.html) 的 [分析](https://python.langchain.com/en/latest/use_cases/question_answering.html) 或 [总结](https://python.langchain.com/en/latest/use_cases/summarization.html)。

> LLMs 正在改变我们构建 AI 驱动产品的方式。

凭借其奇特的 API 和提示工程，LLMs 正在改变我们构建 AI 驱动产品的方式。这就是为什么在 [“LLMOps”](https://wandb.ai/iamleonie/Articles/reports/Understanding-LLMOps-Large-Language-Model-Operations--Vmlldzo0MDgyMDc2) 这一术语下，新开发者工具层出不穷。

这些新工具之一是 [LangChain](https://github.com/hwchase17/langchain)。
