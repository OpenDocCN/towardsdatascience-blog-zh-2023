# Flapjax：使用 Plotly 和 Flask 在网页上进行数据可视化

> 原文：[https://towardsdatascience.com/flapjax-data-visualization-on-the-web-with-plotly-and-flask-465090fa3fba?source=collection_archive---------3-----------------------#2023-11-17](https://towardsdatascience.com/flapjax-data-visualization-on-the-web-with-plotly-and-flask-465090fa3fba?source=collection_archive---------3-----------------------#2023-11-17)

## 构建一个使用 Plotly 和 Flask 的数据可视化网页，并通过一些 UI 组件使其具备交互性

[](https://medium.com/@alan-jones?source=post_page-----465090fa3fba--------------------------------)[![Alan Jones](../Images/359379fab1d6685ff08080b98173e67c.png)](https://medium.com/@alan-jones?source=post_page-----465090fa3fba--------------------------------)[](https://towardsdatascience.com/?source=post_page-----465090fa3fba--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----465090fa3fba--------------------------------) [Alan Jones](https://medium.com/@alan-jones?source=post_page-----465090fa3fba--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7d3f5fb94faa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fflapjax-data-visualization-on-the-web-with-plotly-and-flask-465090fa3fba&user=Alan+Jones&userId=7d3f5fb94faa&source=post_page-7d3f5fb94faa----465090fa3fba---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----465090fa3fba--------------------------------) · 17分钟阅读 · 2023年11月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F465090fa3fba&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fflapjax-data-visualization-on-the-web-with-plotly-and-flask-465090fa3fba&user=Alan+Jones&userId=7d3f5fb94faa&source=-----465090fa3fba---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F465090fa3fba&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fflapjax-data-visualization-on-the-web-with-plotly-and-flask-465090fa3fba&source=-----465090fa3fba---------------------bookmark_footer-----------)![](../Images/d6e1e3b1af74219738061df64cef799c.png)

图片由 [Mae Mu](https://unsplash.com/@picoftasty?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

哪种框架最适合构建数据可视化应用程序？是[Streamlit](https://medium.com/towards-data-science/streamlit-from-scratch-getting-started-f4baa7dd6493)，还是Dash？或者您可以使用[Jupyter Notebook](https://medium.com/towards-data-science/build-a-web-app-with-jupyter-and-mercury-9d59661441b7)将其转换为Web应用程序，或者使用[Voilá](https://medium.com/towards-data-science/how-to-share-your-jupyter-notebook-with-mercury-or-voil%C3%A0-2177110d2f6e)？

所有这些都是创建应用程序的绝佳方法，并且非常容易入门。但是，随着您变得更加冒险，刚开始时显得简单的事情往往会变得更加复杂。因此，我打算试图说服您回归基础，使用Python服务器代码以及HTML页面作为用户界面并不像看起来那么令人畏惧。

我们可以使用相当数量的样板代码和模板来构建引人入胜的交互式应用程序，这意味着您仍然可以专注于您的Python代码，对HTML和Javascript的接触是最少的。我称这种方法为*Flapjax* — 我稍后会解释原因。

在Python中创建Web应用程序的最简单方法之一是使用Flask，所以我们将这样做，并且我们将创建一个类似下面图片中的应用程序。
