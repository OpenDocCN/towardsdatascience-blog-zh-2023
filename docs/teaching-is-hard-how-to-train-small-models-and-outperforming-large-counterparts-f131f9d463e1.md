# 教学很难：如何训练小模型并超越大型对手

> 原文：[`towardsdatascience.com/teaching-is-hard-how-to-train-small-models-and-outperforming-large-counterparts-f131f9d463e1`](https://towardsdatascience.com/teaching-is-hard-how-to-train-small-models-and-outperforming-large-counterparts-f131f9d463e1)

## |模型蒸馏|人工智能|大型语言模型|

## 蒸馏大型模型的知识是复杂的，但一种新方法显示出惊人的性能

[](https://salvatore-raieli.medium.com/?source=post_page-----f131f9d463e1--------------------------------)![Salvatore Raieli](https://salvatore-raieli.medium.com/?source=post_page-----f131f9d463e1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f131f9d463e1--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f131f9d463e1--------------------------------) [Salvatore Raieli](https://salvatore-raieli.medium.com/?source=post_page-----f131f9d463e1--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f131f9d463e1--------------------------------) ·阅读时间 12 分钟·2023 年 11 月 11 日

--

![](img/a16ec7ef78aa98b058f1e94098af933b.png)

图片由 [JESHOOTS.COM](https://unsplash.com/@jeshoots?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

[大型语言模型](https://en.wikipedia.org/wiki/Large_language_model)（LLMs）和少样本学习已经证明我们可以将这些模型用于未见过的任务。然而，这些技能是有代价的：大量的参数。这意味着你还需要一个专业化的基础设施，并且将最先进的 LLMs 限制在只有少数几家公司和研究团队中。

+   我们真的需要为每个任务设计一个独特的模型吗？

+   是否有可能创建专门的模型来替代它们用于特定的应用？

+   我们如何才能拥有一个在特定应用中与大型 LLMs 竞争的小模型？我们是否确实需要大量的数据？

在这篇文章中，我对这些问题给出了答案。

> “教育是人生成功的关键，教师在学生的生活中留下了深远的影响。” ——所罗门·奥尔蒂斯

# 匹配冠军！

![](img/b6319bfeb892fa586fd258fd3e5904bd.png)

图片由 [Fauzan Saari](https://unsplash.com/@fznsr_?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> 教学的艺术是协助发现的艺术。——马克·范·多伦

[大型语言模型（LLMs）](https://en.wikipedia.org/wiki/Large_language_model)展现了革命性的能力。例如，研究人员对像[上下文学习](https://ai.stanford.edu/blog/understanding-incontext/)这样的难以捉摸的行为感到惊讶。这导致模型规模的增加，越来越大的模型[寻找新能力](https://openai.com/research/scaling-laws-for-neural-language-models)，这些能力超出了参数的数量。

[](/all-you-need-to-know-about-in-context-learning-55bde1180610?source=post_page-----f131f9d463e1--------------------------------) ## 关于上下文学习的一切

### 什么是大型语言模型，它是如何工作的，以及是什么使大型语言模型如此强大

[towardsdatascience.com [](/emergent-abilities-in-ai-are-we-chasing-a-myth-fead754a1bf9?source=post_page-----f131f9d463e1--------------------------------) ## 人工智能中的涌现能力：我们是否在追逐一个神话？

### 对大型语言模型出现特性的观点变化

[towardsdatascience.com

但这会有代价；例如，[GPT-3](https://en.wikipedia.org/wiki/GPT-3)（超过 175 万亿个参数）至少需要 350 GB 的 GPU 来运行。这意味着你需要专门的基础设施来训练和使用它进行[推理](https://cloud.google.com/bigquery/docs/inference-overview)。将这样的模型部署以使其公开访问需要克服重大挑战和成本（尤其是如果你想减少延迟）。**因此，只有少数公司能够负担得起为实际应用部署一定规模的模型。**

拥有超过 100 B 参数的模型具有大型建模能力，但这些能力分散在许多技能上。相比之下，少于 10 B 的模型建模能力较弱，但可以将这种能力集中于单一任务。例如，推理是超过 100 B 参数模型展示的一种能力，但在小型模型中缺失。[这项研究的作者](https://arxiv.org/abs/2301.12726)表明，推理只是大型 LLM 中的众多能力之一。因此，将小型模型的训练重点放在推理上，即使模型小于 100 B，也可以获得显著的结果。

当然，专注于小型模型会有代价：对其他任务的表现。但通常你只对一个任务感兴趣，因此可以使用小型模型。

![](img/2511991c35ab392135d6736ea8a5e38a.png)

以前的研究表明，推理能力随着规模的增加而突然出现（左侧图）。这项研究的作者表明，通过专注于推理任务（专业化），你可以在推理方面取得良好的结果。图片来源：[这里](https://arxiv.org/abs/2301.12726)

因此，几家公司专注于仅对特定任务表现良好的小模型。此外，[**微调**](https://en.wikipedia.org/wiki/Fine-tuning_(deep_learning))的使用使得为特定应用创建小型专业模型成为可能。对于一些任务，如分类，微调需要一个带注释的数据集。收集这些带注释的数据集是昂贵的，因此使用的另一种技术是[蒸馏](https://en.wikipedia.org/wiki/Knowledge_distillation)。

**蒸馏**是一种技术，通过它你可以利用从更大模型生成的标签来训练一个小模型。收集这些未标记的数据集可能同样昂贵（例如，在医疗领域）。性能要求越高，成本也就越高。因此，使用微调或蒸馏来实现与大型语言模型（LLM）相同的性能可能在计算上是昂贵的。

> 因此，我们如何才能使小模型以数据和时间高效的方式从 LLM 中学习呢？

# 如何让 LLM 成为高效的教师

![](img/45bb89ea853b0b0f6619552f333d2df3.png)

照片由 [ThisisEngineering RAEng](https://unsplash.com/@thisisengineering?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> 我不能教任何人任何东西；我只能让他们思考。——苏格拉底

当我们想训练一个小模型时，LLM 要么用来为未标记的文本生成标签，要么用于[数据增强](https://en.wikipedia.org/wiki/Data_augmentation)（从 LLM 生成的示例数据集中提取）。直观上，这可能不足以使模型学习高效。

例如，如果我想让我的小模型学习如何对推文进行排序（积极、消极或中立），我可以下载大量推文，通过 LLM 生成标签，然后用这些标签训练小模型。

![](img/9b2f5ce4a0dbf71ce5fdbfc0b6dc6ec6.png)

蒸馏的示意图。图片由作者提供

**然而，虽然这对于像推文分类这样的简单任务有效，但对于更复杂的任务来说是不够的。** 我们可能会从互联网上下载谜题并让 LLM 解决它们，但解决方案本身并未提供关于解决过程的任何信息。一个通过解决方案训练的小模型不会学会如何解谜。

确实，要学会解决困难的任务（例如解谜），你需要比仅仅解决方案更多的信息。

实际上，这对于 LLM 也是如此，对于推理任务（算术、常识和符号推理），提供[链式思维](https://www.promptingguide.ai/techniques/cot)的上下文有助于模型得出解决方案而不会产生幻觉。

![](img/f304565b4553c321fc000579a7519ba6.png)

图片来源：[这里](https://arxiv.org/abs/2201.11903)

基于这一意图，一些[谷歌研究人员](https://blog.research.google/2023/09/distilling-step-by-step-outperforming.html)甚至训练了在特定任务上超越 LLM 的小模型（770M 参数与 540B[PaLM](https://ai.google/discover/palm2/)）。他们随后在最近发表的一篇论文中描述了这种方法。

[](https://arxiv.org/abs/2305.02301?source=post_page-----f131f9d463e1--------------------------------) [## 逐步提炼！用更少的训练数据和更小的模型超越大型语言模型…](https://arxiv.org/abs/2305.02301?source=post_page-----f131f9d463e1--------------------------------)

### 部署大型语言模型（LLMs）具有挑战性，因为它们在内存使用上效率低下，并且计算密集型……

[arxiv.org](https://arxiv.org/abs/2305.02301?source=post_page-----f131f9d463e1--------------------------------)

简而言之，作者利用了 LLM 进行推理的能力（超越单纯生成标签）。**通过使用一个未标记的数据集，他们要求 LLM 生成正确的标签和推理**（为什么这是最合适的标签的自然语言解释）。之后，他们使用标签和推理来训练小模型。

![](img/260f62b7e824bd0a1843e11651594841.png)

该方法的示意图。图像由作者提供

**通过这种方式，他们不仅向小模型提供了问题的解决方案，还提供了老师（LLMs）如何得出该解决方案的过程。** 此外，推理不仅包含解释，还包含理解任务的有用元素（这些元素从简单的输入中不易推断出，特别是对于参数有限的模型）。

![](img/1705884062947f3167cb0b1628621e0e.png)

图片来源：[这里](https://arxiv.org/abs/2305.02301)

## 逐步提炼

更详细地说，作者使用了与[链式思维（CoT）](https://arxiv.org/abs/2201.11903)相同的提示。一个提示包括一个问题、背景或推理，以及问题的答案。之后，将推理附加到问题上，模型必须给出答案。

![](img/8d47874aced51dc7f61a3787faf8ecd2.png)

图片来源：[这里](https://arxiv.org/abs/2305.02301)

小模型通过**简单的多任务方法**进行训练：它不仅需要预测正确的标签，还需要生成相应的推理。[损失函数](https://en.wikipedia.org/wiki/Loss_function)也会考虑生成推理时是否出现错误。

![](img/22a15086a1ef2fee84b316f0fe4f05d0.png)

通过这种方式，作者迫使模型生成中间推理步骤，从而引导模型找到正确的答案。

从比喻的角度来看，这就像是一个老师强迫学生写下所有的推理步骤，而不是直接给出答案。这种方法的优点是，在测试时，模型将不再需要老师模型（LLM），而应该学会进行推理。

# 我们能否教会学生推理？

![](img/d9501579775396f5c29ba346354f5818.png)

图片由[Element5 Digital](https://unsplash.com/@element5digital?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> 你告诉我，我会忘记。你教我，我会记住。你让我参与，我会学习。 – 本杰明·富兰克林

作者使用 PaLM (540 B 参数)作为 LLM 生成理由。他们选择使用[T5](https://arxiv.org/abs/1910.10683)作为小模型，使用现有的预训练权重检查点。有趣的是，作者使用了一个已经训练过的非常小的模型。**通过这种方式，他们使用一个已经具备一般语言知识的模型，但可以适应特定任务。**

![](img/2bb515c5763c2f132286a68a6666a01c.png)

模型比较，以更好地理解大小差异（圆圈按比例）。图片由作者提供，生成图片的脚本可以在[这里](https://github.com/SalvatoreRa/tutorial/blob/main/other/Code_for_Teaching_is%C2%A0Hard.ipynb)找到

他们选择了三个特定的自然语言处理任务：

+   [**自然语言推理**](https://nlpprogress.com/english/natural_language_inference.html)**。** 他们使用了两个不同的数据集：e-SNLI 和 ANLI。

+   [**常识问答**](https://arxiv.org/abs/1811.00937) **(CQA)。**

+   **算术数学应用题 (SVAMP)。**

如所示，这些任务和数据集要求模型展示推理能力。

在文章中，该方法与两种经典方法进行了比较：

+   [**微调**](https://d2l.ai/chapter_computer-vision/fine-tuning.html)**。** 其中预训练模型在带有正确标签的注释示例上进行训练。

+   [**蒸馏**](https://neptune.ai/blog/knowledge-distillation)**。** 在该方法中，LLM 用于生成真实标签。

结果显示，新的方法（*逐步提炼*）在所有基准数据集和任务中都优于标准微调，同时所需示例也远少于达到更好表现的标准。因此，这种方法性能更佳，同时成本更低（仅有 12.5%的示例表现超过传统微调）。

![](img/89f5d43ffd244f5e602ddf7126606701.png)

图片来源：[这里](https://arxiv.org/abs/2305.02301)

对于标准蒸馏而言，同样的新方法在性能上更优，并且所需的示例数量也少得多。

![](img/93ab3c85614ed66d327f10649bce775b.png)

图片来源：[这里](https://arxiv.org/abs/2305.02301)

作者们随后使用不同版本的模型（220M、770M、11B）采用相同的方法，并与 LLM 基线（PaLM）进行比较。结果表明，新方法根据规模提高了性能（更大的模型表现更好）。此外，逐步蒸馏在某些任务上似乎甚至超越了 LLM 基线。换句话说，770M 模型在 ANLI 中超越了一个大 700 倍的模型。更令人印象深刻的是，在 e-SNLI 中，一个 220M 的模型超越了一个大 2000 倍的模型。

![](img/c910902ea5c7d937496b0624ed16a6cb.png)

图片来源: [这里](https://arxiv.org/abs/2305.02301)

在标准微调中，我们使用人工标注的数据，而在蒸馏中，我们使用未标注的数据。结果类似，显示模型即使从 LLM 标注的数据中也能学习。

![](img/543183a4dd5a3cf5b84665b2b0b95845.png)

图片来源: [这里](https://arxiv.org/abs/2305.02301)

**这些结果本身已经很令人印象深刻，但令人难以置信的是你不需要整个数据集。** 即使仅用 0.1% 的数据集，该方法仍然有效。对于标准的微调和任务蒸馏，您需要更多的示例才能看到显著的性能。在 ANLI 中，对于 T5-770M，80% 的示例足以超越 PaLM 540B。即使使用完整的数据集，标准微调也无法达到 LLM 基线。

![](img/160a8530e0ac01d2159ae2238c7b235b.png)

图片来源: [这里](https://arxiv.org/abs/2305.02301)

![](img/a51fd497ee7c1462d2a33983a1345dfc.png)

图片来源: [这里](https://arxiv.org/abs/2305.02301)

正如作者所提到的，尽管这种方法也适用于其他模型（如 20B GPT-NeoX 模型），但结果不如预期。这是因为 PaLM 提供了更高质量和更详细的推理。

![](img/e5999ff2b5c293d03d099e888ff1f58a.png)

图片来源: [这里](https://arxiv.org/abs/2305.02301)

在一个消融研究中，他们注意到多任务训练效果更好。换句话说，让模型生成推理有助于它的学习。

![](img/4c69bf30d74f4a09174cecd2ecdc626e.png)

图片来源: [这里](https://arxiv.org/abs/2305.02301)

作者们也发布了供社区测试的代码：

[## GitHub - google-research/distilling-step-by-step](https://github.com/google-research/distilling-step-by-step?source=post_page-----f131f9d463e1--------------------------------)

### 通过在 GitHub 上创建帐户，您可以为 google-research/distilling-step-by-step 的开发做出贡献。

[GitHub - google-research/distilling-step-by-step](https://github.com/google-research/distilling-step-by-step?source=post_page-----f131f9d463e1--------------------------------)

# 结束语

![](img/21e76ab1b41f50cfdf35f35c010f0b68.png)

照片由 [Saif71.com](https://unsplash.com/@saif71?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> 教育是创造所有其他职业的唯一职业。 – 无名氏

**本文展示了如何利用 LLM 教导较小的模型解决特定任务。** 超越结果，本文还展示了即使是较小的模型，通过提供上下文也能得出解决方案。因此，这种方法使用户能够用更少的数据提炼出一个小模型，并超越大型 LLM：

![](img/6f31a123753708a494011954a7a8ef64.png)

文章的示意图。图片来源：[这里](https://arxiv.org/pdf/2305.02301.pdf)

作者在本文中展示了比 LLM 小 2000 倍的模型能够学习并在复杂任务（如推理任务）上超越教师模型。此外，与经典的逐步提炼方法相比，它需要的数据要少得多。

一般来说，近年来模型学习研究发生了范式转变，试图将记忆与实际学习分开。

[](https://levelup.gitconnected.com/grokking-learning-is-generalization-and-not-memorization-52c43c9025e4?source=post_page-----f131f9d463e1--------------------------------) [## 理解：学习是泛化而非记忆

### 理解神经网络如何学习可以帮助我们避免模型忘记所学内容。

[levelup.gitconnected.com](https://levelup.gitconnected.com/grokking-learning-is-generalization-and-not-memorization-52c43c9025e4?source=post_page-----f131f9d463e1--------------------------------)

确实，本文表明，要执行特定任务，你并不需要大容量（记忆）。你可以教导一个小模型通过提供解决问题的信息来学习任务（泛化）。

这项工作很重要，因为用少量数据，可以训练出一个更小的模型在任务上表现出色。这些模型可以以更低的成本更容易地部署。此外，这种方法适用于任何模型，因此用户可以使用开源模型（如 LLaMA）或专有模型（GPT-4 或 PaLM）的 API 进行逐步提炼，创建自己的专业模型。

这项工作开辟了许多令人兴奋的可能性，如以低成本开发适用于多个应用的专业模型，并且其性能优于巨型模型。这些模型不仅可以在线部署，还可以在桌面计算机或手机应用中使用。因此，拥有一个小而专有的数据集，你可以用有限的资源开发和部署专家模型。

例如，你可以设想一个用户开发一个专门解决谜题的小模型。你只需与 LLM 创建推理，使用*逐步提炼*来训练你的专家模型，然后甚至可以将其部署到手机应用上。

# TL;DR

+   Google 公布了一种新的简单方法来从大型模型中提取知识。通过使用推理和答案，你可以教导一个小模型（甚至小 2000 倍）在推理任务中超越 LLM。

+   这种方法超越了之前的最新技术。

+   这种方法只需要一个小的训练集和较小的模型尺寸

+   这种方法使得可以为专业任务部署独立的语言模型。现在模型尺寸与网页应用兼容，并且可以在设备上进行推理，无需复杂的基础设施。

## 你怎么看？在评论中告诉我

# 如果你觉得这很有趣：

*你可以查看我的其他文章，你也可以* [***订阅***](https://salvatore-raieli.medium.com/subscribe) *以便在我发布文章时收到通知，也可以在*[***LinkedIn***](https://www.linkedin.com/in/salvatore-raieli/)***上联系或找到我。***

*这是我 GitHub 仓库的链接，我计划在这里收集与机器学习、人工智能等相关的代码和资源。*

[](https://github.com/SalvatoreRa/tutorial?source=post_page-----f131f9d463e1--------------------------------) [## GitHub - SalvatoreRa/tutorial: 机器学习、人工智能、数据科学的教程…

### 关于机器学习、人工智能、数据科学的教程，包括数学解释和可重复使用的代码（用 Python 编写…

github.com](https://github.com/SalvatoreRa/tutorial?source=post_page-----f131f9d463e1--------------------------------)

*或者你可能对我的一篇近期文章感兴趣：*

[](/reshaping-the-models-memory-without-the-need-for-retraining-9ade69f56296?source=post_page-----f131f9d463e1--------------------------------) ## 无需重新训练即可重塑模型的记忆

### 擦除大型语言模型所学到的有问题内容的任何回响

towardsdatascience.com [](https://levelup.gitconnected.com/beyond-words-unraveling-speech-from-brain-waves-with-ai-7ff81862dfff?source=post_page-----f131f9d463e1--------------------------------) [## 超越语言：用 AI 解码脑波中的言语

### AI 能够从非侵入性脑记录中解码语言

levelup.gitconnected.com](https://levelup.gitconnected.com/beyond-words-unraveling-speech-from-brain-waves-with-ai-7ff81862dfff?source=post_page-----f131f9d463e1--------------------------------)

# 参考文献

这是我在撰写本文时参考的主要文献列表（只引用了每篇文章的第一作者姓名）。

1.  傅, 2023, 《将较小的语言模型专门化为多步骤推理》, [链接](https://arxiv.org/abs/2301.12726)

1.  辛顿, 2015, 《提炼神经网络中的知识》, [链接](https://arxiv.org/abs/1503.02531)

1.  霍华德, 2018, 《通用语言模型微调用于文本分类》, [链接](https://arxiv.org/abs/1801.06146)

1.  卡普兰, 2020, 《神经语言模型的规模定律》, [链接](https://arxiv.org/abs/2001.08361)

1.  韦, 2022, 《链式思维提示在大型语言模型中引发推理》, [链接](https://arxiv.org/abs/2201.11903)

1.  Hsieh, 2023, 逐步提炼！以更少的训练数据和更小的模型尺寸超越更大的语言模型，[链接](https://arxiv.org/abs/2305.02301)

1.  Chowdhery, 2022, PaLM: 通过路径扩展语言建模，[链接](https://arxiv.org/abs/2204.02311)

1.  Raffel, 2019, 使用统一的文本到文本转换器探索迁移学习的极限，[链接](https://arxiv.org/abs/1910.10683)
