# ChatGPT：自动化提示评分

> 原文：[https://towardsdatascience.com/chatgpt-automated-prompt-scoring-c972f9ee2c4f?source=collection_archive---------4-----------------------#2023-04-10](https://towardsdatascience.com/chatgpt-automated-prompt-scoring-c972f9ee2c4f?source=collection_archive---------4-----------------------#2023-04-10)

![](../Images/1b9b1e6bc46d2e359537b4b1d0aff68d.png)

此图像的创作得益于DALL·E 2的帮助。

## 指南

## 如何使用Python客观选择和改进你的ChatGPT提示

[](https://michael-malin.medium.com/?source=post_page-----c972f9ee2c4f--------------------------------)[![Michael Malin](../Images/070604c68a50e8f2996f2c8837df3ec9.png)](https://michael-malin.medium.com/?source=post_page-----c972f9ee2c4f--------------------------------) [](https://towardsdatascience.com/?source=post_page-----c972f9ee2c4f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c972f9ee2c4f--------------------------------) [Michael Malin](https://michael-malin.medium.com/?source=post_page-----c972f9ee2c4f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8225885ee2a7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchatgpt-automated-prompt-scoring-c972f9ee2c4f&user=Michael+Malin&userId=8225885ee2a7&source=post_page-8225885ee2a7----c972f9ee2c4f---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c972f9ee2c4f--------------------------------) ·10分钟阅读·2023年4月10日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc972f9ee2c4f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchatgpt-automated-prompt-scoring-c972f9ee2c4f&user=Michael+Malin&userId=8225885ee2a7&source=-----c972f9ee2c4f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc972f9ee2c4f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchatgpt-automated-prompt-scoring-c972f9ee2c4f&source=-----c972f9ee2c4f---------------------bookmark_footer-----------)

大型语言模型（LLM），如 ChatGPT，正在产生巨大的影响。它们也仅仅是开始。在接下来的一年里，大大小小的公司将开始推出领域/角色专业化的 LLM 模型。实际上，像金融专业化的 [BloombergGPT](https://www.bloomberg.com/company/press/bloomberggpt-50-billion-parameter-llm-tuned-finance/) 和微软开发者专注的 [Copilot](https://blogs.microsoft.com/blog/2023/03/16/introducing-microsoft-365-copilot-your-copilot-for-work/) 等新产品已经使这一点成为现实。我们将很快看到 AI 个人教练、健康教练、顾问、法律助理以及更多应用场景。虽然一些情况可能需要在特定领域数据上进行微调模型，但大多数可以通过简单的提示工程来完成。但是，怎么知道你的提示是否足够好？我们如何对主观文本生成客观准确性评分？

本指南将涵盖：

+   理论

+   提示工程

+   提示测试

+   提示评分

+   提示反馈

# 理论

测试大型语言模型（LLM）提示输出的难点在于结果是主观的。我可能觉得结果很完美，而你可能觉得结果平平。两种观点都是同样有效的。这使得纯科学的方法来评分变得非常困难……
