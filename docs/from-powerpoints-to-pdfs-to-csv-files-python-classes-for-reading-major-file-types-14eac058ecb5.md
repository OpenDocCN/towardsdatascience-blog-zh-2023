# 从 Powerpoints 到 PDFs 再到 CSV 文件：用于读取主要文件类型的 Python 类

> 原文：[https://towardsdatascience.com/from-powerpoints-to-pdfs-to-csv-files-python-classes-for-reading-major-file-types-14eac058ecb5?source=collection_archive---------24-----------------------#2023-01-06](https://towardsdatascience.com/from-powerpoints-to-pdfs-to-csv-files-python-classes-for-reading-major-file-types-14eac058ecb5?source=collection_archive---------24-----------------------#2023-01-06)

## 在你的下一个数据科学项目中，能够从多种不同的文件类型中提取和比较信息！

[](https://ben-mccloskey20.medium.com/?source=post_page-----14eac058ecb5--------------------------------)[![Benjamin McCloskey](../Images/7118f5933f2affe2a7a4d3375452fa4c.png)](https://ben-mccloskey20.medium.com/?source=post_page-----14eac058ecb5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----14eac058ecb5--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----14eac058ecb5--------------------------------) [Benjamin McCloskey](https://ben-mccloskey20.medium.com/?source=post_page-----14eac058ecb5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F503796fc1483&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-powerpoints-to-pdfs-to-csv-files-python-classes-for-reading-major-file-types-14eac058ecb5&user=Benjamin+McCloskey&userId=503796fc1483&source=post_page-503796fc1483----14eac058ecb5---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----14eac058ecb5--------------------------------) · 6 分钟阅读 · 2023年1月6日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F14eac058ecb5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-powerpoints-to-pdfs-to-csv-files-python-classes-for-reading-major-file-types-14eac058ecb5&user=Benjamin+McCloskey&userId=503796fc1483&source=-----14eac058ecb5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F14eac058ecb5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-powerpoints-to-pdfs-to-csv-files-python-classes-for-reading-major-file-types-14eac058ecb5&source=-----14eac058ecb5---------------------bookmark_footer-----------)![](../Images/ae884e2e91a90681e265bf02b77cd8e0.png)

图片来源：[Glen Carrie](https://unsplash.com/@glencarrie?utm_source=medium&utm_medium=referral) 摄于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

数据科学家在项目中可能需要读取多种不同类型的文件。虽然在 Python 中读取*文本*文件特别简单，但其他文件类型可能需要借助各种 Python API 来确保它们可被 Python 读取和使用。今天的代码提供了多个不同的 Python 类，可以用于读取多种文件类型。每个类的输出是一个文本字符串，数据科学家可以用来提取信息以及对各种文档进行相似性分析。

# PDF 文件

我过去已经多次讨论了 PDF 文件的重要性以及如何在 Python 中处理它们。

[## PDF 解析仪表板与 Plotly Dash](https://towardsdatascience.com/pdf-parsing-dashboard-with-plotly-dash-256bf944f536?source=post_page-----14eac058ecb5--------------------------------)

### 如何在下一个仪表板中读取和显示 PDF 文件的介绍。

[towardsdatascience.com](https://towardsdatascience.com/pdf-parsing-dashboard-with-plotly-dash-256bf944f536?source=post_page-----14eac058ecb5--------------------------------)
