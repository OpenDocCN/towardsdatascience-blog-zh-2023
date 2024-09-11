# LLM 巨头之战：Google PaLM 2 对 OpenAI GPT-3.5

> 原文：[`towardsdatascience.com/battle-of-the-llm-giants-google-palm-2-vs-openai-gpt-3-5-798802ddb53c?source=collection_archive---------1-----------------------#2023-06-26`](https://towardsdatascience.com/battle-of-the-llm-giants-google-palm-2-vs-openai-gpt-3-5-798802ddb53c?source=collection_archive---------1-----------------------#2023-06-26)

## 使用 Outside 的真实数据、Pinecone 和 Langchain 的实用比较

[](https://medium.com/@wen_yang?source=post_page-----798802ddb53c--------------------------------)![Wen Yang](https://medium.com/@wen_yang?source=post_page-----798802ddb53c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----798802ddb53c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----798802ddb53c--------------------------------) [Wen Yang](https://medium.com/@wen_yang?source=post_page-----798802ddb53c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fcbb5383bd438&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbattle-of-the-llm-giants-google-palm-2-vs-openai-gpt-3-5-798802ddb53c&user=Wen+Yang&userId=cbb5383bd438&source=post_page-cbb5383bd438----798802ddb53c---------------------post_header-----------) 发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----798802ddb53c--------------------------------) · 11 分钟阅读 · 2023 年 6 月 26 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F798802ddb53c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbattle-of-the-llm-giants-google-palm-2-vs-openai-gpt-3-5-798802ddb53c&user=Wen+Yang&userId=cbb5383bd438&source=-----798802ddb53c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F798802ddb53c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbattle-of-the-llm-giants-google-palm-2-vs-openai-gpt-3-5-798802ddb53c&source=-----798802ddb53c---------------------bookmark_footer-----------)![](img/0e166d585075cebb67623c314149ca6a.png)

作者使用 Midjourney 生成的图像以庆祝 Pride Outside

Google 于 2023 年 5 月 10 日发布了 PaLM 2，以对 OpenAI 的 GPT-4 进行有力回应。在最近的 I/O 事件中，Google 揭示了引人注目的 PaLM 2 模型家族，从最小到最大包括：**Gecko、Otter、Bison 和 Unicorn**。根据 Google [PaLM 2 技术报告](https://www.notion.so/133e1a64b8ed4329851394435eb41adb?pvs=21) **（**（见表 5 和表 7），PaLM 2 不仅在性能、速度和体积上优于之前的 PaLM，还在某些推理领域超越了 GPT-4。

像许多人一样，在[Outside](https://www.outsideonline.com/)我们也在学习如何采用 LLMs，以更好地服务我们的户外社区。最近，我们有机会通过 Outside 的实际用例来测试 PaLM2 和 GPT-3.5。如果你在考虑选择 Google 还是 OpenAI 作为你的 LLM 提供商，或者你只是想了解如何构建一个配备了搜索和基于知识库的问答功能的 Langchain 代理，我希望这篇文章能够为制定适合你领域的评估框架提供一些灵感。

在这篇文章中，我将分享我们在四个关键领域的探索：

1.  方法论和技术方案：Pinecone、Langchain、LLMs（PaLM2 和 GPT-3.5）
