# 揭开辍学层的面纱：增强神经网络的必要工具

> 原文：[`towardsdatascience.com/unveiling-the-dropout-layer-an-essential-tool-for-enhancing-neural-networks-e090b726561e?source=collection_archive---------8-----------------------#2023-05-19`](https://towardsdatascience.com/unveiling-the-dropout-layer-an-essential-tool-for-enhancing-neural-networks-e090b726561e?source=collection_archive---------8-----------------------#2023-05-19)

## 理解辍学层：通过辍学正则化改进神经网络训练和减少过渡拟合

[](https://medium.com/@niklas_lang?source=post_page-----e090b726561e--------------------------------)![Niklas Lang](https://medium.com/@niklas_lang?source=post_page-----e090b726561e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e090b726561e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e090b726561e--------------------------------) [Niklas Lang](https://medium.com/@niklas_lang?source=post_page-----e090b726561e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3631074637a9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funveiling-the-dropout-layer-an-essential-tool-for-enhancing-neural-networks-e090b726561e&user=Niklas+Lang&userId=3631074637a9&source=post_page-3631074637a9----e090b726561e---------------------post_header-----------) 发表在[数据科学近距离](https://towardsdatascience.com/?source=post_page-----e090b726561e--------------------------------) ·7 分钟阅读·2023 年 5 月 19 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe090b726561e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funveiling-the-dropout-layer-an-essential-tool-for-enhancing-neural-networks-e090b726561e&user=Niklas+Lang&userId=3631074637a9&source=-----e090b726561e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe090b726561e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funveiling-the-dropout-layer-an-essential-tool-for-enhancing-neural-networks-e090b726561e&source=-----e090b726561e---------------------bookmark_footer-----------)![](img/8cbac42a6864089508adf36e1897f5f7.png)

照片由[Martin Sanchez](https://unsplash.com/@martinsanchez?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上提供

dropout 层是用于构建[神经网络](https://databasecamp.de/en/ml/artificial-neural-networks)的一种层，用于防止[过拟合](https://databasecamp.de/en/ml/overfitting-en)。在这个过程中，个别节点在各种训练运行中会根据概率被排除，就好像它们根本不属于网络结构一样。

然而，在我们深入了解这一层的细节之前，我们应首先理解神经网络的工作原理以及[过拟合](https://databasecamp.de/en/ml/overfitting-en)为何会发生。

# 快速回顾：感知器是如何工作的？

感知器是一个受人脑结构启发的数学模型。它由一个接收不同权重数值输入的单一神经元组成。输入被乘以它们的权重后相加，结果通过一个激活函数。在最简单的形式中，感知器根据激活函数产生二进制输出，例如“是”或“否”。 sigmoid 函数通常作为激活函数使用，将加权和映射到 0 和 1 之间的值。如果加权和超过某个阈值，输出将从…
