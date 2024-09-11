# 讲述故事的图表：将数据可视化转化为更有意义的东西

> 原文：[https://towardsdatascience.com/charts-that-tell-a-story-turn-a-plotly-visualization-into-something-more-a723e427d5aa?source=collection_archive---------8-----------------------#2023-03-29](https://towardsdatascience.com/charts-that-tell-a-story-turn-a-plotly-visualization-into-something-more-a723e427d5aa?source=collection_archive---------8-----------------------#2023-03-29)

[](https://medium.com/@kurt.klingensmith?source=post_page-----a723e427d5aa--------------------------------)[![Kurt Klingensmith](../Images/2249e99f12d10f81598c754b1aaf76cc.png)](https://medium.com/@kurt.klingensmith?source=post_page-----a723e427d5aa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a723e427d5aa--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a723e427d5aa--------------------------------) [Kurt Klingensmith](https://medium.com/@kurt.klingensmith?source=post_page-----a723e427d5aa--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbaf16815de65&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcharts-that-tell-a-story-turn-a-plotly-visualization-into-something-more-a723e427d5aa&user=Kurt+Klingensmith&userId=baf16815de65&source=post_page-baf16815de65----a723e427d5aa---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a723e427d5aa--------------------------------) · 9 分钟阅读 · 2023年3月29日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa723e427d5aa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcharts-that-tell-a-story-turn-a-plotly-visualization-into-something-more-a723e427d5aa&user=Kurt+Klingensmith&userId=baf16815de65&source=-----a723e427d5aa---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa723e427d5aa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcharts-that-tell-a-story-turn-a-plotly-visualization-into-something-more-a723e427d5aa&source=-----a723e427d5aa---------------------bookmark_footer-----------)![](../Images/2d64ae634a4dc6519ffaf708147a18f7.png)

照片由 [Erik Mclean](https://unsplash.com/@introspectivedsgn?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，刊登于 [Unsplash](https://unsplash.com/photos/bOJC9SwFfQk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)。

数据可视化传达了数据框和表格无法表达的想法。但要通过数据可视化有效地讲述一个故事，需要一个美观、可解释的图表，并提供必要的背景，使可视化能够独立存在。

幸运的是，Python 包含了众多数据可视化库，例如 [Plotly Express](https://plotly.com/python/plotly-express/)，这些库能够通过一行代码快速创建图表[1]。虽然这些图表很有用，但在正式出版物中单独使用它们或在没有进一步背景的情况下进行审查通常是不够的；一个专业的、能够通过数据讲述故事的独立图表需要额外的工作。本文将逐步讲解如何将数据可视化提升到一个新的水平。

## 代码：

本文的代码可以在[**GitHub 页面链接**](https://github.com/kurtklingensmith/Visualizations)处找到。随意下载代码并在 Jupyter notebook 中跟随操作 — 点击“code”和“Download ZIP”以获取 ipynb 文件。

# 1. 数据准备与初步可视化

使用的库有：

```py
# Data Handling
import pandas as pd

# Data visualization Libraries
import seaborn as sns
import plotly.express as px
import plotly.io as pio
```
