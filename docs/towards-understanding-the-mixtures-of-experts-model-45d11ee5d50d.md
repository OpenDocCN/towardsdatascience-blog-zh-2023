# 《深入理解专家混合模型》

> 原文：[`towardsdatascience.com/towards-understanding-the-mixtures-of-experts-model-45d11ee5d50d?source=collection_archive---------9-----------------------#2023-11-14`](https://towardsdatascience.com/towards-understanding-the-mixtures-of-experts-model-45d11ee5d50d?source=collection_archive---------9-----------------------#2023-11-14)

## 新研究揭示了我们训练 MoE 模型时底层发生的情况

[](https://medium.com/@samuel.flender?source=post_page-----45d11ee5d50d--------------------------------)![Samuel Flender](https://medium.com/@samuel.flender?source=post_page-----45d11ee5d50d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----45d11ee5d50d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----45d11ee5d50d--------------------------------) [Samuel Flender](https://medium.com/@samuel.flender?source=post_page-----45d11ee5d50d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fce56d9dcd568&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftowards-understanding-the-mixtures-of-experts-model-45d11ee5d50d&user=Samuel+Flender&userId=ce56d9dcd568&source=post_page-ce56d9dcd568----45d11ee5d50d---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----45d11ee5d50d--------------------------------) ·8 分钟阅读·2023 年 11 月 14 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F45d11ee5d50d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftowards-understanding-the-mixtures-of-experts-model-45d11ee5d50d&user=Samuel+Flender&userId=ce56d9dcd568&source=-----45d11ee5d50d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F45d11ee5d50d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftowards-understanding-the-mixtures-of-experts-model-45d11ee5d50d&source=-----45d11ee5d50d---------------------bookmark_footer-----------)![](img/84a6c24700294b529c1ea0523bea9a2a.png)

作者用 Midjourney 创建的图像

专家混合模型（MoE） 已迅速成为现代机器学习应用中最强大的技术之一，使得像 Switch Transformer 和 GPT-4 这样的突破成为可能。实际上，我们才刚刚开始看到它们的全部影响！

然而，令人惊讶的是，对于 MoE 为何有效的具体原因知之甚少。MoE 在什么情况下有效？为什么门控机制不会简单地将所有训练样本都发送给同一个专家？为什么模型不会陷入所有专家都相同的状态？专家们究竟如何专门化，他们专门化的内容是什么？门控机制究竟学到了什么？

幸运的是，研究已经开始揭示这些问题的答案。我们来看看。

## MoE 模型——一个基础入门

![](img/6243d227d3ea0a06796504c80d1766a8.png)

图片来源：[局部专家的自适应混合](https://www.cs.toronto.edu/~hinton/absps/jjnh91.pdf)

简单回顾一下，MoE（Mixtures of Experts）最早在 1991 年的论文“[局部专家的自适应混合](https://www.cs.toronto.edu/~hinton/absps/jjnh91.pdf)”中被提出，该论文的共同作者正是人工智能的奠基人**杰弗里·辛顿**。MoE 的关键思想是通过结合多个专家来对给定输入 x 的输出 y 进行建模…
