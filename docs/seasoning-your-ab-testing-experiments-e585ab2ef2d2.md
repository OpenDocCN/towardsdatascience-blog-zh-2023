# 调味你的 AB 测试实验

> 原文：[`towardsdatascience.com/seasoning-your-ab-testing-experiments-e585ab2ef2d2?source=collection_archive---------18-----------------------#2023-03-13`](https://towardsdatascience.com/seasoning-your-ab-testing-experiments-e585ab2ef2d2?source=collection_archive---------18-----------------------#2023-03-13)

## 盐如何帮助你的实验？

[](https://markeltsefon.medium.com/?source=post_page-----e585ab2ef2d2--------------------------------)![Mark Eltsefon](https://markeltsefon.medium.com/?source=post_page-----e585ab2ef2d2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e585ab2ef2d2--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e585ab2ef2d2--------------------------------) [Mark Eltsefon](https://markeltsefon.medium.com/?source=post_page-----e585ab2ef2d2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F88f461f6049a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fseasoning-your-ab-testing-experiments-e585ab2ef2d2&user=Mark+Eltsefon&userId=88f461f6049a&source=post_page-88f461f6049a----e585ab2ef2d2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e585ab2ef2d2--------------------------------) · 5 分钟阅读 · 2023 年 3 月 13 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe585ab2ef2d2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fseasoning-your-ab-testing-experiments-e585ab2ef2d2&user=Mark+Eltsefon&userId=88f461f6049a&source=-----e585ab2ef2d2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe585ab2ef2d2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fseasoning-your-ab-testing-experiments-e585ab2ef2d2&source=-----e585ab2ef2d2---------------------bookmark_footer-----------)![](img/c5d127a9d96144e0d73a0d474cca9844.png)

图片由 [Manuel Asturias](https://unsplash.com/es/@manuel_asturias?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

AB 测试是衡量新特性实施效果的最知名方法之一。其主要思想是将流量（或仅部分流量）随机分成两组或更多组。然而，确保分组是真正随机且没有偏差的，这一点非常重要，以便对结果有信心。这时，盐值就派上用场了。盐值的目的是消除可能影响 AB 测试结果的偏见或可预测性来源。如果没有盐值，某些用户可能更有可能基于他们的特征、行为或其他因素被分配到特定的网页或应用程序变体，从而导致结果偏差。

## 哈希和分割。

让我们探讨如何将用户分配到不同的实验和组。为了更清晰，最好使用一个例子。我们可以将整个流量分成 10 个桶。

在实际将用户分配到桶之前，让我们刷新一下哈希函数是什么。

哈希函数用于将输入数据转换为固定大小的唯一值，以代表原始数据。这有什么好处呢？它可以转化为我们用户的独特特征……
