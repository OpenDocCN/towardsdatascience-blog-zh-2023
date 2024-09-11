# 如何使用 Python 和 Matplotlib 创建引人注目的国家排名

> 原文：[`towardsdatascience.com/how-to-create-eye-cathing-country-rankings-using-python-and-matplotlib-d57e4594fe13?source=collection_archive---------1-----------------------#2023-08-18`](https://towardsdatascience.com/how-to-create-eye-cathing-country-rankings-using-python-and-matplotlib-d57e4594fe13?source=collection_archive---------1-----------------------#2023-08-18)

## Matplotlib 教程

## 标准折线图的一个美丽替代方案

[](https://medium.com/@oscarleo?source=post_page-----d57e4594fe13--------------------------------)![Oscar Leo](https://medium.com/@oscarleo?source=post_page-----d57e4594fe13--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d57e4594fe13--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d57e4594fe13--------------------------------) [Oscar Leo](https://medium.com/@oscarleo?source=post_page-----d57e4594fe13--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd7e5c1ca65b7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-eye-cathing-country-rankings-using-python-and-matplotlib-d57e4594fe13&user=Oscar+Leo&userId=d7e5c1ca65b7&source=post_page-d7e5c1ca65b7----d57e4594fe13---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d57e4594fe13--------------------------------) · 7 分钟阅读 · 2023 年 8 月 18 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd57e4594fe13&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-eye-cathing-country-rankings-using-python-and-matplotlib-d57e4594fe13&user=Oscar+Leo&userId=d7e5c1ca65b7&source=-----d57e4594fe13---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd57e4594fe13&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-eye-cathing-country-rankings-using-python-and-matplotlib-d57e4594fe13&source=-----d57e4594fe13---------------------bookmark_footer-----------)![](img/4ba0dfcde1b6c985eb2b2fac436d2c9d.png)

图表由作者创建

嗨，欢迎来到这个教程，在这里我将教你如何使用 Python 和 Matplotlib 创建上面的图表。

我喜欢这个数据可视化的地方在于它以干净、美丽的方式展示了国家在特定指标上的排名情况。

使用标准折线图来展示实际值，当一些国家彼此接近或一些国家表现远超其他国家时，图表会变得混乱。

如果你想访问本教程的代码，可以在这个[GitHub 仓库](https://github.com/oscarleoo/country-ranking-tutorial)中找到。

如果你喜欢本教程，记得查看我其他的 Matplotlib 教程。

![Oscar Leo](img/a3badd168c6bfbbdc3d060f9191ca1d2.png)

[Oscar Leo](https://medium.com/@oscarleo?source=post_page-----d57e4594fe13--------------------------------)

## Matplotlib 教程

[查看列表](https://medium.com/@oscarleo/list/matplotlib-tutorials-262e5d7f0847?source=post_page-----d57e4594fe13--------------------------------)8 个故事！[](../Images/51b77b8f6d7ea69abdcd113427d4a52a.png)![](img/56c078b5447338a07b7bce2b23cf7133.png)![](img/c3088ee7cd4994f027ddddbc6ae423cd.png)

让我们开始吧。

## 关于数据

我创建了一个简单的 CSV 文件，包含了今天十大经济体的 GDP 值，供此用……
