# 使用ONets进行形状重建

> 原文：[https://towardsdatascience.com/shape-reconstruction-with-onets-1c1afe89c50?source=collection_archive---------12-----------------------#2023-02-07](https://towardsdatascience.com/shape-reconstruction-with-onets-1c1afe89c50?source=collection_archive---------12-----------------------#2023-02-07)

## 使用可学习函数表示3D空间

[](https://wolfecameron.medium.com/?source=post_page-----1c1afe89c50--------------------------------)[![Cameron R. Wolfe, Ph.D.](../Images/52bb88d7cf1105501be2fae5ccbe7a03.png)](https://wolfecameron.medium.com/?source=post_page-----1c1afe89c50--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1c1afe89c50--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1c1afe89c50--------------------------------) [Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----1c1afe89c50--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F28aa6026c553&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshape-reconstruction-with-onets-1c1afe89c50&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=post_page-28aa6026c553----1c1afe89c50---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1c1afe89c50--------------------------------) · 11分钟阅读 · 2023年2月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1c1afe89c50&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshape-reconstruction-with-onets-1c1afe89c50&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=-----1c1afe89c50---------------------clap_footer-----------)

-- 

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1c1afe89c50&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshape-reconstruction-with-onets-1c1afe89c50&source=-----1c1afe89c50---------------------bookmark_footer-----------)![](../Images/7b032ee38e423c492d6d1afd44c4febf.png)

（照片由 [Tareq Ajalyakin](https://unsplash.com/@tareq_aj?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/s/photos/points?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)）

3D 重建问题旨在根据对象的噪声视图（例如，部分点云、2D 图像等）生成高分辨率的对象表示。近年来，深度神经网络成为3D重建的一种流行方法，因为它们能够编码有助于处理歧义的先验信息。简单来说，这意味着如果从输入中无法明确知道正确的重建方式，*神经网络可以利用在训练过程中遇到的其他数据点的经验来生成合理的输出*。

大多数3D重建方法最初在表示高分辨率对象的能力上有限。体素、点云和网格都无法以内存高效的方式建模高分辨率对象。与可以生成高分辨率、逼真图像的模型如 GANs [2] 相比，生成3D几何体的类似方法显得原始。

![](../Images/d5698249a6c8526e8d37cfb37058c157.png)

（来自 [1]）

为了解决这个问题，[1] 的作者开发了一种使用神经网络建模占据函数的3D重建方法。更具体地说，我们训练…
