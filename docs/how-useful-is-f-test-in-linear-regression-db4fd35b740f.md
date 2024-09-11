# F 检验在线性回归中有多有用？

> 原文：[`towardsdatascience.com/how-useful-is-f-test-in-linear-regression-db4fd35b740f?source=collection_archive---------3-----------------------#2023-04-29`](https://towardsdatascience.com/how-useful-is-f-test-in-linear-regression-db4fd35b740f?source=collection_archive---------3-----------------------#2023-04-29)

[](https://medium.com/@jaekim8080?source=post_page-----db4fd35b740f--------------------------------)![Jae Kim](https://medium.com/@jaekim8080?source=post_page-----db4fd35b740f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----db4fd35b740f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----db4fd35b740f--------------------------------) [Jae Kim](https://medium.com/@jaekim8080?source=post_page-----db4fd35b740f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3a7641c3f8c1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-useful-is-f-test-in-linear-regression-db4fd35b740f&user=Jae+Kim&userId=3a7641c3f8c1&source=post_page-3a7641c3f8c1----db4fd35b740f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----db4fd35b740f--------------------------------) ·8 分钟阅读·2023 年 4 月 29 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdb4fd35b740f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-useful-is-f-test-in-linear-regression-db4fd35b740f&user=Jae+Kim&userId=3a7641c3f8c1&source=-----db4fd35b740f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdb4fd35b740f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-useful-is-f-test-in-linear-regression-db4fd35b740f&source=-----db4fd35b740f---------------------bookmark_footer-----------)

并不是很有用，但我们可以改进它。

![](img/dd0b6417c98f50de2f898ff4447538ba.png)

图片由[Greg Rakozy](https://unsplash.com/@grakozy?utm_source=medium&utm_medium=referral)提供，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

回归分析中斜率系数联合显著性的 F 检验统计量通常与 R²和 t 比值等其他关键统计量一起报告。

问题在于它作为关键统计量是否有用或提供信息。它是否为你的回归结果增加了任何价值？虽然它被常规报告，但在实际应用中，F 统计量几乎总是拒绝 H0。这告诉我们关于回归的拟合优度什么？你会发现 R²的值非常低，但 F 检验却说模型具有统计显著的解释力。这难道不是一个矛盾的结果吗？我们如何调和这个问题？

在这篇文章中，我解释了与 F 检验相关的问题以及如何对其进行修改，使其成为有用的工具。我要感谢 Venkat Raman 的[LinkedIn 帖子](https://www.linkedin.com/posts/venkat-raman-analytics_statistics-datascience-datascientists-activity-7053981090830594048-COe0?utm_source=share&utm_medium=member_desktop)，它激发了这篇文章。R 代码、数据和支持文档可以从[这里](https://github.com/jh8080/Ftest/tree/main)获取。

内容如下：

+   线性回归中的 F 检验是什么？

+   针对样本大小（T）和解释变量数量（K）的临界值

+   对 T 和 K 的 F 统计量
