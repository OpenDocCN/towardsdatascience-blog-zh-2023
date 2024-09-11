# 7 个 Scikit-Learn 数据科学最佳实践

> 原文：[`towardsdatascience.com/7-scikit-learn-best-practices-for-data-scientists-f232a6ed2756?source=collection_archive---------22-----------------------#2023-01-10`](https://towardsdatascience.com/7-scikit-learn-best-practices-for-data-scientists-f232a6ed2756?source=collection_archive---------22-----------------------#2023-01-10)

## 充分利用这个机器学习包的技巧

[](https://medium.com/@aashishnair?source=post_page-----f232a6ed2756--------------------------------)![Aashish Nair](https://medium.com/@aashishnair?source=post_page-----f232a6ed2756--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f232a6ed2756--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f232a6ed2756--------------------------------) [Aashish Nair](https://medium.com/@aashishnair?source=post_page-----f232a6ed2756--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3087ba81e065&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F7-scikit-learn-best-practices-for-data-scientists-f232a6ed2756&user=Aashish+Nair&userId=3087ba81e065&source=post_page-3087ba81e065----f232a6ed2756---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f232a6ed2756--------------------------------) ·5 分钟阅读·2023 年 1 月 10 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff232a6ed2756&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F7-scikit-learn-best-practices-for-data-scientists-f232a6ed2756&user=Aashish+Nair&userId=3087ba81e065&source=-----f232a6ed2756---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff232a6ed2756&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F7-scikit-learn-best-practices-for-data-scientists-f232a6ed2756&source=-----f232a6ed2756---------------------bookmark_footer-----------)![](img/0ac004ef554c3720bab10cad7e2a3756.png)

图片由 [John Schnobrich](https://unsplash.com/@johnschno?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

Scikit Learn 是机器学习的首选库之一，很容易理解为什么。这个包由简单但有效的工具组成，并附有非常详尽的文档。

然而，尽管使用起来很简单，如果你不遵循某些实践，尤其是作为初学者时，很容易犯错。我自己看到一些在我以前的工作中使用该模块时出现的明显错误，真的很想捂脸。

**最终**，即使你严格遵循文档，仍然容易遗漏某些关键特征或做出次优决策。

因此，我结合过去的经验，*深入探讨* 7 个 Scikit Learn 最佳实践，以有效进行预测数据分析。

## 1\. 使用 Scikit Learn（而非 Pandas）进行特征工程

Scikit Learn 专为机器学习任务设计，这当然包括特征工程。然而，一些人习惯使用 Pandas 进行某些操作（例如，一热编码），因为这是大多数人首先学习的包。
