# 引导转移学习：如何利用‘侦察员的力量’提升机器学习性能

> 原文：[https://towardsdatascience.com/guided-transfer-learning-how-to-use-the-power-of-scouts-to-boost-machine-learning-performance-7e478d1ec5e4?source=collection_archive---------9-----------------------#2023-03-27](https://towardsdatascience.com/guided-transfer-learning-how-to-use-the-power-of-scouts-to-boost-machine-learning-performance-7e478d1ec5e4?source=collection_archive---------9-----------------------#2023-03-27)

## 对一种革命性神经网络训练方法的独家预览

[](https://katherineamunro.medium.com/?source=post_page-----7e478d1ec5e4--------------------------------)[![Katherine Munro](../Images/8013140495c7b9bd25ef08d712f097bf.png)](https://katherineamunro.medium.com/?source=post_page-----7e478d1ec5e4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7e478d1ec5e4--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7e478d1ec5e4--------------------------------) [Katherine Munro](https://katherineamunro.medium.com/?source=post_page-----7e478d1ec5e4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb84716d39740&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fguided-transfer-learning-how-to-use-the-power-of-scouts-to-boost-machine-learning-performance-7e478d1ec5e4&user=Katherine+Munro&userId=b84716d39740&source=post_page-b84716d39740----7e478d1ec5e4---------------------post_header-----------) 发布于 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----7e478d1ec5e4--------------------------------) ·10分钟阅读·2023年3月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7e478d1ec5e4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fguided-transfer-learning-how-to-use-the-power-of-scouts-to-boost-machine-learning-performance-7e478d1ec5e4&user=Katherine+Munro&userId=b84716d39740&source=-----7e478d1ec5e4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7e478d1ec5e4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fguided-transfer-learning-how-to-use-the-power-of-scouts-to-boost-machine-learning-performance-7e478d1ec5e4&source=-----7e478d1ec5e4---------------------bookmark_footer-----------)![](../Images/482d6e71232893524e6b0b5e6d2cf3bd.png)

在这一新提出的技术中，小型‘侦察模型’被派往探索问题领域，并‘向主模型报告’。照片由 [@ansgarscheffold](http://twitter.com/ansgarscheffold) 提供，来自 Unsplash。

我的好朋友、谦逊的天才**丹科·尼科利奇**最近与我分享了一篇未发表的论文，他认为我可能会感兴趣。我确实感兴趣。阅读它让我感觉像是目睹了一个历史性时刻，而别人还未意识到，我立刻迫不及待地想分享。幸运的是，丹科同意了。所以这是我对一种可能革新深度神经网络训练方法的日常语言翻译。它甚至还未在arXiv上发布（更新： [现在发布了！](https://arxiv.org/abs/2303.16154)），但 [NASA已经在使用它](https://www.linkedin.com/posts/robots-go-mental_nasa-genelab-open-science-for-life-in-space-activity-7041340569012301824-xTk3?utm_source=share&utm_medium=member_desktop)。所以，一旦它大火，记住：你首先在这里听到了。😉

# 从问题开始

我相信你知道：机器学习，尤其是深度神经网络，确实需要大量的数据、计算能力和模型参数。这使得它们只能被最富有的公司和研究机构所接触，从而将开发塑造我们技术未来的AI技术的权力集中在少数几个人手中。这一点不太酷。

## 问题存在的原因
