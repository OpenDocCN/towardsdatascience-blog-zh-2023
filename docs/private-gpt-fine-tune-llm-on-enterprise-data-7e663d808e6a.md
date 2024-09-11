# 私有GPT：在企业数据上微调大语言模型

> 原文：[https://towardsdatascience.com/private-gpt-fine-tune-llm-on-enterprise-data-7e663d808e6a?source=collection_archive---------0-----------------------#2023-07-05](https://towardsdatascience.com/private-gpt-fine-tune-llm-on-enterprise-data-7e663d808e6a?source=collection_archive---------0-----------------------#2023-07-05)

## 用数据做一些酷炫的事情

[](https://priya-dwivedi.medium.com/?source=post_page-----7e663d808e6a--------------------------------)[![Priya Dwivedi](../Images/73087cb699750466312cc4752e2044d4.png)](https://priya-dwivedi.medium.com/?source=post_page-----7e663d808e6a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7e663d808e6a--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7e663d808e6a--------------------------------) [Priya Dwivedi](https://priya-dwivedi.medium.com/?source=post_page-----7e663d808e6a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb040ce924438&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprivate-gpt-fine-tune-llm-on-enterprise-data-7e663d808e6a&user=Priya+Dwivedi&userId=b040ce924438&source=post_page-b040ce924438----7e663d808e6a---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----7e663d808e6a--------------------------------) · 9 分钟阅读 · 2023年7月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7e663d808e6a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprivate-gpt-fine-tune-llm-on-enterprise-data-7e663d808e6a&user=Priya+Dwivedi&userId=b040ce924438&source=-----7e663d808e6a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7e663d808e6a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprivate-gpt-fine-tune-llm-on-enterprise-data-7e663d808e6a&source=-----7e663d808e6a---------------------bookmark_footer-----------)![](../Images/b1c22ad1ee60a9ba1c68c1dc31d44dc6.png)

图片来源于 [Robynne Hu](https://unsplash.com/@robynnexy?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 由 [Unsplash](https://unsplash.com/s/photos/technology?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供

# **简介**

在大数据和先进人工智能的时代，语言模型作为处理和生成类人文本的强大工具已然崭露头角。像 ChatGPT 这样的**大语言模型**是通用的聊天机器人，能够就许多话题进行对话。然而，大语言模型也可以在特定领域的数据上进行微调，从而在特定领域的企业问题上变得更为准确和切题。

许多行业和应用将需要微调的 LLMs。原因包括：

+   在特定数据上训练的聊天机器人能提供更好的性能。

+   OpenAI 模型，如 chatgpt，是一个黑箱，企业可能会对通过 API 共享其机密数据感到犹豫。

+   ChatGPT API 成本对于大型应用可能是难以承受的。

微调 LLM 的挑战在于该过程不明确，训练一个没有优化的十亿参数模型所需的计算资源可能是巨大的。

幸运的是，许多关于训练技术的研究已经完成，这使得我们现在可以在较小的 GPU 上微调 LLMs。
