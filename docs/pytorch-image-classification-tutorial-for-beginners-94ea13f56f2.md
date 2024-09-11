# PyTorch 图像分类初学者教程

> 原文：[https://towardsdatascience.com/pytorch-image-classification-tutorial-for-beginners-94ea13f56f2?source=collection_archive---------1-----------------------#2023-05-09](https://towardsdatascience.com/pytorch-image-classification-tutorial-for-beginners-94ea13f56f2?source=collection_archive---------1-----------------------#2023-05-09)

## 微调预训练的深度学习模型（Python）

[](https://medium.com/@iamleonie?source=post_page-----94ea13f56f2--------------------------------)[![Leonie Monigatti](../Images/4044b1685ada53a30160b03dc78f9626.png)](https://medium.com/@iamleonie?source=post_page-----94ea13f56f2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----94ea13f56f2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----94ea13f56f2--------------------------------) [Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----94ea13f56f2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3a38da70d8dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpytorch-image-classification-tutorial-for-beginners-94ea13f56f2&user=Leonie+Monigatti&userId=3a38da70d8dc&source=post_page-3a38da70d8dc----94ea13f56f2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----94ea13f56f2--------------------------------) ·22分钟阅读·2023年5月9日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F94ea13f56f2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpytorch-image-classification-tutorial-for-beginners-94ea13f56f2&user=Leonie+Monigatti&userId=3a38da70d8dc&source=-----94ea13f56f2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F94ea13f56f2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpytorch-image-classification-tutorial-for-beginners-94ea13f56f2&source=-----94ea13f56f2---------------------bookmark_footer-----------)![](../Images/f968859c4ae9f104408eddb0177d11ab.png)

“不确定这应该是狮子还是猎豹……”

这个实用教程展示了如何使用 PyTorch 框架通过预训练的深度学习模型对图像进行分类。

这篇适合初学者的图像分类教程与其他教程的区别在于，我们并不是从头开始构建和训练深度神经网络。实际上，只有少数人会从头训练神经网络。大多数深度学习从业者使用预训练模型，并将其微调以适应新任务。

> 实际上，只有少数人会从头训练神经网络。

具体的问题设置是构建一个二分类图像分类模型，以根据一个小数据集来分类猎豹和狮子的图像。为此，我们将使用 PyTorch 对一个预训练的图像分类模型进行微调。

![](../Images/70915a988ae6fb23d0bca9760fa94edc.png)

数据集中的样本图像 [1]。

本教程遵循一个基本的机器学习工作流程：

1.  [准备和探索数据](#758f)

1.  [建立基准](#9377)

1.  [运行实验](#a980)
