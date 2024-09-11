# 关于评估大型语言模型的一切你需要知道的

> 原文：[`towardsdatascience.com/everything-you-should-know-about-evaluating-large-language-models-dce69ef8b2d2?source=collection_archive---------1-----------------------#2023-08-28`](https://towardsdatascience.com/everything-you-should-know-about-evaluating-large-language-models-dce69ef8b2d2?source=collection_archive---------1-----------------------#2023-08-28)

## 开放语言模型

## 从困惑度到衡量一般智能

[](https://donatoriccio.medium.com/?source=post_page-----dce69ef8b2d2--------------------------------)![Donato Riccio](https://donatoriccio.medium.com/?source=post_page-----dce69ef8b2d2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dce69ef8b2d2--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----dce69ef8b2d2--------------------------------) [Donato Riccio](https://donatoriccio.medium.com/?source=post_page-----dce69ef8b2d2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe384fc71d292&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feverything-you-should-know-about-evaluating-large-language-models-dce69ef8b2d2&user=Donato+Riccio&userId=e384fc71d292&source=post_page-e384fc71d292----dce69ef8b2d2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dce69ef8b2d2--------------------------------) ·10 分钟阅读·2023 年 8 月 28 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdce69ef8b2d2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feverything-you-should-know-about-evaluating-large-language-models-dce69ef8b2d2&user=Donato+Riccio&userId=e384fc71d292&source=-----dce69ef8b2d2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdce69ef8b2d2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feverything-you-should-know-about-evaluating-large-language-models-dce69ef8b2d2&source=-----dce69ef8b2d2---------------------bookmark_footer-----------)![](img/1557949e969c2f9d2a1f22fa5916c168.png)

图片由作者使用 Stable Diffusion 生成。

随着开源语言模型变得越来越容易获取，陷入各种选项中变得很容易。

我们如何确定它们的性能并进行比较？我们如何自信地说一个模型比另一个更好？

本文通过呈现训练和评估指标，以及一般和具体的基准测试，提供了一些答案，以便对模型的性能有一个清晰的了解。

如果你错过了，请查看《开放语言模型系列》的第一篇文章：

[](/a-gentle-introduction-to-open-source-large-language-models-3643f5ca774?source=post_page-----dce69ef8b2d2--------------------------------) ## 开源大型语言模型的简要介绍

### 为什么每个人都在谈论 Llamas、Alpacas、Falcons 和其他动物

towardsdatascience.com

# 困惑度

语言模型通过定义词汇表上的概率分布来选择序列中最可能的下一个词。给定一段文本，语言模型会为语言中的每个词分配一个概率，最终选择概率最高的词。
