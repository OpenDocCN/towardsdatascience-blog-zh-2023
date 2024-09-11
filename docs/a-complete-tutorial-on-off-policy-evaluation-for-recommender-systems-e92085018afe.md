# 完整的离线策略评估教程：针对推荐系统

> 原文：[`towardsdatascience.com/a-complete-tutorial-on-off-policy-evaluation-for-recommender-systems-e92085018afe?source=collection_archive---------4-----------------------#2023-03-11`](https://towardsdatascience.com/a-complete-tutorial-on-off-policy-evaluation-for-recommender-systems-e92085018afe?source=collection_archive---------4-----------------------#2023-03-11)

## 如何缩小离线-在线评估差距

[](https://biarnes-adrien.medium.com/?source=post_page-----e92085018afe--------------------------------)![Adrien Biarnes](https://biarnes-adrien.medium.com/?source=post_page-----e92085018afe--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e92085018afe--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e92085018afe--------------------------------) [Adrien Biarnes](https://biarnes-adrien.medium.com/?source=post_page-----e92085018afe--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F151fca431deb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-complete-tutorial-on-off-policy-evaluation-for-recommender-systems-e92085018afe&user=Adrien+Biarnes&userId=151fca431deb&source=post_page-151fca431deb----e92085018afe---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e92085018afe--------------------------------) ·18 分钟阅读·2023 年 3 月 11 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe92085018afe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-complete-tutorial-on-off-policy-evaluation-for-recommender-systems-e92085018afe&user=Adrien+Biarnes&userId=151fca431deb&source=-----e92085018afe---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe92085018afe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-complete-tutorial-on-off-policy-evaluation-for-recommender-systems-e92085018afe&source=-----e92085018afe---------------------bookmark_footer-----------)![](img/601eef605ca244782eb4bd4437802193.png)

一种跳过离线-在线评估差距的推荐系统（AI 生成的图像）

# 介绍

在本教程中，我将阐述为什么应该关注离线策略评估的理由。然后我会介绍相关的理论，但不会深入探讨最新的方法，而是停留在经验上有效的内容。这将为接下来的实际实施部分提供足够的信息。

# 评估推荐系统

假设你有一个提高推荐系统性能的想法。简单来说，你获得了关于项目的新元数据，例如项目的主要主题，这应该有助于更准确地预测用户在平台上的参与度。那么你如何利用这个新特性？在对新数据进行全面探索之后，你需要对数据管道进行必要的修改，以转换特性并将其与其他特性一起输入模型。现在，你可以触发学习阶段，以生成新版本的模型。

很棒！现在，你如何评估新模型的性能？你怎么判断新特性是否有助于推荐系统并推动更多的…
