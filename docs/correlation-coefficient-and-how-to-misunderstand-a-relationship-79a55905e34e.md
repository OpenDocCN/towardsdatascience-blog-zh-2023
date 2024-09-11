# 相关系数及如何误解关系

> 原文：[`towardsdatascience.com/correlation-coefficient-and-how-to-misunderstand-a-relationship-79a55905e34e?source=collection_archive---------9-----------------------#2023-01-09`](https://towardsdatascience.com/correlation-coefficient-and-how-to-misunderstand-a-relationship-79a55905e34e?source=collection_archive---------9-----------------------#2023-01-09)

## 统计学

## 解读相关性比大多数人认为的要困难得多。而误解相关性，反而不难。

[](https://medium.com/@nyggus?source=post_page-----79a55905e34e--------------------------------)![Marcin Kozak](https://medium.com/@nyggus?source=post_page-----79a55905e34e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----79a55905e34e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----79a55905e34e--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----79a55905e34e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4762f0cff9b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcorrelation-coefficient-and-how-to-misunderstand-a-relationship-79a55905e34e&user=Marcin+Kozak&userId=4762f0cff9b2&source=post_page-4762f0cff9b2----79a55905e34e---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----79a55905e34e--------------------------------) · 8 分钟阅读 · 2023 年 1 月 9 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F79a55905e34e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcorrelation-coefficient-and-how-to-misunderstand-a-relationship-79a55905e34e&user=Marcin+Kozak&userId=4762f0cff9b2&source=-----79a55905e34e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F79a55905e34e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcorrelation-coefficient-and-how-to-misunderstand-a-relationship-79a55905e34e&source=-----79a55905e34e---------------------bookmark_footer-----------)![](img/35088476b13eff4dd60d43eaee2f0779.png)

完美相关性！照片由 [Matt Seymour](https://unsplash.com/@mattseymour?utm_source=medium&utm_medium=referral) 通过 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

相关性……谁没听说过相关性？我们经常使用这个术语，但对它了解并不多。当你说，“这两件事是相关的”，你实际上是什么意思？或者当你说两者之间的相关性很强时，你的意思是什么？

对于数据科学家和统计学家来说，相关性比对其他人更为重要。这是因为我们是那些处理数据的人，因此我们负责理解数据中发生的任何现象——并将这些现象解释给业务同事、研究伙伴或任何我们合作的人员。

我曾经发现自己处于这样的情况一百次，甚至更多。从我的经验来看，尽管相关性是一个简单的概念，但向他人解释却是异常困难的。为什么？因为大多数受过教育的人对这个概念有某种理解——而且往往，他们对这种理解非常执着。因此，我们通常必须先突破这堵墙，才能…
