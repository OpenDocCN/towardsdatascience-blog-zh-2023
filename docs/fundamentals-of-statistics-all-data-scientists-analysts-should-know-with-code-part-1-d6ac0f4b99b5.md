# 统计学基础：所有数据科学家和分析师都应该知道的内容 — 带代码 — 第一部分

> 原文：[`towardsdatascience.com/fundamentals-of-statistics-all-data-scientists-analysts-should-know-with-code-part-1-d6ac0f4b99b5?source=collection_archive---------13-----------------------#2023-01-31`](https://towardsdatascience.com/fundamentals-of-statistics-all-data-scientists-analysts-should-know-with-code-part-1-d6ac0f4b99b5?source=collection_archive---------13-----------------------#2023-01-31)

## 本文是针对数据科学家和数据分析师统计学基础的全面概述。

[](https://zoumanakeita.medium.com/?source=post_page-----d6ac0f4b99b5--------------------------------)![Zoumana Keita](https://zoumanakeita.medium.com/?source=post_page-----d6ac0f4b99b5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d6ac0f4b99b5--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d6ac0f4b99b5--------------------------------) [Zoumana Keita](https://zoumanakeita.medium.com/?source=post_page-----d6ac0f4b99b5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe6ae785a30d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffundamentals-of-statistics-all-data-scientists-analysts-should-know-with-code-part-1-d6ac0f4b99b5&user=Zoumana+Keita&userId=e6ae785a30d&source=post_page-e6ae785a30d----d6ac0f4b99b5---------------------post_header-----------) 发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----d6ac0f4b99b5--------------------------------) ·8 分钟阅读·2023 年 1 月 31 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd6ac0f4b99b5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffundamentals-of-statistics-all-data-scientists-analysts-should-know-with-code-part-1-d6ac0f4b99b5&user=Zoumana+Keita&userId=e6ae785a30d&source=-----d6ac0f4b99b5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd6ac0f4b99b5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffundamentals-of-statistics-all-data-scientists-analysts-should-know-with-code-part-1-d6ac0f4b99b5&source=-----d6ac0f4b99b5---------------------bookmark_footer-----------)![](img/882412b9f5ac2270822bb3c04c517e8c.png)

图片由[Clay Banks](https://unsplash.com/@claybanks)提供，来源于[Unsplash](https://unsplash.com/photos/no2blvVYoJw)。

# 动机

构建机器学习模型用于预测是很酷的。然而，当涉及到更好地理解你的业务问题时，机器学习模型并不合适，这需要在统计建模上花费更多时间。

本文将首先帮助你建立统计学基础知识的理解，这对数据科学家和数据分析师的日常工作非常有益，从而帮助企业做出可行的决策。它还将指导你通过实践来应用这些统计概念，使用 Python 进行操作。

如果你更喜欢视频而不是阅读文章，那么这个适合你 👇🏽

# 总体与样本之间有什么区别？
