# 使用 Python 的 Pandas 库轻松进行非常规的日期时间转换

> 原文：[https://towardsdatascience.com/unconventional-datetime-transformation-made-easy-using-pythons-pandas-library-aa9a8c65d885?source=collection_archive---------23-----------------------#2023-07-25](https://towardsdatascience.com/unconventional-datetime-transformation-made-easy-using-pythons-pandas-library-aa9a8c65d885?source=collection_archive---------23-----------------------#2023-07-25)

## 用实际案例进行解释

[](https://jin-cui.medium.com/?source=post_page-----aa9a8c65d885--------------------------------)[![Jin Cui](../Images/e5ddcbaa6d7da38f960d2c5fea71b538.png)](https://jin-cui.medium.com/?source=post_page-----aa9a8c65d885--------------------------------)[](https://towardsdatascience.com/?source=post_page-----aa9a8c65d885--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----aa9a8c65d885--------------------------------) [Jin Cui](https://jin-cui.medium.com/?source=post_page-----aa9a8c65d885--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F333e9ae026bc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funconventional-datetime-transformation-made-easy-using-pythons-pandas-library-aa9a8c65d885&user=Jin+Cui&userId=333e9ae026bc&source=post_page-333e9ae026bc----aa9a8c65d885---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----aa9a8c65d885--------------------------------) ·5分钟阅读·2023年7月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Faa9a8c65d885&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funconventional-datetime-transformation-made-easy-using-pythons-pandas-library-aa9a8c65d885&user=Jin+Cui&userId=333e9ae026bc&source=-----aa9a8c65d885---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Faa9a8c65d885&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funconventional-datetime-transformation-made-easy-using-pythons-pandas-library-aa9a8c65d885&source=-----aa9a8c65d885---------------------bookmark_footer-----------)![](../Images/49d6e85e7f4bde965d3e8a9f34f2d78e.png)

图片来源：[Debby Hudson](https://unsplash.com/@hudsoncrafted?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 背景

最近，我的任务是分析客户公司员工的请假记录。特别是，我需要了解某个员工是否在特定时期内请过假，最终设定一个基准来衡量员工对返回办公室政策的遵守情况。

我获得了以下两个请假数据集：

1.  **休假** **数据**（“数据集 A”），其中列出了员工的短期假期，如年假或病假。这些假期在每个员工每个日期级别上是唯一的（即数据集中的每一行代表特定员工请假的一天）。

1.  **请假数据**（“数据集 B”），其中列出了员工长期请假的*起始*和*结束*日期。这些假期的例子包括育儿假、产假、无薪假和职业休假。该数据集基于‘逐次请假’的方式，对于已经请了长期假期的员工，每一行代表员工的一个日期范围，该员工可能在数据集中出现多行，每行有多个日期范围（例如，员工可能会选择将育儿假分成每周 3 天的 30 周批次进行…）。
