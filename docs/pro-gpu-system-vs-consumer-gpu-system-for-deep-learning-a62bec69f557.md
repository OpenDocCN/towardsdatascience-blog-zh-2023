# 专业级 GPU 系统 vs 消费级 GPU 系统用于深度学习

> 原文：[`towardsdatascience.com/pro-gpu-system-vs-consumer-gpu-system-for-deep-learning-a62bec69f557`](https://towardsdatascience.com/pro-gpu-system-vs-consumer-gpu-system-for-deep-learning-a62bec69f557)

## 硬件

## 为什么你可能会考虑使用专业级 GPU

[](https://medium.com/@maclayton?source=post_page-----a62bec69f557--------------------------------)![Mike Clayton](https://medium.com/@maclayton?source=post_page-----a62bec69f557--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a62bec69f557--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a62bec69f557--------------------------------) [Mike Clayton](https://medium.com/@maclayton?source=post_page-----a62bec69f557--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a62bec69f557--------------------------------) ·阅读时间 21 分钟·2023 年 4 月 19 日

--

![](img/f24ad59210450dc7f7fab7e6456b0d01.png)

*本文将使用的专业工作站。图片来源于* [*Exxact Corporation*](https://www.exxactcorp.com/category/Deep-Learning-Solutions?page=1&utm_source=web+referral&utm_medium=backlink&utm_campaign=Michael+Clayton&utm_term=Medium+Towards+Data+Science) *，授权给 Michael Clayton*

**在训练神经网络，特别是深度神经网络时，系统中拥有一个 GPU（或显卡）几乎是必需的。与 CPU 相比，即使是相当普通的 GPU，其训练速度的差异也是天壤之别。**

**……但你什么时候可能会考虑跳入专业级而非消费级 GPU 的领域？训练和推理速度有很大的差别吗？还是说其他因素使得转向专业级 GPU 更具吸引力？**

# 介绍

本文的目的是让你了解作为普通消费者（或刚开始从事机器学习/深度学习）的 GPU 与高端系统中使用的 GPU 之间的主要区别。这些系统可能用于开发和/或推理高级深度学习模型。

除了作为一个有趣的练习来理解前沿专业设备和消费级硬件在纯处理速度上的区别外，它还将突出一些消费级 GPU 及其相关系统在处理前沿深度学习模型时存在的其他限制。

# 你提到的“专业”或“消费级”GPU 指的是哪些 GPU？

“现实世界”的差异将在文章的其余部分中讨论，但如果你想要一个明确的技术区分，包括示例显卡和规格，那么这一部分应该能涵盖。

正如我在之前的文章中详细说明的那样，NVIDIA 是目前深度学习和神经网络 GPU 的唯一明智选择。这主要是由于它们与 TensorFlow 和 PyTorch 等平台的更全面集成。

因此，在制造商的规格方面区分专业级和消费级显卡是相对直接的。

从以下页面中了解当前的 NVIDIA 消费级显卡批次：

[](https://www.nvidia.com/en-gb/geforce/graphics-cards/?source=post_page-----a62bec69f557--------------------------------) [## NVIDIA GeForce 显卡

### 探索 NVIDIA GeForce 显卡。RTX 40 系列、RTX 30 系列、RTX 20 系列和 GTX 16 系列。

[www.nvidia.com](https://www.nvidia.com/en-gb/geforce/graphics-cards/?source=post_page-----a62bec69f557--------------------------------)

…以及专业级 GPU：

[](https://www.nvidia.com/en-us/design-visualization/desktop-graphics/?source=post_page-----a62bec69f557--------------------------------) [## NVIDIA RTX & Quadro 台式工作站

### 了解 3D 艺术家、建筑师和产品设计师如何利用 NVIDIA RTX ™ 和 Omniverse ™ 的强大功能…

[www.nvidia.com](https://www.nvidia.com/en-us/design-visualization/desktop-graphics/?source=post_page-----a62bec69f557--------------------------------)

还有一些 GPU，主要用于数据中心，超越了上述的专业级显卡。A100 是一个很好的例子：

[](https://www.nvidia.com/en-us/data-center/a100/?source=post_page-----a62bec69f557--------------------------------) [## NVIDIA A100 GPU 驱动现代数据中心

### NVIDIA EGX ™ 平台包括优化的软件，提供基础设施上的加速计算…

[www.nvidia.com](https://www.nvidia.com/en-us/data-center/a100/?source=post_page-----a62bec69f557--------------------------------)

你还可以了解 NVIDIA 认证的专业数据中心和工作站系统中使用的系统规格和 GPU：

[## NVIDIA 认证系统

### NVIDIA 认证系统程序汇集了业界最完整的加速工作负载性能集合…

[docs.nvidia.com](https://docs.nvidia.com/ngc/ngc-deploy-on-premises/nvidia-certified-systems/index.html?source=post_page-----a62bec69f557--------------------------------)

# 计划

我通常认为实际演示（或实验）是说明一个观点的最佳方式，而不仅仅依赖于制造商提供的规格和统计数据。

有鉴于此，虽然文章将讨论相关统计数据，但它还将直接比较三种不同的 GPU（专业级和消费级），在相同的深度学习模型上进行不同层次的比较。

这应该有助于突出在考虑是否需要专业级 GPU 时哪些因素重要，哪些因素不重要。

# GPU 规格 — 概述

![](img/652e61b5741fc1ee64f48d026de6b88b.png)

图片由 [Ann H](https://www.pexels.com/photo/sand-sign-texture-writing-11022641/) 提供，发布在 [Pexels](https://www.pexels.com/)

对于这项实验，将会有三种不同的显卡，但有四个比较级别：

1.  NVIDIA RTX 1070（基础）

1.  NVIDIA Tesla T4（中端）

1.  NVIDIA RTX 6000 Ada（高端）

1.  2 x NVIDIA RTX 6000 Ada（双高端！）

那么，这些不同的显卡在原始规格方面如何比较？

![](img/31ffd6303b1c9dc911b2332516c9b24a.png)

不同显卡的比较 — 表格由作者提供

***注意：*** *我在上面的表格中包含了 RTX 4090，因为它是当前消费级显卡的巅峰，可能是与 RTX 6000 Ada 最直接的比较对象。我将在整篇文章中参考 4090 作为比较点，尽管它不会出现在基准测试中。*

如果上面的表格只是一些没有意义的数字，那么我推荐我的上一篇文章，它介绍了一些术语：

[](/how-to-pick-the-best-graphics-card-for-machine-learning-32ce9679e23b?source=post_page-----a62bec69f557--------------------------------) ## 如何选择最佳的机器学习显卡

### 加快训练速度，迭代更快

towardsdatascience.com

# 专业系统

![](img/0df47511780765ce613b7afceb97a5cd.png)

*从各个角度展示了本文使用的专业工作站。图片由* [*Exxact Corporation*](https://www.exxactcorp.com/category/Deep-Learning-Solutions?page=1&utm_source=web+referral&utm_medium=backlink&utm_campaign=Michael+Clayton&utm_term=Medium+Towards+Data+Science) *提供，授权给 Michael Clayton*

制作这样的文章的一个问题是，你需要访问专业级系统，因此主要障碍之一就是……成本。

幸运的是，有些公司会提供设备试用机会，以便你可以查看它是否满足你的需求。在这个特定的案例中，[Exxact](https://www.exxactcorp.com/) 足够友好地允许远程访问他们的一台设备，以便我可以进行所需的比较。

> …工作站的价值大约为 25,000 美元

为了强调这些系统可能的成本，我估计我所获得访问权限的工作站价值大约为**25,000 美元**。如果你想更深刻（或者更惊讶？）了解这些内容，可以查看 [**配置器**](https://www.exxactcorp.com/VWS-148320247/configurator)，看看实际能达到什么水平。

顺便提一下，如果您正在认真考虑这一水平的硬件，您也可以申请远程“试用”：

[## NVIDIA H100 试用 | Exxact

### 使用最新 NVIDIA 技术试用您的应用程序，注册以远程访问我们设备齐全的 GPU 服务器……

[www.exxactcorp.com](https://www.exxactcorp.com/Services/Test-Drive?utm_source=web+referral&utm_medium=backlink&utm_campaign=Michael+Clayton&utm_term=Test+Drive&source=post_page-----a62bec69f557--------------------------------)

对于感兴趣的人，这些是“专业”系统的完整规格：

![](img/0ad06958c07074b8fa0fb9888eab08b5.png)

专业系统的规格 — 作者提供的表格

***注意：*** *您可以参考本文中包含两张金色 GPU 的黑色计算机机箱的任何图片，因为这些是上述系统的实际照片。*

有趣的是，拥有高端系统不仅仅是将您能找到的最佳显卡装入当前系统中。其他组件也需要升级。系统 RAM、主板、CPU、冷却系统以及电源，当然还有电力。

# 竞争者

![](img/afed16c91fe7b379c2ddad574b3321ed.png)

本文中将用作基准的 NVIDIA GeForce GTX 1070 FTW — 作者提供的图片

## NVIDIA GeForce GTX 1070

在底部的是 GTX 1070，这对于大多数人来说很容易获得，但仍然***显著*** 比 CPU 更快。它还具有 8GB 的相当数量的 GPU RAM。一个不错的简单消费级基准。

## NVIDIA Tesla T4

Tesla T4 可能是一个奇怪的补充，但有几个原因。

首先要注意的是，Tesla T4 实际上是一款专业显卡，只是几代之前的产品。

从处理速度来看，它大致相当于 RTX 2070，但其 GPU RAM 是 16GB 的双倍。这额外的 RAM 使其在此测试中处于中端范围。目前一代的消费级显卡通常有这种范围的 RAM（RTX 4070 [12GB] 和 RTX 4080 [16GB]），因此它在 GPU RAM 方面代表了消费级显卡。

最终原因是您可以在 [Colab](https://colab.research.google.com/) 免费访问这些显卡。这意味着任何阅读本文的人都可以亲自动手运行代码以查看效果！

# 专业 GPU — NVIDIA RTX 6000 Ada

![](img/17dfd95d7bee19fde31b3b39dd1f0fb1.png)

*这张图片展示了在本文中使用的专业工作站中安装的两张 NVIDIA RTX 6000 Ada 显卡。图片来源于* [*Exxact Corporation*](https://www.exxactcorp.com/category/Deep-Learning-Solutions?page=1&utm_source=web+referral&utm_medium=backlink&utm_campaign=Michael+Clayton&utm_term=Medium+Towards+Data+Science) *，由 Michael Clayton 授权*

毋庸置疑，RTX 6000 Ada 无论在规格还是价格上都是一款令人印象深刻的显卡。**MSRP 为 6800 美元**，绝对不是便宜的显卡。那么，如果你可以用仅仅 1599 美元（MSRP）购买 RTX 4090，为什么还要买一张（或者更多！？）RTX 6000 Ada 呢？

> RTX 4090 的 RAM 只有 RTX 6000 Ada 的一半，且功耗比 RTX 6000 Ada 高出 50%

我在桌子上放了一张 RTX 4090 来尝试回答这个问题。这有助于展示消费者显卡和专业显卡之间最明显的两个区别（至少从规格上来看）：

1.  可用的 GPU RAM 数量

1.  使用中的最大功耗

RTX 4090 的 **RAM 只有一半**，且 **功耗高出 50%** 比 RTX 6000 Ada。这绝非偶然，随着文章的深入将会显现出原因。

此外，考虑到 RTX 4090 更高的功耗，值得注意的是 RTX 6000 Ada 的速度仍然快了大约**10%**。

这些额外的 RAM 和降低的功耗真的有区别吗？希望比较能在文章后面帮助回答这个问题。

## 还有其他不太明显的优点吗？

是的，获得专业级显卡确实有一些额外的好处。

***可靠性***

![](img/db2c37140746242f9b9fda939760a2d1.png)

图片来源于 [WikiImages](https://pixabay.com/users/wikiimages-1897/?utm_source=link-attribution&amp%3Butm_medium=referral&amp%3Butm_campaign=image&amp%3Butm_content=62827)，来自 [Pixabay](https://pixabay.com//?utm_source=link-attribution&amp%3Butm_medium=referral&amp%3Butm_campaign=image&amp%3Butm_content=62827)

> NVIDIA RTX 专业显卡经过广泛的专业应用认证，经过领先的独立软件供应商（ISVs）和工作站制造商的测试，并由全球支持专家团队提供支持。
> 
> -[nvidia.com](https://www.nvidia.com/content/dam/en-zz/Solutions/design-visualization/rtx-6000/proviz-print-rtx6000-datasheet-web-2504660.pdf)

从本质上讲，这意味着显卡在软件（驱动程序）和硬件层面上可能更可靠、更抗崩溃，如果遇到问题，还有一个广泛的专业网络可以解决问题。这些因素在企业应用中显然非常重要，因为时间就是金钱。

想象一下运行一个复杂的深度学习模型几天，然后由于崩溃或错误丢失结果。接着还需要花费更多的时间来解决问题。真是不妙！

这种安心感是否成为额外的理由来支付更多的费用？这真的取决于你的优先级和规模...

***规模***

![](img/d1bde7e0264bca185dd044513861d617.png)

照片由 [Daniele Levis Pelusi](https://unsplash.com/@yogidan2012?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/4mpsEm3EGak?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上提供

如果你在设计一个具有最佳 GPU 处理能力的计算机系统，那么可能需要多个 GPU。显然，系统中可容纳的 GPU 数量主要取决于主板插槽的数量以及机箱内的物理空间限制。

然而，还有其他直接与 GPU 相关的限制因素，这也是消费级 GPU 和专业 GPU 在设计上开始偏离的地方。

考虑到专业主板可能有四个双槽 GPU 的插槽（如本文中的专业系统）。理论上，你可以将 4 x RTX 6000 Ada GPU 装入系统中没有任何问题。然而，你只能在同一主板上装入 2 x RTX 4090。为什么？因为 4090 是三槽显卡（约 61mm 厚），而 6000 是双槽显卡（约 40mm 厚）。

消费级 GPU 的设计并没有考虑到相同的限制（即高密度构建），因此随着规模的扩大，它们的实用性开始下降。

***冷却***

接着讨论可能的尺寸问题……即使消费级显卡采用相同的双槽设计，还有其他问题。

专业级 GPU 通常配备有冷却系统（吹风机类型），这些系统设计用于从前到后通过显卡抽取空气，并且有封闭的罩子将空气**直接排出机箱**（即没有热空气在机箱内部循环）。这允许专业 GPU 紧密堆叠在机箱内，同时仍能高效地进行自我冷却，对机箱内其他组件或 GPU 的影响最小。

![](img/9b7183e130e50d9105f71d7289223a3f.png)

两张显卡，一张采用‘吹风机’冷却系统，另一张采用更常见的风扇冷却系统。图片由[Nana Dua](https://www.pexels.com/photo/black-and-silver-car-wheel-4581613/)拍摄，发布在[Pexels](https://www.pexels.com/)上。注释由作者提供。

消费级 GPU 通常使用从上/下方的风扇冷却。这不可避免地意味着 GPU 的热空气在机箱内会有一定程度的循环，因此需要优秀的机箱通风。

然而，在多个 GPU 的机箱中，其他显卡的紧密接近会使风扇冷却变得非常无效，并且不可避免地导致 GPU 和其他接近组件的温度不理想。

总的来说，专业显卡设计为紧凑高效地打包到系统中，同时保持冷却和自我封闭。

***准确性***

![](img/68d46195a35471852d50fa101bcc2036.png)

图片由[Ricardo Arce](https://unsplash.com/@jrarce?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄，发布在[Unsplash](https://unsplash.com/photos/cY_TCKr5bek?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)上。

这实际上与深度学习并不特别相关，但专业级 GPU 通常具有 ECC（错误更正码）RAM。在需要高精度（即，位翻转的潜在随机错误低）的处理过程中，这将非常有用。

然而，深度学习模型有时会调优为**较少**的数值精度（半精度 8 位计算），因此这对正在运行的计算可能并不会造成实际问题。

尽管如果这些随机位翻转导致你的模型崩溃，那么这也可能值得考虑。

# 深度学习模型

![](img/9cb88fa46938284cad845cb26ae8024c.png)

图片来源：[Pixabay](https://www.pexels.com/photo/adult-blur-books-close-up-261909/)

对于深度学习模型，我希望它既先进又领先行业，并对 GPU 具有较高要求。同时，它也必须在难度上具有可扩展性，因为测试中的 GPU 具有广泛的能力范围。

## 一个专业级模型，适用于专业级显卡

要使模型符合行业标准，就排除了从头开始构建模型的可能性，因此在此比较中，将利用迁移学习使用现有的、经过验证的模型。

## 大数据

为确保输入数据的重量，分析将基于图像，特别是图像分类。

## 可扩展性

最终标准是可扩展性，有一组特定的模型完全符合这一标准……

# EfficientNet

[EfficientNet](https://keras.io/api/applications/efficientnet/) 由一系列图像分类模型（B0 到 B7）组成。每个模型都变得越来越复杂（也更准确）。随着你在模型系列中的进展，它也有不同的预期输入形状，从而增加了数据输入大小。

![](img/c161d9c0ab0fcf926c4bbb9aa17306c1.png)

不同 EfficientNet 模型的比较——数据来源于[EfficientNet: Rethinking Model Scaling for Convolutional Neural Networks](https://arxiv.org/pdf/1905.11946.pdf)——表格由作者提供

这有两个方面的效果：

1.  随着你在不同的 EfficientNet 模型中进展，模型参数会增加（即，对 GPU 处理要求更复杂、更高的模型）。

1.  需要处理的原始数据量也会增加（从 224x224 像素到 600x600 像素不等）。

*最终*，这为加载 GPU 提供了广泛的可能性，包括处理速度和 GPU RAM 需求。

# 数据

本文中使用的数据集是一组图像，描绘了游戏石头剪子布中手势的三种可能组合。[数据](https://www.kaggle.com/datasets/drgfreeman/rockpaperscissors)¹。

![](img/36774ae755eb3b167772996581fd5ac3.png)

来自[数据集](https://www.kaggle.com/datasets/drgfreeman/rockpaperscissors)的四个示例，分为三种不同的类别。合成图像由作者提供。

每张图片为 PNG 格式，尺寸为 300（宽）像素 x 200（高）像素，全彩色。

原始数据集总共包含 2188 张图片，但为了这篇文章使用了一个较小的选择，共包含 2136 张图片（每个类别 712 张）。从原始数据集中略微减少总图片数，是为了平衡类别。

本文使用的平衡数据集可以在这里找到：

[## notebooks/datasets/rock_paper_scissors at main · thetestspecimen/notebooks](https://github.com/thetestspecimen/notebooks/tree/main/datasets/rock_paper_scissors?source=post_page-----a62bec69f557--------------------------------)

### 这些数据集是原始“石头剪子布”数据集的一个选择，详细信息请见参考部分…

[github.com](https://github.com/thetestspecimen/notebooks/tree/main/datasets/rock_paper_scissors?source=post_page-----a62bec69f557--------------------------------)

# 测试

![](img/ce6041dc727b1d38f2fd19a3f8f4261a.png)

图片来源：[凯利·西克马](https://unsplash.com/@kellysikkema?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄于[Unsplash](https://unsplash.com/photos/4JxV3Gs42Ks?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

如前所述，EfficientNet 提供了各种不同的级别，因此为了测试，以下将在每个 GPU 上运行：

1.  **EfficientNet B0**（简单）

1.  **EfficientNet B3**（中等）

1.  **EfficientNet B7**（高强度）

这将测试显卡的速度能力，由于每个模型的总体参数不同，也会测试内存需求的范围，因为输入图像大小也会有所不同。

EfficientNet 模型将解锁所有层，并允许进行学习。

三个最终模型：

```py
EfficientNetB0
_________________________________________________________________
 Layer (type)                Output Shape              Param #   
=================================================================
 input_layer (InputLayer)    [(None, 224, 224, 3)]     0         

 data_augmentation (Sequenti  (None, 224, 224, 3)      0         
 al)                                                             

 efficientnetb0 (Functional)  (None, None, None, 1280)  4049571  

 global_avg_pool_layer (Glob  (None, 1280)             0         
 alAveragePooling2D)                                             

 output_layer (Dense)        (None, 3)                 3843      

=================================================================
Total params: 4,053,414
Trainable params: 4,011,391
Non-trainable params: 42,023
_________________________________________________________________

EfficientNetB3
_________________________________________________________________
 Layer (type)                Output Shape              Param #   
=================================================================
 input_layer (InputLayer)    [(None, 300, 300, 3)]     0         

 data_augmentation (Sequenti  (None, 300, 300, 3)      0         
 al)                                                             

 efficientnetb3 (Functional)  (None, None, None, 1536)  10783535 

 global_avg_pool_layer (Glob  (None, 1536)             0         
 alAveragePooling2D)                                             

 output_layer (Dense)        (None, 3)                 4611      

=================================================================
Total params: 10,788,146
Trainable params: 10,700,843
Non-trainable params: 87,303
_________________________________________________________________

EfficientNetB7
_________________________________________________________________
 Layer (type)                Output Shape              Param #   
=================================================================
 input_layer (InputLayer)    [(None, 600, 600, 3)]     0         

 data_augmentation (Sequenti  (None, 600, 600, 3)      0         
 al)                                                             

 efficientnetb7 (Functional)  (None, None, None, 2560)  64097687 

 global_avg_pool_layer (Glob  (None, 2560)             0         
 alAveragePooling2D)                                             

 output_layer (Dense)        (None, 3)                 7683      

=================================================================
Total params: 64,105,370
Trainable params: 63,794,643
Non-trainable params: 310,727
_________________________________________________________________
```

## 速度测试

GPU 的速度将通过其完成一个周期的速度来评估。

更具体地说，每个显卡上将运行至少两个周期，第二个周期将用于判断处理速度。第一个周期通常会有额外的加载时间，因此不适合作为一般执行时间的参考。

第一个周期的运行时间仅供参考。

## GPU 内存测试

为了测试 GPU 内存的极限，每个显卡的批量大小和每个 EfficientNet 模型（即 B0、B3 或 B7）已调整到尽可能接近该显卡的极限（即尽可能填满 GPU 内存）。

实际的峰值 GPU 内存使用情况也会披露，以便进行比较。

# 代码

![](img/58a7cdf931b08f72018c64ec24fa5128.png)

图片来源：[奥斯卡·伊尔迪兹](https://unsplash.com/@oskaryil?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄于[Unsplash](https://unsplash.com/photos/cOkpTiJMGzA?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

一如既往，我将所有的 Python 脚本（GTX 1070 和 RTX 6000 Ada）以及笔记本（Tesla T4）都提供在 GitHub 上：

[](https://github.com/thetestspecimen/notebooks/tree/main/pro-vs-consumer-graphics-card?source=post_page-----a62bec69f557--------------------------------) [## notebooks/pro-vs-consumer-graphics-card at main · thetestspecimen/notebooks

### 你现在无法执行该操作。你在另一个标签或窗口中登录了。你在另一个标签中登出了…

github.com](https://github.com/thetestspecimen/notebooks/tree/main/pro-vs-consumer-graphics-card?source=post_page-----a62bec69f557--------------------------------)

如果你愿意，也可以直接在 Colab 上访问 Tesla T4 的笔记本：

EfficientNetB0：

![](https://colab.research.google.com/github/thetestspecimen/notebooks/blob/main/pro-vs-consumer-graphics-card/rps_t4_tf_B0.ipynb)

EfficientNetB3：

![](https://colab.research.google.com/github/thetestspecimen/notebooks/blob/main/pro-vs-consumer-graphics-card/rps_t4_tf_B3.ipynb)

EfficientNetB7：

![](https://colab.research.google.com/github/thetestspecimen/notebooks/blob/main/pro-vs-consumer-graphics-card/rps_t4_tf_B7.ipynb)

# 结果

![](img/07b2b58f8ce44db7d0d0703e05a3e93f.png)

照片由 [Pixabay](https://www.pexels.com/photo/business-commerce-computer-delivery-263194/)

## EfficientNet B0

![](img/777a63908da503045aeb462939760cba.png)

## EfficientNet B3

![](img/06c1457350384a0f6731185373fefbff.png)

## EfficientNet B7

![](img/7a6c34f8712e1ac26d7e22315665111d.png)

***注意：*** *在第一轮训练中，我列出了括号中的秒数。这是第一轮和第二轮训练之间的时间差。*

# 讨论 — 执行速度

![](img/ef885c6083787cb3235968f4817c2f8f.png)

图片由 [Arek Socha](https://pixabay.com/users/qimono-1962238/?utm_source=link-attribution&amp%3Butm_medium=referral&amp%3Butm_campaign=image&amp%3Butm_content=1249610) 提供，来自 [Pixabay](https://pixabay.com//?utm_source=link-attribution&amp%3Butm_medium=referral&amp%3Butm_campaign=image&amp%3Butm_content=1249610)

第一个要查看的项是执行速度。

对于这个特定的数据集，EfficientNet B0 对任何显卡都没有造成太大的挑战，所有显卡都在几秒钟内完成了一轮训练。

然而，需要记住的是，本文章使用的数据集很小，实际上，两块 RTX 6000 Ada 显卡在执行速度上大约比 GTX 1070（和 Tesla T4）快**17 倍**。对于 EfficientNet B3（快 8 倍）和 B7（快 11 倍），情况也基本相同。

区别在于，当将其视为执行时间时，这种速度的减慢会随着模型的复杂性增加而变得更具障碍。

例如，在这个非常小的数据集上，使用 GTX 1070 执行一个周期大约需要 15 分钟。与一对 RTX 6000 Ada 的 1 分钟多一点相比。

…情况更糟。

## 扩展规模

让我们现实一点。没有模型会在一个周期内收敛。对于像 EfficientNet 这样的模型，四百可能是一个更合理的数字。

这将是使用像 GTX 1070 这样的 GPU 需要**4 天**的差距，而在双 RTX 6000 Ada 设置下仅需几个小时（准确地说是 6.5 小时）。然后考虑到实际数据集不只有 2188 张图片，它可能有数百万张（作为参考，[ImageNet](https://www.image-net.org/)有刚刚超过**1400 万张图片**）。

## 行业进展

另一个需要记住的是行业进展。EfficientNet 已经有几年历史了，情况已经发生了变化。

举个小例子，[NoisyStudent](https://arxiv.org/pdf/1911.04252v4.pdf)在标准 EfficientNets 的基础上增加了一种叫做 EfficientNet-L2 的变体，并表示：

> 由于模型大小较大，EfficientNet-L2 的训练时间大约是 EfficientNet-B7 的五倍。
> 
> -[自我训练与 Noisy Student 改进 ImageNet 分类](https://arxiv.org/pdf/1911.04252v4.pdf)

…所以如果你需要保持在前沿，速度确实很重要。

## 那么这对专业图形卡与消费者图形卡意味着什么呢？

事实是，如果你**仅仅**看执行速度，专业和消费者 GPU 之间几乎没有区别，如果你对比相同条件下的产品。RTX 4090 的速度几乎和 RTX 6000 Ada 一样。

> RTX 4090 的速度几乎和 RTX 6000 Ada 一样。

目前为止，这个小实验仅仅说明了速度非常重要，因为行业标准模型的复杂性发展很快。老一代图形卡已经明显较慢。要跟上，就需要**至少**保持在硬件的前沿。

> …规模在回答这个问题时确实很重要。

…但随着进展速度的加快（只需看看 GTP-3 和 GTP-4 的迅速发展），如果你想保持在前沿，即使是 RTX 4090 或 RTX 6000 Ada 级别的单个 GPU 也可能不够。如果是这样的话，专业级图形卡在构建系统时优越的散热、较低的功耗和更紧凑的尺寸就是一个显著的优势。

本质上，规模在回答这个问题时非常重要。

然而，速度只是一个方面。现在让我们转到 GPU RAM，这里情况会有些更有趣…

# 讨论 — GPU RAM

GPU RAM 在某些情况下是一个重要的考虑因素，甚至可能是是否可以使用某些模型或数据集的实际限制因素。

让我们看看一对 RTX 6000 Ada 的全面表现：

![](img/d888542c0d46ff23fd76e6d112f96c4c.png)

两张 RTX 6000 Ada GPU 正在运行深度学习模型。图片由作者提供

你可能会注意到上面的图片中两个 GPU 的 GPU RAM 都达到了 100%。然而，这并不是真实的使用情况：

> 默认情况下，TensorFlow 会映射几乎所有 GPU 的 GPU 内存（受 `[*CUDA_VISIBLE_DEVICES*](https://docs.nvidia.com/cuda/cuda-c-programming-guide/index.html#env-vars)`）到进程中。这是为了通过减少内存碎片来更有效地利用设备上相对宝贵的 GPU 内存资源。
> 
> [-tensorflow.org](https://www.tensorflow.org/guide/gpu)

## 限制

绝对的限制被 GTX 1070（它有 8GB GPU RAM）所凸显，它只能以批量大小为 1 运行 EfficientNet B7（即它每次只能处理 1 张图片，然后更新模型参数并将下一张图片加载到 GPU RAM 中）。

这会引发两个问题：

1.  由于频繁的参数更新和更频繁地将新数据加载到 GPU RAM 中（即更大的批量大小本质上更快），你会失去执行速度。

1.  如果输入图像尺寸再大一点，模型将无法运行，因为它无法将单张图像放入 GPU RAM 中。

即使是 Tesla T4，它拥有不算差的 16GB GPU 内存，在 EfficientNet B7 上也只能处理批量大小为 2 的任务。

如前所述，16GB 的 GPU RAM 是大多数当前一代消费级 GPU 的良好代表，只有 RTX 4090 拥有 24GB。因此，如果你处理的是内存密集型原始数据，这对消费级 GPU 来说是一个相当显著的缺点。

此时，为什么所有专业 GPU 相比于消费级 GPU 拥有如此大量的 RAM 突然变得清晰。正如执行速度讨论中提到的，EfficientNet 已经不再处于前沿，因此今天的现实可能比这篇文章中的测试所述还要苛刻。

## 系统密度

关于 GPU RAM 的另一个考虑因素是系统密度。

例如，我可以访问的系统有一个可以容纳 4 张双高显卡的主板（我也见过最多可安装 8 张显卡的系统）。这意味着如果你的系统对 GPU RAM 有优先需求，那么专业 GPU 就是不二选择：

4 x RTX 6000 Ada = 192GB GPU RAM 和 1200W 功耗

4 x RTX 4090 = 96GB GPU RAM 和 1800W 功耗

（……正如我在文章前面提到的，RTX 4090 是一款三槽 GPU，所以这甚至不切实际。实际上，只有两张 RTX 4090 显卡才能实际适配，但为了方便比较，我们假设它是可行的。）

这不是一个小差异。要匹配 RTX 6000 Ada 系统的 GPU RAM，你将需要两个分别消耗至少 **三倍功率** 的系统。

> 要匹配 RTX 6000 Ada 系统的 RAM，你将需要两个分别消耗至少三倍功率的系统。

别忘了，因为你需要两个独立的系统，你还需要额外支付 CPU、电源、主板、冷却、机箱等费用。

## 关于系统 RAM 的一个附注…

![](img/ffbcf362988e23943cc958ed6a9f0a7b.png)

*你是否注意到在专业系统中，CPU 上下各有 8 根 64GB 的系统 RAM？图片来自* [*Exxact Corporation*](https://www.exxactcorp.com/category/Deep-Learning-Solutions?page=1&utm_source=web+referral&utm_medium=backlink&utm_campaign=Michael+Clayton&utm_term=Medium+Towards+Data+Science) *，由 Michael Clayton 授权使用*

还值得指出的是，重要的并不仅仅是 GPU RAM。随着 GPU RAM 的增加，你需要同步增加系统 RAM。

你可能会注意到，在 Tesla T4 的 Jupyter notebooks 中，我已注释掉了以下优化：

```py
train_data = train_data.cache().prefetch(buffer_size=tf.data.AUTOTUNE)
val_data = val_data.cache().prefetch(buffer_size=tf.data.AUTOTUNE)
```

这是因为，对于 EfficientNet B7，如果启用这些设置，训练将崩溃。

为什么？

因为“.cache()”优化将数据保留在系统内存中，以便有效地传递给 GPU，而 Colab 实例只有 12GB 的系统内存。这还不够，即使 GPU RAM 峰值为 9.9GB：

> 这个 [.cache()] 会将一些操作（如文件打开和数据读取）从每个训练周期中省略。
> 
> -[tensorflow.org](https://www.tensorflow.org/guide/data_performance)

然而，专业系统有 8 根 64GB 的系统 RAM，总计 512GB 的系统 RAM。所以即使两张 RTX 6000 Ada GPU 合计有 96GB 的 GPU RAM，系统 RAM 仍有足够的余量来处理大量缓存。

# 结论

![](img/921383d94b40284165c0c98bef51c2b2.png)

*图片来自* [*Exxact Corporation*](https://www.exxactcorp.com/category/Deep-Learning-Solutions?page=1&utm_source=web+referral&utm_medium=backlink&utm_campaign=Michael+Clayton&utm_term=Medium+Towards+Data+Science) *，由 Michael Clayton 授权使用*

那么，专业级显卡在深度学习中是否优于消费级显卡？

如果钱不是问题，那么是的，它们更好。

这是否意味着你应该放弃考虑用于深度学习的消费级显卡？

不，并不是。

这完全取决于具体的需求，通常情况下，更是规模。

## 大型数据集

如果你知道你的工作负载将是内存密集型的（例如大型语言模型、图像或视频分析），那么同一代和处理速度的专业显卡通常拥有大约两倍的 GPU RAM。

> 这完全取决于具体的需求和规模。

这是一个显著的优势，特别是考虑到与消费级显卡相比，实现这一点不需要额外增加能源需求。

## 较小的数据集

如果你的 RAM 需求不高，那么问题就更复杂了，涉及到可靠性、兼容性、支持、能源消耗，以及额外 10% 的速度是否值得那显著的价格上涨。

## 规模

如果你打算投资重大基础设施，那么可靠性、能源消耗和系统密度可能从低优先级变为相当重要的考虑因素。这些都是专业 GPU 擅长的领域。

相反，如果你需要一个较小的系统，并且高 GPU 内存需求并不重要，那么考虑消费者级别的显卡可能会有利。与大规模相关的因素，如可靠性和能源消耗将变得不那么重要，系统密度也不会成为问题。

## 最终结论

总的来说，这是一个平衡的过程，但如果我必须选择两个因素来总结选择消费者级 GPU 与专业级 GPU 之间最重要的因素，那就是：

1.  GPU 内存

1.  系统规模

如果你有**较高**的 GPU 内存需求，或需要配备多个 GPU 的大型系统，那么你需要一个专业级的 GPU/多个 GPU。

否则，大多数情况下，消费者级别可能会是一个更好的选择。

如果你觉得这篇文章有趣或有用，记得关注我，或[订阅我的通讯](https://medium.com/@maclayton/subscribe)以获取更多类似的内容。

如果你还没有，可以考虑[订阅 Medium](https://medium.com/@maclayton/membership)。你的会员费不仅直接支持我，还支持你阅读的其他作者。你还将获得 Medium 上每一篇文章的完全无限制访问权限。

使用我的推荐链接注册将为我带来少量佣金，但不会影响你的会员资格，所以如果你选择这样做，谢谢你。

[](https://medium.com/@maclayton/membership?source=post_page-----a62bec69f557--------------------------------) [## 通过我的推荐链接加入 Medium - Mike Clayton

### 阅读 Mike Clayton 的每一篇文章（以及 Medium 上的其他成千上万的作者）。你的会员费直接支持…

medium.com](https://medium.com/@maclayton/membership?source=post_page-----a62bec69f557--------------------------------)

# 参考文献

[1] Julien de la Bruère-Terreault, [石头剪子布图像](https://www.kaggle.com/datasets/drgfreeman/rockpaperscissors) (2018), Kaggle, 许可协议：[CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)
