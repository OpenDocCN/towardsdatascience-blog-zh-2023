# 如何使用 Streamlit 的 st.write 函数来提升你的 Streamlit 仪表盘

> 原文：[`towardsdatascience.com/how-to-use-streamlits-st-write-function-to-improve-your-streamlit-dashboard-1586333eb24d?source=collection_archive---------4-----------------------#2023-01-17`](https://towardsdatascience.com/how-to-use-streamlits-st-write-function-to-improve-your-streamlit-dashboard-1586333eb24d?source=collection_archive---------4-----------------------#2023-01-17)

## Streamlit 函数的瑞士军刀

[](https://andymcdonaldgeo.medium.com/?source=post_page-----1586333eb24d--------------------------------)![Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----1586333eb24d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1586333eb24d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1586333eb24d--------------------------------) [Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----1586333eb24d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9c280f85f15c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-streamlits-st-write-function-to-improve-your-streamlit-dashboard-1586333eb24d&user=Andy+McDonald&userId=9c280f85f15c&source=post_page-9c280f85f15c----1586333eb24d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1586333eb24d--------------------------------) ·8 分钟阅读·2023 年 1 月 17 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1586333eb24d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-streamlits-st-write-function-to-improve-your-streamlit-dashboard-1586333eb24d&user=Andy+McDonald&userId=9c280f85f15c&source=-----1586333eb24d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1586333eb24d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-streamlits-st-write-function-to-improve-your-streamlit-dashboard-1586333eb24d&source=-----1586333eb24d---------------------bookmark_footer-----------)![](img/fc9fcf4c1510ad514f6ae258e5ee6e19.png)

图片由 [Bram Naus](https://unsplash.com/es/@bramnaus?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在开始使用 [Streamlit](https://streamlit.io/) 时，你会遇到的第一个函数之一就是 `st.write()`。 [Streamlit 文档将这个函数描述为](https://docs.streamlit.io/library/api-reference/write-magic/st.write)“Streamlit 命令的瑞士军刀”。

它是一个非常多才多艺的函数，允许您显示文本、表情符号、Markdown 等等。

在本文中，我们将探讨`st.write()`函数可用于改进您的 Streamlit 仪表板的方法。

[](https://docs.streamlit.io/library/api-reference/write-magic/st.write?source=post_page-----1586333eb24d--------------------------------) [## st.write — Streamlit 文档

### 函数签名 st.write(*args, unsafe_allow_html=False, **kwargs)参数一个或多个要打印到…

docs.streamlit.io](https://docs.streamlit.io/library/api-reference/write-magic/st.write?source=post_page-----1586333eb24d--------------------------------)

本文是我在 Streamlit 上创建的一系列文章的一部分。您可以在下面的链接中探索它们。

+   开始使用基于 Streamlit 的 Web 应用程序

+   创建真正的多页面 Streamlit 应用程序 - 新型方法（2022）

+   [开始使用 Streamlit：初学者需要知道的 5 个函数](https://medium.com/towards-data-science/getting-started-with-streamlit-5-functions-you-need-to-know-when-starting-out-b35ed7d872b9)

+   Streamlit 颜色选择器：一个简单的方法…
