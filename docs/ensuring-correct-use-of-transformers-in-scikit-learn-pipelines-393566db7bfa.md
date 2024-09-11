# 确保在 Scikit-learn 管道中正确使用转换器

> 原文：[https://towardsdatascience.com/ensuring-correct-use-of-transformers-in-scikit-learn-pipelines-393566db7bfa?source=collection_archive---------8-----------------------#2023-12-20](https://towardsdatascience.com/ensuring-correct-use-of-transformers-in-scikit-learn-pipelines-393566db7bfa?source=collection_archive---------8-----------------------#2023-12-20)

## 机器学习项目中的有效数据处理

[](https://qtalen.medium.com/?source=post_page-----393566db7bfa--------------------------------)[![Peng Qian](../Images/9ce9aeb381ec6b017c1ee5d4714937e2.png)](https://qtalen.medium.com/?source=post_page-----393566db7bfa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----393566db7bfa--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----393566db7bfa--------------------------------) [Peng Qian](https://qtalen.medium.com/?source=post_page-----393566db7bfa--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8e2fe735546d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fensuring-correct-use-of-transformers-in-scikit-learn-pipelines-393566db7bfa&user=Peng+Qian&userId=8e2fe735546d&source=post_page-8e2fe735546d----393566db7bfa---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----393566db7bfa--------------------------------) ·11分钟阅读·2023年12月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F393566db7bfa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fensuring-correct-use-of-transformers-in-scikit-learn-pipelines-393566db7bfa&user=Peng+Qian&userId=8e2fe735546d&source=-----393566db7bfa---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F393566db7bfa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fensuring-correct-use-of-transformers-in-scikit-learn-pipelines-393566db7bfa&source=-----393566db7bfa---------------------bookmark_footer-----------)![](../Images/b6187b45b1a6ac0f9da54deec3eb9b02.png)

确保在 Scikit-learn 管道中正确使用转换器。作者提供的图像

本文将解释如何在 Scikit-Learn（sklearn）项目中正确使用 [Pipeline](https://scikit-learn.org/stable/modules/compose.html?ref=dataleadsfuture.com) 和 [Transformers](https://scikit-learn.org/stable/data_transforms.html?ref=dataleadsfuture.com)，以加速和重复使用我们的模型训练过程。

本文补充并澄清了官方文档中的管道示例及一些常见的误解。

我希望在阅读完这篇文章后，你能够利用Pipeline这一优秀的设计，更好地完成你的机器学习任务。

# 介绍

在全球的中国餐馆中，有一道著名的菜肴叫做“左宗棠鸡”，我想知道你是否尝试过它。

![](../Images/fe3cd203f33891ed74abe3884196dd74.png)

左宗棠鸡。标准化烹饪过程的典范。图片来源：作者创作，[Canva](https://www.canva.com/?ref=dataleadsfuture.com)。

“左宗棠鸡”的一个特点是每块鸡肉都经过厨师处理，使其尺寸相同。这确保了：

1.  所有的食材都经过相同时间的腌制。

1.  在烹饪过程中，每块鸡肉都达到相同的熟度。

1.  使用筷子时，均匀的尺寸使得夹取食物更加容易……
