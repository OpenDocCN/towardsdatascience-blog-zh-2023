# 归纳偏差的童话故事

> 原文：[https://towardsdatascience.com/a-fairy-tale-of-the-inductive-bias-d418fc61726c?source=collection_archive---------4-----------------------#2023-07-10](https://towardsdatascience.com/a-fairy-tale-of-the-inductive-bias-d418fc61726c?source=collection_archive---------4-----------------------#2023-07-10)

## |归纳偏差| 变换器| 计算机视觉|

## 我们是否需要归纳偏差？简单模型如何达到复杂模型的性能

[](https://salvatore-raieli.medium.com/?source=post_page-----d418fc61726c--------------------------------)[![Salvatore Raieli](../Images/6bb4520e2df40d20283e7283141b5e06.png)](https://salvatore-raieli.medium.com/?source=post_page-----d418fc61726c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d418fc61726c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d418fc61726c--------------------------------) [Salvatore Raieli](https://salvatore-raieli.medium.com/?source=post_page-----d418fc61726c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff1a08d9452cd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-fairy-tale-of-the-inductive-bias-d418fc61726c&user=Salvatore+Raieli&userId=f1a08d9452cd&source=post_page-f1a08d9452cd----d418fc61726c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d418fc61726c--------------------------------) ·18 分钟阅读·2023年7月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd418fc61726c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-fairy-tale-of-the-inductive-bias-d418fc61726c&user=Salvatore+Raieli&userId=f1a08d9452cd&source=-----d418fc61726c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd418fc61726c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-fairy-tale-of-the-inductive-bias-d418fc61726c&source=-----d418fc61726c---------------------bookmark_footer-----------)![](../Images/278a752aca7a7bf5da388e32bb3ac8a8.png)

图片由 [Natalia Y.](https://unsplash.com/@foxfox?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

正如我们在近年来看到的，深度学习在使用频率和模型数量上都有了指数级的增长。促成这一成功的可能是 [迁移学习](https://en.wikipedia.org/wiki/Transfer_learning) 本身——即模型可以通过大量数据进行训练，然后用于各种具体任务的理念。

近年来，出现了一种新范式：用于自然语言处理应用的[transformer](https://en.wikipedia.org/wiki/Transformer_(machine_learning_model))（或基于此模型的其他技术）。而对于图像，则使用[vision transformers](https://en.wikipedia.org/wiki/Vision_transformer)或[卷积网络](https://en.wikipedia.org/wiki/Convolutional_neural_network)。

[](/the-infinite-babel-library-of-llms-90e203b2f6b0?source=post_page-----d418fc61726c--------------------------------) [## LLMs的无限巴别图书馆

### 开源、数据和关注点：LLM（大语言模型）未来的发展将如何改变。

towardsdatascience.com](/the-infinite-babel-library-of-llms-90e203b2f6b0?source=post_page-----d418fc61726c--------------------------------) [](/metas-hiera-reduce-complexity-to-increase-accuracy-30f7a147ad0b?source=post_page-----d418fc61726c--------------------------------) [## META的Hiera：减少复杂性以提高准确性

### 简单性使得人工智能能够达到令人难以置信的性能和惊人的速度。

towardsdatascience.com](/metas-hiera-reduce-complexity-to-increase-accuracy-30f7a147ad0b?source=post_page-----d418fc61726c--------------------------------)
