# 本地光场融合

> 原文：[https://towardsdatascience.com/local-light-field-fusion-14c07ed36117?source=collection_archive---------18-----------------------#2023-04-11](https://towardsdatascience.com/local-light-field-fusion-14c07ed36117?source=collection_archive---------18-----------------------#2023-04-11)

## 如何在智能手机上渲染3D场景

[](https://wolfecameron.medium.com/?source=post_page-----14c07ed36117--------------------------------)[![Cameron R. Wolfe, Ph.D.](../Images/52bb88d7cf1105501be2fae5ccbe7a03.png)](https://wolfecameron.medium.com/?source=post_page-----14c07ed36117--------------------------------)[](https://towardsdatascience.com/?source=post_page-----14c07ed36117--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----14c07ed36117--------------------------------) [Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----14c07ed36117--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F28aa6026c553&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flocal-light-field-fusion-14c07ed36117&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=post_page-28aa6026c553----14c07ed36117---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----14c07ed36117--------------------------------) ·12分钟阅读·2023年4月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F14c07ed36117&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flocal-light-field-fusion-14c07ed36117&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=-----14c07ed36117---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F14c07ed36117&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flocal-light-field-fusion-14c07ed36117&source=-----14c07ed36117---------------------bookmark_footer-----------)![](../Images/d1c22270367a41b360ab04e39462e1a2.png)

（照片由 [Clyde He](https://unsplash.com/@clyde_he?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/backgrounds/colors/light?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)）

到现在为止，我们应该知道深度学习是表示3D场景和从任意视点生成这些场景新渲染的好方法。然而，我们迄今为止看到的方法（例如，[ONets](https://cameronrwolfe.substack.com/p/shape-reconstruction-with-onets)和[SRNs](https://cameronrwolfe.substack.com/p/scene-representation-networks) [2, 3]）的问题在于，它们需要大量场景图像来训练模型。考虑到这一点，我们可能会想是否有可能使用更少的底层场景样本来获得基于深度学习的场景表示。*我们实际需要多少图像来训练高分辨率的场景表示？*

这个问题由局部光场融合（LLFF）[1]方法解决，该方法用于在3D中合成场景。LLFF是[光场](http://lightfield-forum.com/what-is-the-lightfield/)渲染[4]的扩展，LLFF通过将几组现有视图扩展为多平面图像（MPI）表示，生成场景视点，然后通过混合这些表示来渲染新视点。生成的方法：

1.  准确地建模复杂场景和效果，如反射。

1.  理论上，减少了生成准确场景表示所需的样本/图像数量。

此外，*LLFF是规定性的*，这意味着该框架可以用于告知用户需要多少和什么类型的图像……
