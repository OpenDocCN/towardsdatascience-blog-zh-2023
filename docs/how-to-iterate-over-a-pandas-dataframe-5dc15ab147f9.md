# 如何遍历 Pandas 数据框

> 原文：[https://towardsdatascience.com/how-to-iterate-over-a-pandas-dataframe-5dc15ab147f9?source=collection_archive---------11-----------------------#2023-05-18](https://towardsdatascience.com/how-to-iterate-over-a-pandas-dataframe-5dc15ab147f9?source=collection_archive---------11-----------------------#2023-05-18)

![](../Images/653feebb30296df7a7f51350e1ba4052.png)

照片由[Sid Balachandran](https://unsplash.com/@itookthose?utm_source=medium&utm_medium=referral)提供，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 行优先与列优先，Pandas 最佳实践

[](https://medium.com/@marcellopoliti?source=post_page-----5dc15ab147f9--------------------------------)[![Marcello Politi](../Images/484e44571bd2e75acfe5fef3146ab3c2.png)](https://medium.com/@marcellopoliti?source=post_page-----5dc15ab147f9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5dc15ab147f9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5dc15ab147f9--------------------------------) [Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----5dc15ab147f9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7390355d40fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-iterate-over-a-pandas-dataframe-5dc15ab147f9&user=Marcello+Politi&userId=7390355d40fe&source=post_page-7390355d40fe----5dc15ab147f9---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----5dc15ab147f9--------------------------------) · 4分钟阅读 · 2023年5月18日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5dc15ab147f9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-iterate-over-a-pandas-dataframe-5dc15ab147f9&source=-----5dc15ab147f9---------------------bookmark_footer-----------)

如果你有一些数据科学的经验，你肯定遇到过从表格数据中开发算法的挑战，这类常见的挑战包括[泰坦尼克号——灾难中的机器学习](https://www.kaggle.com/c/titanic)或波士顿住房数据。

以表格形式表示的数据（如CSV文件）可以区分为**行优先格式**和**列优先格式**。在计算中，行优先顺序和列优先顺序是一种将[多维数组](https://en.wikipedia.org/wiki/Multidimensional_array)存储在线性存储器（如[随机访问存储器](https://en.wikipedia.org/wiki/Random_access_memory)）中的方法。根据格式设计的范例，有最佳实践可遵循以优化文件的读取和写入时间。非常遗憾，数据科学家经常错误地使用像pandas这样的库，浪费了宝贵的时间。

行优先格式意味着在表格中，连续的行在内存中也是连续保存的。因此，如果我正在读取第i行，那么访问第i+1行将是一个非常快的操作。

遵循列优先格式范例的格式，比如Parquet，连续地将列保存在内存中。

在机器学习中，我们经常遇到行是数据样本而列是特征的情况。因此，如果我们需要快速访问样本，我们将使用CSV文件，而Parquet…
