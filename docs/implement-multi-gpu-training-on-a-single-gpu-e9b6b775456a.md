# 在 3 分钟内实现单 GPU 系统上的多 GPU 训练

> 原文：[https://towardsdatascience.com/implement-multi-gpu-training-on-a-single-gpu-e9b6b775456a?source=collection_archive---------13-----------------------#2023-05-15](https://towardsdatascience.com/implement-multi-gpu-training-on-a-single-gpu-e9b6b775456a?source=collection_archive---------13-----------------------#2023-05-15)

## TensorFlow 高级指南

[](https://medium.com/@SaschaKirch?source=post_page-----e9b6b775456a--------------------------------)[![Sascha Kirch](../Images/a0d45da9dc9c602075b2810786c660c9.png)](https://medium.com/@SaschaKirch?source=post_page-----e9b6b775456a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e9b6b775456a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e9b6b775456a--------------------------------) [Sascha Kirch](https://medium.com/@SaschaKirch?source=post_page-----e9b6b775456a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5c38dace9d5e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplement-multi-gpu-training-on-a-single-gpu-e9b6b775456a&user=Sascha+Kirch&userId=5c38dace9d5e&source=post_page-5c38dace9d5e----e9b6b775456a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e9b6b775456a--------------------------------) · 3 分钟阅读 · 2023年5月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe9b6b775456a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplement-multi-gpu-training-on-a-single-gpu-e9b6b775456a&user=Sascha+Kirch&userId=5c38dace9d5e&source=-----e9b6b775456a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe9b6b775456a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplement-multi-gpu-training-on-a-single-gpu-e9b6b775456a&source=-----e9b6b775456a---------------------bookmark_footer-----------)![](../Images/dbabbc57ed819e80bd93ba392243de25.png)

图片由 [Chris Liverani](https://unsplash.com/@chrisliverani?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我想和你分享一个小技巧，介绍我如何在单个 GPU 上测试我的多 GPU 训练代码。

# 动机

我想问题很明显，你可能也经历过。你想训练一个深度学习模型，并且希望利用多个 GPU、TPU 甚至多个工作节点来提高速度或扩大批量大小。但当然，你不能（或者说不应该，因为我见过很多次 😅）占用通常共享的硬件进行调试，甚至花费大量的钱购买付费云实例。

让我告诉你，重要的不是你的系统有多少物理 GPU，而是你的软件认为它有多少。关键词是：***(设备) 虚拟化***。

# 让我们来实现它

首先让我们看一看通常如何**检测和连接到您的 GPU**：
