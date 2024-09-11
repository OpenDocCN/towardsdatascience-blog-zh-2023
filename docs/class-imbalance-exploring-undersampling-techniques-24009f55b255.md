# 类别不平衡：探索欠采样技术

> 原文：[https://towardsdatascience.com/class-imbalance-exploring-undersampling-techniques-24009f55b255?source=collection_archive---------10-----------------------#2023-10-07](https://towardsdatascience.com/class-imbalance-exploring-undersampling-techniques-24009f55b255?source=collection_archive---------10-----------------------#2023-10-07)

## 让我们了解一下欠采样及其如何帮助解决类别不平衡问题

[](https://essamwissam.medium.com/?source=post_page-----24009f55b255--------------------------------)[![Essam Wisam](../Images/6320ce88ba2e5d56d70ce3e0f97ceb1d.png)](https://essamwissam.medium.com/?source=post_page-----24009f55b255--------------------------------)[](https://towardsdatascience.com/?source=post_page-----24009f55b255--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----24009f55b255--------------------------------) [Essam Wisam](https://essamwissam.medium.com/?source=post_page-----24009f55b255--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fccb82b9f3b87&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclass-imbalance-exploring-undersampling-techniques-24009f55b255&user=Essam+Wisam&userId=ccb82b9f3b87&source=post_page-ccb82b9f3b87----24009f55b255---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----24009f55b255--------------------------------) ·5分钟阅读·2023年10月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F24009f55b255&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclass-imbalance-exploring-undersampling-techniques-24009f55b255&user=Essam+Wisam&userId=ccb82b9f3b87&source=-----24009f55b255---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F24009f55b255&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclass-imbalance-exploring-undersampling-techniques-24009f55b255&source=-----24009f55b255---------------------bookmark_footer-----------)

我们已经正式解释了[之前](https://essamwissam.medium.com/class-imbalance-and-oversampling-a-formal-introduction-c77b918e586d)类别不平衡的影响及其原因，并解释了几种能够解决这一问题的过采样技术，如随机过采样、ROSE、RWO、SMOTE、BorderlineSMOTE1、SMOTE-NC和SMOTE-N。在这个故事中，我们将尝试对欠采样技术进行类似的探讨，同时假设在之前的解释中，欠采样如何帮助解决不平衡问题已经显而易见。

## 目录

∘ [简介](#86fd)

∘ [朴素随机欠采样](#3667)

∘ [K均值欠采样](#42ea)

∘ [Tomek 链下采样](#1967)

∘ [编辑最近邻下采样](#f69a)

## 介绍

下采样技术通常分为两大类：控制型和非控制型。在控制型技术中，算法接收一个数字，指示最终数据集中应有多少样本；而在非控制型技术中，下采样通常通过简单地删除满足某些条件的点来进行。事先无法知道有多少点会满足这些条件，显然也无法控制。在这个故事中，我们将介绍两种控制型下采样技术（随机和 K-Means 下采样）以及两种非控制型下采样技术（Tomek 链和编辑最近邻）。

## 天真随机下采样

在这种技术中，如果给定要从类 *k* 中移除 *N_k* 点，则会从该类中随机选择 *N_k* 个点进行删除（也可以随机选择要保留的点，以便移除 *N_k* 个点）。

下面展示了一个在有三个类 0、1 和 2 的数据中下采样两个主要类的示例。

![](../Images/f371e62e5265461af19aec389093eb71.png)

作者使用 Julia 的 Imbalance.jl 包绘制的图

下面是一个动画，展示了不同下采样程度下的输出

![](../Images/b306cbc84ead4662b94625913ee41ae9.png)

作者使用 Julia 的 Imbalance.jl 包制作的动画

注意这是一个完全随机的过程；没有做出关于保留哪些点的具体选择。由于此过程，数据的分布可能会严重改变。

## K-Means 下采样

我们可以通过更仔细地选择要移除（或保留）的点来保持数据的分布。在 K-Means 下采样中，如果需要为类 k 保留 *N_k* 点，则执行 *K=N_k* 的 K-Means 算法，得到 *N_k* 个最终中心点。K-Means 下采样让这些中心点（或每个中心点的最近邻；这是一个超参数）成为最终的 *N_k* 个点。由于中心点本身保留了数据的分布，这导致保留数据分布的点集更小。

下面展示了一个在有三个类 0、1 和 2 的数据中下采样两个主要类的示例。

![](../Images/425a287df6bfd8bb15bfa0a504d221b5.png)

作者使用 Julia 的 Imbalance.jl 包绘制的图

注意，相比于随机下采样，这种方法在保持数据结构方面更为小心，这在更多下采样的情况下尤为明显。我们用一个动画进一步说明这一点：

![](../Images/a08a5d2c19a54be9f15ab1ec0bcc83bf.png)

作者使用 Julia 的 Imbalance.jl 包制作的动画

注意中心点依赖于初始化，通常涉及随机性。

## Tomek 链下采样

这是一种非控制型下采样技术，如果一个点是 Tomek 链的一部分，则可以被删除。如果两个点形成 Tomek 链，则：

+   它们属于不同的类别

+   每两个点都是彼此的最近邻

这里的理由是，这些点无法帮助改进决策边界（例如，可能更容易导致过拟合），并且它们可能是噪声。以下是应用托梅克链接的示例：

![](../Images/944b071a947c077d41ce2bcb3031a2c4.png)

作者使用Julia中的Imbalance.jl包制作的图

注意下采样后如何更容易找到更线性的决策边界，并且这也使数据更好地平衡。在此过程中，我们跳过了绿色的少数类下采样，并在一个类别的点数接近时停止下采样。

要更近距离地观察这种情况，其中所有类别最终都被下采样，可以参考以下动画：

![](../Images/2ad59401e111a8c0347accb48b982d67.png)

作者使用Julia中的Imbalance.jl包制作的动画

## 编辑最近邻下采样

尽管托梅克链接大多是那些无法形成更好决策边界的点或噪声点，并不是所有的噪声点都会形成托梅克链接。如果类别*k_1*中的一个噪声点存在于类别*k_2*中的一个密集区域内，那么噪声点的最近邻的最近点可能不是噪声点，这意味着它不会形成托梅克链接。与此条件相比，编辑最近邻下采样默认保留一个点，只有当它的大多数邻居来自同一类别时。也可以选择仅在所有邻居都来自同一类别时保留点，或者在存在来自同一类别的邻居时进行最小下采样。

这个动画展示了算法的实际操作：

![](../Images/d70c87c8af65cfb5033eb6a0cc0592ad.png)

作者使用Julia中的Imbalance.jl包制作的动画

注意它如何清除更多不利于决策边界或是噪声的点。如果邻居数k或保留条件以正确的方式改变，还可以进一步清除。这是另一种说明效果的动画。

![](../Images/837b337e4d2de73c33609370c1bf3d86.png)

作者使用Julia中的Imbalance.jl包制作的动画

“模式”和“仅模式”条件之间的区别在于，前者保留一个点，只有当它的类别是邻居中最常见的类别之一时；而后者仅在它的类别是唯一最常见的类别时保留点。

这结束了我们对一些有趣的下采样算法的介绍。希望这有助于你更多地了解受控和非受控的下采样。下次见，再见。

**参考文献：**

[1] 魏超，李志锋，夏汉，& 井尚杰。（2017）。基于聚类的类别不平衡数据下采样。信息科学，409–410，17–26。

[2] 伊万·托梅克。cnn的两种修改。IEEE 系统、人类与控制论杂志，6:769–772，1976年。

[3] Dennis L Wilson. 使用编辑数据的最近邻规则的渐近性质。IEEE系统、人与控制论汇刊，页面408–421，1972年。
