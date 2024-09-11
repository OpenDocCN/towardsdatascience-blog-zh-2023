# 将 AI 驱动的数据分析师投入测试

> 原文：[https://towardsdatascience.com/putting-an-ai-powered-data-analyst-to-the-test-e8971337264?source=collection_archive---------13-----------------------#2023-07-12](https://towardsdatascience.com/putting-an-ai-powered-data-analyst-to-the-test-e8971337264?source=collection_archive---------13-----------------------#2023-07-12)

## 探索 Langchain 和 OpenAI 实现临时分析自动化

[](https://johnadeojo.medium.com/?source=post_page-----e8971337264--------------------------------)[![John Adeojo](../Images/f6460fae462b055d36dce16fefcd142c.png)](https://johnadeojo.medium.com/?source=post_page-----e8971337264--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e8971337264--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e8971337264--------------------------------) [John Adeojo](https://johnadeojo.medium.com/?source=post_page-----e8971337264--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff933e1637e40&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fputting-an-ai-powered-data-analyst-to-the-test-e8971337264&user=John+Adeojo&userId=f933e1637e40&source=post_page-f933e1637e40----e8971337264---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e8971337264--------------------------------) · 8 分钟阅读 · 2023年7月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe8971337264&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fputting-an-ai-powered-data-analyst-to-the-test-e8971337264&user=John+Adeojo&userId=f933e1637e40&source=-----e8971337264---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe8971337264&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fputting-an-ai-powered-data-analyst-to-the-test-e8971337264&source=-----e8971337264---------------------bookmark_footer-----------)![](../Images/3eeea8b2369c47c7421aec3d7030bd83.png)

作者提供的图像：使用 Midjourney 生成

# 背景 — 高效分析的需求

在我看来，由于大量的临时请求，分析一直是一个最具挑战性的领域之一。通常，这涉及到编写 SQL 查询或在电子表格上进行一些分析，这些工作往往会比预期花费更多时间。这导致分析团队大部分时间都在处理突发问题，构建战术解决方案，而没有机会进行主动的工作。

我经常思考一个能够处理临时分析请求的AI助手的想法，这类似于现在在客服领域随处可见的聊天机器人。然而，由于某些分析查询的复杂性，这个想法总是显得有些遥不可及。现在，随着生成性AI的进步，我们已经到了一个可以自动化处理琐碎临时请求的阶段。在这篇文章中，我展示了一个原型分析机器人。我对该机器人在一些“典型”分析请求上的表现进行了评估，并简要讨论了其对商业分析的影响。

# **AI驱动的数据分析师**
