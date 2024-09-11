# 提升RAG的回答：自我调试技术和认知负荷减少

> 原文：[https://towardsdatascience.com/enhancing-rags-answer-self-debugging-techniques-and-cognitive-load-reduction-8c4273013d39?source=collection_archive---------1-----------------------#2023-11-26](https://towardsdatascience.com/enhancing-rags-answer-self-debugging-techniques-and-cognitive-load-reduction-8c4273013d39?source=collection_archive---------1-----------------------#2023-11-26)

## 让LLM进行自我诊断和自我修正，以提高回答质量。

[](https://agustinus-nalwan.medium.com/?source=post_page-----8c4273013d39--------------------------------)[![Agustinus Nalwan](../Images/7c5ade9ab8bca1d27a317b5c09d1b734.png)](https://agustinus-nalwan.medium.com/?source=post_page-----8c4273013d39--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8c4273013d39--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8c4273013d39--------------------------------) [Agustinus Nalwan](https://agustinus-nalwan.medium.com/?source=post_page-----8c4273013d39--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8b7ab157b0a4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fenhancing-rags-answer-self-debugging-techniques-and-cognitive-load-reduction-8c4273013d39&user=Agustinus+Nalwan&userId=8b7ab157b0a4&source=post_page-8b7ab157b0a4----8c4273013d39---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8c4273013d39--------------------------------) ·22分钟阅读·2023年11月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8c4273013d39&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fenhancing-rags-answer-self-debugging-techniques-and-cognitive-load-reduction-8c4273013d39&user=Agustinus+Nalwan&userId=8b7ab157b0a4&source=-----8c4273013d39---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8c4273013d39&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fenhancing-rags-answer-self-debugging-techniques-and-cognitive-load-reduction-8c4273013d39&source=-----8c4273013d39---------------------bookmark_footer-----------)![](../Images/ed765e2b448efe4ab6655b25da525fab.png)

LLM执行自我调试（图像由MidJourney生成）

检索增强生成（RAG）无疑是一个强大的工具，可以轻松地使用像LangChain或LlamaIndex这样的框架进行构建。这种集成的简便性可能会给人一种RAG是一个对每种使用场景都容易构建的魔法解决方案的印象。然而，在我们升级编辑文章搜索工具以提供语义更丰富的搜索结果和直接回答查询的过程中，我们发现基本的RAG设置不足，遇到了许多挑战。构建一个RAG以进行演示是快速而简单的，通常能在小范围场景中产生令人满意的结果。然而，要实现生产级别的状态，即需要卓越质量，最终阶段面临着重大挑战。这一点在处理包含数千篇领域特定文章的大型知识库时尤为真实，这种情况并不罕见。

我们的RAG方法包括两个不同的步骤：

1.  相关文档检索 通过结合稠密和稀疏的嵌入，我们从Pinecone数据库中提取相关的文档片段，考虑到内容和标题。这些片段是…
