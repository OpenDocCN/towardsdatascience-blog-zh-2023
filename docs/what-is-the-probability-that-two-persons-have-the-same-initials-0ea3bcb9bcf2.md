# 两个人拥有相同首字母的概率是多少？

> 原文：[https://towardsdatascience.com/what-is-the-probability-that-two-persons-have-the-same-initials-0ea3bcb9bcf2?source=collection_archive---------9-----------------------#2023-12-06](https://towardsdatascience.com/what-is-the-probability-that-two-persons-have-the-same-initials-0ea3bcb9bcf2?source=collection_archive---------9-----------------------#2023-12-06)

## 学习如何使用模拟、重复实验和 R 语言中的循环来回答许多概率问题

[](https://antoinesoetewey.medium.com/?source=post_page-----0ea3bcb9bcf2--------------------------------)[![Antoine Soetewey](../Images/51d7837d18ff15a62cac2343a485e35d.png)](https://antoinesoetewey.medium.com/?source=post_page-----0ea3bcb9bcf2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----0ea3bcb9bcf2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----0ea3bcb9bcf2--------------------------------) [Antoine Soetewey](https://antoinesoetewey.medium.com/?source=post_page-----0ea3bcb9bcf2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fca32a96e6dc7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-the-probability-that-two-persons-have-the-same-initials-0ea3bcb9bcf2&user=Antoine+Soetewey&userId=ca32a96e6dc7&source=post_page-ca32a96e6dc7----0ea3bcb9bcf2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----0ea3bcb9bcf2--------------------------------) ·14分钟阅读·2023年12月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F0ea3bcb9bcf2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-the-probability-that-two-persons-have-the-same-initials-0ea3bcb9bcf2&user=Antoine+Soetewey&userId=ca32a96e6dc7&source=-----0ea3bcb9bcf2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F0ea3bcb9bcf2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-the-probability-that-two-persons-have-the-same-initials-0ea3bcb9bcf2&source=-----0ea3bcb9bcf2---------------------bookmark_footer-----------)![](../Images/b36732ed29bfc20fcc73ac6fe8f5bb8a.png)

图片来源：[Mario Gogh](https://unsplash.com/@mariogogh?utm_source=medium&utm_medium=referral)

# 介绍

上周，我加入了一个团队来参与一个合作项目。这个团队已经成立了几个月，几位科学家一起在这个项目上工作。为了简化起见，他们通常用首字母签署文件、在邮件中提及同事等（即名字的首字母加上姓氏的首字母）。

加入项目几天后，当我需要用我的首字母签署第一份文件时，我们意识到团队中还有另一个人的首字母与我完全相同。

这实际上并没有成为问题，因为我们决定我将用反向首字母签署，即用“SA”代替“AS”，而另一个人则继续用“AS”签名。

本来可以到此为止。然而，当团队领导在会议中间声称：“你们俩有相同的首字母真是非常不幸！这种事发生在我们身上的概率有多大？！”时，我突然想到写一篇关于这个相当琐碎的轶事的文章。
