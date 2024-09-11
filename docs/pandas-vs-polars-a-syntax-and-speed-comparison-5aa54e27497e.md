# Pandas 与 Polars：语法和速度比较

> 原文：[https://towardsdatascience.com/pandas-vs-polars-a-syntax-and-speed-comparison-5aa54e27497e?source=collection_archive---------0-----------------------#2023-01-11](https://towardsdatascience.com/pandas-vs-polars-a-syntax-and-speed-comparison-5aa54e27497e?source=collection_archive---------0-----------------------#2023-01-11)

## 理解 Python 库 Pandas 和 Polars 在数据科学中的主要区别

[](https://medium.com/@iamleonie?source=post_page-----5aa54e27497e--------------------------------)[![Leonie Monigatti](../Images/4044b1685ada53a30160b03dc78f9626.png)](https://medium.com/@iamleonie?source=post_page-----5aa54e27497e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5aa54e27497e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5aa54e27497e--------------------------------) [Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----5aa54e27497e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3a38da70d8dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpandas-vs-polars-a-syntax-and-speed-comparison-5aa54e27497e&user=Leonie+Monigatti&userId=3a38da70d8dc&source=post_page-3a38da70d8dc----5aa54e27497e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5aa54e27497e--------------------------------) · 7分钟阅读 · 2023年1月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5aa54e27497e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpandas-vs-polars-a-syntax-and-speed-comparison-5aa54e27497e&user=Leonie+Monigatti&userId=3a38da70d8dc&source=-----5aa54e27497e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5aa54e27497e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpandas-vs-polars-a-syntax-and-speed-comparison-5aa54e27497e&source=-----5aa54e27497e---------------------bookmark_footer-----------)![](../Images/306c1a690661bae1c6ed3ad256c31693.png)

作者提供的图片

*本文根据* [*Jakob Ullmann*](https://medium.com/u/614ba3d0ee10?source=post_page-----5aa54e27497e--------------------------------)*,* [*Dr. Robert Kübler*](https://medium.com/u/6d6b5fb431bf?source=post_page-----5aa54e27497e--------------------------------)*，和* [*Thiago Jaworski*](https://medium.com/u/7191497d8bc1?source=post_page-----5aa54e27497e--------------------------------) *的意见进行了修订，日期为2023年1月19日。感谢您的反馈！*

Pandas是数据科学中必不可少的Python库。但它最大的缺点是对于大数据集的操作可能会很慢。Polars是一个旨在更快处理数据的Pandas替代方案。

> Polars是一个旨在更快处理数据的Pandas替代方案。

本文简要介绍了Polars Python包，并将其与流行的数据科学库Pandas在语法和速度上进行比较。

· [什么是Polars以及为什么它比Pandas更快？](#454c)

· [基准设置](#133c)

· [快速入门Polars](#eb80)

· [Pandas与Polars的比较](#e87a)

∘ [读取数据](#0602)

∘ [选择和过滤数据](#cfde)

∘ [创建新列](#b6f5)

∘ [分组与聚合](#96ab)

∘ [缺失数据](#7524)

· [结论](#e4ef)
