# 逆向工程数据库架构：GPT vs. Bard vs. LLama2（第2集）

> 原文：[https://towardsdatascience.com/retro-engineering-a-database-schema-gpt-vs-bard-vs-llama2-episode-2-e7f144a6753b?source=collection_archive---------12-----------------------#2023-10-06](https://towardsdatascience.com/retro-engineering-a-database-schema-gpt-vs-bard-vs-llama2-episode-2-e7f144a6753b?source=collection_archive---------12-----------------------#2023-10-06)

## 在我之前的文章中，我对比了GPT-4模型和Bard。现在Llama-2登场，是时候看看它在竞争对手面前的表现了！

[](https://pl-bescond.medium.com/?source=post_page-----e7f144a6753b--------------------------------)[![Pierre-Louis Bescond](../Images/bb236055962b420fb3ab22088ab28f11.png)](https://pl-bescond.medium.com/?source=post_page-----e7f144a6753b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e7f144a6753b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e7f144a6753b--------------------------------) [Pierre-Louis Bescond](https://pl-bescond.medium.com/?source=post_page-----e7f144a6753b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4ef7c1e10597&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fretro-engineering-a-database-schema-gpt-vs-bard-vs-llama2-episode-2-e7f144a6753b&user=Pierre-Louis+Bescond&userId=4ef7c1e10597&source=post_page-4ef7c1e10597----e7f144a6753b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e7f144a6753b--------------------------------) · 6分钟阅读 · 2023年10月6日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe7f144a6753b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fretro-engineering-a-database-schema-gpt-vs-bard-vs-llama2-episode-2-e7f144a6753b&source=-----e7f144a6753b---------------------bookmark_footer-----------)![](../Images/2941f397ecb8ea1cf96ab9c325122f57.png)

照片由 [Dustin Humes](https://unsplash.com/@dustinhumes_photography?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 初始（及最终）数据集

如 [在这篇文章中](https://medium.com/p/2e2776e8af86) 所解释的，我们将从一个包含员工信息的虚假AI生成数据集开始。

[## 反向工程数据库模式和质量检查：GPT与Bard](https://towardsdatascience.com/retro-engineering-a-database-schema-and-quality-checks-gpt-vs-bard-2e2776e8af86?source=post_page-----e7f144a6753b--------------------------------)

### LLM能否通过反向工程整合数据集以设计原始数据库，并建议相应的数据…

[towardsdatascience.com](/retro-engineering-a-database-schema-and-quality-checks-gpt-vs-bard-2e2776e8af86?source=post_page-----e7f144a6753b--------------------------------)

原始表格有11列 x 7688行，但我们将提取50行样本，以适应当前LLM的令牌限制。

![](../Images/0c36c37aab2a4cf97caf94e274d64dcd.png)

数据源示例（图片来自作者）

*(注意：笔记本和数据源在文章末尾提供)*

# 数据模型的反向工程
