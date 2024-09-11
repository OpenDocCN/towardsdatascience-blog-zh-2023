# 现代推荐系统中的哈希：入门

> 原文：[`towardsdatascience.com/hashing-in-modern-recommender-systems-a-primer-9c6b2cf4497a?source=collection_archive---------7-----------------------#2023-03-28`](https://towardsdatascience.com/hashing-in-modern-recommender-systems-a-primer-9c6b2cf4497a?source=collection_archive---------7-----------------------#2023-03-28)

## 理解应用机器学习中最被低估的技巧

[](https://medium.com/@samuel.flender?source=post_page-----9c6b2cf4497a--------------------------------)![Samuel Flender](https://medium.com/@samuel.flender?source=post_page-----9c6b2cf4497a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9c6b2cf4497a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9c6b2cf4497a--------------------------------) [Samuel Flender](https://medium.com/@samuel.flender?source=post_page-----9c6b2cf4497a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fce56d9dcd568&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhashing-in-modern-recommender-systems-a-primer-9c6b2cf4497a&user=Samuel+Flender&userId=ce56d9dcd568&source=post_page-ce56d9dcd568----9c6b2cf4497a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9c6b2cf4497a--------------------------------) ·6 分钟阅读·2023 年 3 月 28 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9c6b2cf4497a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhashing-in-modern-recommender-systems-a-primer-9c6b2cf4497a&user=Samuel+Flender&userId=ce56d9dcd568&source=-----9c6b2cf4497a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9c6b2cf4497a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhashing-in-modern-recommender-systems-a-primer-9c6b2cf4497a&source=-----9c6b2cf4497a---------------------bookmark_footer-----------)![](img/9c2812cf751beb430b8fbe23eefabfa7.png)

(Midjourney)

哈希是一种在工业机器学习应用中常用的“技巧”，但它并没有获得应有的关注。

哈希的最大优势，尤其是在现代[推荐系统](https://medium.com/towards-data-science/biases-in-recommender-systems-top-challenges-and-recent-breakthroughs-edcda59d30bf)中，是其有限内存保证：如果没有哈希，对于数十亿用户的数十亿个视频、新闻文章、照片或网页的相关性进行学习将变得极为不切实际，容易耗尽内存。

但我们现在走得有点前了。这篇文章是一个入门介绍，所以让我们回到一切开始的地方：著名的 2009 年“哈希技巧”论文。

## 引发这一切的论文

使用哈希作为处理特征的一种方法，以及“哈希技巧”一词，最早在 2009 年的一篇[论文](https://arxiv.org/abs/0902.2206)中由 Yahoo 的研究团队提出，该团队由 Kilian Weinberger 领导，研究内容涉及电子邮件垃圾邮件检测。毕竟，电子邮件是一系列单词，每个单词可以被视为一个特征。正如作者解释的那样，通过哈希，我们可以将电子邮件表示为向量（其中每个索引对应一个特定的单词），并在垃圾邮件协同过滤模型中使用这些向量。
