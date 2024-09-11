# ChatGPT 生成的食品行业评论：真实性评估

> 原文：[https://towardsdatascience.com/chatgpt-generated-food-industry-reviews-realism-assessment-2ee28155970f?source=collection_archive---------11-----------------------#2023-06-09](https://towardsdatascience.com/chatgpt-generated-food-industry-reviews-realism-assessment-2ee28155970f?source=collection_archive---------11-----------------------#2023-06-09)

## 探讨 ChatGPT 生成的数据如何支持食品行业公司的评论和调查收集。

[](https://ben-mccloskey20.medium.com/?source=post_page-----2ee28155970f--------------------------------)[![Benjamin McCloskey](../Images/7118f5933f2affe2a7a4d3375452fa4c.png)](https://ben-mccloskey20.medium.com/?source=post_page-----2ee28155970f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2ee28155970f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2ee28155970f--------------------------------) [Benjamin McCloskey](https://ben-mccloskey20.medium.com/?source=post_page-----2ee28155970f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F503796fc1483&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchatgpt-generated-food-industry-reviews-realism-assessment-2ee28155970f&user=Benjamin+McCloskey&userId=503796fc1483&source=post_page-503796fc1483----2ee28155970f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2ee28155970f--------------------------------) ·13 分钟阅读·2023 年 6 月 9 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2ee28155970f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchatgpt-generated-food-industry-reviews-realism-assessment-2ee28155970f&user=Benjamin+McCloskey&userId=503796fc1483&source=-----2ee28155970f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2ee28155970f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchatgpt-generated-food-industry-reviews-realism-assessment-2ee28155970f&source=-----2ee28155970f---------------------bookmark_footer-----------)![](../Images/45ea0b6597515a537982a20603e6b762.png)

[Annie Spratt](https://unsplash.com/@anniespratt?utm_source=medium&utm_medium=referral) 的照片来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 从哪里开始

我过去的大部分研究使用*生成对抗网络（GAN）*来创建数据集中深度伪造的图像。我想这样做是为了增加数据集中的信息多样性，我预测这将导致更好的目标检测模型（[更多关于此研究的信息请见这里！](https://journals.sagepub.com/doi/abs/10.1177/15485129231170225?journalCode=dmsa)）。虽然这与深度伪造图像创建是完全不同的任务，但我想知道；***是否有办法增加我用于不同公司食品行业评论的数据集的大小？***

我可以训练一个GAN吗？可以，但是GAN在生成表格数据方面表现不佳，对我来说，文本中的单词更符合电子表格中的数据。于是ChatGPT出现了。瞧！我是否可以通过简单地*要求*ChatGPT使用不同的提示生成新的评论来创建新的数据集？

# 为什么这很重要

我们有几个理由想要增加数据集的大小。

> 缺乏足够的数据来训练模型。
> 
> 有偏见的数据集（因此我们…
