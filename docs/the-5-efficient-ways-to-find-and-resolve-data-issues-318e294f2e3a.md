# 高效解决数据问题的5种方法

> 原文：[https://towardsdatascience.com/the-5-efficient-ways-to-find-and-resolve-data-issues-318e294f2e3a?source=collection_archive---------14-----------------------#2023-06-16](https://towardsdatascience.com/the-5-efficient-ways-to-find-and-resolve-data-issues-318e294f2e3a?source=collection_archive---------14-----------------------#2023-06-16)

## 揭示隐藏的异常和不一致

[](https://hanzalaqureshi.medium.com/?source=post_page-----318e294f2e3a--------------------------------)[![Hanzala Qureshi](../Images/6241f863e3e0159e39fdec753095f792.png)](https://hanzalaqureshi.medium.com/?source=post_page-----318e294f2e3a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----318e294f2e3a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----318e294f2e3a--------------------------------) [汉扎拉·库雷希](https://hanzalaqureshi.medium.com/?source=post_page-----318e294f2e3a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F467270b83111&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-5-efficient-ways-to-find-and-resolve-data-issues-318e294f2e3a&user=Hanzala+Qureshi&userId=467270b83111&source=post_page-467270b83111----318e294f2e3a---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----318e294f2e3a--------------------------------) ·6分钟阅读·2023年6月16日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F318e294f2e3a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-5-efficient-ways-to-find-and-resolve-data-issues-318e294f2e3a&user=Hanzala+Qureshi&userId=467270b83111&source=-----318e294f2e3a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F318e294f2e3a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-5-efficient-ways-to-find-and-resolve-data-issues-318e294f2e3a&source=-----318e294f2e3a---------------------bookmark_footer-----------)![](../Images/c11217931ff507b1132644369faf2ab2.png)

图片由 [Pierre Bamin](https://unsplash.com/@bamin?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

[根据Gartner调查](https://www.gartner.com/smarterwithgartner/how-to-stop-data-quality-undermining-your-business)，近60%的组织没有测量低质量数据的年度财务成本。我认为另外40%的人在掩盖事实。在我的经验中，数据质量造成的损失很少被组织量化，尽管它每天都直接影响他们。

我认为我们的现状并不是因为缺乏尝试；更多是因为我们不知道从何开始。就像糟糕的习惯一样，试图一次性解决所有问题或进行一年的项目会导致失败。需要有责任心的文化转变、明确的流程以及一点技术帮助。

今天，我们将深入探讨五种查找和解决问题的方法。开始吧！

## 1\. 审核来自源系统的数据

像大多数大型组织一样，如果你的数据仓库/数据湖信息是由陈旧的源系统提供的，那么你会知道源数据是一个大问题。

如果系统较旧，可能不灵活以接受更改。在这种情况下，当数据被接收或暂存于你的数据仓库时，应用重复性检查和对账将确保你在数据污染更广泛的数据之前发现问题……
