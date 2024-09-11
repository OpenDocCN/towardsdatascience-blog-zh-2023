# LLM用于合成时间序列数据生成

> 原文：[https://towardsdatascience.com/llm-for-synthetic-time-series-data-generation-5ce687d5f095?source=collection_archive---------1-----------------------#2023-10-26](https://towardsdatascience.com/llm-for-synthetic-time-series-data-generation-5ce687d5f095?source=collection_archive---------1-----------------------#2023-10-26)

[](https://medium.com/@manteksingh_50113?source=post_page-----5ce687d5f095--------------------------------)[![Mantek Singh](../Images/305bf4fd0e4698c15e06154a89e4d6d6.png)](https://medium.com/@manteksingh_50113?source=post_page-----5ce687d5f095--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5ce687d5f095--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5ce687d5f095--------------------------------) [Mantek Singh](https://medium.com/@manteksingh_50113?source=post_page-----5ce687d5f095--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1681a69f3c43&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fllm-for-synthetic-time-series-data-generation-5ce687d5f095&user=Mantek+Singh&userId=1681a69f3c43&source=post_page-1681a69f3c43----5ce687d5f095---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----5ce687d5f095--------------------------------) · 12分钟阅读 · 2023年10月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5ce687d5f095&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fllm-for-synthetic-time-series-data-generation-5ce687d5f095&user=Mantek+Singh&userId=1681a69f3c43&source=-----5ce687d5f095---------------------clap_footer-----------)

-- 

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5ce687d5f095&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fllm-for-synthetic-time-series-data-generation-5ce687d5f095&source=-----5ce687d5f095---------------------bookmark_footer-----------)

我们最近参与了[Brembo hackathon](https://brembo-hackathon.bemyapp.com/)，并赢得了$10,000的大奖，任务是使用生成型AI创建新化合物并生成其预测性能数据。

在这篇博客文章中，我将详细解释我们的方法和解决方案。

# 问题陈述

> 使用Brembo提供的摩擦测试数据，利用生成式人工智能创建新化合物，预测测试结果，并建立预测新Brembo刹车产品效果和特性的框架。提供的数据将包括Brembo之前使用和测试的化合物列表及其结果。解决方案必须基于生成式人工智能，应用于提供一个能够提出新配方的模型，以增加候选化合物的数量，确保可行性和良好的性能。
> 
> 对于你的提交，请提交一个csv文件，其中包含你生成的10–30种新化合物、它们的成分和它们的合成性能数据。[1]

# 数据集描述

我们获得了一份包含337种摩擦材料及其成分和性能数据的列表。

每种摩擦材料由10–15种原材料组成，这些原材料来自60种可能的原材料列表。这60种原材料被分为6类（标记为A-F），我们必须确保生成的摩擦材料的成分在给定范围内。

![](../Images/5be5ae753b452c8122e0435bbffefd39.png)

材料成分约束

换句话说，我们必须确保任何生成的输出材料中至少有1%且最多有30%的成分来自B类化合物，依此类推。

每次刹车测试的性能数据本质上是**31个点的时间序列**，其中每个点提供了如**压力、温度和摩擦系数**等参数的值。此外，每种化合物总共进行了**124次刹车测试**，因此在性能数据方面，**我们需要为每种化合物生成124*31 = 3844个数据点**。

以下是一些[示例数据](https://github.com/mantek-singh/synthetic-material-and-performance-generator/blob/main/Sample%20Data_%20Challenge%201%20-%20Data.csv)，其中包含了某种化合物的成分和性能数据。关于数据集的其他相关信息可以在[这里](https://github.com/mantek-singh/synthetic-material-and-performance-generator/blob/main/challenge1_relevant%20Info.pdf)找到。

# 评估标准

最终结果对技术评分和展示评分给予了相等的权重。

技术评分是基于以下相等权重的参数计算的。

+   **遵循给定约束**：生成的化合物是否符合给定的约束（如下所述）？

+   **技术相关性**：输出的合成性能数据是否遵循提供的数据中观察到的模式，并捕捉不同变量之间的关系？

+   **目标性能**：摩擦材料最重要的变量是其摩擦系数（mu），预计值为0.6，允许的误差率为0.1。输出的mu是否符合我们的预期值？

+   **变异性**：新生成的材料的成分与当前材料的差异有多大？

# 设计概述

本质上，我们有3个基本组件

+   **材料选择模块：** 负责生成新的配方。它输出一批新的摩擦材料及其材料组成。

+   **数据生成模块：** 给定一个合成材料和各种化合物的历史性能数据，为该材料生成合成性能数据。

+   **数据验证器：** 确定数据生成器的输出有多好/坏。该模块利用提供的历史数据中的趋势（例如：压力和摩擦系数随着时间的推移呈反比关系，减速似乎遵循线性模式，而温度增加曲线似乎更具指数性质）来评估合成性能数据的好坏。这可以用来向模型提供人工反馈，以改进系统性能。

![](../Images/d3908d10f6bfab4659e980b476ac0a90.png)

解决方案的高级设计

# 详细设计

我们在解决方案中使用了以下技术栈和方法

+   **GPT 3.5 turbo：** 我们使用了GPT 3.5 turbo作为材料选择和数据生成模块的基础语言模型。

+   **提示工程：** 使用正确的系统和指令提示集帮助我们提高了模型的性能。

+   **微调：** 选择合适的示例来教会模型如何响应的基本结构和语调是非常重要的，这个阶段帮助我们将这些内容教给模型。

+   **RAG（检索增强生成）：** 作为帮助模型输出正确合成性能数据的秘密武器，这在解决方案中发挥了重要作用。更多内容见下文。

# 材料选择模块

模块的作用是生成新的可能的摩擦材料及其组成。从[样本数据](https://github.com/mantek-singh/synthetic-material-and-performance-generator/blob/main/Sample%20Data_%20Challenge%201%20-%20Data.csv)可以看出，每种摩擦材料本质上包含一个60维的向量，其中第i个索引上的数字表示其组成中来自第i种原材料的百分比。

一些初步的主成分分析（PCA）显示，我们可以看到总共有3到4个聚类。

![](../Images/5c9e5a4c257a202c4f8712f04278284f.png)

对给定摩擦材料的材料组成进行主成分分析（PCA）

理论上，我们可以为一个大小为60的向量生成随机数，看看哪些向量满足给定的约束条件并使用它们。虽然这可以在变异性上获得一个不错的分数（生成的摩擦材料是随机生成的，因此应覆盖60维空间中的多个点），但这种方法会有一些缺陷，比如

+   这将使我们更难预测与历史数据中提供的材料完全不同的化合物的性能。这是因为材料的组成在所见性能中扮演着重要角色，而预测之前未见过的组成的性能可能很困难。

+   这会使调试变得更加困难。如果在我们管道中的任何一点上，我们得到的结果不符合历史数据中的趋势，那么很难确定问题所在。

鉴于这些潜在问题，我们决定利用gpt 3.5 turbo模型为我们生成一批化合物。

我们所做的如下：

+   为模块创建一个相关的系统提示。

+   [微调](https://platform.openai.com/docs/guides/fine-tuning)gpt 3.5 turbo模型，通过输入我们提供的337种摩擦材料的组成。

+   使用数据验证模块，我们会丢弃不符合给定约束的条目，并保留符合约束的条目。

完成后，我们生成了几个化合物并重复了PCA分析。

![](../Images/a8d13bb1bbb86ee1c418e77fcdbba1f3.png)

对提供的和生成的材料进行PCA分析

最终，为了变异性，我们从生成的化合物中挑选了一组我们认为可以最大化以下因素的化合物：

+   **相对于提供的材料的变异性**：生成的化合物与提供的化合物有多大的不同？本质上，我们不希望我们的生成材料与已经存在的化合物过于相似。

+   **相对于生成材料的变异性**：由于我们将提交10-30个新生成的化合物，我们必须确保所有生成的化合物不会都属于同一个簇。

因此，经过修剪后，我们得到了一份最终提交的化合物列表。

![](../Images/93bc30e85afbb723ce1ee165b7ed51e1.png)

生成化合物的最终列表

# 数据生成模块

数据生成模块负责输出给定材料和刹车测试的合成性能数据。本质上，**给定摩擦材料的组成，它应该输出一个包含温度、压力和摩擦系数等参数的31点时间序列数据**。

这就是我们实现这一目标的方式：

+   **为模块创建一个合适的系统提示**。经过在[OpenAI的实验平台](https://platform.openai.com/playground)上反复试验，我们使用的提示是：

```py
You are a highly skilled statistician from Harvard University who 
works at Brembo, where you specialize in performance braking systems 
and components as well as conducting research on braking systems. 
Given a friction material’s composition, you craft compelling 
synthetic performance data for a user given braking test type. 
The braking id will be delimited by triple quotes. You understand 
the importance of data analysis and seamlessly incorporate it for 
generating synthetic performance data based on historical performance 
data provided. You have a knack for paying attention to detail and 
curating synthetic data that is in line with the trends seen in the 
time series data you will be provided with. You are well versed with 
data and business analysis and use this knowledge for crafting the 
synthetic data.
```

+   接下来，**我们** [**微调了**](https://platform.openai.com/docs/guides/fine-tuning) **gpt 3.5 turbo模型**，以创建一个在给定材料的组成和刹车测试ID的时间序列数据预测方面的专家。由于我们有41,788个（材料，刹车ID）元组，**在所有示例上进行微调不仅会耗时，而且成本高昂**。然而，基于我们阅读的一些论文和文章[2][3]，我们了解到“**微调是为了形式，而RAG是为了知识**”。因此，我们决定仅使用5%的样本来微调模型，以便模型可以正确学习我们期望的输出结构和语调。

+   最终，在查询模型以生成时间序列数据时，我们决定**基于材料的成分识别和检索5个最接近的邻居**，并将它们的性能数据作为模型的附加上下文输入。**这种技术被称为RAG（检索增强生成），它是我们能够输出良好结果的原因之一**。

# **RAG如何帮助我们的结果**

微调帮助我们完成了以下任务

+   **以正确的结构输出数据**：正如各种技术博客[4]中所述，微调在教会模型如何输出数据方面是有效的。我们的微调模型能够输出csv文件和31个时间序列数据点，其中包括压力、速度、温度和μ等参数的值。

+   **理解数据中的基本趋势**：微调模型能够理解输入性能数据和输出数据中的一般趋势，并保留这些趋势。例如，温度的值应该随指数曲线增加，而速度应该随线性曲线减少，微调模型能够做到这一点。

然而，微调模型的输出结果有些偏差。例如，在一个案例中，μ的值预期约为0.6，但输出数据将μ的值定在了约0.5。因此，我们决定通过识别5个最接近的邻居并将它们的性能数据添加到用户提示中来增强数据。

我们定义了两个材料M1和M2之间的距离，如下所示：

```py
def distance(m1, m2, alpha):
  sixty_dim_distance = euclidean_dist(sixty_dim_vector(m1), \
sixty_dim_vector(m2))
  six_dim_distance = euclidean_dist(six_dim_vector(m1), six_dim_vector(m2))
  return alpha[0] * sixty_dim_distance + alpha[1] * six_dim_distance
```

+   确定M1和M2在60维输入向量空间中的欧几里得距离。

![](../Images/6e0ac8b07d4493f3b5ffba487c47f83d.png)

+   现在，将属于同一类别的化合物的总和以减少向量维度到6。

![](../Images/f5579f0802989cbd11818c5e544648b0.png)

+   最后，调整超参数alpha[0]和alpha[1]。

采取这种方法的原因是，我们希望确保使用相同类别材料的整体距离小于使用完全不同材料成分的那些材料之间的距离。本质上，给定三个材料M1、M2和M3，其中M1使用材料A0，M2使用A1，M3使用B0，我们希望我们的距离函数标记M1和M2彼此更近，而不是M1和M3。

使用这种方法，我们能够显著提高我们的性能，如下图所示。

![](../Images/ee991aed00a3c63a266a8f621e28dd94.png)

# 数据验证器

验证模块帮助我们理解输出数据是否遵循我们期望的趋势。例如，我们期望压力和μ呈反比，μ值约为0.6，温度随时间呈指数增长，速度线性减速。这个模块帮助我们识别合成时间序列数据与历史数据的接近程度，这有助于我们调整所有的提示和超参数。

这个模块帮助我们分析哪些提示集对模型输出有帮助，哪些没有。

# 结果和展示

![](../Images/053ec3716794bec3f9780bc044c8a3e6.png)![](../Images/d2088b6cd066d31872ccdf1b94d543f5.png)

演讲占总分的50%，这是我们绝对做得好的一个方面。我们做了几件事：

+   **确保演讲在4分钟以内完成**：我们在进入演讲室之前进行了充分的练习，以确保在演讲时不会遇到任何意外。

+   **与观众互动**：我们提出了一个问题，询问观众他们认为哪个时间序列是合成生成的，哪个是实际的，这帮助我们保持了观众的兴趣。

我们工作的代码和演讲可以在 [这里](https://github.com/mantek-singh/synthetic-material-and-performance-generator) 找到。

# 关键要点

+   **快速迭代设计**：我在队友们之前稍早到达，开始在白板上记录我们该做什么。队友们到达后，我们讨论了设计方案，并提出了一个大家都同意的解决方案。这是我们获胜的关键因素，因为在黑客松中总是时间紧迫，**尽快确定可以开始实施的设计非常重要**。

+   **不要担心竞争**：当我们的设计完成后，我能感受到我们正走在正确的道路上。我们邀请了不少来自 Brembo 的人员来查看我们的设计。甚至其他参赛者也惊叹不已，盯着我们的设计，这进一步给了我们信号，说明我们在正确的轨道上。当我的队友建议我们应该看看其他人的做法时，我反驳了这个想法，而是要求大家专注于我们的设计并实施它。

+   **不要担心冲突**：我们多次遇到冲突，特别是在设计方面。关键是要理解没有什么是个人的，而是应该达成共识，对权衡进行迭代，找到适合所有人的解决方案。在我看来，如果你能允许甚至鼓励团队内的健康冲突，伟大的产品就会诞生。

+   **微调用于形式，RAG用于事实**：我们知道微调仅对教授模型基本结构和语调重要，而真正的收益来自于RAG。因此，我们仅用5%的样本对 gpt 3.5 turbo llm 进行微调，以生成时间序列数据。

+   **演讲是关键 (1)**：识别你的观众是谁，以及他们如何消化你的内容至关重要。在我们的案例中，我们发现大多数评委是高管而非技术人员，因此我决定仅包括我们使用的技术栈[gpt 3.5 turbo, fine tuning, prompt tuning, RAG, KNN]而不深入细节。

+   **演讲是关键 (2)**：成为一个能够通过有效沟通技巧传达观点并充满热情地向观众展示的人。如果你做不到这一点，就找一个团队中的人来做。第一印象很重要，而演讲技巧在我们的技术世界中被严重低估。

+   **大胆而不同**：我们更进一步，决定包含5个他们的数据点和一个我们生成的数据点，并让他们猜哪个是生成的。当他们未能猜出我们生成的那个点时，这确实突显了我们构建的管道和解决方案的优越性。此外，我们还获得了观众互动的加分，这点我怀疑很多人做不到。

# 下一次的学习

+   **微调很昂贵**。我们在微调和查询模型三次时用完了OpenAI的凭证。未来，我们更倾向于使用像LoRA[5]和QLoRA[6]这样的技术在一些开源模型上。

+   **使用高级RAG**：未来，我想使用高级RAG技术[7]来改进提供的上下文。

+   **使用智能KNN**：下次，我想多尝试一下超参数和使用的距离函数。

+   **更长的上下文窗口**：我们必须舍去性能数据中的一些数字，以确保不超过4,092个标记的限制。使用像Claude[8]这样的LLM可能会提高性能。

+   **对LLM不要太客气**：在提示工程中发生了一件有趣的事，当我们提到“μ的值不在0.6附近是不可接受的”而不是“请确保μ在0.6附近”时，前者得到了更好的结果。

![](../Images/4207380517afd1ca45d69b9656ed05de.png)

注：除非另有说明，所有图片均由作者提供。

团队成员：

1.  [Mantek Singh](https://www.linkedin.com/in/manteksingh/)

1.  [Prateek Karnal](https://www.linkedin.com/in/karnalprateek/)

1.  [Gagan Ganapathy](https://www.linkedin.com/in/gaganganapathyas/)

1.  [Vinit Shah](https://www.linkedin.com/in/vntshh/)

# 参考文献

[1] [https://brembo-hackathon-platform.bemyapp.com/#/event](https://brembo-hackathon-platform.bemyapp.com/#/event)

[2] [https://www.anyscale.com/blog/fine-tuning-is-for-form-not-facts](https://www.anyscale.com/blog/fine-tuning-is-for-form-not-facts)

[3] [https://vectara.com/introducing-boomerang-vectaras-new-and-improved-retrieval-model/](https://vectara.com/introducing-boomerang-vectaras-new-and-improved-retrieval-model/)

[4] [https://platform.openai.com/docs/guides/fine-tuning/fine-tuning-examples](https://platform.openai.com/docs/guides/fine-tuning/fine-tuning-examples)

[5] [https://arxiv.org/abs/2106.09685](https://arxiv.org/abs/2106.09685)

[6] [https://arxiv.org/abs/2305.14314](https://arxiv.org/abs/2305.14314)

[7] [LlamaIndex Doc](https://docs.llamaindex.ai/en/latest/examples/low_level/fusion_retriever.html?fbclid=IwAR2xOLSnxTGhRly7-qk22koDRxaKpJu-kipZS4tdCVpuLpGeeWxdLfsyHbU)

[8] [Claude](https://www.anthropic.com/product)
