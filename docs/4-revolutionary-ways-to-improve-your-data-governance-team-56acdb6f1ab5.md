# 改善数据治理团队的 4 种革命性方法

> 原文：[`towardsdatascience.com/4-revolutionary-ways-to-improve-your-data-governance-team-56acdb6f1ab5`](https://towardsdatascience.com/4-revolutionary-ways-to-improve-your-data-governance-team-56acdb6f1ab5)

## 使用这 4 种变革性方法提升您的数据治理实践。

[](https://hanzalaqureshi.medium.com/?source=post_page-----56acdb6f1ab5--------------------------------)![Hanzala Qureshi](https://hanzalaqureshi.medium.com/?source=post_page-----56acdb6f1ab5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----56acdb6f1ab5--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----56acdb6f1ab5--------------------------------) [Hanzala Qureshi](https://hanzalaqureshi.medium.com/?source=post_page-----56acdb6f1ab5--------------------------------)

·发布在[数据科学的前沿](https://towardsdatascience.com/?source=post_page-----56acdb6f1ab5--------------------------------) ·阅读时间 5 分钟·2023 年 7 月 24 日

--

![](img/54b3ebd34c2152b05dfd0a20553933c3.png)

[照片由 Shubham Dhage](https://unsplash.com/@theshubhamdhage?utm_source=medium&utm_medium=referral)提供，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上。

如果没有有效的治理，组织将发现自己在数字化的荒野中徘徊。

最近，[欧盟人工智能法案](https://artificialcorner.com/eu-ai-regulation-demystified-the-top-10-things-you-need-to-know-94502c90bde4)的进展也指出，对于处理高风险 AI 系统的组织来说，数据治理是必不可少的，以保持合规。尽管数据规模不断扩大，组织却在治理努力上退步，导致数据治理实践变得更加薄弱。

让我们探讨使用现代技术的四种方法，以改进您组织的数据治理。

# 1. 用副驾驶取代数据管家

在我多年的客户数据治理实施过程中，我还没遇到一个满意的数据管家。

他们面临着将数据转化为业务及反向转换的艰巨任务，却几乎没有权力来强制执行合规性。失败的原因通常包括治理模型设计不良、高层管理支持不足或将治理视为一次性项目，投资最小。

[利用 LLM 的副驾驶](https://atlan.com/ai/)可以创建政策文档，捕捉业务和技术元数据，验证数据创建是否符合已商定的标准等。即使您不能投资新工具，副驾驶也可以是一个 Slack/Teams Bot，连接到您组织的元数据工具，简单地回答"常见问题"，如数据的所有者是谁，哪个数据是主数据，某列的定义是什么等，以供最终用户查询。

## **这对数据团队有何影响？**

数据负责人可以通过减少全职资源来找到节省成本的方法。数据工程师可以确保足够的元数据被记录并传递给副驾驶工具，从而防止最终用户的查询流向他们。数据分析师/科学家可以依赖副驾驶工具来回答足够的问题，以帮助他们进行分析查询。

# 2\. 减少论坛和委员会，并在决策中加入智能

论坛从未能做到 100%的高效；总有一些人参加只是为了在电话结束时说“谢谢，再见！”。

一个每周的数据工作组和元数据论坛，一个每两周的数据质量论坛，以及一个每月的数据委员会会议。虽然是出于良好的意图开始的，但大多数这些论坛往往有类似的听众，重复相同或类似的信息。

决策矩阵和智能工作流自动化应该能够处理 90%以上的来袭数据治理请求。例如，如果请求是关于个人位置数据的访问，最终的业务用例是人口统计分析。治理工具可以使用如下的矩阵来生成答案。

![](img/0ef3e8d7f5d47d0ef64c7de1f3e3eac4.png)

作者提供的图片 — 示例决策矩阵

## 这对数据团队有什么影响？

数据团队可以减少在论坛上的时间，把精力集中在战略决策上。数据分析师/科学家可以依赖治理工具（这也可以是一个 Slack/Teams 机器人）提供的智能决策输出，以了解数据是否可访问。

# 3\. 使用语义嵌入智能分类和标记数据

对数据进行分类和标记为数据治理决策添加了急需的上下文。

从历史上看，对数据进行分类一直是最具挑战性的工作之一。这要么是资源密集型的，因为有人必须手动查看数据资产并确定其分类，要么是技术密集型的，需要不断扫描数据以跟踪变化和更新。

[语义嵌入](https://medium.com/towards-data-science/3-simple-and-powerful-ways-this-ai-technique-will-transform-data-management-fe2b66fb9a03)正在革新上下文。对于每个数据资产，可以以向量的形式创建一个上下文嵌入。通过使用相似度评分，这些向量可以按分类排名。例如，涉及客户的数据，如地址、出生日期等，可以通过向量相似度搜索归类为“个人数据”。这也适用于非结构化的[数据如文档](https://hanzalaqureshi.medium.com/the-5-simple-steps-to-create-a-powerful-ai-driven-document-search-tool-ecd502fa6637)。

## 这对数据团队有什么影响？

数据治理团队可以减少手动分类和标记数据的时间。可以围绕这些数据分类构建智能工作流，以提供决策支持，例如“数据是否可以访问？”、“数据是否个人或敏感？”。这些信息可以通过 Co-Pilots 提供给数据分析师/科学家，并用于决策，如 1 和 2 所讨论的。

# 4. 数据产品市场用于编目、发现和访问数据

数据产品市场可以成为您所有数据需求的一站式商店。

我们在数据管理上各自为政，一个工具用于工程，另一个用于管理，还有一个用于治理，还有一个用于隐私。组织通常需要提高在整个系统中整合数据治理的能力，特别是当系统敏捷且不断变化时。

市场是您用于编目和发现数据的区域，它将与您所有不同的数据资产连接。上述所有技术的汇总可以集中到数据产品市场中。例如，包含客户信息的表格将被编目到市场中，使用语义嵌入（3）进行分类和标记，通过智能治理工作流（2）进行访问，并由 Co-Pilot（1）进行上下文化。

## 这对数据团队有什么影响？

数据科学家可以轻松地在市场中找到数据，并通过 Co-Pilot 确定数据是否可用。数据工程师可以重用市场中的数据，而不是创建更多的重复数据。数据分析师可以使用市场中数据的概要来确定其是否适合他们的用例。数据领导者可以依赖一个平台来满足他们的治理和报告需求。

# 结论

现代数据治理功能的目标是减少对人力的依赖，更贴近最终用户，作为帮助者而非障碍和繁文缛节。

尽管我提出了许多新的 AI 技术来利用这些，但有一件事将永远重要：打好基础。如果您的数据质量差，您不需要 Co-Pilot 来重复这一消息。

实现数据质量的正确性是最难完成的任务之一，但因此，也是最显著的投资回报。查看我的免费《终极数据质量手册》，帮助您打好基础，并加入我的邮件列表。

[## 终极数据质量手册 - 免费！](https://hanzalaqureshi.gumroad.com/l/cfijx?layout=profile&source=post_page-----56acdb6f1ab5--------------------------------)

### 介绍《终极数据质量手册：完善数据的最佳实践》。在数据驱动的世界中……

[hanzalaqureshi.gumroad.com](https://hanzalaqureshi.gumroad.com/l/cfijx?layout=profile&source=post_page-----56acdb6f1ab5--------------------------------)
