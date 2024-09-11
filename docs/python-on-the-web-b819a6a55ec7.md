# Python 在网络上的应用

> 原文：[`towardsdatascience.com/python-on-the-web-b819a6a55ec7?source=collection_archive---------1-----------------------#2023-10-11`](https://towardsdatascience.com/python-on-the-web-b819a6a55ec7?source=collection_archive---------1-----------------------#2023-10-11)

## 无需任何服务器即可展示 Python 应用程序

[](https://pierpaoloippolito28.medium.com/?source=post_page-----b819a6a55ec7--------------------------------)![Pier Paolo Ippolito](https://pierpaoloippolito28.medium.com/?source=post_page-----b819a6a55ec7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b819a6a55ec7--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b819a6a55ec7--------------------------------) [Pier Paolo Ippolito](https://pierpaoloippolito28.medium.com/?source=post_page-----b819a6a55ec7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb8391a6a5f1a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-on-the-web-b819a6a55ec7&user=Pier+Paolo+Ippolito&userId=b8391a6a5f1a&source=post_page-b8391a6a5f1a----b819a6a55ec7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b819a6a55ec7--------------------------------) ·9 分钟阅读·2023 年 10 月 11 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb819a6a55ec7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-on-the-web-b819a6a55ec7&user=Pier+Paolo+Ippolito&userId=b8391a6a5f1a&source=-----b819a6a55ec7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb819a6a55ec7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-on-the-web-b819a6a55ec7&source=-----b819a6a55ec7---------------------bookmark_footer-----------)![](img/318b3a5fe6458ff8549c4a678f90e7c6.png)

照片由 [Ales Nesetril](https://unsplash.com/@alesnesetril?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

使用 流行的 Python 可视化库 可以相对简单地在本地创建各种形式的图表和仪表板。然而，将你的结果与其他人分享在网上可能要复杂得多。

一种可能的方法是使用如 Streamlit、Flask、Plotly Dash 等库，并支付网络托管服务费用以处理服务器端，并运行你的 Python 脚本以在网页上显示。或者，一些提供商如 Plotly Chart 或 Datapane 也提供免费的云支持，以便你上传 Python 可视化结果并将其嵌入到网页上。在这两种情况下，如果你有一个小预算用于项目，你将能够实现你的需求，但有没有任何方式可以免费实现类似的结果？

在本文中，我们将探索三种可能的方法：

+   [Panel by Holoviz](https://panel.holoviz.org/)

+   [Shiny for Python](https://shiny.posit.co/py/)

+   [PyScript](https://pyscript.net/)

为了展示这三种方法，我们将创建一个简单的应用程序来探索来自全球的历史通胀数据。为此，我们将使用[世界银行全球数据库](https://www.worldbank.org/en/research/brief/inflation-database)…
