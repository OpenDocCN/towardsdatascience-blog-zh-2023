# 相关性——当皮尔逊 r 不足以解释时

> 原文：[`towardsdatascience.com/correlation-when-pearsons-r-is-not-enough-aded72308635?source=collection_archive---------3-----------------------#2023-02-01`](https://towardsdatascience.com/correlation-when-pearsons-r-is-not-enough-aded72308635?source=collection_archive---------3-----------------------#2023-02-01)

## 各种相关性方法的比较

[](https://medium.com/@fmnobar?source=post_page-----aded72308635--------------------------------)![Farzad Mahmoodinobar](https://medium.com/@fmnobar?source=post_page-----aded72308635--------------------------------)[](https://towardsdatascience.com/?source=post_page-----aded72308635--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----aded72308635--------------------------------) [Farzad Mahmoodinobar](https://medium.com/@fmnobar?source=post_page-----aded72308635--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3c56b7d4893e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcorrelation-when-pearsons-r-is-not-enough-aded72308635&user=Farzad+Mahmoodinobar&userId=3c56b7d4893e&source=post_page-3c56b7d4893e----aded72308635---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----aded72308635--------------------------------) ·15 分钟阅读·2023 年 2 月 1 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Faded72308635&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcorrelation-when-pearsons-r-is-not-enough-aded72308635&user=Farzad+Mahmoodinobar&userId=3c56b7d4893e&source=-----aded72308635---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Faded72308635&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcorrelation-when-pearsons-r-is-not-enough-aded72308635&source=-----aded72308635---------------------bookmark_footer-----------)![](img/9e7d5ed242ff9c85c8f6b6b7bbf72661.png)

只有一个钥匙能够解锁，由[DALL.E 2](https://openai.com/dall-e-2/)

我们对“相关性不等于因果关系”这句话非常熟悉，但让我们通过一个实际例子来理解将相关性与因果关系混淆可能带来的后果。在 1998 年 2 月，一篇[论文](https://pubmed.ncbi.nlm.nih.gov/9500320/)发布，声称某些疫苗与儿童自闭症之间存在因果关联。后来发现这篇论文是伪造的，并在 2010 年[撤回](https://pubmed.ncbi.nlm.nih.gov/20137807/)。可以想象，这样的声明对那些基于该论文发现未接种疫苗的人生活的影响，因为相关性被误认为是因果关系。

在这篇文章中，我们将深入探讨相关性，以更好地理解它是什么。我们将了解，根据研究中变量的类型，推荐使用的相关性方法是什么。最后，我们将在 Python 环境中实现一些最常见的方法。

[## 使用我的推荐链接加入 Medium - Farzad Mahmoodinobar](https://medium.com/@fmnobar/membership?source=post_page-----aded72308635--------------------------------)

### 阅读 Farzad（和 Medium 上的其他作者）的每一个故事。你的会员费直接支持 Farzad 和其他…

[medium.com](https://medium.com/@fmnobar/membership?source=post_page-----aded72308635--------------------------------)

# 什么是相关性？
