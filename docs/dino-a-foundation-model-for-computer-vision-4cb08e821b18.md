# DINO — 计算机视觉的基础模型

> 原文：[`towardsdatascience.com/dino-a-foundation-model-for-computer-vision-4cb08e821b18`](https://towardsdatascience.com/dino-a-foundation-model-for-computer-vision-4cb08e821b18)

## [🚀Sascha 的论文俱乐部](https://towardsdatascience.com/tagged/saschas-paper-club)

## 自监督视觉变换器中的新兴特性，作者 M. Caron 等。

[](https://medium.com/@SaschaKirch?source=post_page-----4cb08e821b18--------------------------------)![Sascha Kirch](https://medium.com/@SaschaKirch?source=post_page-----4cb08e821b18--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4cb08e821b18--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----4cb08e821b18--------------------------------) [Sascha Kirch](https://medium.com/@SaschaKirch?source=post_page-----4cb08e821b18--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4cb08e821b18--------------------------------) ·13 分钟阅读·2023 年 9 月 27 日

--

计算机视觉正迎来一个令人兴奋的十年。来自自然语言领域的巨大成功被转移到视觉领域，包括引入 ViT（视觉变换器），最近大规模的自监督预训练技术在基础模型的名义下成为头条新闻。

今天我们将探讨一个名为 DINO（自**DI**蒸馏，**N**O 标签）的框架，它是建立在 ViTs 有趣特性基础上的视觉基础模型。它也是今天表现最佳的基础模型之一的前身：[DINOv2](https://arxiv.org/abs/2304.07193)。

![](img/826ce8cc3feae5497593efe2c4e9631c.png)

图片来源于 [出版物](https://arxiv.org/abs/2104.14294)，作者 [Sascha Kirch](https://medium.com/@SaschaKirch)

> **论文:** [自监督视觉变换器中的新兴特性](https://arxiv.org/abs/2104.14294)，作者 [Mathilde Caron](https://arxiv.org/search/cs?searchtype=author&query=Caron%2C+M) 等，2021 年 4 月 29 日
> 
> **资源:** [GitHub](https://github.com/facebookresearch/dino) — [博客文章](https://ai.meta.com/blog/dino-paws-computer-vision-with-self-supervised-transformers-and-10x-more-efficient-training/)
> 
> **类别:** 基础模型，计算机视觉，视觉变换器，知识蒸馏，相似性学习，自监督学习
> 
> [**其他详细讲解**](https://medium.com/@SaschaKirch/list/paper-walkthroughs-by-sascha-kirch-89c7847da8e2)**:**
> 
> [BYOL] — [CLIP] — [GLIP] — [Segment Anything] — [DINO] — [[Depth Anything](https://medium.com/towards-data-science/depth-anything-a-foundation-model-for-monocular-depth-estimation-8a7920b5c9cc?sk=fc6197edd68e6137c3396c83e50f65cb)] — [DDPM]

# 大纲

1.  背景与背景

1.  方法

1.  实验

1.  消融测试

1.  结论

1.  进一步阅读与资源

# 背景与背景

时间是 2021 年，准确地说是 4 月。自从发布了带有 [Attention is All You Need](https://arxiv.org/abs/1706.03762) 的 Transformer 模型已经过去了四年。自监督预训练在 NLP 中已经由 [BERT](https://arxiv.org/pdf/1810.04805.pdf) 等模型长期实践，而“基础模型”这一术语在接下来的几个月中尚未被知晓，直到 *“*[*关于基础模型的机遇与风险*](https://arxiv.org/abs/2108.07258)*”* 的发布。六个月前，[Vision Transformer (ViT)](https://arxiv.org/abs/2010.11929) 首次发布在 arxiv 上，距离 ICLR 2021 还有一个月，它将在那里进行展示。

> 让我们稍微消化一下这个信息：ViT 于 2020 年 10 月在 arxiv.org 上首次发布，并在 2021 年 5 月的 ICLR2021 上进行了展示。DINO 于 2021 年 4 月在 arxiv 上发布。所以，实际在会议上展示前的一个月。这意味着他们只有 5 个月的时间，如果他们立即开始的话，来构思项目的想法、组建团队、奠定理论基础、训练模型、进行实验和消融测试，并撰写论文。难怪现在的博士生感到不断的焦虑。至少这就是我有时的感受 *😅*。

尽管 ViT 与卷积网络非常具有竞争力，但它们在计算资源和训练数据量方面的要求很高。

DINO 的作者做出了一个简单的观察：变换器在 NLP 中的成功与自监督预训练相关，而目前视觉领域的自监督方法是基于卷积网络的，比如 BYOL。

[](/byol-the-alternative-to-contrastive-self-supervised-learning-5d0a26983d7c?source=post_page-----4cb08e821b18--------------------------------) ## BYOL -对比自监督学习的替代方案

### 论文分析——《Bootstrap Your Own Latent: A New Approach to Self-Supervised Learning》

[towardsdatascience.com

受到 BYOL 和 [mean teacher](https://arxiv.org/abs/1703.01780) 的启发，作者提出了一个框架来以自监督的方式训练 ViT，并发现：

1.  自监督 ViT 特征明确包含场景布局，特别是对象边界。

1.  自监督 ViT 特征在没有任何微调、线性分类器或数据增强的情况下，与基础的最近邻分类器 (k-NN) 一起表现尤为出色。

与 BYOL 和 mean teacher 相比，DINO 实现了一个知识蒸馏框架，包括一个学生模型和一个教师模型，作用于同一图像的不同视角，并采取额外措施应对相似性学习方法的固有不稳定性，其中解决方案通常会崩溃。

底层视觉变换器架构 (ViT) 的一个有趣发现是，当使用无监督学习技术进行训练时，其特征包含有关图像语义分割的显著信息。可以简单地可视化多头注意力层中选择的头部的自注意力图，如下方视频所示：

![](img/98c910b938eac30dc3417917e4e55d08.png)

图 1：选择的头部的自注意力图。[来源](https://arxiv.org/abs/2104.14294)

让我们深入探讨一下 DINO 实现其框架的方式，如何应对不稳定性，以及与以前的方法相比它的表现如何！

![Sascha Kirch](img/3edf0b4a499cde306202656453c7fe0a.png)

[Sascha Kirch](https://medium.com/@SaschaKirch?source=post_page-----4cb08e821b18--------------------------------)

## 由 Sascha Kirch 进行的论文解读

[查看列表](https://medium.com/@SaschaKirch/list/paper-walkthroughs-by-sascha-kirch-89c7847da8e2?source=post_page-----4cb08e821b18--------------------------------)7 个故事！“DDPM — 去噪扩散概率模型”论文插图，作者：Sascha Kirch![“Depth Anything”论文插图，作者：Sascha Kirch](img/bd8cd71a02e42cf64d0afd39f41f48e0.png)![](img/8708d91a4a1902cef889ced95d46fc39.png)

# 方法

DINO 框架与其他相似性学习框架（如 BYOL 或 [mean teacher](https://arxiv.org/abs/1703.01780)）以及知识蒸馏具有相同的整体结构。让我们首先看看 DINO 是如何做到这一点的，并与其他框架进行区分。

![](img/2dacf33beadadf8b5604e4842686f18f.png)

图 2：DINO 架构。 [来源](https://arxiv.org/abs/2104.14294) + [Sascha Kirch](https://medium.com/@SaschaKirch) 的注释

## **网络和更新规则**

我们从中间开始。DINO 实现了两个具有完全相同架构但权重不同的网络。这些网络分别是学生网络和教师网络。学生网络通过反向传播进行训练，而教师网络则通过其自身权重和学生网络权重的指数移动平均来更新其权重。

![](img/b2fe06104b0d6ee43b9013c3b398e4e8.png)

方程 1：教师权重的更新规则。 [来源](https://arxiv.org/abs/2104.14294) + [Sascha Kirch](https://medium.com/@SaschaKirch) 的注释

骨干网络可以是 ResNet50 或 [DeiT](https://arxiv.org/abs/2012.12877)（这是为知识蒸馏而调整的 [ViT](https://arxiv.org/abs/2010.11929)）。一个基于 MLP 的投影头连接到骨干网络，以减少特征的维度，但在推理时会被移除。

***很好，但用于推理的是哪个模型：学生还是教师？*** — 好问题，实际上论文中并没有提到这个问题的任何信息。直观上你可能会认为是学生，至少我最开始也是这样想的。但正如我们后续将看到的，教师在整个训练过程中表现优于学生。除了更好的性能之外，唯一的线索是，在代码实现中，教师检查点是用于例如 [视频分割](https://github.com/facebookresearch/dino/blob/7c446df5b9f45747937fb0d72314eb9f7b66930a/eval_video_segmentation.py#L257C9-L257C9)、[线性探测](https://github.com/facebookresearch/dino/blob/7c446df5b9f45747937fb0d72314eb9f7b66930a/eval_linear.py#L264C34-L264C34) 和 [k-NN](https://github.com/facebookresearch/dino/blob/7c446df5b9f45747937fb0d72314eb9f7b66930a/eval_knn.py#L203C32-L203C32) 的默认评估点。由于此参数可以更改，因此我不能给出确切的答案。

## **输入和输出**

从输入图像 *x* 创建不同的视图 *x1* 和 *x2*，方法是通过裁剪和应用图像增强，如 BYOL（例如色彩抖动、高斯模糊和太阳化）。用于裁剪的技术称为 [multi-crop](https://arxiv.org/abs/2006.09882)，通过生成不同大小的多个裁剪来节省内存，同时提供更多数据。小裁剪被称为局部视图，由 96x96 像素组成，这些视图专门输入到学生网络中。较大的裁剪被称为全局视图，由 224x224 像素组成，这些视图专门输入到教师网络中。正如我们在消融部分将看到的，训练过程中使用了 2 个全局视图和 10 个局部视图。

> 注意：论文对于多裁剪技术有点混乱，因为提供的伪代码和上面的图 3 所示的架构都没有反映出来。伪代码甚至建议 x1 和 x2 像在 BYOL 中一样输入到学生和教师中，这在使用多裁剪时并非如此。

与相似性学习的目标是最大化嵌入的相似性不同，DINO 最小化教师和学生输出分布之间的交叉熵。如下面的方程所示，交叉熵是对每对全局和局部视图计算的，然后汇总。

![](img/b999355c87ae211c90653c509cea3da3.png)

方程 2：优化目标。 [来源](https://arxiv.org/abs/2104.14294) + [Sascha Kirch](https://medium.com/@SaschaKirch) 的注解

***模型的输出是什么？*** — 就像相似性学习中，学生和教师为给定的图像输出一个嵌入，而不是预测分数。就像在知识蒸馏中，输出通过 SoftMax 转换为概率分布。SoftMax 有一个温度参数，它控制结果分布的平滑或锐化。这个温度在知识蒸馏中起着关键作用，因为它可以控制从教师网络到学生网络转移一般知识和细粒度细节之间的平衡，使蒸馏过程对不同任务更有效。

![](img/4d9f933db5fb3f0e5dfba164632ebfcd.png)

图 3：温度值对 SoftMax 输出的影响。 [Sascha Kirch](https://medium.com/@SaschaKirch) 的插图，使用 [这个 Python 笔记本](https://github.com/sascha-kirch/ML_Notebooks/blob/main/Softmax_Temperature.ipynb) 创建

我为你创建了一个笔记本，以便你可以调查温度对结果分布的影响：

[](https://github.com/sascha-kirch/ML_Notebooks/blob/main/Softmax_Temperature.ipynb?source=post_page-----4cb08e821b18--------------------------------) [## ML_Notebooks/Softmax_Temperature.ipynb 在 main 分支 · sascha-kirch/ML_Notebooks

### 机器学习相关笔记的集合用于共享。— ML_Notebooks/Softmax_Temperature.ipynb 在 main 分支 ·…

github.com](https://github.com/sascha-kirch/ML_Notebooks/blob/main/Softmax_Temperature.ipynb?source=post_page-----4cb08e821b18--------------------------------)

## **避免崩溃**

如前所述，学生和教师具有完全相同的架构。这种设置是不稳定的（如果没有采取对策），可能会导致崩溃解决方案，即所有特征都映射到潜在空间中的某个区域，例如最坏情况下的一个点。BYOL 通过为其中一个模型引入额外的预测头来解决这个问题，从而引入了不对称性。由于 DINO 具有对称模型，因此需要另一种技巧：中心化和锐化。两者仅应用于教师网络。中心化是一种技术，通过向教师输出添加偏置项*c*来防止潜在空间中的单一维度主导，即*g(x) = g(x)+c*。

![](img/268f8c0249d46fa219f3dcad096e9332.png)

方程 3：中心化项的更新规则。[来源](https://arxiv.org/abs/2104.14294) + [Sascha Kirch](https://medium.com/@SaschaKirch) 的注释

虽然中心化具有积极效果，但它也鼓励输出崩溃为均匀分布。锐化具有相反的效果，因此应用两者平衡它们的效果并稳定训练。通过使用较小的温度来实现锐化（见图 3），教师的 SoftMax 温度比学生的低。

为了避免方程 3 中的超参数*m*和教师的温度崩溃是至关重要的。在附录部分的消融研究中，作者展示了*m=0.9…0.999*的效果最佳，并且温度值在预热期间从*0.04*线性增加到*0.07*。

## **DINO 是做什么的？知识蒸馏还是相似性学习？**

答案是两者兼有！

虽然知识蒸馏通常是将知识从已经训练好的、更大且更准确的教师模型蒸馏到较小的学生模型中，但它也可以看作是一种相似性学习，因为它鼓励学生网络生成与教师相似的预测。在相似性学习中，两个模型通常是联合训练的，并且通常对齐它们的潜在空间预测，而不是概率分布。

由于 DINO 的作者将他们的目标表述为知识蒸馏，让我们看看与“标准”知识蒸馏相比的一些差异：

1.  DINO 的教师不是事先可用的，而是与学生一起“训练”的。它甚至可以被认为是一种共同蒸馏，因为知识也从学生蒸馏到教师。

1.  DINO 的教师和学生不是对相同的输入进行操作，而是对裁剪到不同尺寸的图像的不同视图进行操作。

1.  DINO 在两个模型的 SoftMax 中使用不同的温度来进行锐化。

1.  DINO 计算的是嵌入的温度缩放 SoftMax 上的交叉熵，而不是预测分数。

它与知识蒸馏的相似之处在哪里？：

1.  DINO 由一个学生网络和一个教师网络组成，其中教师的表现优于学生，正如我们在实验中将看到的那样。

1.  DINO 不是最大化相似性度量，而是最小化温度缩放的 SoftMax 输出的交叉熵损失。

[](https://medium.com/@SaschaKirch/subscribe?source=post_page-----4cb08e821b18--------------------------------) [## 每当 Sascha Kirch 发布新内容时都会收到邮件 🚀

### 每当 Sascha Kirch 发布新内容时都会收到邮件 🚀 想要了解更多深度学习相关的内容或只是保持最新动态……

medium.com](https://medium.com/@SaschaKirch/subscribe?source=post_page-----4cb08e821b18--------------------------------)

# 实验

论文展示了大量的实验。他们在 ImageNet 上预训练模型，ImageNet 是一个在表征学习中常用的数据集。

对于评估，常见的技术通常要么在冻结特征上训练线性分类器，要么对模型进行微调以适应新的下游任务，在这种情况下，模型的参数会被调整。

DINO 的作者声称这些技术对超参数非常敏感，这使得比较不公平且难以重现。因此，他们建议对预训练模型的特征使用简单的最近邻聚类算法。

## ImageNet 上的线性和 k-NN 分类

在这个实验中，模型在 ImageNet 上的图像分类准确性上进行了测试。测试了多种自监督预训练模型，骨干网包括 ResNet 或 ViT。分类是通过线性探测或 k-NN 聚类完成的。

![](img/8411c4b87ac4d9fab26ed43dfe033c82.png)

表 1：在 ImageNet 上的线性和 k-NN 分类。 [来源](https://arxiv.org/abs/2104.14294) + [Sascha Kirch](https://medium.com/@SaschaKirch) 的注释

我认为主要的收获是：

1.  K-NN 在 ViT 特征上的表现优于 ResNet 特征。

1.  在 ViT 中减少补丁大小比增加骨干网带来的改进更大，但代价是推理速度变慢。

## 视频实例分割

一个重要的实验是视频分割任务，因为论文讨论了 ViT 在用自监督方法训练时捕捉语义分割能力的特性。或者说这是论文所声称的 😁

![](img/9480e3a017e4d94de1b778797fc40226.png)

表 2：视频实例分割。 [来源](https://arxiv.org/abs/2104.14294) + [Sascha Kirch](https://medium.com/@SaschaKirch) 的注释

观察这些结果后，我觉得还缺少两个进一步的实验：

1.  如果能看到在 DINO 框架下监督的 ResNet50 和自监督的 ResNet50 之间的对比，将会很好，这可以支持他们关于 ViT 优于 ResNet 架构的主张。

1.  如果能看到相同的 ViT 骨干网在监督学习和自监督学习下的效果对比，将会非常棒，这样可以观察到对补丁大小和模型大小的影响。

不过正如我总是说的：提出问题很容易 😁 在实际项目中，作者们常常面临资源限制和项目截止日期，所以不可能涵盖每一个细节！

## 探索自注意力图

在这个实验中，作者调查了 ViT 的多头自注意力层中不同头部的自注意力图。他们可视化了 ViT-S/8 最后一层中选定头部的注意力图，精确来说是学习到的[CLS]令牌。

![](img/133db7e45053a51d9d4e14bf554844d1.png)

图 4：来自选定头部的注意力图。 [来源](https://arxiv.org/abs/2104.14294) + [Sascha Kirch](https://medium.com/@SaschaKirch)的注释

## 其他实验

在其他实验中，DINO 在与监督基线的比较中有所改进。这些任务包括图像检索和复制检测。

# 消融实验

在他们的消融研究中，作者对 ViT-S 模型进行了实验。

## 补丁大小的重要性

记住，视觉变换器输入的是一个补丁化的输入图像，将每个补丁转化为令牌，然后应用具有自注意力机制的变换器。这是 ViT 作者的一项技巧，用于减少性能权衡的计算需求，使变换器适用于图像数据。

DINO 声称，较小的补丁大小提高了性能，同时降低了吞吐量（每秒可以处理的图像数量），这正是 ViT 所声称的。

![](img/2103ddc8e040677295c3880a1ff19051.png)

图 5：补丁大小对准确性和吞吐量的影响。 [来源](https://arxiv.org/abs/2104.14294) + [Sascha Kirch](https://medium.com/@SaschaKirch)的注释

直观地说，这并不令人惊讶，因为你增加了输入分辨率，结果是需要处理更多的令牌，因此你得到一个更细粒度的注意力图。

## 不同的教师更新规则

DINO 中的教师通过计算从更新后的学生和当前教师的指数移动平均来更新。这就是他们所称的“动量编码器”方法。

使用动量编码器并绘制教师和学生在训练过程中的准确性，教师在整个过程中表现更好。由此我们可以假设：

1.  教师可以为学生提供强有力的学习信号。

1.  改进的学生由于 EMA 更新规则（共同蒸馏）使教师得到提升。

1.  可以使用教师作为最终模型，该模型具有更好的性能，但与学生具有相同的架构，因此计算需求没有变化。

![](img/36bd26e2335351a2980f6f518e29211d.png)

图 6：教师性能。 [来源](https://arxiv.org/abs/2104.14294) + [Sascha Kirch](https://medium.com/@SaschaKirch)的注释

他们还实验了另外 3 种更新规则：将权重从学生复制到教师，使用优化器前一个迭代的学生权重，和使用前一个时代的学生权重。

## 多裁剪与时间和 GPU 内存

如前所述，DINO 输入相同图像的多个裁剪视图，并将全局视图输入到教师模型中，将局部视图输入到学生模型中。在这项消融实验中，作者试验了不同数量的局部视图，并报告了对性能、训练时间和每 GPU 峰值内存的影响。

![](img/1150cc8dc59aeb8b950bce9b7b52a6be.png)

表 3：多裁剪与时间和 GPU 内存。[来源](https://arxiv.org/abs/2104.14294) + [Sascha Kirch](https://medium.com/@SaschaKirch)的注释

## 避免崩溃

在这项消融实验中，作者评估了其稳定措施在避免崩溃解决方案中的作用：中心化和锐化。

为此，他们将交叉熵分解为熵项和 Kullback-Leibler（KL）散度项。KL 散度是两个概率分布差异的度量。如果 KL 为 0，则认为两个分布相等。

其直观的理解是：如果教师和学生的输出分布的 KL 散度在整个训练过程中保持不变，那么学生的权重更新就没有学习信号。

![](img/65a0d4da56fdaf6c580d5c58625de2aa.png)

图 7：崩溃解决方案分析。[来源](https://arxiv.org/abs/2104.14294) + [Sascha Kirch](https://medium.com/@SaschaKirch)的注释

## 批量大小的影响

一个有趣的特性是，DINO 可以用较小的批量大小进行训练，而不会大幅下降性能。这实际上是 BYOL 的一个动机，DINO 基于此论文，减少了对批量大小的依赖，相比对比自监督学习方法。

![](img/9e0fa8d51ff10f8d989b51eb03eb122c.png)

表 4：批量大小与准确率。[来源](https://arxiv.org/abs/2104.14294) + [Sascha Kirch](https://medium.com/@SaschaKirch)的注释

类似 CLIP 和 GLIP 的对比方法提供了大量的负样本以避免崩溃解决方案。每次优化器更新步骤（因此每批次）的负样本越多，效果越好。

# 结论

总结来说，DINO 是一个知识蒸馏框架。它是一个视觉基础模型，利用了 ViTs 的有趣特性，并且是今天表现最好的基础模型之一 DINOv2 的前身。DINO 的框架由学生模型和教师模型组成，作用于相同图像的不同视图，并采取额外措施来处理相似性学习方法的固有不稳定性。实验表明，DINO 在各种任务上优于其他自监督预训练模型。

# 进一步阅读与资源

## 论文

与此同时，DINO 的改进版本已经发布：

1.  [DINOv2: 在没有监督的情况下学习鲁棒的视觉特征](https://arxiv.org/abs/2304.07193)

1.  [Meta 的 DINOv2 博客文章](https://ai.meta.com/blog/dino-v2-computer-vision-self-supervised-learning/)

## 论文解读

你可能还会喜欢我其他的论文解读，涵盖了我们在本文中讨论的概念：

[](/the-clip-foundation-model-7770858b487d?source=post_page-----4cb08e821b18--------------------------------) ## CLIP 基础模型

### 论文总结—从自然语言监督中学习可转移的视觉模型

towardsdatascience.com [](/glip-introducing-language-image-pre-training-to-object-detection-5ddb601873aa?source=post_page-----4cb08e821b18--------------------------------) ## GLIP：引入语言-图像预训练到目标检测

### 论文总结：**基于语境的语言-图像预训练**

towardsdatascience.com [](/byol-the-alternative-to-contrastive-self-supervised-learning-5d0a26983d7c?source=post_page-----4cb08e821b18--------------------------------) ## BYOL -对比自我监督学习的替代方案

### 论文分析—**自我监督学习的新方法**：Bootstrap Your Own Latent

towardsdatascience.com [](/segment-anything-promptable-segmentation-of-arbitrary-objects-f28958c5612d?source=post_page-----4cb08e821b18--------------------------------) ## Segment Anything — 可提示的任意对象分割

### 论文讲解—Segment Anything

towardsdatascience.com
