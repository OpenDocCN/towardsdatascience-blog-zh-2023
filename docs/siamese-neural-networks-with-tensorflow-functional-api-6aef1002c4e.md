# 使用三元损失和余弦距离的暹罗神经网络

> 原文：[https://towardsdatascience.com/siamese-neural-networks-with-tensorflow-functional-api-6aef1002c4e?source=collection_archive---------12-----------------------#2023-05-12](https://towardsdatascience.com/siamese-neural-networks-with-tensorflow-functional-api-6aef1002c4e?source=collection_archive---------12-----------------------#2023-05-12)

## 理论与代码实践：在CIFAR-10数据集上使用三元损失和余弦距离进行暹罗网络的实验

[](https://tanpengshi.medium.com/?source=post_page-----6aef1002c4e--------------------------------)[![陈鹏石](../Images/449d736eed966b01715f87c6269c88a5.png)](https://tanpengshi.medium.com/?source=post_page-----6aef1002c4e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6aef1002c4e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6aef1002c4e--------------------------------) [陈鹏石](https://tanpengshi.medium.com/?source=post_page-----6aef1002c4e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4898b27c4ef2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsiamese-neural-networks-with-tensorflow-functional-api-6aef1002c4e&user=Tan+Pengshi+Alvin&userId=4898b27c4ef2&source=post_page-4898b27c4ef2----6aef1002c4e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6aef1002c4e--------------------------------) ·11分钟阅读·2023年5月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6aef1002c4e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsiamese-neural-networks-with-tensorflow-functional-api-6aef1002c4e&user=Tan+Pengshi+Alvin&userId=4898b27c4ef2&source=-----6aef1002c4e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6aef1002c4e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsiamese-neural-networks-with-tensorflow-functional-api-6aef1002c4e&source=-----6aef1002c4e---------------------bookmark_footer-----------)![](../Images/f7edeabf5d107ac8fe08ae568bf787fb.png)

图片由 [Alex Meier](https://unsplash.com/@alexmeier19) 提供在 [Unsplash](https://unsplash.com/)

如果我们能够将每个对象图像（例如人脸等）编码为模板——一组数字向量，那会怎么样？随后，通过数值比较——找到它们模板之间的距离，我们可以客观地确定对象之间的相似性。在深度学习中，这正是暹罗神经网络希望实现的。

孪生神经网络基本上是一些模型，经过训练后，为每个输入对象生成独特的特征向量（模板）。虽然这些模型通常应用于模板化对象图像（计算机视觉），但它们也可以用于文本和声音数据。

除了安全认证，如面部识别和签名比较，孪生神经网络在电子商务平台中也常用于测量产品相似度。例如，一些电子商务平台允许你通过上传你寻找的物品的图片来搜索类似产品。在Kaggle上，还有一个由东南亚领先电子商务公司Shopee主办的[产品匹配比赛](https://www.kaggle.com/competitions/shopee-product-matching)。
