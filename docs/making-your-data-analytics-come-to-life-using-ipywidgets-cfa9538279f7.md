# 使用 ipywidgets 使您的数据分析焕发生机

> 原文：[https://towardsdatascience.com/making-your-data-analytics-come-to-life-using-ipywidgets-cfa9538279f7?source=collection_archive---------8-----------------------#2023-02-21](https://towardsdatascience.com/making-your-data-analytics-come-to-life-using-ipywidgets-cfa9538279f7?source=collection_archive---------8-----------------------#2023-02-21)

## 学习如何使用小部件动态更新数据分析

[](https://weimenglee.medium.com/?source=post_page-----cfa9538279f7--------------------------------)[![Wei-Meng Lee](../Images/10fc13e8a6858502d6a7b89fcaad7a10.png)](https://weimenglee.medium.com/?source=post_page-----cfa9538279f7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cfa9538279f7--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----cfa9538279f7--------------------------------) [Wei-Meng Lee](https://weimenglee.medium.com/?source=post_page-----cfa9538279f7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6599e1e08a48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmaking-your-data-analytics-come-to-life-using-ipywidgets-cfa9538279f7&user=Wei-Meng+Lee&userId=6599e1e08a48&source=post_page-6599e1e08a48----cfa9538279f7---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cfa9538279f7--------------------------------) ·8分钟阅读·2023年2月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcfa9538279f7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmaking-your-data-analytics-come-to-life-using-ipywidgets-cfa9538279f7&user=Wei-Meng+Lee&userId=6599e1e08a48&source=-----cfa9538279f7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcfa9538279f7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmaking-your-data-analytics-come-to-life-using-ipywidgets-cfa9538279f7&source=-----cfa9538279f7---------------------bookmark_footer-----------)![](../Images/1b6b04b8bf8137635d612dde9f00c378.png)

图片由 [John Schnobrich](https://unsplash.com/@johnschno?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

对于我的日常数据分析任务，我最喜欢的开发环境无疑是 Jupyter Notebook。Jupyter Notebook 允许我快速修改代码并重新运行单元格以查看更新。然而，这一功能对于使用我的 Jupyter Notebook 查看数据分析结果的用户并不友好。如果能够有一种方法，让用户在不需要修改代码的情况下与我的程序互动，那将非常有用。这就是**ipywidgets**包的作用所在。

**ipywidgets**是一个包含交互式 HTML 小部件的包，适用于 Jupyter Notebook。使用 ipywidgets 中的小部件，你的 Jupyter Notebook 将变得生动起来，用户可以直接控制数据并可视化数据的变化。在本文中，我将带你了解如何将 ipywidgets 与数据集一起使用。

# 安装 ipywidgets

在你的 Jupyter Notebook 中，按如下方式安装**ipywidgets**和**widgetsnbextension**包：

```py
!pip install ipywidgets widgetsnbextension
```

然后，按如下方式启用**widgetsnbextension**：

```py
!jupyter nbextension enable --py widgetsnbextension…
```
