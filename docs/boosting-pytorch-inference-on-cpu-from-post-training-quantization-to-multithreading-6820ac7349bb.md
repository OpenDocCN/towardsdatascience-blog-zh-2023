# 提升 PyTorch 在 CPU 上的推理速度：从后训练量化到多线程

> 原文：[https://towardsdatascience.com/boosting-pytorch-inference-on-cpu-from-post-training-quantization-to-multithreading-6820ac7349bb?source=collection_archive---------2-----------------------#2023-06-13](https://towardsdatascience.com/boosting-pytorch-inference-on-cpu-from-post-training-quantization-to-multithreading-6820ac7349bb?source=collection_archive---------2-----------------------#2023-06-13)

## [Kaggle 蓝图](/the-kaggle-blueprints-unlocking-winning-approaches-to-data-science-competitions-24d7416ef5fd)

## 如何通过巧妙的模型选择、使用 ONNX Runtime 或 OpenVINO 进行后训练量化，以及使用 ThreadPoolExecutor 实现多线程，来减少 CPU 上的推理时间

[](https://medium.com/@iamleonie?source=post_page-----6820ac7349bb--------------------------------)[![Leonie Monigatti](../Images/4044b1685ada53a30160b03dc78f9626.png)](https://medium.com/@iamleonie?source=post_page-----6820ac7349bb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6820ac7349bb--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6820ac7349bb--------------------------------) [Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----6820ac7349bb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3a38da70d8dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fboosting-pytorch-inference-on-cpu-from-post-training-quantization-to-multithreading-6820ac7349bb&user=Leonie+Monigatti&userId=3a38da70d8dc&source=post_page-3a38da70d8dc----6820ac7349bb---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6820ac7349bb--------------------------------) ·8分钟阅读·2023年6月13日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6820ac7349bb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fboosting-pytorch-inference-on-cpu-from-post-training-quantization-to-multithreading-6820ac7349bb&user=Leonie+Monigatti&userId=3a38da70d8dc&source=-----6820ac7349bb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6820ac7349bb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fboosting-pytorch-inference-on-cpu-from-post-training-quantization-to-multithreading-6820ac7349bb&source=-----6820ac7349bb---------------------bookmark_footer-----------)![](../Images/fc7894bf7fe58528c51d5ad9341c6009.png)

欢迎来到另一期[《Kaggle 蓝图》](/the-kaggle-blueprints-unlocking-winning-approaches-to-data-science-competitions-24d7416ef5fd)，在这里我们将分析[Kaggle](https://www.kaggle.com/)竞赛的获胜解决方案，以提取可以应用于我们自己数据科学项目的经验教训。

本期将回顾[“BirdCLEF 2023](https://www.kaggle.com/competitions/birdclef-2023/)”]竞赛的技术和方法，该竞赛于2023年5月结束。

# 问题陈述：在有限时间和计算约束下的深度学习推断

BirdCLEF 竞赛是一系列每年在 Kaggle 上重复举办的竞赛。BirdCLEF 竞赛的主要目标通常是通过声音识别特定鸟类。参赛者会获得单个鸟鸣的短音频文件，然后必须预测在较长的录音中是否出现了特定的鸟类。

在早期版本的[《Kaggle 蓝图》](/the-kaggle-blueprints-unlocking-winning-approaches-to-data-science-competitions-24d7416ef5fd)中，我们已经回顾了音频获胜的方法……
