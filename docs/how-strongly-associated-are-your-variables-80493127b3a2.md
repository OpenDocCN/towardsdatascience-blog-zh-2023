# 你的变量关联程度有多强？

> 原文：[`towardsdatascience.com/how-strongly-associated-are-your-variables-80493127b3a2?source=collection_archive---------6-----------------------#2023-02-28`](https://towardsdatascience.com/how-strongly-associated-are-your-variables-80493127b3a2?source=collection_archive---------6-----------------------#2023-02-28)

## 使用 Cramer’s V 测试来检查两个分类变量的关联程度

[](https://gustavorsantos.medium.com/?source=post_page-----80493127b3a2--------------------------------)![Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----80493127b3a2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----80493127b3a2--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----80493127b3a2--------------------------------) [Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----80493127b3a2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4429d99b1245&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-strongly-associated-are-your-variables-80493127b3a2&user=Gustavo+Santos&userId=4429d99b1245&source=post_page-4429d99b1245----80493127b3a2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----80493127b3a2--------------------------------) ·7 分钟阅读·2023 年 2 月 28 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F80493127b3a2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-strongly-associated-are-your-variables-80493127b3a2&user=Gustavo+Santos&userId=4429d99b1245&source=-----80493127b3a2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F80493127b3a2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-strongly-associated-are-your-variables-80493127b3a2&source=-----80493127b3a2---------------------bookmark_footer-----------)![](img/f1fcc9151c3e67267cfaafc6bc6af713.png)

图片由 [Susan Holt Simpson](https://unsplash.com/pt-br/@shs521?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来自 [Unsplash](https://unsplash.com/photos/H7SCRwU1aiM?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

特征选择是任何数据科学项目中的重要步骤。如果你对这个领域不陌生，我可以想象你已经听过无数次，如果你是新手，我相信你也已经听过，但我还是要再说一遍：*如果你用垃圾数据喂给模型，你最终得到的也会是垃圾结果*。

好了，现在我们把这个说清楚了，就继续吧。有几种不错的方法可以为你的模型选择最佳特征，比如运行一个随机森林模型，然后检查`feature_importances_`属性，使用 sklearn 的`SelectKBest`，单独执行统计测试，以及其他技术。

这些来自**sklearn**的自动化测试非常方便，是让我们快速进行特征选择的绝佳选择。它们是一种自动化的方式，使用统计测试如 F 检验、相关性检验、卡方检验，并快速进行假设测试，以根据结果选择变量。

当我们谈论分类变量时，例如，如果我们运行`SelectKBest`，我们必须使用评分函数`chi2`来确定 p 值是否低于统计显著性阈值…
