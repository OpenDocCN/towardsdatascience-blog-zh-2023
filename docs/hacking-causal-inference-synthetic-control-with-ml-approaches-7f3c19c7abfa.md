# **因果推断破解：利用 ML 方法的合成控制**

> 原文：[`towardsdatascience.com/hacking-causal-inference-synthetic-control-with-ml-approaches-7f3c19c7abfa?source=collection_archive---------11-----------------------#2023-03-14`](https://towardsdatascience.com/hacking-causal-inference-synthetic-control-with-ml-approaches-7f3c19c7abfa?source=collection_archive---------11-----------------------#2023-03-14)

## 使用 PCA 测试任何治疗方法随时间的有效性

[](https://medium.com/@cerlymarco?source=post_page-----7f3c19c7abfa--------------------------------)![Marco Cerliani](https://medium.com/@cerlymarco?source=post_page-----7f3c19c7abfa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7f3c19c7abfa--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7f3c19c7abfa--------------------------------) [Marco Cerliani](https://medium.com/@cerlymarco?source=post_page-----7f3c19c7abfa--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc843902314c7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhacking-causal-inference-synthetic-control-with-ml-approaches-7f3c19c7abfa&user=Marco+Cerliani&userId=c843902314c7&source=post_page-c843902314c7----7f3c19c7abfa---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7f3c19c7abfa--------------------------------) ·5 分钟阅读·2023 年 3 月 14 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7f3c19c7abfa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhacking-causal-inference-synthetic-control-with-ml-approaches-7f3c19c7abfa&user=Marco+Cerliani&userId=c843902314c7&source=-----7f3c19c7abfa---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7f3c19c7abfa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhacking-causal-inference-synthetic-control-with-ml-approaches-7f3c19c7abfa&source=-----7f3c19c7abfa---------------------bookmark_footer-----------)![](img/3094292f2d245e5eb9a1ccffa073efdb.png)

图片由 [Raul Petri](https://unsplash.com/@raulpetri?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

文献中介绍并被大规模公司采用的标准，用于研究商业行为（如设计变更、折扣优惠和临床试验）的因果影响，肯定是 AB 测试。**进行 AB 测试时，我们是在做一个随机实验**。换句话说，我们将受我们控制的人群（患者、用户、客户）随机分成两组：治疗组和对照组。治疗组接受治疗措施，而对照组保持不变。经过一段时间后，我们回过头来记录感兴趣的指标，并分析治疗对人群行为的影响。

**闪光的东西不一定是金子！** 已证明 AB 测试存在不同的缺陷。随机试验的主要假设是：

+   没有治疗组和对照组之间的交互作用（即网络效应）。

+   大规模实验的成本不断增加。

在现实世界中，**网络效应是常见的，因为我们可能会预期到人们之间存在“污染”**。这就是社交媒体上意见分享的情况（在营销试验中），它会影响彼此的选择。要克服这个问题……
