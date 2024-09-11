# 超越Numpy和Pandas：发掘鲜为人知的Python库的潜力

> 原文：[https://towardsdatascience.com/beyond-numpy-and-pandas-unlocking-the-potential-of-lesser-known-python-libraries-86d2bdc4d230?source=collection_archive---------0-----------------------#2023-07-06](https://towardsdatascience.com/beyond-numpy-and-pandas-unlocking-the-potential-of-lesser-known-python-libraries-86d2bdc4d230?source=collection_archive---------0-----------------------#2023-07-06)

## 作为数据专业人士，你应该了解的3个用于科学计算的Python库

[](https://federicotrotta.medium.com/?source=post_page-----86d2bdc4d230--------------------------------)[![Federico Trotta](../Images/e997e3a96940c16ab5071629016d82fd.png)](https://federicotrotta.medium.com/?source=post_page-----86d2bdc4d230--------------------------------)[](https://towardsdatascience.com/?source=post_page-----86d2bdc4d230--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----86d2bdc4d230--------------------------------) [Federico Trotta](https://federicotrotta.medium.com/?source=post_page-----86d2bdc4d230--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F654cd4bbe899&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeyond-numpy-and-pandas-unlocking-the-potential-of-lesser-known-python-libraries-86d2bdc4d230&user=Federico+Trotta&userId=654cd4bbe899&source=post_page-654cd4bbe899----86d2bdc4d230---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----86d2bdc4d230--------------------------------) ·12分钟阅读·2023年7月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F86d2bdc4d230&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeyond-numpy-and-pandas-unlocking-the-potential-of-lesser-known-python-libraries-86d2bdc4d230&user=Federico+Trotta&userId=654cd4bbe899&source=-----86d2bdc4d230---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F86d2bdc4d230&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeyond-numpy-and-pandas-unlocking-the-potential-of-lesser-known-python-libraries-86d2bdc4d230&source=-----86d2bdc4d230---------------------bookmark_footer-----------)![](../Images/8d87b395925ef2d403f2cee6542c46b9.png)

图片由[OrMaVaredo](https://pixabay.com/it/users/ormavaredo-14515736/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=5826755)提供，来源于[Pixabay](https://pixabay.com/it//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=5826755)

Python是世界上最常用的编程语言之一，并为开发者提供了广泛的库。

无论如何，谈到数据处理和科学计算，我们通常会想到诸如`Numpy`、`Pandas`或`SciPy`等库。

在这篇文章中，我们介绍了 3 个你可能感兴趣的 Python 库。

# 1\. Dask

## 介绍 Dask

Dask 是一个灵活的并行计算库，它能够支持大规模数据处理的分布式计算和并行处理。

那么，为什么我们应该使用 Dask 呢？正如他们在[官方网站](https://www.dask.org/)上所说：

> Python 已经成长为数据分析和通用编程的主导语言。这一增长得益于如 NumPy、pandas 和 scikit-learn 等计算库。然而，这些包并没有设计成能够超越单台机器的规模。Dask 的开发就是为了本地扩展这些包和周边生态系统，以支持多核机器和分布式集群，尤其是在数据集超出内存时。
