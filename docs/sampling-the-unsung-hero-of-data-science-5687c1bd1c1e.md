# 采样——数据科学中的无名英雄

> 原文：[`towardsdatascience.com/sampling-the-unsung-hero-of-data-science-5687c1bd1c1e?source=collection_archive---------14-----------------------#2023-01-18`](https://towardsdatascience.com/sampling-the-unsung-hero-of-data-science-5687c1bd1c1e?source=collection_archive---------14-----------------------#2023-01-18)

## 采样：方法论、实施与比较

[](https://medium.com/@fmnobar?source=post_page-----5687c1bd1c1e--------------------------------)![Farzad Mahmoodinobar](https://medium.com/@fmnobar?source=post_page-----5687c1bd1c1e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5687c1bd1c1e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5687c1bd1c1e--------------------------------) [Farzad Mahmoodinobar](https://medium.com/@fmnobar?source=post_page-----5687c1bd1c1e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3c56b7d4893e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsampling-the-unsung-hero-of-data-science-5687c1bd1c1e&user=Farzad+Mahmoodinobar&userId=3c56b7d4893e&source=post_page-3c56b7d4893e----5687c1bd1c1e---------------------post_header-----------) 发表在 [数据科学的前沿](https://towardsdatascience.com/?source=post_page-----5687c1bd1c1e--------------------------------) ·6 分钟阅读·2023 年 1 月 18 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5687c1bd1c1e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsampling-the-unsung-hero-of-data-science-5687c1bd1c1e&user=Farzad+Mahmoodinobar&userId=3c56b7d4893e&source=-----5687c1bd1c1e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5687c1bd1c1e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsampling-the-unsung-hero-of-data-science-5687c1bd1c1e&source=-----5687c1bd1c1e---------------------bookmark_footer-----------)![](img/ccd2f34f0dc6dea7154094484aec2969.png)

一、代表一切，由 [DALL.E 2](https://openai.com/dall-e-2/)

采样在各种业务中被广泛应用于审计和衡量变化——我知道这听起来很简单，但实际上比看起来要复杂得多。我看到今天的数据科学工作中很多关注点都集中在机器学习上，但如果没有设计良好且具有代表性的样本，所有的努力可能都不会取得成果。例如，在训练一个全新的机器学习模型变体之后，我们需要一个具有代表性的样本来确定改进（或退化）的程度，相较于模型的前一个版本——而仅仅收集一个随机样本并不总是正确的解决方案。一个设计不佳的样本如果不能很好地代表总体，可能会导致错误的结论和商业决策。

在这篇文章中，我将介绍并比较各种采样方法，希望能作为未来采样策略设计的参考。

让我们开始吧！

[](https://medium.com/@fmnobar/membership?source=post_page-----5687c1bd1c1e--------------------------------) [## 使用我的推荐链接加入 Medium - Farzad Mahmoodinobar

### 阅读 Farzad（以及 Medium 上的其他作者）的每一个故事。你的会员费用直接支持 Farzad 和其他…

medium.com](https://medium.com/@fmnobar/membership?source=post_page-----5687c1bd1c1e--------------------------------)

# 什么是采样？
