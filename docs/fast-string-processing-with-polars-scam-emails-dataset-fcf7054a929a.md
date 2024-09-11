# 使用 Polars 进行快速字符串处理 — 骗局电子邮件数据集

> 原文：[https://towardsdatascience.com/fast-string-processing-with-polars-scam-emails-dataset-fcf7054a929a?source=collection_archive---------2-----------------------#2023-05-28](https://towardsdatascience.com/fast-string-processing-with-polars-scam-emails-dataset-fcf7054a929a?source=collection_archive---------2-----------------------#2023-05-28)

## 使用内置的 Polars 字符串表达式在毫秒级别清理、处理和分词文本

[](https://medium.com/@antonsruberts?source=post_page-----fcf7054a929a--------------------------------)[![Antons Tocilins-Ruberts](../Images/363a4f32aa793cca7a67dea68e76e3cf.png)](https://medium.com/@antonsruberts?source=post_page-----fcf7054a929a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fcf7054a929a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----fcf7054a929a--------------------------------) [Antons Tocilins-Ruberts](https://medium.com/@antonsruberts?source=post_page-----fcf7054a929a--------------------------------)

·

[阅读](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9dee50b0374b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffast-string-processing-with-polars-scam-emails-dataset-fcf7054a929a&user=Antons+Tocilins-Ruberts&userId=9dee50b0374b&source=post_page-9dee50b0374b----fcf7054a929a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fcf7054a929a--------------------------------) ·10分钟阅读·2023年5月28日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffcf7054a929a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffast-string-processing-with-polars-scam-emails-dataset-fcf7054a929a&user=Antons+Tocilins-Ruberts&userId=9dee50b0374b&source=-----fcf7054a929a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffcf7054a929a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffast-string-processing-with-polars-scam-emails-dataset-fcf7054a929a&source=-----fcf7054a929a---------------------bookmark_footer-----------)![](../Images/130cbb064ff780aa7b2bc919defabc8a.png)

图片由 [Stephen Phillips - Hostreviews.co.uk](https://unsplash.com/es/@hostreviews?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

随着大型语言模型（LLMs）的广泛应用，我们可能会觉得我们已经过了需要手动清理和处理文本数据的阶段。不幸的是，我和其他NLP从业者可以证明，情况远非如此。在NLP的每个复杂阶段——从基础文本分析到机器学习和LLMs——都需要干净的文本数据。这篇文章将展示如何通过使用Polars显著加快这一繁琐而枯燥的过程。

# Polars

[Polars](https://github.com/pola-rs/polars)是一个用Rust编写的极其快速的数据框架库，在处理字符串时效率非常高（得益于其Arrow后端）。Polars使用`Arrow`后端以`Utf8`格式存储字符串，这使得字符串遍历在缓存中最优化并且可预测。此外，它在`str`命名空间下暴露了许多内建的字符串操作，这使得字符串操作能够并行化。这两个因素使得处理字符串变得极其简单和快速。

该库与Pandas共享了许多语法，但也有许多你需要习惯的特性。这篇文章将带你了解…
