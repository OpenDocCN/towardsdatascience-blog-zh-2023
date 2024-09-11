# 使用 Pandas 在 Python 中的数据聚合：分析地质岩石学数据

> 原文：[https://towardsdatascience.com/data-aggregation-in-python-with-pandas-analysing-geological-lithology-data-94192f5631c0?source=collection_archive---------15-----------------------#2023-06-29](https://towardsdatascience.com/data-aggregation-in-python-with-pandas-analysing-geological-lithology-data-94192f5631c0?source=collection_archive---------15-----------------------#2023-06-29)

## 探索挪威大陆架中泽克斯坦群的岩石学变化

[](https://andymcdonaldgeo.medium.com/?source=post_page-----94192f5631c0--------------------------------)[![安迪·麦克唐纳](../Images/df11d647be032aeb3d31852affb33a64.png)](https://andymcdonaldgeo.medium.com/?source=post_page-----94192f5631c0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----94192f5631c0--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----94192f5631c0--------------------------------) [安迪·麦克唐纳](https://andymcdonaldgeo.medium.com/?source=post_page-----94192f5631c0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9c280f85f15c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-aggregation-in-python-with-pandas-analysing-geological-lithology-data-94192f5631c0&user=Andy+McDonald&userId=9c280f85f15c&source=post_page-9c280f85f15c----94192f5631c0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----94192f5631c0--------------------------------) · 6 分钟阅读 · 2023年6月29日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F94192f5631c0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-aggregation-in-python-with-pandas-analysing-geological-lithology-data-94192f5631c0&user=Andy+McDonald&userId=9c280f85f15c&source=-----94192f5631c0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F94192f5631c0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-aggregation-in-python-with-pandas-analysing-geological-lithology-data-94192f5631c0&source=-----94192f5631c0---------------------bookmark_footer-----------)![](../Images/d088a32dac22c1b514824f1a9acdc8f6.png)

图片由作者使用 Midjourney（付费订阅计划）生成。

使用数据聚合技术可以帮助我们将令人难以处理和几乎不可理解的数值数据集转化为易于消化且更具可读性的信息。数据聚合过程包括将多个数据点总结为单一指标，以提供数据的高级概览。

我们可以在岩石物理学和地球科学中应用这一过程的一种方法是总结从井测量数据中解释出的地质层位的岩性组成。

在这个简短的教程中，我们将看到如何处理一个包含90多个挪威大陆架井的大型数据集，并提取齐赫斯坦组的岩性组成。

# 导入库和加载数据

首先，我们需要导入[**pandas**](https://pandas.pydata.org)库，该库将用于从CSV文件加载数据并进行汇总操作。

```py
import pandas as pd
```
