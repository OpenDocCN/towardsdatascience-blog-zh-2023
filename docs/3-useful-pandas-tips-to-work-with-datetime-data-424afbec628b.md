# 3 个有用的 Pandas 技巧来处理日期时间数据

> 原文：[https://towardsdatascience.com/3-useful-pandas-tips-to-work-with-datetime-data-424afbec628b?source=collection_archive---------3-----------------------#2023-07-19](https://towardsdatascience.com/3-useful-pandas-tips-to-work-with-datetime-data-424afbec628b?source=collection_archive---------3-----------------------#2023-07-19)

## 快速学习如何在 Python Pandas 中转换时间序列数据，并充分利用它

[](https://medium.com/@17.rsuraj?source=post_page-----424afbec628b--------------------------------)[![Suraj Gurav](../Images/f5dca32861f8c1c428e66fbe2174c04b.png)](https://medium.com/@17.rsuraj?source=post_page-----424afbec628b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----424afbec628b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----424afbec628b--------------------------------) [Suraj Gurav](https://medium.com/@17.rsuraj?source=post_page-----424afbec628b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1fdda183cca2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-useful-pandas-tips-to-work-with-datetime-data-424afbec628b&user=Suraj+Gurav&userId=1fdda183cca2&source=post_page-1fdda183cca2----424afbec628b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----424afbec628b--------------------------------) · 9分钟阅读 · 2023年7月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F424afbec628b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-useful-pandas-tips-to-work-with-datetime-data-424afbec628b&user=Suraj+Gurav&userId=1fdda183cca2&source=-----424afbec628b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F424afbec628b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-useful-pandas-tips-to-work-with-datetime-data-424afbec628b&source=-----424afbec628b---------------------bookmark_footer-----------)![](../Images/51a444fd569b4ea7673cea5ac33288b9.png)

照片由 [Jack Hunter](https://unsplash.com/@jacktthunter?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

日期和时间数据无处不在！掌握操作它的技巧吧！

无论你工作在哪个行业，时间序列数据总是存在的。通过分析这些数据，你可以探索其中隐藏的趋势和模式，从而获取有价值的见解。这将最终帮助你根据历史数据做出数据驱动的决策和计划。

在我之前的文章中，你可以探索[**3 个实用技巧**](/3-powerful-tricks-to-work-with-date-time-data-in-python-67c2d3834338)来处理 Python 中的此类数据。然而，当你在 pandas DataFrame 中处理时间序列数据时，最好使用**pandas 类和函数**。

当你将数据读取到 pandas DataFrame 中时，pandas 通常将其存储为字符串或对象数据类型。为了有效分析此类数据，你需要将其转换为 DateTime 数据类型。

在这篇文章中，你将探索 3 个经过验证的节省时间的技巧来处理**pandas 中的时间序列数据**。你将探索 pandas 中的一系列类和函数，以有效地处理日期和时间数据。

让我们设定场景——在 DataFrame **df** 中读取虚拟销售数据！

```py
df = pd.read_csv("Dummy_dates_sales.csv")
df.head()
```
