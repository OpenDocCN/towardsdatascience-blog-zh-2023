# 在 Python 中创建梯度下降动画

> 原文：[https://towardsdatascience.com/creating-a-gradient-descent-animation-in-python-3c4dcd20ca51?source=collection_archive---------8-----------------------#2023-11-11](https://towardsdatascience.com/creating-a-gradient-descent-animation-in-python-3c4dcd20ca51?source=collection_archive---------8-----------------------#2023-11-11)

## 如何在复杂表面上绘制点的轨迹

[](https://medium.com/@luisdamed?source=post_page-----3c4dcd20ca51--------------------------------)[![Luis Medina](../Images/d83d326290ae3272f0618d0bd28bd875.png)](https://medium.com/@luisdamed?source=post_page-----3c4dcd20ca51--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3c4dcd20ca51--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3c4dcd20ca51--------------------------------) [Luis Medina](https://medium.com/@luisdamed?source=post_page-----3c4dcd20ca51--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F562a027a34f0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-a-gradient-descent-animation-in-python-3c4dcd20ca51&user=Luis+Medina&userId=562a027a34f0&source=post_page-562a027a34f0----3c4dcd20ca51---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3c4dcd20ca51--------------------------------) ·10 min 阅读·2023年11月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3c4dcd20ca51&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-a-gradient-descent-animation-in-python-3c4dcd20ca51&user=Luis+Medina&userId=562a027a34f0&source=-----3c4dcd20ca51---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3c4dcd20ca51&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-a-gradient-descent-animation-in-python-3c4dcd20ca51&source=-----3c4dcd20ca51---------------------bookmark_footer-----------)![](../Images/7d7b4b9e511a33f93c362a2a92aaedc2.png)

图片由 [Todd Diemer](https://unsplash.com/@todd_diemer?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

让我告诉你我是如何创建一个梯度下降的动画来说明博客文章中的一个点的。这样做是值得的，因为我通过这个过程学到了更多 Python 知识，并掌握了一项新技能：制作动画图表。

![](../Images/99b27b5838acc1c636dfd987cb31ace8.png)

在 Python 中创建的梯度下降动画。图片由作者提供。

我将带你逐步了解我所遵循的过程。

## 背景介绍

几天前，我发布了一篇关于梯度下降的[博客文章](https://medium.com/@luisdamed/gradient-descent-f09f19eb35fb)，介绍了作为训练人工神经网络的优化算法的梯度下降。

我想包含一个动画图形，以展示选择不同初始化点对于梯度下降优化可以产生不同结果。

就在这时，我发现了这些[**令人惊叹的动画**](https://imgur.com/a/Hqolp)，它们是多年前由[Alec Radford](https://twitter.com/alecrad)创建的，并在[Reddit 评论](https://www.reddit.com/r/MachineLearning/comments/2gopfa/comment/cklhott/?utm_source=share&utm_medium=web2x&context=3)中分享，展示了如[Adagrad](http://www.jmlr.org/papers/volume12/duchi11a/duchi11a.pdf)、[Adadelta](http://arxiv.org/abs/1212.5701)和[RMSprop](https://class.coursera.org/neuralnets-2012-001/lecture/67)等一些先进梯度下降算法之间的差异。

自从我一直在推动自己去[用 Python 替代 Matlab](https://medium.com/@luisdamed/replacing-matlab-with-python-part-1-322271314e4d)，我决定尝试编写一个类似的动画…
