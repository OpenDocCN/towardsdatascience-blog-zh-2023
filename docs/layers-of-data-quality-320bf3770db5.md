# 数据质量层次

> 原文：[https://towardsdatascience.com/layers-of-data-quality-320bf3770db5?source=collection_archive---------7-----------------------#2023-07-18](https://towardsdatascience.com/layers-of-data-quality-320bf3770db5?source=collection_archive---------7-----------------------#2023-07-18)

## 如何处理数据中的问题

[](https://medium.com/@cisenbe?source=post_page-----320bf3770db5--------------------------------)[![Chad Isenberg](../Images/56e50c1ee292ac672df4b8062e460c8e.png)](https://medium.com/@cisenbe?source=post_page-----320bf3770db5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----320bf3770db5--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----320bf3770db5--------------------------------) [Chad Isenberg](https://medium.com/@cisenbe?source=post_page-----320bf3770db5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb9113837f160&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flayers-of-data-quality-320bf3770db5&user=Chad+Isenberg&userId=b9113837f160&source=post_page-b9113837f160----320bf3770db5---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----320bf3770db5--------------------------------) ·8 min read·2023年7月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F320bf3770db5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flayers-of-data-quality-320bf3770db5&user=Chad+Isenberg&userId=b9113837f160&source=-----320bf3770db5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F320bf3770db5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flayers-of-data-quality-320bf3770db5&source=-----320bf3770db5---------------------bookmark_footer-----------)![](../Images/1ca0f77d5f6b6a577e793b25b54ba17c.png)

图片由 [Stephen Dawson](https://unsplash.com/@dawson2406?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

随着对生成性AI和大语言模型（LLMs）兴趣的激增，数据质量也重新引起了关注。这一领域本不缺乏关注：像[Monte Carlo](https://www.montecarlodata.com/)、[Soda](https://www.soda.io/)、[Bigeye](https://www.bigeye.com/)、[Sifflet](https://www.siffletdata.com/)、[Great Expectations](https://greatexpectations.io/)和[dbt Labs](https://www.getdbt.com/)等公司已经开发出一系列解决方案，从专有到开放核心。虽然其中一些解决方案是直接竞争对手，但它们并不都解决相同的问题。例如，定义一个明确的[dbt测试](https://docs.getdbt.com/docs/build/tests)以确保某列包含唯一值，与对指标进行异常检测（例如，你的dim_orders过程一天生成了500,000条记录，而通常是50,000条）是非常不同的。数据可以以各种壮观的方式失败。

你可能听说过数据质量维度；我特别喜欢[Richard Farnworth](https://medium.com/u/74abb61a4947?source=post_page-----320bf3770db5--------------------------------)的[观点](/the-six-dimensions-of-data-quality-and-how-to-deal-with-them-bdcf9a3dba71)¹，虽然快速的Google搜索会显示出几十种不同的看法。不过，核心思想是数据在一个方面可能是“正确的”，但在其他方面则是错误的。如果你的数据是正确的但迟到，它有价值吗？如果数字客观上是错误的，但它们是[一致的](https://benn.substack.com/p/all-i-want-is-to-know-whats-different)²呢？这是数据产品管理和识别你的利益相关者优先事项的重要方面。

目前有很多关注点集中在数据的错误格式、缺失、延迟、不完整等方面，对根本原因的关注却较少……
