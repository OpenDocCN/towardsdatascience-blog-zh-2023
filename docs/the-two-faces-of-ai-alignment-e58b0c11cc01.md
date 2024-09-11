# 《人工智能对齐的双面性》

> 原文：[https://towardsdatascience.com/the-two-faces-of-ai-alignment-e58b0c11cc01?source=collection_archive---------14-----------------------#2023-07-18](https://towardsdatascience.com/the-two-faces-of-ai-alignment-e58b0c11cc01?source=collection_archive---------14-----------------------#2023-07-18)

## 不对齐的模型与不对齐的代理

[](https://medium.com/@maxhilsdorf?source=post_page-----e58b0c11cc01--------------------------------)[![Max Hilsdorf](../Images/01da76c553e43d5ed6b6849bdbfd00da.png)](https://medium.com/@maxhilsdorf?source=post_page-----e58b0c11cc01--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e58b0c11cc01--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e58b0c11cc01--------------------------------) [Max Hilsdorf](https://medium.com/@maxhilsdorf?source=post_page-----e58b0c11cc01--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd0c085a74ae8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-two-faces-of-ai-alignment-e58b0c11cc01&user=Max+Hilsdorf&userId=d0c085a74ae8&source=post_page-d0c085a74ae8----e58b0c11cc01---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e58b0c11cc01--------------------------------) ·12分钟阅读·2023年7月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe58b0c11cc01&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-two-faces-of-ai-alignment-e58b0c11cc01&user=Max+Hilsdorf&userId=d0c085a74ae8&source=-----e58b0c11cc01---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe58b0c11cc01&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-two-faces-of-ai-alignment-e58b0c11cc01&source=-----e58b0c11cc01---------------------bookmark_footer-----------)![](../Images/95518eb41d8df5b41fc07d61b3b3ce53.png)

图片改编自 [Tara Winstead](https://www.pexels.com/de-de/foto/hande-verbindung-zukunft-roboter-8386434/)。

# 什么是 AI 对齐？

人工智能（AI）不再只是一个流行词；它是一个快速发展的领域，其中智能系统正日益融入我们的日常生活。从 Netflix 上的推荐算法到通过 ChatGPT 或 Midjourney 自动化创意工作流，AI 正在改变我们世界的各个方面。然而，这一显著进步也带来了重大挑战，其中 AI 对齐是最需要解决的关键问题之一。

AI 对齐是确保 AI 系统以与人类认为的理想行为一致的方式运行的过程。这就像试图教一个幼儿表现得体一样——就像你希望孩子理解并尊重你的价值观一样，我们也需要对 AI 系统提出相同的标准。然而，事实证明，我们在这方面的能力并不如我们想象的那么好——无论是对待幼儿还是 AI。

# 当前讨论中的 AI 对齐

生成型 AI 模型如 ChatGPT 或 Midjourney 被发现生成了有偏见、冒犯性或有害的内容。这些系统从它们接收的数据中学习，如果这些数据包含偏见或有害的模式，系统可能会不经意间重复…
