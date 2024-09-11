# 计算机视觉中的自监督学习

> 原文：[https://towardsdatascience.com/self-supervised-learning-in-computer-vision-fd43719b1625?source=collection_archive---------1-----------------------#2023-01-29](https://towardsdatascience.com/self-supervised-learning-in-computer-vision-fd43719b1625?source=collection_archive---------1-----------------------#2023-01-29)

## 如何仅用少量标记样本训练模型

[](https://michaloleszak.medium.com/?source=post_page-----fd43719b1625--------------------------------)[![Michał Oleszak](../Images/61b32e70cec4ba54612a8ca22e977176.png)](https://michaloleszak.medium.com/?source=post_page-----fd43719b1625--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fd43719b1625--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----fd43719b1625--------------------------------) [Michał Oleszak](https://michaloleszak.medium.com/?source=post_page-----fd43719b1625--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc58320fab2a8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fself-supervised-learning-in-computer-vision-fd43719b1625&user=Micha%C5%82+Oleszak&userId=c58320fab2a8&source=post_page-c58320fab2a8----fd43719b1625---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fd43719b1625--------------------------------) ·18分钟阅读·2023年1月29日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffd43719b1625&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fself-supervised-learning-in-computer-vision-fd43719b1625&user=Micha%C5%82+Oleszak&userId=c58320fab2a8&source=-----fd43719b1625---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffd43719b1625&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fself-supervised-learning-in-computer-vision-fd43719b1625&source=-----fd43719b1625---------------------bookmark_footer-----------)![](../Images/94977b17c73bc401bb5cd47c4a23f8ad.png)

迄今为止，AI所带来的大部分价值来自于在越来越大数据集上训练的监督模型。这些数据集中的许多数据由人工标注，标注工作既单调乏味，又耗时、易出错且有时昂贵。自监督学习（SSL）是一种不同的学习范式，允许机器从未标注的数据中学习。在本文中，我们将讨论SSL如何工作以及如何将其应用于计算机视觉。我们将比较简单方法与最先进的方法，并看到SSL在医学诊断中的实际应用，这一领域可以从中受益良多，但同时需要对该方法有深刻理解才能正确实施。

# 什么是自监督学习？

[根据Yann LeCun](https://ai.facebook.com/blog/self-supervised-learning-the-dark-matter-of-intelligence/)，Meta的首席AI科学家，自监督学习是“构建背景知识和在AI系统中近似常识的最有前途的方法之一”。自监督方法的核心思想是对无注释的数据进行模型训练。

> 自监督学习是构建背景知识和近似AI系统中常识的一种最有前途的方法之一。
