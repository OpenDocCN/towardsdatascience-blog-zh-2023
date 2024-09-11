# 场景表示网络

> 原文：[https://towardsdatascience.com/scene-representation-networks-bae6186d00d9?source=collection_archive---------14-----------------------#2023-02-22](https://towardsdatascience.com/scene-representation-networks-bae6186d00d9?source=collection_archive---------14-----------------------#2023-02-22)

## 在无限分辨率下建模复杂的3D场景

[](https://wolfecameron.medium.com/?source=post_page-----bae6186d00d9--------------------------------)[![Cameron R. Wolfe, Ph.D.](../Images/52bb88d7cf1105501be2fae5ccbe7a03.png)](https://wolfecameron.medium.com/?source=post_page-----bae6186d00d9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bae6186d00d9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----bae6186d00d9--------------------------------) [Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----bae6186d00d9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F28aa6026c553&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fscene-representation-networks-bae6186d00d9&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=post_page-28aa6026c553----bae6186d00d9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bae6186d00d9--------------------------------) ·12分钟阅读·2023年2月22日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbae6186d00d9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fscene-representation-networks-bae6186d00d9&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=-----bae6186d00d9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbae6186d00d9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fscene-representation-networks-bae6186d00d9&source=-----bae6186d00d9---------------------bookmark_footer-----------)![](../Images/b9e69450b3fa7f9dc92eb4c50cf7c7bc.png)

（图片由 [Alexandra Gorn](https://unsplash.com/@alexagorn?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/s/photos/living-room?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)）

我们最近已经看到（例如，[DeepSDF](https://cameronrwolfe.substack.com/p/3d-generative-modeling-with-deepsdf)和[ONets](https://cameronrwolfe.substack.com/p/shape-reconstruction-with-onets) [3, 5]）如何通过神经网络来表示3D几何体。但这些方法存在一些限制，例如需要真实的3D几何体进行训练和推断。此外，*如果我们想表示整个场景而不仅仅是一个物体或几何体*，该如何处理？这需要同时建模几何体和外观；下面有一个示例。幸运的是，只要方法正确，神经网络完全能够以这种方式建模3D场景。

![](../Images/0c7b3bc733de45910ed93862eab0bc9f.png)

（来自 [1] 和 [3]）

一种建模3D场景的方法是通过场景表示网络（SRNs）[1]。SRNs将场景建模为一个连续函数，该函数将每个3D坐标映射到一个描述该位置物体形状和外观的表示。这个函数是通过前馈神经网络学习的。然后，SRNs使用一个可学习的渲染算法生成底层3D场景的新视角（即，仅2D图像）。前馈网络和渲染算法都可以仅使用2D场景图像进行端到端训练。
