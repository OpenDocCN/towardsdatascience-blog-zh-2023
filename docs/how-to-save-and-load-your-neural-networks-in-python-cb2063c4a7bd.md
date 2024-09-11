# 如何在 Python 中保存和加载神经网络

> 原文：[https://towardsdatascience.com/how-to-save-and-load-your-neural-networks-in-python-cb2063c4a7bd?source=collection_archive---------10-----------------------#2023-04-05](https://towardsdatascience.com/how-to-save-and-load-your-neural-networks-in-python-cb2063c4a7bd?source=collection_archive---------10-----------------------#2023-04-05)

## 完整指南：在 PyTorch 和 TensorFlow/Keras 中保存和加载检查点及整个深度学习模型

[](https://medium.com/@iamleonie?source=post_page-----cb2063c4a7bd--------------------------------)[![Leonie Monigatti](../Images/4044b1685ada53a30160b03dc78f9626.png)](https://medium.com/@iamleonie?source=post_page-----cb2063c4a7bd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cb2063c4a7bd--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----cb2063c4a7bd--------------------------------) [Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----cb2063c4a7bd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3a38da70d8dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-save-and-load-your-neural-networks-in-python-cb2063c4a7bd&user=Leonie+Monigatti&userId=3a38da70d8dc&source=post_page-3a38da70d8dc----cb2063c4a7bd---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cb2063c4a7bd--------------------------------) · 8 分钟阅读 · 2023年4月5日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcb2063c4a7bd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-save-and-load-your-neural-networks-in-python-cb2063c4a7bd&user=Leonie+Monigatti&userId=3a38da70d8dc&source=-----cb2063c4a7bd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcb2063c4a7bd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-save-and-load-your-neural-networks-in-python-cb2063c4a7bd&source=-----cb2063c4a7bd---------------------bookmark_footer-----------)![](../Images/2eeee1c5b7d1e2288a2af1e575047b0d.png)

如何在 PyTorch 和 Tensorflow/Keras 中保存和加载神经网络（图像由作者绘制）

训练神经网络通常需要大量时间和计算资源。如果在投入时间和计算后丢失模型将是非常遗憾的。

这就是为什么你应该能够根据使用情况在不同阶段（训练中或训练完成后）保存和加载深度学习模型的原因：

+   [保存和加载模型检查点](#293e) — 在中断时恢复训练进度或选择最佳检查点

+   [保存和加载整个深度学习模型](#9b7c) — 用于[模型版本控制](https://medium.com/@iamleonie/intro-to-mlops-data-and-model-versioning-fa623c220966)、部署和推理

+   [最佳检查点选择](#b3bf) — 用于[模型版本控制](https://medium.com/@iamleonie/intro-to-mlops-data-and-model-versioning-fa623c220966)、部署和推理

本文涵盖了如何保存和加载检查点及整个模型，适用于两个主要的深度学习框架：

+   PyTorch

```py
import torch
```

+   TensorFlow/Keras

```py
import tensorflow as tf
from tensorflow import keras
```

通常，当加载保存的模型时，**确保模型的版本**…
