# 如何利用 Matplotlib 马赛克增强你的可视化

> 原文：[`towardsdatascience.com/how-to-enhance-your-visualizations-with-matplotlib-mosaic-adcaf0008aa6?source=collection_archive---------5-----------------------#2023-06-08`](https://towardsdatascience.com/how-to-enhance-your-visualizations-with-matplotlib-mosaic-adcaf0008aa6?source=collection_archive---------5-----------------------#2023-06-08)

## 我从 Matplotlib 学到的最酷的方法之一

[](https://gustavorsantos.medium.com/?source=post_page-----adcaf0008aa6--------------------------------)![古斯塔沃·桑托斯](https://gustavorsantos.medium.com/?source=post_page-----adcaf0008aa6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----adcaf0008aa6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----adcaf0008aa6--------------------------------) [古斯塔沃·桑托斯](https://gustavorsantos.medium.com/?source=post_page-----adcaf0008aa6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4429d99b1245&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-enhance-your-visualizations-with-matplotlib-mosaic-adcaf0008aa6&user=Gustavo+Santos&userId=4429d99b1245&source=post_page-4429d99b1245----adcaf0008aa6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----adcaf0008aa6--------------------------------) ·5 分钟阅读·2023 年 6 月 8 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fadcaf0008aa6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-enhance-your-visualizations-with-matplotlib-mosaic-adcaf0008aa6&user=Gustavo+Santos&userId=4429d99b1245&source=-----adcaf0008aa6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fadcaf0008aa6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-enhance-your-visualizations-with-matplotlib-mosaic-adcaf0008aa6&source=-----adcaf0008aa6---------------------bookmark_footer-----------)![](img/6fa68ce84e5bfd42fc18ef09855c0ba9.png)

微笑吧！马赛克子图来了！图像由作者创作。

# 介绍

数据可视化非常重要。我们可以从这句话中展开很多内容，但我相信你已经“明白”了。我确定这不是你第一次听到这句话。即便是第一次，我也假设这不会让你感到惊讶，对吧？

> 一图胜千观测。

如果 *一图胜千言*，我会说，对数据科学而言，*一图胜千观测*。

好的，我们来看看这里重要的内容。这篇简短的帖子旨在向你展示方法`subplot_mosaic()`。这个函数真是太棒了。最近我在浏览一些数据科学相关的内容时发现了它。我对能够如此轻松地在同一图形中快速创建多个图表感到惊讶。

让我们在下一节中看看它是如何完成的。

将使用的数据集是*Tips*，它是 Python 中 Seaborn 包的原生数据集。

```py
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Data
df = sns.load_dataset('tips')
```

# 马赛克
