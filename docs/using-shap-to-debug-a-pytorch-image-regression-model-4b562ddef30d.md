# 使用 SHAP 调试 PyTorch 图像回归模型

> 原文：[`towardsdatascience.com/using-shap-to-debug-a-pytorch-image-regression-model-4b562ddef30d?source=collection_archive---------14-----------------------#2023-01-10`](https://towardsdatascience.com/using-shap-to-debug-a-pytorch-image-regression-model-4b562ddef30d?source=collection_archive---------14-----------------------#2023-01-10)

## 使用 DeepShap 理解和改进驱动自动驾驶汽车的模型

[](https://conorosullyds.medium.com/?source=post_page-----4b562ddef30d--------------------------------)![Conor O'Sullivan](https://conorosullyds.medium.com/?source=post_page-----4b562ddef30d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4b562ddef30d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----4b562ddef30d--------------------------------) [Conor O'Sullivan](https://conorosullyds.medium.com/?source=post_page-----4b562ddef30d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4ae48256fb37&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-shap-to-debug-a-pytorch-image-regression-model-4b562ddef30d&user=Conor+O%27Sullivan&userId=4ae48256fb37&source=post_page-4ae48256fb37----4b562ddef30d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4b562ddef30d--------------------------------) ·11 min read·2023 年 1 月 10 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4b562ddef30d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-shap-to-debug-a-pytorch-image-regression-model-4b562ddef30d&user=Conor+O%27Sullivan&userId=4ae48256fb37&source=-----4b562ddef30d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4b562ddef30d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-shap-to-debug-a-pytorch-image-regression-model-4b562ddef30d&source=-----4b562ddef30d---------------------bookmark_footer-----------)![](img/965f5964b6c98ef14db3694ad7588c2f.png)

(source: author)

自动驾驶汽车让我感到恐惧。大块的金属在空中飞行，如果出现问题没有人能阻止它们。为了降低这种风险，评估这些庞然大物所驱动的模型是不够的。我们还需要了解它们如何做出预测。这是为了避免任何可能导致意外的边缘情况。

好的，我们的应用并不是那么重要。我们将调试用于驱动迷你自动化车的模型（你能预期的最糟情况只是脚踝受伤）。不过，IML 方法仍然可能很有用。我们将看到它们如何甚至能提升模型的性能。

具体来说，我们将：

+   使用 PyTorch 对图像数据和连续目标变量进行 ResNet-18 的微调

+   使用 MSE 和散点图评估模型

+   使用 DeepSHAP 解读模型

+   通过更好的数据收集来修正模型

+   讨论图像增强如何进一步提升模型

在此过程中，我们将讨论一些关键的 Python 代码。你还可以在 [GitHub](https://github.com/conorosully/SHAP-tutorial/blob/main/src/image_data.ipynb) 上找到完整的项目。
