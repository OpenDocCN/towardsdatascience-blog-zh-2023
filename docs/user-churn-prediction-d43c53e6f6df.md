# 用户流失预测

> 原文：[https://towardsdatascience.com/user-churn-prediction-d43c53e6f6df?source=collection_archive---------4-----------------------#2023-12-23](https://towardsdatascience.com/user-churn-prediction-d43c53e6f6df?source=collection_archive---------4-----------------------#2023-12-23)

## 现代数据仓库和机器学习

[](https://mshakhomirov.medium.com/?source=post_page-----d43c53e6f6df--------------------------------)[![💡Mike Shakhomirov](../Images/bc6895c7face3244d488feb97ba0f68e.png)](https://mshakhomirov.medium.com/?source=post_page-----d43c53e6f6df--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d43c53e6f6df--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d43c53e6f6df--------------------------------) [💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----d43c53e6f6df--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe06a48b3dd48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuser-churn-prediction-d43c53e6f6df&user=%F0%9F%92%A1Mike+Shakhomirov&userId=e06a48b3dd48&source=post_page-e06a48b3dd48----d43c53e6f6df---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----d43c53e6f6df--------------------------------) ·12分钟阅读·2023年12月23日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd43c53e6f6df&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuser-churn-prediction-d43c53e6f6df&source=-----d43c53e6f6df---------------------bookmark_footer-----------)![](../Images/0a9d2bfb8a03495210b04f4972d48852.png)

照片由[马丁·亚当斯](https://unsplash.com/@martinadams?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

毫无疑问，**用户保留**是许多公司和在线应用的重要性能指标。我们将讨论如何利用内置的数据仓库机器学习功能来运行用户行为数据上的倾向模型，以确定**用户流失**的可能性。在这个故事中，我想专注于数据集准备和使用标准 SQL 进行模型训练。现代数据仓库允许这样做。确实，保留是一个重要的业务指标，有助于了解用户行为的机制。它提供了一个高层次的概述，说明我们的应用在保留用户方面有多成功，通过一个简单的问题来回答：我们的应用是否足够好，能够保留用户？一个众所周知的事实是，保留现有用户比获取新用户要便宜。

在我之前的一篇文章中，我写到了现代数据仓库[1]。

[](/modern-data-warehousing-2b1b0486ce4a?source=post_page-----d43c53e6f6df--------------------------------) [## 现代数据仓库

### 先进的数据平台设计

towardsdatascience.com](/modern-data-warehousing-2b1b0486ce4a?source=post_page-----d43c53e6f6df--------------------------------)

现代数据仓库具有许多有用的功能和组件，使其与其他数据平台类型有所不同[2]。

> ML 模型支持似乎是…
