# 如何使用 Python 创建一个合成社交网络

> 原文：[https://towardsdatascience.com/how-to-create-a-synthetic-social-network-using-python-eff6451cab14?source=collection_archive---------21-----------------------#2023-01-31](https://towardsdatascience.com/how-to-create-a-synthetic-social-network-using-python-eff6451cab14?source=collection_archive---------21-----------------------#2023-01-31)

## 理解图生成算法在创建合成图中的应用

[](https://medium.com/@rohithtejam?source=post_page-----eff6451cab14--------------------------------)[![Rohith Teja](../Images/3b83438350f3eb69309b9abf715d6ee7.png)](https://medium.com/@rohithtejam?source=post_page-----eff6451cab14--------------------------------)[](https://towardsdatascience.com/?source=post_page-----eff6451cab14--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----eff6451cab14--------------------------------) [Rohith Teja](https://medium.com/@rohithtejam?source=post_page-----eff6451cab14--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd66134dd9f0d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-a-synthetic-social-network-using-python-eff6451cab14&user=Rohith+Teja&userId=d66134dd9f0d&source=post_page-d66134dd9f0d----eff6451cab14---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----eff6451cab14--------------------------------) ·4 分钟阅读·2023年1月31日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Feff6451cab14&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-a-synthetic-social-network-using-python-eff6451cab14&user=Rohith+Teja&userId=d66134dd9f0d&source=-----eff6451cab14---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Feff6451cab14&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-a-synthetic-social-network-using-python-eff6451cab14&source=-----eff6451cab14---------------------bookmark_footer-----------)![](../Images/feb1f7238807a43cbd60f8c1921413a7.png)

照片由 [Maxim Berg](https://unsplash.com/@maxberg?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/kE8-rUKjtQU?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

找到合适的图数据集来评估算法有时可能是一项艰巨的任务。可用的选项有很多，通常需要花费相当长的时间来逐一检查。

即使你找到了完美的图数据集，你也必须验证其使用、共享和隐私政策。

这引出了我们的讨论。

有没有更快的方法找到用于评估目的的图数据集？幸运的是，有！我们需要一种叫做合成图的东西。这些图数据集是人工生成的。

# 合成图的需求

一开始，第一个理由是方便性。

生成自己的数据集很方便，并且不必担心如下问题：

+   控制图的大小

+   隐私和数据共享限制

+   图数据格式

这些理由可能无法说服所有人，还有一些人需要真实的图形用于若干图形分析任务，而合成图无法完成这些任务。
