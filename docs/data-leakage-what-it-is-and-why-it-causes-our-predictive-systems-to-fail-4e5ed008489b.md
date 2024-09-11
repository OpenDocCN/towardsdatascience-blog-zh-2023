# 数据泄漏：它是什么，为什么会导致我们的预测系统失败

> 原文：[https://towardsdatascience.com/data-leakage-what-it-is-and-why-it-causes-our-predictive-systems-to-fail-4e5ed008489b?source=collection_archive---------7-----------------------#2023-08-04](https://towardsdatascience.com/data-leakage-what-it-is-and-why-it-causes-our-predictive-systems-to-fail-4e5ed008489b?source=collection_archive---------7-----------------------#2023-08-04)

## *数据泄漏与过拟合/欠拟合一起，代表了导致机器学习项目进入生产后失败的主要原因*

[](https://medium.com/@theDrewDag?source=post_page-----4e5ed008489b--------------------------------)[![Andrea D'Agostino](../Images/58c7c218815f25278aae59cea44d8771.png)](https://medium.com/@theDrewDag?source=post_page-----4e5ed008489b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4e5ed008489b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4e5ed008489b--------------------------------) [Andrea D'Agostino](https://medium.com/@theDrewDag?source=post_page-----4e5ed008489b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4e8f67b0b09b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-leakage-what-it-is-and-why-it-causes-our-predictive-systems-to-fail-4e5ed008489b&user=Andrea+D%27Agostino&userId=4e8f67b0b09b&source=post_page-4e8f67b0b09b----4e5ed008489b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4e5ed008489b--------------------------------) ·5分钟阅读·2023年8月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4e5ed008489b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-leakage-what-it-is-and-why-it-causes-our-predictive-systems-to-fail-4e5ed008489b&user=Andrea+D%27Agostino&userId=4e8f67b0b09b&source=-----4e5ed008489b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4e5ed008489b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-leakage-what-it-is-and-why-it-causes-our-predictive-systems-to-fail-4e5ed008489b&source=-----4e5ed008489b---------------------bookmark_footer-----------)![](../Images/76015a8e90a6ce9fe834ebd8b66a25a7.png)

[Grianghraf](https://unsplash.com/@grianghraf?utm_source=medium&utm_medium=referral) 摄影作品，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

数据泄漏无疑是一个威胁，它困扰着数据科学家，无论其资历如何。

这是一个现象，可能影响到每个人——即使是拥有多年经验的专业人士。

连同过拟合/欠拟合，**它是导致机器学习项目在生产中失败的主要原因。**

> 数据泄漏发生在训练集中存在的信息泄漏到评估集中（无论是验证集还是测试集）时。

*但是，为什么数据泄漏会导致如此多的受害者？*

因为即使经过了开发阶段的多次实验和评估，**我们的模型在生产环境中仍可能会表现得非常糟糕。**

避免数据泄漏并不容易。我希望通过这篇文章，你能了解为什么以及如何在你的项目中避免数据泄漏！

# 数据泄漏的示例

这里有一个例子，可以帮助你理解什么是数据泄漏。
