# 线性代数：欧几里得向量空间

> 原文：[https://towardsdatascience.com/linear-algebra-euclidean-vector-space-9f88f69cf240?source=collection_archive---------16-----------------------#2023-03-06](https://towardsdatascience.com/linear-algebra-euclidean-vector-space-9f88f69cf240?source=collection_archive---------16-----------------------#2023-03-06)

## 第五部分：欧几里得向量空间的温和介绍

[](https://chaodeyu.medium.com/?source=post_page-----9f88f69cf240--------------------------------)[![Chao De-Yu](../Images/8d6805b4797dcc71fa722bbb3d06a91b.png)](https://chaodeyu.medium.com/?source=post_page-----9f88f69cf240--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9f88f69cf240--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9f88f69cf240--------------------------------) [Chao De-Yu](https://chaodeyu.medium.com/?source=post_page-----9f88f69cf240--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5b7be08f8f4c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flinear-algebra-euclidean-vector-space-9f88f69cf240&user=Chao+De-Yu&userId=5b7be08f8f4c&source=post_page-5b7be08f8f4c----9f88f69cf240---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9f88f69cf240--------------------------------) ·4分钟阅读·2023年3月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9f88f69cf240&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flinear-algebra-euclidean-vector-space-9f88f69cf240&user=Chao+De-Yu&userId=5b7be08f8f4c&source=-----9f88f69cf240---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9f88f69cf240&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flinear-algebra-euclidean-vector-space-9f88f69cf240&source=-----9f88f69cf240---------------------bookmark_footer-----------)![](../Images/7db52c533a6259da2ec2ff9d61a28347.png)

[Karsten Würth](https://unsplash.com/@karsten_wuerth) 摄影，来源于 [Unsplash](https://unsplash.com)

# 介绍

在机器学习和深度学习中，我们大多数时间都在处理向量。而向量空间模型可以表示数据之间的关系。此外，从几何的角度来看，它也能够比较两个向量的相似性，方法包括使用两个向量之间的距离（欧几里得距离）或两个向量之间的角度（余弦相似度）。

# 向量

让我们从二维空间中向量的几何开始。

![](../Images/78d618d48df0427c8e479bda7530c3c7.png)

图像 1：二维空间中的向量示例。（图片由作者提供）

+   两个向量 u = (u₁, u₂) 和 v = (v₁, v₂) 是相等的，如果 u₁ = v₁ 且 u₂ = v₂。

+   向量u和v的和定义为u + v = (u₁ + v₁, u₂ + v₂)

![](../Images/7aae0b686667eecd293b85bd76084fef.png)

图2\. 向量和的示例。（图像由作者提供）

+   标量k与向量u的乘积定义为ku = (ku₁, ku₂)
