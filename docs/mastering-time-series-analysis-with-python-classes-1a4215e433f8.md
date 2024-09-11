# 精通 Python 类的时间序列分析

> 原文：[`towardsdatascience.com/mastering-time-series-analysis-with-python-classes-1a4215e433f8?source=collection_archive---------9-----------------------#2023-01-10`](https://towardsdatascience.com/mastering-time-series-analysis-with-python-classes-1a4215e433f8?source=collection_archive---------9-----------------------#2023-01-10)

## 面向对象编程用于时间序列分析

[](https://spierre91.medium.com/?source=post_page-----1a4215e433f8--------------------------------)![Sadrach Pierre, Ph.D.](https://spierre91.medium.com/?source=post_page-----1a4215e433f8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1a4215e433f8--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1a4215e433f8--------------------------------) [Sadrach Pierre, Ph.D.](https://spierre91.medium.com/?source=post_page-----1a4215e433f8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F120b86134681&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-time-series-analysis-with-python-classes-1a4215e433f8&user=Sadrach+Pierre%2C+Ph.D.&userId=120b86134681&source=post_page-120b86134681----1a4215e433f8---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1a4215e433f8--------------------------------) ·8 分钟阅读·2023 年 1 月 10 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1a4215e433f8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-time-series-analysis-with-python-classes-1a4215e433f8&user=Sadrach+Pierre%2C+Ph.D.&userId=120b86134681&source=-----1a4215e433f8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1a4215e433f8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-time-series-analysis-with-python-classes-1a4215e433f8&source=-----1a4215e433f8---------------------bookmark_footer-----------)![](img/197b5de13a8d4ab8c00930b07a2ff313.png)

图片由 [Tima Miroshnichenko](https://www.pexels.com/@tima-miroshnichenko/) 提供，发布在 [Pexels](https://www.pexels.com/photo/vintage-alarm-clocks-displayed-on-a-wooden-shelf-8327953/)

时间序列分析是最常见的数据科学任务之一。它涉及分析按时间顺序排列的数据点中的趋势。时间序列数据种类繁多，包括股市数据、天气数据、消费者需求数据等。时间序列分析在各个行业都有应用，因此它是数据科学家和数据分析师必备的技能。

时间序列分析涉及许多无法在单一文章中总结的技术。一些最常见的方法包括通过折线图可视化时间序列数据、建立时间序列预测模型、进行谱分析以发现周期趋势、分析季节性趋势等。

时间序列分析涉及许多不同的技术，因此它自然适合于面向对象编程。Python 类使得组织与时间序列任务相关的属性和方法变得简单。例如，如果作为数据科学家，你经常进行折线图可视化、季节性分析和时间序列预测，类可以让你轻松组织这些任务的方法和属性。
