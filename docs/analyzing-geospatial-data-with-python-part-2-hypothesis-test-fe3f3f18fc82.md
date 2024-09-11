# 使用 Python 分析地理空间数据（第 2 部分 — 假设检验）

> 原文：[https://towardsdatascience.com/analyzing-geospatial-data-with-python-part-2-hypothesis-test-fe3f3f18fc82?source=collection_archive---------1-----------------------#2023-08-31](https://towardsdatascience.com/analyzing-geospatial-data-with-python-part-2-hypothesis-test-fe3f3f18fc82?source=collection_archive---------1-----------------------#2023-08-31)

## 学习阿什维尔的 AirBnb 房源的地理空间假设检验

[](https://gustavorsantos.medium.com/?source=post_page-----fe3f3f18fc82--------------------------------)[![古斯塔沃·桑托斯](../Images/a19a9f4525cdeb6e7a76cd05246aa622.png)](https://gustavorsantos.medium.com/?source=post_page-----fe3f3f18fc82--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fe3f3f18fc82--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----fe3f3f18fc82--------------------------------) [古斯塔沃·桑托斯](https://gustavorsantos.medium.com/?source=post_page-----fe3f3f18fc82--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4429d99b1245&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanalyzing-geospatial-data-with-python-part-2-hypothesis-test-fe3f3f18fc82&user=Gustavo+Santos&userId=4429d99b1245&source=post_page-4429d99b1245----fe3f3f18fc82---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fe3f3f18fc82--------------------------------) · 12 min 阅读 · 2023年8月31日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffe3f3f18fc82&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanalyzing-geospatial-data-with-python-part-2-hypothesis-test-fe3f3f18fc82&user=Gustavo+Santos&userId=4429d99b1245&source=-----fe3f3f18fc82---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffe3f3f18fc82&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanalyzing-geospatial-data-with-python-part-2-hypothesis-test-fe3f3f18fc82&source=-----fe3f3f18fc82---------------------bookmark_footer-----------)![](../Images/8bee11a1f9d0a8e4d3e2d581a3586452.png)

阿什维尔，北卡罗来纳州的蓝岭山脉。照片来自作者个人收藏。

# 介绍

在第一篇文章中，链接如下，我们介绍了地理空间数据分析，我们从*AirBnb*下载了北卡罗来纳州阿什维尔市的房源列表，并通过一些步骤从地理空间数据中提取洞察。

[](/analyzing-geospatial-data-with-python-7244c1b9e302?source=post_page-----fe3f3f18fc82--------------------------------) [## 使用 Python 分析地理空间数据

### 一篇实用的包含Python代码的数据分析文章。

[towardsdatascience.com](/analyzing-geospatial-data-with-python-7244c1b9e302?source=post_page-----fe3f3f18fc82--------------------------------)

在那篇文章中，我们更多地关注了租赁房产的集中区域和价格。因此，我们得出结论，阿什维尔的房源主要集中在市区，最高价格出现在蓝岭公园路沿线，可能是因为美丽的景色和乡村环境。

很好。我建议你先阅读第一篇文章，这样你可以将初始代码和思路结合起来，然后继续学习第二部分提供的知识。

## 数据集
