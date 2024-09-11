# p-值神话：为什么它们并非数据科学的圣杯

> 原文：[`towardsdatascience.com/the-myth-of-p-values-why-theyre-not-the-holy-grail-in-data-science-a6636e27e489?source=collection_archive---------4-----------------------#2023-04-27`](https://towardsdatascience.com/the-myth-of-p-values-why-theyre-not-the-holy-grail-in-data-science-a6636e27e489?source=collection_archive---------4-----------------------#2023-04-27)

## p-值谬误：为何我们需要停止将其视为真理

[](https://federicotrotta.medium.com/?source=post_page-----a6636e27e489--------------------------------)![费德里科·特罗塔](https://federicotrotta.medium.com/?source=post_page-----a6636e27e489--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a6636e27e489--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a6636e27e489--------------------------------) [费德里科·特罗塔](https://federicotrotta.medium.com/?source=post_page-----a6636e27e489--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F654cd4bbe899&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-myth-of-p-values-why-theyre-not-the-holy-grail-in-data-science-a6636e27e489&user=Federico+Trotta&userId=654cd4bbe899&source=post_page-654cd4bbe899----a6636e27e489---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a6636e27e489--------------------------------) ·15 min read·Apr 27, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa6636e27e489&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-myth-of-p-values-why-theyre-not-the-holy-grail-in-data-science-a6636e27e489&user=Federico+Trotta&userId=654cd4bbe899&source=-----a6636e27e489---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa6636e27e489&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-myth-of-p-values-why-theyre-not-the-holy-grail-in-data-science-a6636e27e489&source=-----a6636e27e489---------------------bookmark_footer-----------)![](img/2a838e36bf1a6ea412b22d9f2d369324.png)

Image by [Gerd Altmann](https://pixabay.com/it/users/geralt-9301/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2620923) on [Pixabay](https://pixabay.com/it//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2620923)

在[最近的一篇文章](https://medium.com/towards-data-science/mastering-linear-regression-the-definitive-guide-for-aspiring-data-scientists-7abd37fcb9ed)中，我提到在数据科学中我们并不总是需要计算`p-values`，这表明我们找到了一个解决机器学习问题的好模型，而无需计算它们。

现在是撰写澄清此声明的文章的时候了。我知道：这将是一篇有争议的文章，我真的很想听听你在评论中的意见。

如果你是一名有抱负的数据科学家，这里你会找到`p-values`的定义以及对其使用的有趣讨论。相反，如果你是一名专家，我知道你知道`p-values`的定义，希望你会喜欢对其使用的讨论。

我的想法并不是要说服任何人：这些讨论在科学界是非常严肃的，正如你将在文章中读到的那样。我只是想探讨一下`p-values`的话题，建议我们作为数据科学家是否必须使用`p-value`，或者我们是否可以通过其他方式测试我们的机器学习模型（我们将看到如何）。

```py
**Table of Contents:**

Definingthe p-values
Why we could avoid p-values as Data Scientists
What to use instead of p-values as Data Scientists
An example in Python
```
