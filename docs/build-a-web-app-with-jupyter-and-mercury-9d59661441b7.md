# 使用 Jupyter 和 Mercury 构建 Web 应用

> 原文：[`towardsdatascience.com/build-a-web-app-with-jupyter-and-mercury-9d59661441b7?source=collection_archive---------7-----------------------#2023-05-09`](https://towardsdatascience.com/build-a-web-app-with-jupyter-and-mercury-9d59661441b7?source=collection_archive---------7-----------------------#2023-05-09)

## 教程

## Mercury 提供了一种简单的方法，将 Jupyter Notebooks 转换为交互式 Web 应用。

[](https://medium.com/@alan-jones?source=post_page-----9d59661441b7--------------------------------)![Alan Jones](https://medium.com/@alan-jones?source=post_page-----9d59661441b7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9d59661441b7--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9d59661441b7--------------------------------) [Alan Jones](https://medium.com/@alan-jones?source=post_page-----9d59661441b7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7d3f5fb94faa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-a-web-app-with-jupyter-and-mercury-9d59661441b7&user=Alan+Jones&userId=7d3f5fb94faa&source=post_page-7d3f5fb94faa----9d59661441b7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9d59661441b7--------------------------------) ·10 分钟阅读·2023 年 5 月 9 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9d59661441b7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-a-web-app-with-jupyter-and-mercury-9d59661441b7&user=Alan+Jones&userId=7d3f5fb94faa&source=-----9d59661441b7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9d59661441b7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-a-web-app-with-jupyter-and-mercury-9d59661441b7&source=-----9d59661441b7---------------------bookmark_footer-----------)![](img/50813b13ceab9c877f4a450854919ac3.png)

我在提到代码开发还是 CO2 排放？照片由 [Etienne Girardet](https://unsplash.com/@etiennegirardet?utm_source=medium&utm_medium=referral) 贡献于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

如果没有 Jupyter Notebooks，我们将会在哪里呢？它们无疑是数据科学社区的基本工具之一。

它们非常适合原型设计和/或逐步构建和展示数据科学应用。但它们在展示方面不太好。

如果你想向你的利益相关者展示你的工作结果作为网络应用程序，但不想展示所有背后的复杂代码，你可以将其重新编码为 Streamlit 或 Dash，或者使用 Flask 或 Django 构建一个网络应用程序。

但也有其他选择。

Mercury 是一个可以将你的笔记本转换成完全交互式网络应用程序的系统。你可以添加诸如滑块和选择框等小部件，制作一个完全交互式的应用程序。

一切都是 100% Python，非常简单直接，尽管最终效果可能不如使用其他工具那样复杂，但如果你在寻找一个简单的解决方案，值得一试。

# 一个示例应用程序

我们将创建的应用程序将如下图所示。
