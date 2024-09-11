# Python 数据框中的日期处理 第二部分——基础知识

> 原文：[`towardsdatascience.com/dealing-with-dates-in-pythons-dataframe-part-2-the-basics-9ad5edacd2f8?source=collection_archive---------15-----------------------#2023-01-04`](https://towardsdatascience.com/dealing-with-dates-in-pythons-dataframe-part-2-the-basics-9ad5edacd2f8?source=collection_archive---------15-----------------------#2023-01-04)

## Python 中的数据处理

## 本文解释了处理数据框中的 DateTime 序列的基本 pandas 方法和属性

[](https://kahemchu.medium.com/?source=post_page-----9ad5edacd2f8--------------------------------)![KahEm Chu](https://kahemchu.medium.com/?source=post_page-----9ad5edacd2f8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9ad5edacd2f8--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9ad5edacd2f8--------------------------------) [KahEm Chu](https://kahemchu.medium.com/?source=post_page-----9ad5edacd2f8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7cc8007b91ef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdealing-with-dates-in-pythons-dataframe-part-2-the-basics-9ad5edacd2f8&user=KahEm+Chu&userId=7cc8007b91ef&source=post_page-7cc8007b91ef----9ad5edacd2f8---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9ad5edacd2f8--------------------------------) ·8 分钟阅读·2023 年 1 月 4 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9ad5edacd2f8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdealing-with-dates-in-pythons-dataframe-part-2-the-basics-9ad5edacd2f8&user=KahEm+Chu&userId=7cc8007b91ef&source=-----9ad5edacd2f8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9ad5edacd2f8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdealing-with-dates-in-pythons-dataframe-part-2-the-basics-9ad5edacd2f8&source=-----9ad5edacd2f8---------------------bookmark_footer-----------)![](img/018a7772507981c5c6abbae9ca862bd1.png)

图片来自 [Lukas Blazek](https://unsplash.com/@goumbik?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/time?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

如标题所述，本文是我的《Python 数据框中的日期处理》系列的第二部分。以下展示了《Python 数据框中的日期处理》系列的每一部分的内容。

![](img/31bdd968f0779f2b31bcfdec88c1c29a.png)

图片来自作者。

在 我之前的文章 中，我展示了日期时间系列的创建方法。接下来，在本文中，我将展示处理数据框中日期时间系列的基本属性和方法。

有了这些内容，本文将按以下结构安排：

1.  将数据类型转换为日期时间

1.  一般的日期时间信息提取

1.  检查日期是否为周期的开始或结束

1.  检查日期是否属于闰年

1.  检查月份中的天数

> 让我们开始吧！

# 将数据类型转换为……
