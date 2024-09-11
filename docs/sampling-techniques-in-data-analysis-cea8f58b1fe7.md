# 数据分析中的抽样技术

> 原文：[`towardsdatascience.com/sampling-techniques-in-data-analysis-cea8f58b1fe7?source=collection_archive---------4-----------------------#2023-09-06`](https://towardsdatascience.com/sampling-techniques-in-data-analysis-cea8f58b1fe7?source=collection_archive---------4-----------------------#2023-09-06)

## 如何选择适合你数据的抽样方法

[](https://medium.com/@john_lenehan?source=post_page-----cea8f58b1fe7--------------------------------)![John Lenehan](https://medium.com/@john_lenehan?source=post_page-----cea8f58b1fe7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cea8f58b1fe7--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----cea8f58b1fe7--------------------------------) [John Lenehan](https://medium.com/@john_lenehan?source=post_page-----cea8f58b1fe7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2eb00da71bb6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsampling-techniques-in-data-analysis-cea8f58b1fe7&user=John+Lenehan&userId=2eb00da71bb6&source=post_page-2eb00da71bb6----cea8f58b1fe7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cea8f58b1fe7--------------------------------) ·6 分钟阅读·2023 年 9 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcea8f58b1fe7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsampling-techniques-in-data-analysis-cea8f58b1fe7&user=John+Lenehan&userId=2eb00da71bb6&source=-----cea8f58b1fe7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcea8f58b1fe7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsampling-techniques-in-data-analysis-cea8f58b1fe7&source=-----cea8f58b1fe7---------------------bookmark_footer-----------)![](img/811dd1f1c983cbc25248680f36c27c4c.png)

图片由 [Ryoji Iwata](https://unsplash.com/@ryoji__iwata?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

数据科学项目中，对分析方法和算法的重视程度相当高，包括从数据中提取有意义的见解和发现有价值的信息。但同样重要（甚至可以说更重要）的是项目开始前的数据准备；数据的质量是任何数据分析或机器学习项目的基础。期望从不佳的数据输入中得到高质量的输出是不切实际的——正如谚语所说的“垃圾进，垃圾出”。因此，确保收集的数据样本具有足够的质量至关重要。但如何选择适合你的数据的抽样技术呢？

![](img/76a29f22283eb29e95128206f9e849b5.png)

照片由 [Ian Parker](https://unsplash.com/@evanescentlight?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在这篇文章中，我打算概述一些数据收集的抽样技术，并提供如何为你的数据选择最优方法的建议。我将描述的抽样方法如下：

1.  简单随机抽样

1.  分层抽样

1.  聚类抽样

1.  系统抽样
