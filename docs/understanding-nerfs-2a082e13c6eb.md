# 理解 NeRFs

> 原文：[`towardsdatascience.com/understanding-nerfs-2a082e13c6eb?source=collection_archive---------4-----------------------#2023-04-28`](https://towardsdatascience.com/understanding-nerfs-2a082e13c6eb?source=collection_archive---------4-----------------------#2023-04-28)

## 场景表示的重大突破

[](https://wolfecameron.medium.com/?source=post_page-----2a082e13c6eb--------------------------------)![Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----2a082e13c6eb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2a082e13c6eb--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2a082e13c6eb--------------------------------) [Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----2a082e13c6eb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F28aa6026c553&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-nerfs-2a082e13c6eb&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=post_page-28aa6026c553----2a082e13c6eb---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2a082e13c6eb--------------------------------) ·11 分钟阅读·2023 年 4 月 28 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2a082e13c6eb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-nerfs-2a082e13c6eb&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=-----2a082e13c6eb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2a082e13c6eb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-nerfs-2a082e13c6eb&source=-----2a082e13c6eb---------------------bookmark_footer-----------)![](img/0f3a2a80515f0f2c516a9ede3117054f.png)

（照片由 [nuddle](https://unsplash.com/@nuddle?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/s/photos/3d-scene?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)）

正如我们在像[DeepSDF](https://cameronrwolfe.substack.com/p/3d-generative-modeling-with-deepsdf) [2]和[SRNs](https://cameronrwolfe.substack.com/p/scene-representation-networks) [4]

这种情况随着神经辐射场（NeRFs）[1]的提出而发生了变化，该方法使用前馈神经网络来建模场景和物体的连续表示。NeRFs 使用的表示，称为辐射场，与[早期](https://cameronrwolfe.substack.com/i/94634004/signed-distance-functions) [提案](https://cameronrwolfe.substack.com/i/94842305/occupancy-functions)略有不同。特别是，NeRFs 将五维坐标（即空间位置和视角方向）映射到体积密度和视角相关的 RGB 颜色。通过在不同的视角和位置上累积这些密度和外观信息，我们可以渲染出逼真的新视图。

像[SRNs](https://cameronrwolfe.substack.com/p/scene-representation-networks) [4]一样，NeRFs 可以仅使用一组图像（以及其相关的[相机姿态](https://cameronrwolfe.substack.com/i/97472888/background)）对基础场景进行训练。与以前的方法相比，NeRF 渲染效果更佳。
