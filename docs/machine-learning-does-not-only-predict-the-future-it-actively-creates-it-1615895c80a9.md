# 机器学习不仅预测未来，它还积极地创造未来

> 原文：[`towardsdatascience.com/machine-learning-does-not-only-predict-the-future-it-actively-creates-it-1615895c80a9?source=collection_archive---------10-----------------------#2023-01-11`](https://towardsdatascience.com/machine-learning-does-not-only-predict-the-future-it-actively-creates-it-1615895c80a9?source=collection_archive---------10-----------------------#2023-01-11)

## 位置偏差入门（以及它为什么重要）

[](https://medium.com/@samuel.flender?source=post_page-----1615895c80a9--------------------------------)![Samuel Flender](https://medium.com/@samuel.flender?source=post_page-----1615895c80a9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1615895c80a9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1615895c80a9--------------------------------) [Samuel Flender](https://medium.com/@samuel.flender?source=post_page-----1615895c80a9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fce56d9dcd568&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmachine-learning-does-not-only-predict-the-future-it-actively-creates-it-1615895c80a9&user=Samuel+Flender&userId=ce56d9dcd568&source=post_page-ce56d9dcd568----1615895c80a9---------------------post_header-----------) 发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1615895c80a9--------------------------------) ·4 min read·2023 年 1 月 11 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1615895c80a9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmachine-learning-does-not-only-predict-the-future-it-actively-creates-it-1615895c80a9&source=-----1615895c80a9---------------------bookmark_footer-----------)![](img/d5f8f135bbb8c7670a8290bcd5c6cea9.png)

使用 Stable Diffusion 生成的图像

标准的机器学习课程教导我们，机器学习模型通过学习过去存在的模式来对未来进行预测。

这是一个简化的说法，但当这些模型的预测被投入生产并创建反馈循环时，情况会发生剧变：此时，模型的预测本身正在影响模型试图学习的世界。我们的模型不再只是预测未来，它们实际上在创造未来。

这种反馈循环之一是**位置偏差**，这一现象在排名模型中已经被观察到，这些模型为搜索引擎、推荐系统、社交媒体动态和广告排名器提供支持，广泛应用于行业中。

## 什么是位置偏差？

位置偏差意味着排名最高的项目（Netflix 上的视频、Google 上的页面、Amazon 上的产品、Facebook 上的帖子或 Twitter 上的推文）之所以能引发最多的互动，并不是因为它们实际上是用户最需要的内容，而只是因为它们的排名最高。

这种偏差的表现形式是因为排名模型非常优秀，以至于用户开始盲目信任排名靠前的内容…
