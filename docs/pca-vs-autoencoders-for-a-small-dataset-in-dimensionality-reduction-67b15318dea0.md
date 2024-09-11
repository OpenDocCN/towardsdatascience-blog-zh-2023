# 小数据集的降维：PCA与自编码器

> 原文：[https://towardsdatascience.com/pca-vs-autoencoders-for-a-small-dataset-in-dimensionality-reduction-67b15318dea0?source=collection_archive---------7-----------------------#2023-02-16](https://towardsdatascience.com/pca-vs-autoencoders-for-a-small-dataset-in-dimensionality-reduction-67b15318dea0?source=collection_archive---------7-----------------------#2023-02-16)

## 神经网络和深度学习课程：第45部分

[](https://rukshanpramoditha.medium.com/?source=post_page-----67b15318dea0--------------------------------)[![Rukshan Pramoditha](../Images/b80426aff64ff186cb915795644590b1.png)](https://rukshanpramoditha.medium.com/?source=post_page-----67b15318dea0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----67b15318dea0--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----67b15318dea0--------------------------------) [Rukshan Pramoditha](https://rukshanpramoditha.medium.com/?source=post_page-----67b15318dea0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff90a3bb1d400&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpca-vs-autoencoders-for-a-small-dataset-in-dimensionality-reduction-67b15318dea0&user=Rukshan+Pramoditha&userId=f90a3bb1d400&source=post_page-f90a3bb1d400----67b15318dea0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----67b15318dea0--------------------------------) · 8 分钟阅读 · 2023年2月16日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F67b15318dea0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpca-vs-autoencoders-for-a-small-dataset-in-dimensionality-reduction-67b15318dea0&user=Rukshan+Pramoditha&userId=f90a3bb1d400&source=-----67b15318dea0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F67b15318dea0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpca-vs-autoencoders-for-a-small-dataset-in-dimensionality-reduction-67b15318dea0&source=-----67b15318dea0---------------------bookmark_footer-----------)![](../Images/40244380e27388a744474a8be2aa482a.png)

图片由 [Robert Katzki](https://unsplash.com/@ro_ka?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/jbtfM0XBeRc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

***一般的机器学习算法能在小数据集上超越神经网络吗？***

一般来说，深度学习算法如神经网络需要大量的数据才能获得合理的性能。因此，像自编码器这样的神经网络可以从我们用来训练模型的非常大的数据集中获益。

有时，当通用机器学习算法在非常小的数据集上训练时，它们可能会优于神经网络算法。

自编码器也可以用于降维应用，尽管它们在其他流行应用中也被广泛使用，例如图像去噪、图像生成、图像着色、图像压缩、图像超分辨率等。

早些时候，我们通过在*非常大的* MNIST 数据集上训练模型，比较了自编码器在降维方面与 PCA 的性能。在那里，自编码器模型轻松超越了 PCA 模型 [ref¹]，因为 MNIST 数据集大且非线性。

ref¹: [*自编码器如何在降维中超越 PCA*](/how-autoencoders-outperform-pca-in-dimensionality-reduction-1ae44c68b42f)

> 自编码器在处理大型和非线性数据时表现良好…
