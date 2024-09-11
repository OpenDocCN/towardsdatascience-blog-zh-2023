# 每位数据科学家都应了解的前10个图像嵌入预训练模型

> 原文：[https://towardsdatascience.com/top-10-pre-trained-models-for-image-embedding-every-data-scientist-should-know-88da0ef541cd?source=collection_archive---------3-----------------------#2023-04-19](https://towardsdatascience.com/top-10-pre-trained-models-for-image-embedding-every-data-scientist-should-know-88da0ef541cd?source=collection_archive---------3-----------------------#2023-04-19)

## 转移学习的基本指南

[](https://satyam-kumar.medium.com/?source=post_page-----88da0ef541cd--------------------------------)[![Satyam Kumar](../Images/2360baa87ea7a20f41589c5f8d783288.png)](https://satyam-kumar.medium.com/?source=post_page-----88da0ef541cd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----88da0ef541cd--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----88da0ef541cd--------------------------------) [Satyam Kumar](https://satyam-kumar.medium.com/?source=post_page-----88da0ef541cd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3d8bf96a415f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftop-10-pre-trained-models-for-image-embedding-every-data-scientist-should-know-88da0ef541cd&user=Satyam+Kumar&userId=3d8bf96a415f&source=post_page-3d8bf96a415f----88da0ef541cd---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----88da0ef541cd--------------------------------) ·9 分钟阅读·2023年4月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F88da0ef541cd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftop-10-pre-trained-models-for-image-embedding-every-data-scientist-should-know-88da0ef541cd&user=Satyam+Kumar&userId=3d8bf96a415f&source=-----88da0ef541cd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F88da0ef541cd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftop-10-pre-trained-models-for-image-embedding-every-data-scientist-should-know-88da0ef541cd&source=-----88da0ef541cd---------------------bookmark_footer-----------)![](../Images/bcf5c3734418b63acaacc768035b72e8.png)

图片来源于 [Chen](https://pixabay.com/users/chenspec-7784448/?utm_source=link-attribution&amp%3Butm_medium=referral&amp%3Butm_campaign=image&amp%3Butm_content=5692896) 从 [Pixabay](https://pixabay.com//?utm_source=link-attribution&amp%3Butm_medium=referral&amp%3Butm_campaign=image&amp%3Butm_content=5692896)

计算机视觉领域的快速发展——图像分类用例因迁移学习的出现而进一步加速。在大量图像数据集上训练计算机视觉神经网络模型需要大量的计算资源和时间。

幸运的是，通过使用预训练模型，这次时间和资源可以缩短。利用预训练模型中的特征表示的技术称为迁移学习。预训练模型通常在高端计算资源和大规模数据集上进行训练。

预训练模型可以以各种方式使用：

+   使用预训练权重，直接对测试数据进行预测

+   使用预训练权重进行初始化，并利用自定义数据集训练模型

+   仅使用预训练网络的架构，并在自定义数据集上从头开始训练

本文介绍了获取图像嵌入的前10种最先进的预训练模型。所有这些预训练模型都可以通过[keras.application](https://keras.io/api/applications/) API作为keras模型加载。
