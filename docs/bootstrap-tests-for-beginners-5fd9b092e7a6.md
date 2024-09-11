# 初学者的自助法测试

> 原文：[`towardsdatascience.com/bootstrap-tests-for-beginners-5fd9b092e7a6?source=collection_archive---------14-----------------------#2023-06-19`](https://towardsdatascience.com/bootstrap-tests-for-beginners-5fd9b092e7a6?source=collection_archive---------14-----------------------#2023-06-19)

## 非参数检验初学者指南第二部分

[](https://medium.com/@jaekim8080?source=post_page-----5fd9b092e7a6--------------------------------)![Jae Kim](https://medium.com/@jaekim8080?source=post_page-----5fd9b092e7a6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5fd9b092e7a6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5fd9b092e7a6--------------------------------) [Jae Kim](https://medium.com/@jaekim8080?source=post_page-----5fd9b092e7a6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3a7641c3f8c1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbootstrap-tests-for-beginners-5fd9b092e7a6&user=Jae+Kim&userId=3a7641c3f8c1&source=post_page-3a7641c3f8c1----5fd9b092e7a6---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----5fd9b092e7a6--------------------------------) ·10 分钟阅读·2023 年 6 月 19 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5fd9b092e7a6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbootstrap-tests-for-beginners-5fd9b092e7a6&user=Jae+Kim&userId=3a7641c3f8c1&source=-----5fd9b092e7a6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5fd9b092e7a6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbootstrap-tests-for-beginners-5fd9b092e7a6&source=-----5fd9b092e7a6---------------------bookmark_footer-----------)![](img/e426ee31bdecd2e830fa13188b8047c3.png)

照片由[Mohamed Nohassi](https://unsplash.com/@coopery?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在本系列的第一部分中，我介绍了简单的秩和符号检验，作为非参数检验的入门介绍。如第一部分所述，自助法也是一种流行的非参数统计推断方法，基于对观察数据的重抽样。自 1980 年代[布拉德利·艾弗隆](https://scholar.google.com/citations?user=duBlF_YAAAAJ&hl=en)首次介绍以来，它在学术界获得了广泛的认可。[艾弗隆和蒂布希拉尼（1994）](https://www.taylorfrancis.com/books/mono/10.1201/9780429246593/introduction-bootstrap-bradley-efron-tibshirani)提供了自助法的入门和全面概述。该方法在统计科学领域的应用广泛，上述书籍至今已获得超过 50,000 次 Google Scholar 引用。

在这篇文章中，我以直观的方式向初学者介绍了自助法，配有简单的示例和 R 代码。

# 介绍

如第一部分中所述，假设检验的关键要素包括

1.  原假设和备择假设（H0 和 H1）

1.  检验统计量

1.  在 H0 下的检验统计量的抽样分布

1.  决策规则（p 值或临界值，在给定的显著性水平下）

在生成检验统计量的抽样分布时，

+   *参数检验*（例如…
