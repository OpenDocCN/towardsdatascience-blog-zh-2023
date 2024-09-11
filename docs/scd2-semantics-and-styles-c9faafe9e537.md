# 不同的 SCD2 技术的语义

> 原文：[`towardsdatascience.com/scd2-semantics-and-styles-c9faafe9e537?source=collection_archive---------11-----------------------#2023-11-16`](https://towardsdatascience.com/scd2-semantics-and-styles-c9faafe9e537?source=collection_archive---------11-----------------------#2023-11-16)

## 小差异如何产生重大影响

[](https://medium.com/@cisenbe?source=post_page-----c9faafe9e537--------------------------------)![查德·伊森伯格](https://medium.com/@cisenbe?source=post_page-----c9faafe9e537--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c9faafe9e537--------------------------------)![走向数据科学](https://towardsdatascience.com/?source=post_page-----c9faafe9e537--------------------------------) [查德·伊森伯格](https://medium.com/@cisenbe?source=post_page-----c9faafe9e537--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb9113837f160&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fscd2-semantics-and-styles-c9faafe9e537&user=Chad+Isenberg&userId=b9113837f160&source=post_page-b9113837f160----c9faafe9e537---------------------post_header-----------) 发表在 [走向数据科学](https://towardsdatascience.com/?source=post_page-----c9faafe9e537--------------------------------) ·7 分钟阅读·2023 年 11 月 16 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc9faafe9e537&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fscd2-semantics-and-styles-c9faafe9e537&user=Chad+Isenberg&userId=b9113837f160&source=-----c9faafe9e537---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc9faafe9e537&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fscd2-semantics-and-styles-c9faafe9e537&source=-----c9faafe9e537---------------------bookmark_footer-----------)![](img/a1ed07e31161b1274a226214c153a03a.png)

[Brad Starkey](https://unsplash.com/@bradstarkey?utm_source=medium&utm_medium=referral) 的照片在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上

最近，我一直在思考维度建模，特别是我们如何在数据仓库/数据湖中表示不同类型的历史。有许多文章描述了如何在多种语言和平台上构建 SCD2 表。但我想专注于一些更微妙和不常讨论的内容：SCD2 的语义以及各种设计选择对使用案例的重要影响。

我想要讨论的两个主要话题是：

1.  您选择的日期来对您的维度进行行版本控制非常重要。选择绝不能是随意的，您最常见的使用情况应该是设计的首要考虑因素。

1.  如何对记录进行行版本化将决定您对表格的访问模式。在某种程度上，这是纯粹的人机工程学，但我认为人机工程学是数据质量的重要方面；使用户轻松做出正确选择应该是我们作为数据建模者的目标。

# 选择参考日期

创建 SCD2 表格的最常见模式是利用数据中的某个日期或时间戳。一旦您确定了一行在含义上发生了重大变化，无论是通过直接比较列还是比较……
