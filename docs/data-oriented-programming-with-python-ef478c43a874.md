# Python 中的数据导向编程

> 原文：[`towardsdatascience.com/data-oriented-programming-with-python-ef478c43a874?source=collection_archive---------1-----------------------#2023-05-12`](https://towardsdatascience.com/data-oriented-programming-with-python-ef478c43a874?source=collection_archive---------1-----------------------#2023-05-12)

## 由 Yehonathan Sharvit 撰写的关于*数据导向编程*的回顾，但使用了 Python 示例（而不是 JavaScript 和 Java）

[](https://medium.com/@tamdtranthe?source=post_page-----ef478c43a874--------------------------------)![Tam D Tran-The](https://medium.com/@tamdtranthe?source=post_page-----ef478c43a874--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ef478c43a874--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ef478c43a874--------------------------------) [Tam D Tran-The](https://medium.com/@tamdtranthe?source=post_page-----ef478c43a874--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff13e13f2829a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-oriented-programming-with-python-ef478c43a874&user=Tam+D+Tran-The&userId=f13e13f2829a&source=post_page-f13e13f2829a----ef478c43a874---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----ef478c43a874--------------------------------) ·12 分钟阅读·2023 年 5 月 12 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fef478c43a874&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-oriented-programming-with-python-ef478c43a874&user=Tam+D+Tran-The&userId=f13e13f2829a&source=-----ef478c43a874---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fef478c43a874&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-oriented-programming-with-python-ef478c43a874&source=-----ef478c43a874---------------------bookmark_footer-----------)![](img/0394f0fd53608e090d14270536bf6869.png)

图片由[AltumCode](https://unsplash.com/@altumcode?utm_source=medium&utm_medium=referral)提供，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

[*数据导向编程*](https://www.manning.com/books/data-oriented-programming?utm_source=medium&utm_medium=referral&utm_campaign=book_sharvit2_data_1_29_21) 是 Yehonathan Sharvit 的一本好书，它为数据导向编程（DOP）这一概念提供了温和的介绍，作为传统面向对象编程（OOP）的替代方案。Sharvit 解构了 OOP 中那些有时看似不可避免的复杂元素，并总结了 DOP 的主要原则，这有助于我们使系统更易于管理。

顾名思义，DOP（数据导向编程）把数据放在首位。这可以通过遵循四个主要原则来实现。这些原则与语言无关。它们可以在面向对象编程（OOP）语言（如 Java、C++ 等）、函数式编程（FP）语言（如 Clojure 等）或通用语言（如 Python、JavaScript）中表示。尽管作者用 JavaScript 和 Java 来举例，这篇文章尝试用 Python 来演示这些思想。

在文章中，你会发现一些简单的 Python 代码片段，展示了如何遵循或违反每个原则。Sharvit 还阐明了每个原则的优点和成本——许多在 Python 中相关，而有些则不然。
