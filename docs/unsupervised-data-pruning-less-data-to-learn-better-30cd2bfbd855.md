# 无监督数据剪枝：用更少的数据学习得更好

> 原文：[https://towardsdatascience.com/unsupervised-data-pruning-less-data-to-learn-better-30cd2bfbd855?source=collection_archive---------7-----------------------#2023-02-27](https://towardsdatascience.com/unsupervised-data-pruning-less-data-to-learn-better-30cd2bfbd855?source=collection_archive---------7-----------------------#2023-02-27)

## 基础模型 | 规模法则 | 大模型 | 数据剪枝

## 数据的增加并不总是意味着模型更加准确，但如何选择你的数据呢？

[](https://salvatore-raieli.medium.com/?source=post_page-----30cd2bfbd855--------------------------------)[![Salvatore Raieli](../Images/6bb4520e2df40d20283e7283141b5e06.png)](https://salvatore-raieli.medium.com/?source=post_page-----30cd2bfbd855--------------------------------)[](https://towardsdatascience.com/?source=post_page-----30cd2bfbd855--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----30cd2bfbd855--------------------------------) [Salvatore Raieli](https://salvatore-raieli.medium.com/?source=post_page-----30cd2bfbd855--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff1a08d9452cd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funsupervised-data-pruning-less-data-to-learn-better-30cd2bfbd855&user=Salvatore+Raieli&userId=f1a08d9452cd&source=post_page-f1a08d9452cd----30cd2bfbd855---------------------post_header-----------) 发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----30cd2bfbd855--------------------------------) ·11 min 阅读·2023年2月27日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F30cd2bfbd855&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funsupervised-data-pruning-less-data-to-learn-better-30cd2bfbd855&source=-----30cd2bfbd855---------------------bookmark_footer-----------)![](../Images/1c41d0d7b79c96998daaf3dda4a30237.png)

图片由作者使用 [DALL-E](https://openai.com/dall-e-2/) 制作

规模法则已经在不同的背景下被观察到（图片、文本、语言、语音等）。增加参数的数量真的是更好模型的唯一秘诀吗？如果不是，你实际上可以做些什么？

# 什么是规模法则，为什么它是个问题

近年来，我们看到模型中的参数数量飞速增长。所有大公司都在不断推动创建越来越强大的模型。这导致了基准数据集中的误差减少以及意想不到的行为的出现。**那么，什么是缩放定律？**

**简而言之，缩放定律指出，“测试误差通常会随着训练数据量、模型规模或计算资源的增加而呈幂律下降。”** 换句话说，要提高模型的性能，必须增加这三者中的一个：训练过程中示例的数量、参数的数量，或训练的持续时间。
