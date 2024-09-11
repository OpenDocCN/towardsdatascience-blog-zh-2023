# 使用检索增强生成（RAG）构建行业特定的 LLM

> 原文：[`towardsdatascience.com/build-industry-specific-llms-using-retrieval-augmented-generation-af9e98bb6f68?source=collection_archive---------0-----------------------#2023-05-31`](https://towardsdatascience.com/build-industry-specific-llms-using-retrieval-augmented-generation-af9e98bb6f68?source=collection_archive---------0-----------------------#2023-05-31)

## 各组织正在竞相采用大型语言模型。让我们深入了解如何通过 RAG 构建行业特定的 LLM。

[](https://skanda-vivek.medium.com/?source=post_page-----af9e98bb6f68--------------------------------)![Skanda Vivek](https://skanda-vivek.medium.com/?source=post_page-----af9e98bb6f68--------------------------------)[](https://towardsdatascience.com/?source=post_page-----af9e98bb6f68--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----af9e98bb6f68--------------------------------) [Skanda Vivek](https://skanda-vivek.medium.com/?source=post_page-----af9e98bb6f68--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F220d9bbb8014&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-industry-specific-llms-using-retrieval-augmented-generation-af9e98bb6f68&user=Skanda+Vivek&userId=220d9bbb8014&source=post_page-220d9bbb8014----af9e98bb6f68---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----af9e98bb6f68--------------------------------) ·10 分钟阅读·2023 年 5 月 31 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Faf9e98bb6f68&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-industry-specific-llms-using-retrieval-augmented-generation-af9e98bb6f68&user=Skanda+Vivek&userId=220d9bbb8014&source=-----af9e98bb6f68---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Faf9e98bb6f68&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-industry-specific-llms-using-retrieval-augmented-generation-af9e98bb6f68&source=-----af9e98bb6f68---------------------bookmark_footer-----------)

公司可以通过像 ChatGPT 这样的 LLM 获得大量的生产力提升。但尝试询问 ChatGPT“当前美国的通货膨胀率是多少”时，它会给出：

> 对于造成的困惑，我深表歉意，但作为一个 AI 语言模型，我没有实时数据或浏览能力。我的回答基于截至 2021 年 9 月的信息。因此，我无法提供当前的美国通货膨胀率。

这就是一个问题。ChatGPT 显然缺少相关的及时背景，这在做出明智的决策时可能至关重要。

# 微软如何解决这个问题

在微软 Build 会议中，[向量搜索还不够](https://build.microsoft.com/en-US/sessions/038984b3-7c5d-4cc6-b24e-5d9f62bc2f0e?wt.mc_ID=Build2023_esc_corp_em_oo_mto_Marketo_FPnews_Elastic)中，他们展示了将不太了解上下文的 LLMs 与向量搜索相结合的产品，以创建更具吸引力的体验。

这次讲座从与这篇文章相反的方向出发——从 Elastic Search（或向量搜索）的角度——并且提出了单纯搜索的局限性，添加 LLM 层可以大大改进…
