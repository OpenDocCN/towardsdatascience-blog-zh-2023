# 迁移学习初学者指南

> 原文：[https://towardsdatascience.com/transfer-learning-for-beginner-9b59490d1b9d?source=collection_archive---------1-----------------------#2023-10-29](https://towardsdatascience.com/transfer-learning-for-beginner-9b59490d1b9d?source=collection_archive---------1-----------------------#2023-10-29)

## 图像分类中的迁移学习实用指南

[](https://medium.com/@mina.ghashami?source=post_page-----9b59490d1b9d--------------------------------)[![Mina Ghashami](../Images/745f53b94f5667a485299b49913c7a21.png)](https://medium.com/@mina.ghashami?source=post_page-----9b59490d1b9d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9b59490d1b9d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9b59490d1b9d--------------------------------) [Mina Ghashami](https://medium.com/@mina.ghashami?source=post_page-----9b59490d1b9d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc99ed9ed7b9a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftransfer-learning-for-beginner-9b59490d1b9d&user=Mina+Ghashami&userId=c99ed9ed7b9a&source=post_page-c99ed9ed7b9a----9b59490d1b9d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9b59490d1b9d--------------------------------) ·7分钟阅读·2023年10月29日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9b59490d1b9d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftransfer-learning-for-beginner-9b59490d1b9d&user=Mina+Ghashami&userId=c99ed9ed7b9a&source=-----9b59490d1b9d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9b59490d1b9d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftransfer-learning-for-beginner-9b59490d1b9d&source=-----9b59490d1b9d---------------------bookmark_footer-----------)

在这篇文章中，我们将探讨*迁移学习*的概念，并在*图像分类任务*中看到一个示例。

# 什么是迁移学习？

> 迁移学习是一种深度学习技术，通过利用在大规模数据集上训练好的预训练模型来解决具有有限标记数据的新任务。

这涉及到使用一个已经在源任务中学习到丰富且通用的特征表示的预训练模型，并在目标任务上进行微调。

例如，ImageNet 是一个大型数据集（包含1400万张图片和1000个类别），通常用于训练大型卷积神经网络，如 VGGNet 或 ResNet。

如果我们在 ImageNet 上训练这些网络，这些模型会学习提取强大且富有信息的特征。我们称这种训练为*预训练*，这些模型是*在 ImageNet 上预训练的*。注意，它们是针对 ImageNet 上的图像分类任务进行训练的，我们称之为*源任务*。

要在新的任务上进行迁移学习，我们称之为*目标任务*，首先我们需要有我们的标注数据集，称为*目标数据集*。目标数据集通常比源数据集小得多。我们这里的源数据集非常庞大（有 1400 万张图像）。

然后，我们拿这些预训练的模型并去掉最后的分类层……
