# SHAP 用于二元和多分类目标变量

> 原文：[https://towardsdatascience.com/shap-for-binary-and-multiclass-target-variables-ff2f43de0cf4?source=collection_archive---------1-----------------------#2023-09-04](https://towardsdatascience.com/shap-for-binary-and-multiclass-target-variables-ff2f43de0cf4?source=collection_archive---------1-----------------------#2023-09-04)

## 一份关于代码和解释 SHAP 图的指南，当你的模型预测的是分类目标变量时

[](https://conorosullyds.medium.com/?source=post_page-----ff2f43de0cf4--------------------------------)[![Conor O'Sullivan](../Images/2dc50a24edb12e843651d01ed48a3c3f.png)](https://conorosullyds.medium.com/?source=post_page-----ff2f43de0cf4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ff2f43de0cf4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ff2f43de0cf4--------------------------------) [Conor O'Sullivan](https://conorosullyds.medium.com/?source=post_page-----ff2f43de0cf4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4ae48256fb37&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshap-for-binary-and-multiclass-target-variables-ff2f43de0cf4&user=Conor+O%27Sullivan&userId=4ae48256fb37&source=post_page-4ae48256fb37----ff2f43de0cf4---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ff2f43de0cf4--------------------------------) · 9 分钟阅读 · 2023 年 9 月 4 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fff2f43de0cf4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshap-for-binary-and-multiclass-target-variables-ff2f43de0cf4&user=Conor+O%27Sullivan&userId=4ae48256fb37&source=-----ff2f43de0cf4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fff2f43de0cf4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshap-for-binary-and-multiclass-target-variables-ff2f43de0cf4&source=-----ff2f43de0cf4---------------------bookmark_footer-----------)![](../Images/b06abc866e3a7134fc4fddbaa12d53a3.png)

图片来源：[Nika Benedictova](https://unsplash.com/@nika_benedictova?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

SHAP 值给出了模型特征对预测的贡献。当我们使用 SHAP 进行分类时，这一点也是成立的。只不过，对于**二元目标变量**，我们将这些值解释为**对数几率**。对于**多分类目标**，我们使用**softmax**。我们将会：

+   更深入地讨论这些解释

+   提供显示 SHAP 图的代码用于分类问题

+   探索用于多类别目标的SHAP值汇总的新方法

你还可以观看关于该主题的视频：

# 之前的SHAP教程

我们继续之前的[SHAP教程](/introduction-to-shap-with-python-d27edc23c454)。它深入讲解了针对连续目标变量的SHAP图。你会发现这些图及其洞察对于分类目标变量也类似。你还可以在[GitHub](https://github.com/conorosully/SHAP-tutorial)上找到完整的项目。
