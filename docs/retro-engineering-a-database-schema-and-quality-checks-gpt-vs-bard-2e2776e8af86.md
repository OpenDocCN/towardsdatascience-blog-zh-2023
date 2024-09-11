# 逆向工程数据库模式和质量检查：GPT 与 Bard

> 原文：[`towardsdatascience.com/retro-engineering-a-database-schema-and-quality-checks-gpt-vs-bard-2e2776e8af86?source=collection_archive---------18-----------------------#2023-07-25`](https://towardsdatascience.com/retro-engineering-a-database-schema-and-quality-checks-gpt-vs-bard-2e2776e8af86?source=collection_archive---------18-----------------------#2023-07-25)

## LLM 是否可以逆向工程一个综合数据集，以设计原始数据库并建议相应的数据质量检查？

[](https://pl-bescond.medium.com/?source=post_page-----2e2776e8af86--------------------------------)![Pierre-Louis Bescond](https://pl-bescond.medium.com/?source=post_page-----2e2776e8af86--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2e2776e8af86--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2e2776e8af86--------------------------------) [Pierre-Louis Bescond](https://pl-bescond.medium.com/?source=post_page-----2e2776e8af86--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4ef7c1e10597&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fretro-engineering-a-database-schema-and-quality-checks-gpt-vs-bard-2e2776e8af86&user=Pierre-Louis+Bescond&userId=4ef7c1e10597&source=post_page-4ef7c1e10597----2e2776e8af86---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2e2776e8af86--------------------------------) ·9 分钟阅读·2023 年 7 月 25 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2e2776e8af86&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fretro-engineering-a-database-schema-and-quality-checks-gpt-vs-bard-2e2776e8af86&user=Pierre-Louis+Bescond&userId=4ef7c1e10597&source=-----2e2776e8af86---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2e2776e8af86&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fretro-engineering-a-database-schema-and-quality-checks-gpt-vs-bard-2e2776e8af86&source=-----2e2776e8af86---------------------bookmark_footer-----------)![](img/9c745c0ff34a55794be637adf06abedc.png)

照片由 [Jake Trotman](https://unsplash.com/@jake_t?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/wallpapers/design/pattern?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在我之前关于如何利用生成型 AI 进行数据活动的文章的延续中，我想探讨这种用例：一个数据团队从一个职能部门（比如人力资源）接收到一个整合的数据集，并需要在他们的数据平台中重新设计一个合适的数据模型，以处理未来的查询。

我们将比较 GPT-4 和 Bard 的回答，以确定哪个模型提供了更相关的答案。

*（注意：笔记本和数据源在文章末尾提供）*

# 初始（及最终）数据集

有时，商业解决方案仅允许你从其专有系统中以报告的形式提取信息……而且，如果你运气好，它们甚至可能通过 API 提供访问。

这就是 “MyCompany” 的情况，其中 HRIS 遗留系统只能提供包含公司许多详细信息（其中一些是保密的）的所有员工的一个提取。
