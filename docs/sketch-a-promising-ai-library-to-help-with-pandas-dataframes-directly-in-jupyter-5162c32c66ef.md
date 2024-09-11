# Sketch：一个有前景的 AI 库，旨在直接在 Jupyter 中处理 Pandas Dataframes

> 原文：[https://towardsdatascience.com/sketch-a-promising-ai-library-to-help-with-pandas-dataframes-directly-in-jupyter-5162c32c66ef?source=collection_archive---------4-----------------------#2023-02-21](https://towardsdatascience.com/sketch-a-promising-ai-library-to-help-with-pandas-dataframes-directly-in-jupyter-5162c32c66ef?source=collection_archive---------4-----------------------#2023-02-21)

## 在 Jupyter Notebook 中利用 AI 的力量

[](https://andymcdonaldgeo.medium.com/?source=post_page-----5162c32c66ef--------------------------------)[![Andy McDonald](../Images/df11d647be032aeb3d31852affb33a64.png)](https://andymcdonaldgeo.medium.com/?source=post_page-----5162c32c66ef--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5162c32c66ef--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5162c32c66ef--------------------------------) [Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----5162c32c66ef--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9c280f85f15c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsketch-a-promising-ai-library-to-help-with-pandas-dataframes-directly-in-jupyter-5162c32c66ef&user=Andy+McDonald&userId=9c280f85f15c&source=post_page-9c280f85f15c----5162c32c66ef---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5162c32c66ef--------------------------------) · 7 分钟阅读 · 2023年2月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5162c32c66ef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsketch-a-promising-ai-library-to-help-with-pandas-dataframes-directly-in-jupyter-5162c32c66ef&user=Andy+McDonald&userId=9c280f85f15c&source=-----5162c32c66ef---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5162c32c66ef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsketch-a-promising-ai-library-to-help-with-pandas-dataframes-directly-in-jupyter-5162c32c66ef&source=-----5162c32c66ef---------------------bookmark_footer-----------)![](../Images/3df9b079ef9aaf5c5ac9a1890cb52d23.png)

[来自 Pexels 的照片，作者：Tara Winstead](https://www.pexels.com/photo/robot-pointing-on-a-wall-8386440/)

最近几个月，利用人工智能和大型语言模型创建像 ChatGPT 这样的互动聊天机器人引起了极大的兴趣。我们能在 Jupyter Notebook 中通过 Python 库直接使用这些模型的能力只是时间问题。

最近推出的 Python 库 Sketch 将 AI 编码助手直接带入 Python，并且可以轻松地在 Jupyter 笔记本和 IDE 中使用。该库旨在使用户更容易理解和探索存储在[pandas](https://pandas.pydata.org/)数据框中的数据，无需额外插件。

sketch 库可以快速总结存储在数据框中的数据。它通过使用近似算法（称为数据草图）创建数据摘要，然后将生成的摘要传递给大型语言模型来实现这一点。

通过自然语言输入和可用功能，我们可以探索数据集。这在许多方面都很有帮助，例如：

+   为非编码用户创建一个应用程序来探索数据

+   用于快速生成绘图代码和管理数据
