# GPT-4 可以解决数学问题——但不是所有语言中

> 原文：[`towardsdatascience.com/gpt-4-can-solve-math-problems-but-not-in-all-languages-d1c2e9c195a0?source=collection_archive---------5-----------------------#2023-10-11`](https://towardsdatascience.com/gpt-4-can-solve-math-problems-but-not-in-all-languages-d1c2e9c195a0?source=collection_archive---------5-----------------------#2023-10-11)

## *几个实验让 GPT-4 解决 16 种不同语言中的数学问题*

[](https://medium.com/@artfish?source=post_page-----d1c2e9c195a0--------------------------------)![Yennie Jun](https://medium.com/@artfish?source=post_page-----d1c2e9c195a0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d1c2e9c195a0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d1c2e9c195a0--------------------------------) [Yennie Jun](https://medium.com/@artfish?source=post_page-----d1c2e9c195a0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F12ca1ab81192&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgpt-4-can-solve-math-problems-but-not-in-all-languages-d1c2e9c195a0&user=Yennie+Jun&userId=12ca1ab81192&source=post_page-12ca1ab81192----d1c2e9c195a0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d1c2e9c195a0--------------------------------) ·12 分钟阅读·2023 年 10 月 11 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd1c2e9c195a0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgpt-4-can-solve-math-problems-but-not-in-all-languages-d1c2e9c195a0&user=Yennie+Jun&userId=12ca1ab81192&source=-----d1c2e9c195a0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd1c2e9c195a0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgpt-4-can-solve-math-problems-but-not-in-all-languages-d1c2e9c195a0&source=-----d1c2e9c195a0---------------------bookmark_footer-----------)![](img/180c9a75eac66a3a19a3313d19599f93.png)

图像由作者使用 Midjourney 创建。

*这篇文章最初发布在我的* [*个人网站*](https://www.artfish.ai/p/gpt4-project-euler-many-languages)*。*

# 介绍

有人说[数学是一种通用语言](https://www.emerald.com/insight/content/doi/10.1108/JME-01-2016-0004/full/html)——数学概念、定理和定义可以用符号表达，无论语言如何都能理解。

**在这篇文章中，我测试了 GPT-4 在十六种不同语言中的数学能力。**

早期的实验显示，GPT-4 在[SAT 数学和 AP 微积分测试](https://arxiv.org/abs/2303.08774)以及[本科数学水平](https://arxiv.org/abs/2301.13867)中得分很高。然而，这些实验中的大多数**仅测试了 GPT-4 在英语中的数学能力**。为了更好地了解 GPT-4 在英语之外的数学能力，我在其他十五种语言中提出了相同的数学问题。

那么，GPT-4 在不同语言中的数学能力如何呢？理论上，它在所有语言中的表现应该是一样好的（或坏的），但不幸的是（正如你可能猜到的），情况并非如此。**GPT-4 在解决英语中的数学问题方面要好得多**。根据语言的不同，GPT-4 能够解决一些问题。然而，对于那些传统上资源匮乏的语言，如缅甸语和阿姆哈拉语…
