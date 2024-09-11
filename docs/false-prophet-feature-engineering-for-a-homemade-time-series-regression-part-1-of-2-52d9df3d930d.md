# 假预言者：自制时间序列回归的特征工程。

> 原文：[`towardsdatascience.com/false-prophet-feature-engineering-for-a-homemade-time-series-regression-part-1-of-2-52d9df3d930d?source=collection_archive---------4-----------------------#2023-10-13`](https://towardsdatascience.com/false-prophet-feature-engineering-for-a-homemade-time-series-regression-part-1-of-2-52d9df3d930d?source=collection_archive---------4-----------------------#2023-10-13)

## 基于 Meta 的 Prophet 包的理念，创造出强大的时间序列机器学习模型特性。

![Bradley Stephen Shaw](https://bradley-stephen-shaw.medium.com/?source=post_page-----52d9df3d930d--------------------------------) ![Towards Data Science](https://towardsdatascience.com/?source=post_page-----52d9df3d930d--------------------------------) [Bradley Stephen Shaw](https://bradley-stephen-shaw.medium.com/?source=post_page-----52d9df3d930d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc5cd0a58b5ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffalse-prophet-feature-engineering-for-a-homemade-time-series-regression-part-1-of-2-52d9df3d930d&user=Bradley+Stephen+Shaw&userId=c5cd0a58b5ae&source=post_page-c5cd0a58b5ae----52d9df3d930d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----52d9df3d930d--------------------------------) · 15 分钟阅读 · 2023 年 10 月 13 日

--

![](img/11454c39c829e4be351a11be4cce5a77.png)

[Scott Rodgerson](https://unsplash.com/@scottrodgerson?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上的照片。

Meta 的 Prophet 包¹ 是最广泛使用的时间序列包之一。至少根据我的经验，在查看了我为稍后阅读而收藏的时间序列文章列表之后，这样说是合适的。

说回正题，我以前用过这个包，我很喜欢它。

另一个很好的时间序列建模资源是 Vincent Warmerdam 的演讲，标题为“通过简单甚至线性模型获胜”²，其中他讨论了使用线性模型建模时间序列（需要一些准备）。

现在，有些数据科学的元素模糊了艺术和科学的界限——比如超参数调优，或者定义神经网络的结构。

我们将倾向于艺术，并借鉴许多伟大艺术家的做法：借用他人的创意。因此，在这一系列文章中，我们将借用来自 Prophet 的特征工程思想，以及来自 Vincent 的线性建模思想，以对真实世界的时间序列进行我们自己的时间序列回归。

# 总体情况
