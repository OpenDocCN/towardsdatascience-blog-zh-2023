# 提升 PyTorch 在 CPU 上的推理：从训练后量化到多线程

> 原文：[`towardsdatascience.com/boosting-pytorch-inference-on-cpu-from-post-training-quantization-to-multithreading-6820ac7349bb`](https://towardsdatascience.com/boosting-pytorch-inference-on-cpu-from-post-training-quantization-to-multithreading-6820ac7349bb)

## Kaggle 蓝图

## 如何通过巧妙的模型选择、使用 ONNX Runtime 或 OpenVINO 进行训练后量化以及使用 ThreadPoolExecutor 实现多线程来减少 CPU 上的推理时间

[](https://medium.com/@iamleonie?source=post_page-----6820ac7349bb--------------------------------)![Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----6820ac7349bb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6820ac7349bb--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6820ac7349bb--------------------------------) [Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----6820ac7349bb--------------------------------)

·发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6820ac7349bb--------------------------------) ·8 分钟阅读·2023 年 6 月 13 日

--

![](img/fc7894bf7fe58528c51d5ad9341c6009.png)

欢迎来到另一期 “Kaggle 蓝图” ，我们将分析 [Kaggle](https://www.kaggle.com/) 竞赛的获胜解决方案，以便将其中的经验应用到我们自己的数据科学项目中。

本版将回顾 [“BirdCLEF 2023](https://www.kaggle.com/competitions/birdclef-2023/)” 竞赛中的技术和方法，该竞赛于 2023 年 5 月结束。

# 问题陈述：在有限时间和计算约束下的深度学习推理

BirdCLEF 竞赛是一系列每年在 Kaggle 上举行的竞赛。BirdCLEF 竞赛的主要目标通常是通过声音识别特定的鸟类。参赛者会获得单只鸟叫的短音频文件，然后必须预测在较长的录音中是否存在特定的鸟类。

在早期版本的 《Kaggle 蓝图》 中，我们已经回顾了去年的 “[BirdCLEF 2022](https://www.kaggle.com/competitions/birdclef-2022/)” 竞赛中的音频分类获胜方法。

[“BirdCLEF 2023](https://www.kaggle.com/competitions/birdclef-2023/)”竞赛中的一个新颖方面是时间和计算限制：**竞争者被要求在 2 小时内在 CPU 笔记本上预测大约 200 个 10 分钟的录音**[**](https://www.kaggle.com/competitions/birdclef-2023/overview/code-requirements)**。**

[](https://www.kaggle.com/competitions/birdclef-2023?source=post_page-----6820ac7349bb--------------------------------) [## BirdCLEF 2023

### 识别声音景观中的鸟类鸣叫

[www.kaggle.com](https://www.kaggle.com/competitions/birdclef-2023?source=post_page-----6820ac7349bb--------------------------------)

你可能会问，为什么有人会选择在 CPU 上推理深度学习模型而不是 GPU。这是一个常见的实际问题声明[4]，因为工作人员（尤其是在保护工作中，但也包括其他行业）常常面临预算限制，因此只能使用有限的计算资源。此外，能够快速做出预测是有帮助的。

由于深入探讨如何用深度学习进行音频分类将重复之前《Kaggle Blueprints》版本关于 BirdCLEF 2022 竞赛的内容，我们将**专注于如何加速 CPU 上深度学习模型推理的创新方面**。

如果你对深度学习音频分类的获胜方法感兴趣，可以查看之前的版本：

[](/audio-classification-with-deep-learning-in-python-cf752b22ba07?source=post_page-----6820ac7349bb--------------------------------) ## 用 Python 进行深度学习音频分类

### 微调图像模型以应对领域偏移和类别不平衡，使用 PyTorch 和 torchaudio 处理音频数据

[towardsdatascience.com

# 深度学习推理在 CPU 上的方法

在有限时间内在 CPU 上进行推理的主要问题是，你无法创建大量强大且多样化的模型来挤出最后几百分点的性能。根据使用的模型，有些竞争者甚至在单个模型下也难以满足有限的时间要求。

然而，通常较弱的模型的集合会比单一强大的模型表现更好。在竞赛总结中，成功的竞争者分享了一些技巧，说明他们如何加速在 CPU 上的推理，以便能够集合多个模型。

本文介绍了在总结中分享的一些技巧：

+   模型选择

+   训练后量化

+   多线程

# 模型选择

模型大小严重影响推理时间。一般规则是：模型越大，推理时间越长。

> 一般规则是：模型越大，推理时间越长。

因此，在为其模型集合选择骨干网络时，竞争者必须评估哪些模型在性能和推理时间之间提供了最佳的折衷。

尽管 NFNet (`eca_nfnet_l0`) [3, 5, 7, 9, 10, 11, 13, 14, 16] 和 EfficientNet 在去年和今年的比赛中仍然是流行的骨干网络，但我们可以看到，在今年的比赛中，竞争者更倾向于使用 EfficientNet 的小版本。

虽然在[BirdCLEF 2022](https://www.kaggle.com/competitions/birdclef-2022/)比赛中，`tf_efficientnet_b0_ns` [8, 11]、`tf_efficientnet_b3_ns` [8]、`tf_efficientnetv2_s_in21k` [11, 16]和`tf_efficientnetv2_m_in21k` [13]非常受欢迎，但今年较小版本的`tf_efficientnet_b0_ns` [1, 5, 6, 7, 10]和`tf_efficientnetv2_s_in21k` [1, 6, 15]更受青睐。

以下是 BirdCLEF 竞赛系列中流行模型的参数数量对比。

![](img/eaf0377d18a6da1792353c7000294d77.png)

不同神经网络架构的参数数量。

因此，我们可以看到，成功的竞争者利用了大型模型（`eca_nfnet_l0`）与小型模型（例如，`tf_efficientnet_b0_ns`）的组合。

# 后训练量化

提高 CPU 推理速度的另一个技巧是对训练后的模型应用量化：后训练量化将模型的权重和激活的精度从浮点精度（32 位）降低到较低位宽表示（例如 8 位）。

该技术将模型转化为更适合硬件的表示，从而提高延迟性能。然而，由于权重和激活表示的精度损失，它也可能导致轻微的性能下降。

量化与硬件密切相关。例如，一个 Kaggle 笔记本具有[4 个 CPU（Intel(R) Xeon(R) CPU @ 2.20GHz，x86_64 架构）](https://www.kaggle.com/questions-and-answers/120979)。这些[带有 x86 架构的 Intel CPU 更倾向于使用量化数据类型](https://www.intel.com/content/www/us/en/developer/articles/technical/accelerate-pytorch-int8-inf-with-new-x86-backend.html) `[INT8](https://www.intel.com/content/www/us/en/developer/articles/technical/accelerate-pytorch-int8-inf-with-new-x86-backend.html)`[。](https://www.intel.com/content/www/us/en/developer/articles/technical/accelerate-pytorch-int8-inf-with-new-x86-backend.html)

*提示：要显示有关 CPU 架构的信息，请运行* `[*lscpu*](https://man7.org/linux/man-pages/man1/lscpu.1.html)` *命令，然后检查制造商的主页，以查看特定 CPU 偏好的量化输入数据类型。*

关于后训练量化的详细解释以及 ONNX Runtime 和 OpenVINO 的比较，我推荐这篇文章：

[## OpenVINO 与 ONNX 在生产中的 Transformers](https://blog.ml6.eu/openvino-vs-onnx-for-transformers-in-production-3e10c01520c8?source=post_page-----6820ac7349bb--------------------------------)

### Transformers 已经彻底改变了自然语言处理（NLP），成为机器翻译、语义理解等应用的首选。

[博客](https://blog.ml6.eu/openvino-vs-onnx-for-transformers-in-production-3e10c01520c8?source=post_page-----6820ac7349bb--------------------------------)

本节将特别关注两种流行的训练后量化技术：

+   ONNX Runtime

+   OpenVINO

## ONNX Runtime

加速 CPU 推理的一个流行方法是将最终模型转换为 [ONNX](https://onnx.ai/)（开放神经网络交换）格式 [2, 7, 9, 10, 14, 15]。

使用 ONNX Runtime 量化和加速 CPU 推理的相关步骤如下：

**准备：** 安装 ONNX Runtime

```py
pip install onnxruntime
```

**第 1 步：** 将 PyTorch 模型转换为 ONNX

```py
import torch
import torchvision

# Define your model here
model = ...

# Train model here
...

# Define dummy_input
dummy_input = torch.randn(1, N_CHANNELS, IMG_WIDTH, IMG_HEIGHT, device="cuda")

# Export PyTorch model to ONNX format
torch.onnx.export(model, dummy_input, "model.onnx")
```

**第 2 步：** 使用 ONNX Runtime 会话进行预测

```py
import onnxruntime as rt

# Define X_test with shape (BATCH_SIZE, N_CHANNELS, IMG_WIDTH, IMG_HEIGHT)
X_test = ...

# Define ONNX Runtime session
sess = rt.InferenceSession("model.onnx")

# Make prediction
y_pred = sess.run([], {'input' : X_test})[0]
```

## OpenVINO

另一个同样流行的加速 CPU 推理的方法是使用 OpenVINO（开放视觉推理和神经网络优化）[5, 6, 12]，如 [这个 Kaggle Notebook](https://www.kaggle.com/code/honglihang/openvino-is-all-you-need) 所示：

[## openvino 是你所需要的一切](https://www.kaggle.com/code/honglihang/openvino-is-all-you-need?source=post_page-----6820ac7349bb--------------------------------)

### 使用 Kaggle Notebooks 探索和运行机器学习代码 | 使用来自多个数据源的数据

[www.kaggle.com](https://www.kaggle.com/code/honglihang/openvino-is-all-you-need?source=post_page-----6820ac7349bb--------------------------------)

使用 OpenVINO 量化和加速深度学习模型的相关步骤如下：

**准备：** 安装 OpenVINO

```py
!pip install openvino-dev[onnx]
```

**第 1 步：** 将 PyTorch 模型转换为 ONNX（见 ONNX Runtime 的第 1 步）

**第 2 步：** 将 ONNX 模型转换为 OpenVINO

```py
mo --input_model model.onnx
```

这将输出一个 XML 文件和一个 BIN 文件——我们将在下一步中使用 XML 文件。

**第 3 步：** 使用 OpenVINO 量化为 `INT8`

```py
import openvino.runtime as ov

core = ov.Core()
openvino_model = core.read_model(model='model.xml')
compiled_model = core.compile_model(openvino_model, device_name="CPU")
```

**第 4 步：** 使用 OpenVINO 推理请求进行预测

```py
# Define X_test with shape (BATCH_SIZE, N_CHANNELS, IMG_WIDTH, IMG_HEIGHT)
X_test = ...

# Create inference request
infer_request = compiled_model.create_infer_request()

# Make prediction
y_pred = infer_request.infer(inputs=[X_test, 2])
```

## 比较：ONNX 与 OpenVINO 与其他替代方案

ONNX 和 OpenVINO 都是优化用于在 CPU 上部署模型的框架。[量化的神经网络在 ONNX 和 OpenVINO 上的推理时间被认为是相当的](https://blog.ml6.eu/openvino-vs-onnx-for-transformers-in-production-3e10c01520c8) [12]。

一些竞争对手使用 PyTorch JIT [3] 或 TorchScript [1] 作为加速 CPU 推理的替代方案。然而，其他竞争对手表示 ONNX 比 TorchScript [10] 快得多。

# 使用 ThreadPoolExecutor 进行多线程

另一种加快 CPU 推断的流行方法是使用多线程和[ThreadPoolExecutor](https://docs.python.org/3/library/concurrent.futures.html) [2, 3, 9, 15]，此外还包括后训练量化，如[这个 Kaggle 笔记本](https://www.kaggle.com/code/leonshangguan/faster-eb0-sed-model-inference)所示：

[](https://www.kaggle.com/code/leonshangguan/faster-eb0-sed-model-inference?source=post_page-----6820ac7349bb--------------------------------) [## Faster eb0_SED 模型推断

### 在 Kaggle 笔记本中探索和运行机器学习代码 | 使用来自多个数据源的数据

www.kaggle.com](https://www.kaggle.com/code/leonshangguan/faster-eb0-sed-model-inference?source=post_page-----6820ac7349bb--------------------------------)

这使得参赛者能够同时运行多个推断。

在比赛中的[ThreadPoolExecutor](https://docs.python.org/3/library/concurrent.futures.html)示例中，我们有一个音频文件列表需要推断。

```py
audios = ['audio_1.ogg', 
          'audio_2.ogg', 
          # ...,  
          'audio_n.ogg',]
```

接下来，你需要定义一个推断函数，该函数以音频文件作为输入并返回预测结果。

```py
def predict(audio_path):
    # Define any preprocessing of the audio file here
    ...

    # Make predictions
    ...

    return predictions
```

有了音频列表（例如`audios`）和推断函数（例如`predict()`），你现在可以使用[ThreadPoolExecutor](https://docs.python.org/3/library/concurrent.futures.html)来并行运行多个推断，这将显著提高推断速度。

```py
import concurrent.futures

with concurrent.futures.ThreadPoolExecutor(max_workers=4) as executor:
    dicts = list(executor.map(predict, audios))
```

# 总结

从审查 Kagglers 在[“BirdCLEF 2023](https://www.kaggle.com/competitions/birdclef-2023/)”比赛过程中创建的学习资源中，可以学习到许多更多的课程。对于这种类型的问题声明，还有许多不同的解决方案。

在本文中，我们关注了许多参赛者所采用的通用方法：

+   模型选择：根据性能和推断时间之间的最佳权衡选择模型大小。同时，在你的集成中利用更大和更小的模型。

+   后训练量化：后训练量化可以通过将模型权重和激活的 datatype 优化为硬件来加快推断时间。然而，这可能会导致模型性能的轻微下降。

+   多线程：并行运行多个推断，而不是按顺序进行。这将提高你的推断时间。

如果你对如何使用深度学习进行音频分类感兴趣，这是本次比赛的主要内容，请查看[BirdCLEF 2022](https://www.kaggle.com/competitions/birdclef-2022/)比赛的总结：

[](/audio-classification-with-deep-learning-in-python-cf752b22ba07?source=post_page-----6820ac7349bb--------------------------------) ## 使用深度学习进行 Python 中的音频分类

### 使用 PyTorch 和 torchaudio 在音频数据中调整图像模型以应对领域偏移和类别不平衡

[towardsdatascience.com

# 喜欢这个故事吗？

[*免费订阅*](https://medium.com/subscribe/@iamleonie) *以便在我发布新故事时收到通知。*

[`medium.com/@iamleonie/subscribe?source=post_page-----6820ac7349bb--------------------------------`](https://medium.com/@iamleonie/subscribe?source=post_page-----6820ac7349bb--------------------------------) [## 每当 Leonie Monigatti 发布新内容时，会收到电子邮件通知。]

### 每当 Leonie Monigatti 发布新内容时，会收到电子邮件通知。通过注册，如果你尚未拥有 Medium 帐户，将创建一个。

[medium.com](https://medium.com/@iamleonie/subscribe?source=post_page-----6820ac7349bb--------------------------------)

*在* [*LinkedIn*](https://www.linkedin.com/in/804250ab/)、[*Twitter*](https://twitter.com/helloiamleonie) *和* [*Kaggle*](https://www.kaggle.com/iamleonie) *上找到我！*

# 参考文献

## 图像参考

除非另有说明，所有图像均由作者创作。

## 网络与文献

[1] adsr (2023). [第 3 名解决方案：在 Mel 频带上的 SED 和注意力](https://www.kaggle.com/competitions/birdclef-2023/discussion/414102) 在 Kaggle 讨论中（访问日期：2023 年 6 月 1 日）

[2] anonamename (2023). [第 6 名解决方案：BirdNET 嵌入 + CNN](https://www.kaggle.com/competitions/birdclef-2023/discussion/412708) 在 Kaggle 讨论中（访问日期：2023 年 6 月 1 日）

[3] atfujita (2023). [第 4 名解决方案：知识蒸馏是你所需要的一切](https://www.kaggle.com/competitions/birdclef-2023/discussion/412753) 在 Kaggle 讨论中（访问日期：2023 年 6 月 1 日）

[4] [beluga](https://www.kaggle.com/gaborfodor) (2023). [推理约束 — CPU 笔记本 <= 120 分钟](https://www.kaggle.com/competitions/birdclef-2023/discussion/393059)（访问日期：2023 年 3 月 27 日）。

[5] Harshit Sheoran (2023). [第 9 名解决方案：7 个 CNN 模型组合](https://www.kaggle.com/competitions/birdclef-2023/discussion/412794) 在 Kaggle 讨论中（访问日期：2023 年 6 月 1 日）

[6] HONG LIHANG (2023). [第二名解决方案：SED + CNN 结合 7 模型](https://www.kaggle.com/competitions/birdclef-2023/discussion/412707) 在 Kaggle 讨论中（访问日期：2023 年 6 月 1 日）

[7] HyeongChan Kim (2023). [第 24 名解决方案 — 预训练 & 单模型（5 折组合与 ONNX）](https://www.kaggle.com/competitions/birdclef-2023/discussion/412996) 在 Kaggle 讨论中（访问日期：2023 年 6 月 1 日）

[8] LeonShangguan (2022). [[公开 #1 私密 #2] + [私密 #7/8（潜在）] 解决方案。主持人获胜。](https://www.kaggle.com/competitions/birdclef-2022/discussion/326950) 在 Kaggle 讨论中（访问日期：2023 年 3 月 13 日）

[9] LeonShangguan (2023). [第 10 名解决方案](https://www.kaggle.com/competitions/birdclef-2023/discussion/412713) 在 Kaggle 讨论中（访问日期：2023 年 6 月 1 日）

[10] moritake04 (2023). [第 20 名解决方案：使用 onnx 的 SED + CNN 组合](https://www.kaggle.com/competitions/birdclef-2023/discussion/412742) 在 Kaggle 讨论中（访问日期：2023 年 6 月 1 日）

[11] slime (2022). [第 3 名解决方案](https://www.kaggle.com/competitions/birdclef-2022/discussion/327193) 在 Kaggle 讨论区（访问日期：2023 年 3 月 13 日）

[12] storm (2023). [第 7 名解决方案 — `sumix` 增强做了所有的工作](https://www.kaggle.com/competitions/birdclef-2023/discussion/412922) 在 Kaggle 讨论区（访问日期：2023 年 6 月 1 日）

[13] Volodymyr (2022). [第 1 名解决方案模型（这不是全部的 BirdNet）](https://www.kaggle.com/competitions/birdclef-2022/discussion/327047) 在 Kaggle 讨论区（访问日期：2023 年 3 月 13 日）

[14] Volodymyr (2023). [第 1 名解决方案：正确的数据就是你所需要的一切](https://www.kaggle.com/competitions/birdclef-2023/discussion/412808) 在 Kaggle 讨论区（访问日期：2023 年 6 月 1 日）

[15] Yevhenii Maslov (2023). [第 5 名解决方案](https://www.kaggle.com/competitions/birdclef-2023/discussion/412903) 在 Kaggle 讨论区（访问日期：2023 年 6 月 1 日）

[16] yokuyama (2022). [第 5 名解决方案](https://www.kaggle.com/competitions/birdclef-2022/discussion/327044) 在 Kaggle 讨论区（访问日期：2023 年 3 月 13 日）
