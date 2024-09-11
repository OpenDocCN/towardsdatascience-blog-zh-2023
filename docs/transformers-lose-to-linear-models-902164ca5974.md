# Transformers 是否输给了线性模型？

> 原文：[https://towardsdatascience.com/transformers-lose-to-linear-models-902164ca5974?source=collection_archive---------11-----------------------#2023-01-23](https://towardsdatascience.com/transformers-lose-to-linear-models-902164ca5974?source=collection_archive---------11-----------------------#2023-01-23)

![](../Images/8d84a209dff6be6c22d99da7e5020a3b.png)

照片由 [Nicholas Cappello](https://unsplash.com/@bash__profile?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 使用 Transformers 进行长期预测可能并非最佳选择

[](https://medium.com/@upadhyan?source=post_page-----902164ca5974--------------------------------)[![Nakul Upadhya](../Images/336cb21272e9b1f098177adbde50e92e.png)](https://medium.com/@upadhyan?source=post_page-----902164ca5974--------------------------------)[](https://towardsdatascience.com/?source=post_page-----902164ca5974--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----902164ca5974--------------------------------) [Nakul Upadhya](https://medium.com/@upadhyan?source=post_page-----902164ca5974--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4d9dddc62a80&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftransformers-lose-to-linear-models-902164ca5974&user=Nakul+Upadhya&userId=4d9dddc62a80&source=post_page-4d9dddc62a80----902164ca5974---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----902164ca5974--------------------------------) · 9分钟阅读 · 2023年1月23日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F902164ca5974&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftransformers-lose-to-linear-models-902164ca5974&user=Nakul+Upadhya&userId=4d9dddc62a80&source=-----902164ca5974---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F902164ca5974&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftransformers-lose-to-linear-models-902164ca5974&source=-----902164ca5974---------------------bookmark_footer-----------)

近年来，基于 Transformer 的解决方案获得了极大的关注。随着 BERT、GTP 以及其他语言 Transformer 的成功，研究人员开始将这种架构应用于其他序列建模问题，特别是在时间序列预测领域（也称为长期时间序列预测或 LTSF）。注意力机制似乎是一种提取长序列中某些长期相关性的完美方法。

但是，最近来自香港中文大学和国际数字经济活动的研究人员决定质疑：[变形金刚是否适用于时间序列预测](https://arxiv.org/abs/2205.13504) [1]? **他们表明，自注意机制（即使加入了位置编码）可能会导致时间信息丢失。他们随后用一组单层线性模型验证了这一说法，在几乎每个实验中都优于变压器基准。**

简而言之，*变压器可能并不是预测问题的最理想架构*。

在本文中，我旨在总结曾等人[1]的发现和实验，得出这一结论，并讨论该工作的一些潜在影响。作者开发的所有实验和模型也可以在他们的[GitHub仓库](https://github.com/cure-lab/LTSF-Linear)中找到。此外，我强烈建议每个人都阅读[原始论文](https://arxiv.org/abs/2205.13504)。

## 模型和数据

在他们的工作中，作者在[Electricity Transformer Dataset (ETDataset)](https://github.com/zhouhaoyi/ETDataset) [3]上评估了5种不同的SOTA变压器模型。这些模型及其主要特点如下：

1.  **LogTrans** [2]:提出了卷积自注意力，以便更好地将本地上下文纳入到注意力机制中。该模型还在注意力方案中编码了一种稀疏偏差。这有助于提高内存复杂性。

1.  **Informer** [3]:通过提出一种新的架构和一种直接多步（DMS）预测策略，解决了由自回归解码器引起的内存/时间复杂性和误差复杂性问题。

1.  **Autoformer** [4]: 在每个神经块后面应用季节趋势分解以提取趋势周期性成分。此外，Autoformer设计了一种系列式自动相关机制，以取代常规的自注意力。

1.  **Pyraformer** [5]:实现了一种新颖的金字塔式注意力机制，捕捉分层多尺度时间依赖关系。像LogTrans一样，该模型还在注意力方案中明确编码了一种稀疏偏差。

1.  **FEDFormer** [6]:通过将季节趋势分解方法合并到体系结构中，有效地开发了*F*requency-*E*nhanced *D*ecomposed *T*ransformer，增强了传统的变压器架构。

这些模型都对变压器架构的各个部分进行了各种改动，以解决传统变压器存在的各种不同问题（完整总结可在图1中找到）。

![](../Images/2761a09135aa7a0cbefbdbd88d36824a.png)

图1：现有基于变压器的时间序列预测解决方案的流程图（由曾等人制作的图1）

为了与这些变压器模型竞争，作者提出了一些“令人尴尬地简单”的模型[1]来进行DMS预测。

![](../Images/ddc1833719a2931a311d6cda91bec641.png)

图2：基本DMS线性模型的示意图（图由Zeng等人[1]提供）

这些模型及其属性如下：

1.  **分解线性（D-Linear）：** D-Linear使用分解方案将原始数据拆分为趋势和季节性组件。然后对每个组件应用两个单层线性网络，并将输出求和以获得最终预测。

1.  **归一化线性（N-Linear）：** N-Linear首先从输入中减去序列的最后一个值。然后将输入传递到一个线性层中，最后将减去的部分加回，以进行最终预测。这有助于解决数据中的分布偏移。

1.  **重复**：只需重复回顾窗口中的最后一个值。

这些是一些非常简单的基准线。线性模型都涉及少量的数据预处理和一个单层网络。重复是一个琐碎的基准。

实验是在各种广泛使用的数据集上进行的，如[Electricity Transformer](https://github.com/zhouhaoyi/ETDataset) (ETDataset)[3]、交通、电力、天气、ILI和汇率[7]数据集。

## 实验

在上述8个模型上，作者进行了一系列实验，以评估模型的性能并确定每个模型中各种组件对最终预测的影响。

第一个实验很简单：每个模型都经过训练并用于预测数据。回顾期也有所变化。完整的测试结果见表1，但总的来说，**FEDFormer [6]在大多数情况下是表现最好的*transformer*，但从未是*总体*最佳的**。

![](../Images/7273ac9b84f4ae18e948f460a40b82ce.png)

表1：实验误差。最佳Transformer结果下划线标记，最佳总体结果加粗。（图由Zeng等人[1]提供）

这种Transformer令人尴尬的表现可以在图3中对电力、汇率和ETDataset的预测中看到。

![](../Images/2608bdfdad62c6fae76d68c87a33535e.png)

图3：输入长度=96，输出长度=192的LTSF输出（图由Zeng等人[1]提供）。

引用作者的话：

> Transformers [28, 30, 31]未能捕捉到电力和ETTh2上未来数据的规模和偏差。此外，它们几乎无法预测诸如汇率等非周期性数据的适当趋势。这些现象进一步表明现有的Transformer解决方案在LTSF任务中的不足。

然而，许多人会认为这对变换器不公平，因为注意机制通常擅长保存长期信息，所以变换器应该在更长的输入序列中表现更好，作者在他们的下一个实验中测试了这个假设。他们将回溯期在24到720个时间步之间变化，并评估均方误差。作者发现，在许多情况下，**变换器的性能没有提高，实际上几个模型的误差还增加了（完整结果见图4）。相比之下，线性模型的性能在加入更多时间步后显著提高**。

![](../Images/8290012dc9797f84a645851454fad722.png)

图4：回溯期长度变化的均方误差结果（图由Zeng等人[1]制作）

然而，仍然有其他因素需要考虑。由于变换器的复杂性，它们通常需要比其他模型更大的训练数据集才能表现良好，因此，作者决定测试训练数据集大小是否是这些变换器架构的限制因素。他们利用了交通数据[7]，并在原始数据集和截断数据集上训练了Autoformer [4]和FEDformer [6]，期望较小的训练集会导致误差更高。**令人惊讶的是，在较小训练集上训练的模型表现稍微更好。这并不意味着应该使用较小的训练集，但这确实意味着数据集大小不是LTSF变换器的限制因素。**

除了改变训练数据大小和回溯期长度外，作者还尝试了不同的回溯窗口起始时间。例如，如果他们试图对*t*=196之后的时期进行预测，而不是使用*t*=100, 101,…, 196（邻近或“接近”窗口），作者尝试了使用*t*=4, 5,…, 100（“远离”窗口）。这个想法是，预测应该依赖于模型是否能够很好地捕捉趋势和周期性，视野越远，预测效果应该越差。**作者发现变换器在“接近”窗口和“远离”窗口之间的性能仅略有下降。**这意味着变换器可能会对提供的数据过拟合，这可能解释了为什么线性模型表现更好。

在评估了各种变换器模型后，作者们还专门探讨了这些模型中自注意力和嵌入策略的有效性。他们的第一个实验涉及拆解现有的变换器，以分析变换器复杂设计是否必要。他们将注意力层拆解为简单的线性层，然后去除了除嵌入机制之外的辅助部分，最后将变换器简化为仅有的线性层。在每一步，他们记录了使用不同回溯期大小的均方误差，并发现**随着逐步简化，变换器的性能有所提升。**

作者们还想研究变换器在保持时间顺序方面的影响。他们假设由于自注意力是排列不变的（忽略顺序），而时间序列对排列敏感，因此位置编码和自注意力不足以捕获时间信息。为了测试这一点，作者通过打乱数据和交换输入序列的前半部分与后半部分来修改序列。模型捕获的时间信息越多，修改数据集时模型性能的下降应该越大。**作者观察到，线性模型的性能下降比任何变换器模型都要大，这表明变换器捕获的时间信息少于线性模型。完整结果见下表**

![](../Images/bb699ce143b7bfdf66afae2bbff1477a.png)

表 2:（图由 Zeng 等人 [1] 制作）

为了进一步探讨变换器的信息捕获能力，作者们通过去除变换器的位置信息和时间编码来检验不同编码策略的有效性。这些结果因模型而异。对于 FEDFormer [6] 和 Autoformer [4]，去除位置编码在大多数回溯窗口大小上提高了交通数据集的性能。然而，Informer [3] 在具有所有位置编码时表现最好。

## 讨论与结论

理解这些结果时需要注意几点。变换器对超参数非常敏感，通常需要大量的调优才能有效地建模问题。然而，作者在实现这些模型时没有进行任何超参数搜索，而是选择了模型实现中使用的默认参数。有观点认为，他们也没有调整线性模型，因此比较是公平的。此外，调整线性模型所需的时间远远少于训练变换器，因为线性模型的简单性。尽管如此，可能会出现变换器在正确的超参数下表现极其出色的情况，而成本和时间可以忽略，以提高准确性。

尽管有这些批评，作者进行的实验详细地分解了变换器的缺陷。这些模型庞大且非常复杂，容易在时间序列数据上过拟合。虽然它们在语言处理和其他任务中表现良好，但自注意力的置换不变特性确实会导致显著的时间损失。此外，相比于复杂的变换器架构，线性模型在解释性和可解释性方面极其优越。如果对这些长期序列预测（LTSF）变换器的组件进行一些修改，我们可能会看到它们最终超越简单的线性模型或解决线性模型难以建模的问题（例如变点识别）。然而，与此同时，数据科学家和决策者不应盲目地将变换器应用于时间序列预测问题，除非有非常充分的理由来利用这种架构。

## 资源和参考文献

[1] A. Zeng, M. Chen, L. Zhang, Q. Xu. [变换器对时间序列预测有效吗？](https://arxiv.org/abs/2205.13504) (2022). 第三十七届AAAI人工智能会议。

[2] S. Li, X. Jin, Y. Xuan, X. Zhou, W. Chen, Y. Wang, X. Yan. [提升变换器在时间序列预测中的局部性并打破记忆瓶颈](https://proceedings.neurips.cc/paper/2019/hash/6775a0635c302542da2c32aa19d86be0-Abstract.html) (2019). 神经信息处理系统进展 32。

[3] H. Zhou, S. Zhang, J. Peng, S. Zhang, J. Li, H. Xiong, W. Zhang. [Informer: 超越高效变换器的长序列时间序列预测](https://arxiv.org/abs/2012.07436) (2021). 第三十五届AAAI人工智能会议，虚拟会议。

[4] H. Wu, J. Xu, J. Wang, M. Long. [Autoformer: 用于长期序列预测的自相关分解变换器](https://arxiv.org/abs/2106.13008) (2021). 神经信息处理系统进展 34。

[5] S. Liu, H. Yu, C. Liao, J. Li, W. Lin, A.X. Liu, S. Dustdar. [Pyraformer: 低复杂度金字塔注意力用于长时间序列建模和预测](https://openreview.net/forum?id=0EXmFzUn5I) (2021). 国际学习表示会议 2021。

[6] T. Zhou, Z. Ma, Q. Wen, X. Wang, L. Sun, R. Jin. [EDformer: 用于长期序列预测的频率增强分解变换器](https://arxiv.org/abs/2201.12740) (2022). 第39届国际机器学习会议。

[7] G. Lai, W-C. Chang, Y. Yang, 和 H. Liu. [使用深度神经网络建模长短期时间模式](https://arxiv.org/abs/1703.07015) (2017). 第41届国际ACM SIGIR信息检索研究与发展会议。
