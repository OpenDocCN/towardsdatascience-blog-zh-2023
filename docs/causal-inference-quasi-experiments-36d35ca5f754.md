# 因果推断：准实验

> 原文：[https://towardsdatascience.com/causal-inference-quasi-experiments-36d35ca5f754?source=collection_archive---------10-----------------------#2023-08-09](https://towardsdatascience.com/causal-inference-quasi-experiments-36d35ca5f754?source=collection_archive---------10-----------------------#2023-08-09)

## 你的项目经理忘记进行**A/B测试**了……那现在怎么办？

[](https://ianhojy.medium.com/?source=post_page-----36d35ca5f754--------------------------------)[![Ian Ho](../Images/1b56c25ee3bedfb5c7369d4bfc93aa91.png)](https://ianhojy.medium.com/?source=post_page-----36d35ca5f754--------------------------------)[](https://towardsdatascience.com/?source=post_page-----36d35ca5f754--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----36d35ca5f754--------------------------------) [Ian Ho](https://ianhojy.medium.com/?source=post_page-----36d35ca5f754--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fca061af8c7b7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcausal-inference-quasi-experiments-36d35ca5f754&user=Ian+Ho&userId=ca061af8c7b7&source=post_page-ca061af8c7b7----36d35ca5f754---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----36d35ca5f754--------------------------------) ·12分钟阅读·2023年8月9日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F36d35ca5f754&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcausal-inference-quasi-experiments-36d35ca5f754&user=Ian+Ho&userId=ca061af8c7b7&source=-----36d35ca5f754---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F36d35ca5f754&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcausal-inference-quasi-experiments-36d35ca5f754&source=-----36d35ca5f754---------------------bookmark_footer-----------)![](../Images/a4b772f76b325ef375d908ed308e86ca.png)

照片由[Isaac Smith](https://unsplash.com/@isaacmsmith?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

*这篇文章是关于使用准实验进行因果推断的系列文章的第1部分（具体篇数取决于我讲述的多少）。简而言之，第1部分将解释准实验的原因和方法，以及应用像PSM这样的方法时的细微差别。在第2部分，我将进一步讨论准实验的局限性，以及在基于准实验做决策时应注意的事项。我还将提出一个异质影响估计的框架，帮助克服外推偏差。在第3部分……我还不太确定。*

*你可能也见过其他解释准实验的文章，但我仍然会尝试用我的方式来解释。请读一读。*

# 为什么选择因果推断？

开发和推出产品及功能的成本最终是通过对消费者产生的积极影响来证明其合理性的。因此，听到产品经理做出各种声明并不令人惊讶，例如“我们很高兴地宣布，我们最新的功能发布带来了令人印象深刻的12%的收入增长！”

听起来很棒，老实说，大多数高级管理人员很乐意接受这些声明为真。我的目标是说服你……
