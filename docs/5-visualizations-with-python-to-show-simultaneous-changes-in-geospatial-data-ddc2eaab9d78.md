# 使用 Python 展示地理空间数据中同时变化的 5 种可视化方法

> 原文：[https://towardsdatascience.com/5-visualizations-with-python-to-show-simultaneous-changes-in-geospatial-data-ddc2eaab9d78?source=collection_archive---------4-----------------------#2023-11-04](https://towardsdatascience.com/5-visualizations-with-python-to-show-simultaneous-changes-in-geospatial-data-ddc2eaab9d78?source=collection_archive---------4-----------------------#2023-11-04)

## 表达多个地点随时间变化的数据值的想法

[](https://medium.com/@borih.k?source=post_page-----ddc2eaab9d78--------------------------------)[![Boriharn K](../Images/1b23a79640f5272c1382918bfdba03b0.png)](https://medium.com/@borih.k?source=post_page-----ddc2eaab9d78--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ddc2eaab9d78--------------------------------)[![数据科学之路](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ddc2eaab9d78--------------------------------) [Boriharn K](https://medium.com/@borih.k?source=post_page-----ddc2eaab9d78--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe20a7f1ba78f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F5-visualizations-with-python-to-show-simultaneous-changes-in-geospatial-data-ddc2eaab9d78&user=Boriharn+K&userId=e20a7f1ba78f&source=post_page-e20a7f1ba78f----ddc2eaab9d78---------------------post_header-----------) 发表在 [数据科学之路](https://towardsdatascience.com/?source=post_page-----ddc2eaab9d78--------------------------------) ·8分钟阅读·2023年11月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fddc2eaab9d78&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F5-visualizations-with-python-to-show-simultaneous-changes-in-geospatial-data-ddc2eaab9d78&user=Boriharn+K&userId=e20a7f1ba78f&source=-----ddc2eaab9d78---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fddc2eaab9d78&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F5-visualizations-with-python-to-show-simultaneous-changes-in-geospatial-data-ddc2eaab9d78&source=-----ddc2eaab9d78---------------------bookmark_footer-----------)![](../Images/d7529a24db9a8de2067b8f1201e8deb3.png)

图片由 [John Matychuk](https://unsplash.com/@john_matychuk?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

时间和空间在一些科幻电影中被设定为主要主题，比如我最喜欢的《星际穿越》。这类电影之所以有趣的一个原因是它们讲述了在不同地点同时发生的故事，同时这些故事也互相影响。

地理空间数据是一种包含空间信息和属性的数据，这些属性是事件或对象的特征。

如果将数据可视化视为讲述故事的一种方式，那么随时间变化的地理空间数据可以成为创建有趣故事情节的好素材，因为它与科幻电影共享一些概念。

![](../Images/ab9be4c734f21a65ccbf89a5f7279cef.png)![](../Images/666a0c1b278bb1f06cc90f360c061175.png)

本文展示了使用 Python 构建的图表示例，以表达地理空间数据的同步变化。图片由作者提供。

本文将引导你使用 Python 代码中的图表和技术来表达地理空间数据的同步变化。

让我们开始吧！！

# 获取数据

## 几何数据
