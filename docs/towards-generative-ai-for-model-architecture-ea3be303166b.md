# 朝向生成式 AI 的模型架构

> 原文：[https://towardsdatascience.com/towards-generative-ai-for-model-architecture-ea3be303166b?source=collection_archive---------4-----------------------#2023-11-10](https://towardsdatascience.com/towards-generative-ai-for-model-architecture-ea3be303166b?source=collection_archive---------4-----------------------#2023-11-10)

## “MAD” AI 如何帮助我们发现下一个变压器

[](https://medium.com/@drwiner?source=post_page-----ea3be303166b--------------------------------)[![David R. Winer](../Images/b35dd80eba0ee2f04fbf58bc1d90587c.png)](https://medium.com/@drwiner?source=post_page-----ea3be303166b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ea3be303166b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ea3be303166b--------------------------------) [David R. Winer](https://medium.com/@drwiner?source=post_page-----ea3be303166b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb2e18ae49c3e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftowards-generative-ai-for-model-architecture-ea3be303166b&user=David+R.+Winer&userId=b2e18ae49c3e&source=post_page-b2e18ae49c3e----ea3be303166b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ea3be303166b--------------------------------) ·11分钟阅读·2023年11月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fea3be303166b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftowards-generative-ai-for-model-architecture-ea3be303166b&user=David+R.+Winer&userId=b2e18ae49c3e&source=-----ea3be303166b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fea3be303166b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftowards-generative-ai-for-model-architecture-ea3be303166b&source=-----ea3be303166b---------------------bookmark_footer-----------)![](../Images/153affe1659f64de6f69bc02d4531694.png)

版权： [https://unsplash.com/photos/person-wearing-gas-mask-in-grayscale-photography-2PV6wdWVAMM](https://unsplash.com/photos/person-wearing-gas-mask-in-grayscale-photography-2PV6wdWVAMM)

“Attention is All You Need” 转换器革命对深度学习模型架构的设计产生了深远的影响。在 BERT 之后不久，出现了 RoBERTa、ALBERT、DistilBERT、SpanBERT、DeBERTa 等众多模型。接着是 ERNIE（现在仍然以“Ernie 4.0”活跃）、GPT 系列、BART、T5 等等。现在 [HuggingFace](https://huggingface.co/docs/transformers/model_doc/bert) 的侧边栏上形成了一个转换器架构的博物馆，新模型的出现速度只增不减。Pythia、Zephyr、Llama、Mistral、MPT 等等，每个都在准确性、速度、训练效率等指标上留下了印记。

我所说的模型架构，是指模型执行的计算图。例如，下面是 [Netron](https://github.com/lutzroeder/netron) 中的一个片段，展示了 T5 的部分计算图。每个节点要么是操作，要么是变量（操作的输入或输出），形成一个 [节点图架构](https://en.wikipedia.org/wiki/Node_graph_architecture)。

![](../Images/cfbf5f61a2251a9f7147279d1ed89529.png)

作者提供的图片

尽管架构如此之多，我们可以相当确定未来还会有更多的修改和突破。但每次都是人类研究者需要理解模型，提出假设，排除故障并进行测试。尽管人类创造力无穷无尽，但随着模型规模的扩大和复杂性的增加，理解架构的任务变得越来越困难。有了 AI 指导，也许人类可以发现那些没有 AI 帮助时需要更多年份或几十年才能发现的模型架构。

智能 **M**odel **A**rchitecture **D**esign (**MAD**) 是这样一个理念，即生成式 AI 可以引导科学家和 AI 研究人员更快、更轻松地找到更好、更有效的模型架构。我们已经看到大型语言模型（LLMs）在总结、分析、图像生成、写作辅助、代码生成等方面提供了巨大的价值和创造力。问题是，*我们能否利用相同的智能辅助和创造力来设计模型架构？* 研究人员可以根据直觉引导 AI 系统提出他们的想法，例如“层次扩展的自注意力”，或者进行更具体的操作，如“在我的模型最后一层添加 LoRA”。通过关联基于文本的模型架构描述，例如使用 [Papers with Code](https://paperswithcode.com/)，我们可以了解哪些技术和名称与特定模型架构相关联。

首先，我将讨论模型架构为何重要。接下来，我将介绍神经架构搜索、代码辅助和图学习中通向智能 MAD 的一些轨迹。最后，我将汇总一些项目步骤，并讨论 AI 设计和通过自主 MAD 自我改进的一些影响。

# 模型中心的 AI 回归

Andrew Ng 推动并创造了 “[以数据为中心](https://landing.ai/data-centric-ai/)” 人工智能，这对人工智能领域非常重要。借助深度学习，拥有干净且高质量的数据的投资回报率是巨大的，这在训练的每个阶段都得到了体现。为了说明背景，在 BERT 之前的文本分类领域，数据的丰度比质量更为重要。拥有足够的例子比例子的完美更为重要。这是因为许多人工智能系统没有使用可以被模型利用以应用实际泛化的预训练嵌入（或者说它们本身并不优越）。在 2018 年，BERT 为下游文本任务带来了突破，但领导者和从业者达成共识还需要更多时间，“以数据为中心”的人工智能理念有助于改变我们向人工智能模型输入数据的方式。

![](../Images/dbade515ee3749cabab4b7fc79e7c26c.png)

图片来源于作者

如今，许多人认为现有的架构“足够好”，比起编辑模型，提升数据质量更加重要。例如，现在有一个巨大的社区推动高质量训练数据集，如 [Red Pajama Data](https://github.com/togethercomputer/RedPajama-Data)。确实，我们看到许多大规模改进 LLM 的地方不在于模型架构，而在于数据质量和准备方法。

与此同时，每周都有一种新的方法涉及某种模型手术，显示出对训练效率、推理速度或整体准确性有很大影响。当一篇论文声称成为“新的 transformer”，如 [RETNET](https://arxiv.org/abs/2307.08621) 所示时，总会引起大家的讨论。*因为尽管现有的架构已经很好，但像自注意力这样的突破将对该领域及人工智能能够生产化的成就产生深远的影响*。即使是小的突破，训练也是昂贵的，因此你希望最小化训练次数。因此，如果你有明确的目标，MAD 对于获得最佳的投入回报也是重要的。

Transformer 架构巨大且复杂，这使得专注于以模型为中心的人工智能变得更加困难。我们正处于生成式人工智能方法日益先进、智能 MAD 即将实现的时期。

# 神经架构搜索

![](../Images/08781f5149cdfddeb8c7a7e3b8ec7424.png)

[https://arxiv.org/pdf/1808.05377.pdf](https://arxiv.org/pdf/1808.05377.pdf)

[**神经架构搜索**](https://en.wikipedia.org/wiki/Neural_architecture_search) **(NAS)** 的前提和目标与智能 MAD 一致，旨在减轻研究人员设计和发现最佳架构的负担。通常，这已经被实现为一种 AutoML，其中超参数包括架构设计，我看到它被纳入了许多超参数配置中。

NAS 数据集（即，[NAS benchmark](https://arxiv.org/pdf/2008.09777.pdf)）是一个机器学习数据集，<X, Y>，其中 X 是表示为图的机器学习架构，而 Y 是在特定数据集上训练和测试该架构时的评估指标。NAS 基准测试仍在发展中。最初，NAS 基准中的学习表示只是有序列表，每个列表表示一个神经架构作为操作序列，例如，[3x3Conv, 10x10Conv, …]等。这种表示方式不够低级，无法捕捉现代架构中的部分排序，例如“跳跃连接”，其中层既向前传递也向模型中的后续层传递。后来，[DARTS](/intuitive-explanation-of-differentiable-architecture-search-darts-692bdadcc69c) 表示法使用节点表示变量，边表示操作。最近，一些新的 NAS 技术被创造出来，以避免需要预定义的搜索空间，例如，[AGNN](https://arxiv.org/pdf/2206.09166.pdf)，它通过应用 NAS 来学习 GNN，以提高在基于图的数据集上的性能。

归根结底，在像 TensorFlow 或 PyTorch 这样的深度学习张量库中，只有大约 250 个原始级别的张量操作。如果搜索空间是从基本原理开始并包括所有可能的模型，那么它应该在其搜索空间中包括下一个 SOTA 架构变体。但实际上，NAS 并非如此设置。*技术可能需要相当于 10 年的 GPU 计算时间，而这还在搜索空间被放宽和以各种方式限制的情况下。* 因此，NAS 主要集中在重新组合现有的模型架构组件。例如，[NAS-BERT](https://arxiv.org/abs/2105.14444) 使用掩码建模目标来训练在 GLUE 下游任务上表现良好的较小 BERT 架构变体，从而实现自我蒸馏或压缩成更少的参数。[Autoformer](https://arxiv.org/abs/2106.13008) 采用了类似的方法，但搜索空间不同。

[高效 NAS](https://arxiv.org/abs/1802.03268)（ENAS）克服了需要全面训练和评估搜索空间中每个模型的问题。这是通过首先训练一个包含多个候选模型的超级网络，这些候选模型作为子图共享相同的权重来完成的。通常，候选模型之间的参数共享使 NAS 更具实用性，并允许搜索专注于架构变体，以最好地利用现有权重。

# 基于文本的 MAD 与基于图的 MAD

从生成式 AI 的角度来看，有机会在模型架构上进行预训练，并将这一基础模型用于将架构作为语言生成。这可以用于 NAS，也可以作为研究人员的一般指导系统，例如使用提示和基于上下文的建议。

从这个角度来看，主要的问题是是否将架构表示为文本还是直接表示为图。我们已经看到代码生成 AI 辅助工具的最近崛起，其中一些代码涉及 PyTorch、TensorFlow、Flax 等，相关于深度学习模型架构。然而，代码生成在这个用例中有许多限制，主要因为代码生成大多涉及表面形式，即文本表示。

另一方面，像图变换器这样的图神经网络（GNNs）非常有效，因为图结构无处不在，包括在 MAD 中。直接处理图的好处在于模型学习的是数据的底层表示，比表面文本表示更接近真实情况。尽管最近一些工作使得 LLMs 在生成图方面表现出色，比如 [InstructGLM](https://arxiv.org/abs/2308.07134)，但图变换器在极限情况下，尤其是与 LLMs 结合使用时，仍然有很大的潜力。

![](../Images/ac92f493b8d9d5f15ab3bfa5f0d8b112.png)

[https://arxiv.org/pdf/2308.07134.pdf](https://arxiv.org/pdf/2308.07134.pdf)

无论你使用 GNNs 还是 LLMs，图比文本更适合作为模型架构的表示，因为重要的是底层计算。TensorFlow 和 PyTorch 的 API 不断变化，代码行涉及的不仅仅是模型架构，还包括一般的软件工程原则和资源基础设施。

有几种方式可以将模型架构表示为图，这里我仅回顾了几种类别。首先，有像 [GLOW](https://github.com/pytorch/glow/blob/master/docs/IR.md)、[MLIR](https://mlir.llvm.org/) 和 [Apache TVM](https://tvm.apache.org/) 这样的代码机器学习编译器。这些可以将像 PyTorch 代码这样的代码编译成中间表示，形式可以是图。TensorFlow 已经有一个中间图表示，你可以使用 TensorBoard 进行可视化。还有一个 [ONNX](https://onnx.ai/) 格式，可以从现有的保存模型编译而来，例如，使用 [HuggingFace](https://huggingface.co/docs/transformers/serialization)，非常简单：

```py
optimum-cli export onnx --model google/flan-t5-small flan-t5-small-onnx/
```

这个 ONNX 图序列化看起来类似于（小片段）：

![](../Images/483a102cd859f6e1d8a5e1b992ee7645.png)

图片由作者提供

这些编译后的中间表示的一个问题是它们在高级别上难以理解。Netron中T5的ONNX图非常庞大。一个更易于人类理解的模型架构图选项是[Graphbook](https://github.com/cerbrec/graphbook)。免费的Graphbook平台可以显示图中每个操作所产生的[值](https://medium.com/towards-artificial-intelligence/visual-walkthrough-for-vectorized-bertscore-to-evaluate-text-generation-b9ed61e6fdfe)，并且可以显示张量形状和类型不匹配的地方，同时支持编辑。此外，AI生成的模型架构可能并不完美，因此能够轻松进入并编辑图表，排查其无法工作的原因是非常有用的。

![](../Images/ac99e40a0f439e8144486c08201ae95c.png)

Graphbook图的示例视图，作者提供的图像

虽然Graphbook模型只是JSON格式的，但它们是分层的，因此允许更好的[抽象层次](https://medium.com/@drwiner/a-fully-visualized-and-honest-implementation-of-bert-9dbc066e7bd)。见下图，GPT2架构的分层结构侧视图。

![](../Images/8ebaf0205c6ea8f4237a52ea57400a3b.png)

显示Graphbook图分层结构的图像，这里以GPT为例。完整图像：[https://photos.app.goo.gl/rgJch8y94VN8jjZ5A](https://photos.app.goo.gl/rgJch8y94VN8jjZ5A)，作者提供的图像

# MAD步骤

这是生成MAD的提案大纲。我想包含这些子步骤，以更具体地描述如何处理这个任务。

1.  *代码到图*。从代码（如HuggingFace模型卡）创建一个MAD数据集，将模型转换为图形格式，如ONNX或Graphbook。

1.  *创建数据集*。这些数据集可以是对图中操作类型的分类、对图本身的分类、操作和链接的遮罩/恢复、检测图是否不完整、将不完整的图转换为完整的图等。这些可以是自监督的。

1.  *图标记器*。对图进行标记。例如，将图中的每个变量设为唯一的词汇ID，并生成可以输入到GNN层的邻接矩阵。

1.  *GNN设计*。开发一个GNN，使用图形标记器的输出输入到变换器层中。

1.  *训练和测试*。在数据集上测试GNN，并进行迭代。

一旦这些步骤明确后，它们可以作为NAS方法的一部分，用于指导GNN的设计（步骤4）。

# 最终说明：自我改进的意义

![](../Images/0ddc40735a5f09eb1752ec0fd4ef435e.png)

作者提供的图像

我想给出一些关于自主MAD的影响的说明。AI能够设计模型架构的意义在于它可以改进自身脑部的结构。通过某种链/图的思维过程，模型可以迭代生成自身架构的后续状态并进行测试。

1.  起初，AI给出模型架构，针对生成模型架构的特定数据进行训练，并可以提示生成架构。它可以访问包括自身架构设计在内的训练源，训练源包括围绕架构任务的各种测试，如图分类、操作/节点分类、链接完成等。这些遵循你在[Open Graph Benchmark](https://ogb.stanford.edu/)中找到的一般任务。

1.  起初，在应用级别上，有一种代理可以训练和测试模型架构，并将其添加到AI的训练源中，也许可以提示AI关于什么有效和什么无效的某些指示。

1.  迭代地，AI生成一组新的模型架构，并且代理（让我们称之为MAD代理）训练和测试它们，为它们评分，将其添加到训练源中，指导模型重新训练，依此类推。

本质上，不仅仅使用AutoML/NAS来搜索模型架构空间，学习模型架构作为图，并使用图变换器来学习和生成。让基于图的数据集本身作为图模型架构进行表示。表示图数据集的模型架构**以及**用于学习图数据集的可能模型架构空间变得相同。

这意味着什么？每当一个系统具有自我改进的能力时，就可能存在潜在的风险，可能导致失控效应。如果设计了上述情况，并且它是在更复杂的代理上下文中设计的，一个可以无限期地选择数据源和任务，并协调成为一个多万亿参数的端到端深度学习系统，那么可能存在一些风险。但未说的困难部分是更复杂代理的设计、资源分配，以及支持更广泛能力的许多困难。

# 结论

在自主AI模型架构设计（MAD）中使用的AI技术可能有助于未来AI研究人员发现新的突破技术。历史上，MAD已经通过神经架构搜索（NAS）进行探索。结合生成AI和transformers，可能会有新的机会来帮助研究人员并进行发现。
