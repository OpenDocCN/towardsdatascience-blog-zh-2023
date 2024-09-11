# 2 项任务，提升你的 Python 数据整理技能

> 原文：[`towardsdatascience.com/2-tasks-to-boost-your-python-data-wrangling-skills-3daf6c1c0528?source=collection_archive---------6-----------------------#2023-11-15`](https://towardsdatascience.com/2-tasks-to-boost-your-python-data-wrangling-skills-3daf6c1c0528?source=collection_archive---------6-----------------------#2023-11-15)

## 如何将原始数据转换为更可用和结构化的格式。

[](https://sonery.medium.com/?source=post_page-----3daf6c1c0528--------------------------------)![Soner Yıldırım](https://sonery.medium.com/?source=post_page-----3daf6c1c0528--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3daf6c1c0528--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3daf6c1c0528--------------------------------) [Soner Yıldırım](https://sonery.medium.com/?source=post_page-----3daf6c1c0528--------------------------------)

·

[跟随](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2cf6b549448&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F2-tasks-to-boost-your-python-data-wrangling-skills-3daf6c1c0528&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=post_page-2cf6b549448----3daf6c1c0528---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3daf6c1c0528--------------------------------) ·10 min read·2023 年 11 月 15 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3daf6c1c0528&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F2-tasks-to-boost-your-python-data-wrangling-skills-3daf6c1c0528&source=-----3daf6c1c0528---------------------bookmark_footer-----------)![](img/b5fe9b41518066881b6754622542b13b.png)

（图片由作者使用 Midjourney 创建）

当学习一个新工具时，我们通常会查阅文档，观看教程，阅读文章，并解决例子。这是一个足够好的方法，可以在一定程度上帮助你学习工具。

然而，当我们开始在实际环境中使用工具或解决真实问题时，我们需要超越大多数教程所涵盖的内容。

在本文中，我将逐步解释如何在工作中使用 Python 处理两种不同的数据清洗和预处理任务。对于每个任务，我将展示原始数据和所需的格式。然后，我将解释将数据转换为该格式的代码。

我们将深入探讨 Python 的内置数据结构和 Pandas 库，所以你应该期待学习一些有趣的数据处理技巧。

# 1\. 问题统计

我有一个包含问题及其摘要的 DataFrame。我没有使用或分享我这里的原始数据。相反，我生成了与原始数据格式相同的模拟数据。如果你想通过执行代码来跟进，请从我的[数据集](https://github.com/SonerYldrm/datasets)库中下载“mock_issues.csv”文件。
