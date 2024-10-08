# 选择和运行自己生成模型的逐步指南

> 原文：[`towardsdatascience.com/a-step-by-step-guide-to-selecting-and-running-your-own-generative-model-4e52ecc28540`](https://towardsdatascience.com/a-step-by-step-guide-to-selecting-and-running-your-own-generative-model-4e52ecc28540)

## 如何在每天发布的模型海洋中进行导航？

[](https://medium.com/@kevin.berlemont?source=post_page-----4e52ecc28540--------------------------------)![Kevin Berlemont, PhD](https://medium.com/@kevin.berlemont?source=post_page-----4e52ecc28540--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4e52ecc28540--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----4e52ecc28540--------------------------------) [Kevin Berlemont, PhD](https://medium.com/@kevin.berlemont?source=post_page-----4e52ecc28540--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4e52ecc28540--------------------------------) ·阅读时间 5 分钟·2023 年 10 月 7 日

--

![](img/57dcd30f489d1c312588752fbfe2e016.png)

图片由 [fabio](https://unsplash.com/@fabioha?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

过去几个月，各种生成模型的参数规模大幅减少，比如刚刚发布的新 Mistral AI 模型。规模的减少为获取自己的个人助理 AI 提供了可能性，可以通过你的本地计算机与之绑定。这种本地推理非常诱人，以确保对你的数据进行机密计算。随着这些新发展，部署和管理 AI 工作负载的方式与 6 个月前相比有所不同，并且不断演变。如何使用这些模型来试验，甚至在你公司的基础设施上托管它们？

我认为，在使用任何由其他人托管的 API 模型之前，尝试不同类型的模型是一个好主意，以了解这些不同模型家族的表现。因此，让我们假设你不会立即使用 API 模型。你如何下载并使用一个模型呢？

对于此，你有两种类型的模型：专有模型和开放访问模型。专有模型如 OpenAI、Cohere 等都有各自的 API。开放访问模型可以是完全开放的，也可以是由于其许可证（如商业、非商业、仅研究目的等）而受限的。

找到这些模型的最佳地方是[HuggingFace](https://huggingface.co/)。在模型页面上，你可以看到他们提供了超过 350,000 个模型，覆盖了非常多样的任务。因此，你确实有很多选择！

![](img/f4e3344bdbc5f8808d5e99cb5fdba746.png)

HuggingFace 上的模型主页

需要记住的是，并非所有这些模型都在被使用或将来会被使用。有些模型可能只是某人在一个下午尝试的结果，然后再也没有更新。找到最有用模型的一个关键指标是查看有多少人下载了模型并喜欢它。例如，你可以根据你所寻找的任务类型进行筛选，如文本分类，然后你可以看到哪些模型是下载量最多和最流行的，并按照许可证等进行筛选。这将给你一个关于你所考虑任务的模型的整体概述，并帮助你找到它们。

转到文本生成任务，这是目前生成模型讨论的主要话题，你会发现流行的模型是新的 Mistral 模型 ([`mistral.ai/news/announcing-mistral-7b)`](https://mistral.ai/news/announcing-mistral-7b/)，其参数量为 70 亿。现在，在 HuggingFace 上有这么多模型可用，如何知道哪个模型适合你的任务呢？

首先需要注意的是，如下图所示，当你点击一个模型时，你会进入模型卡片页面，大多数模型已经有一个托管的交互界面，你可以从中获得模型输出的提示。除了这个托管的交互界面外，你还可以在下方看到所谓的 *Spaces*。Spaces 是托管在 HuggingFace 上的应用程序，人们在其中集成了你所查看的模型。所有这些界面都非常方便，让你了解这些模型的功能。

![](img/260bd564f99ea9dd89c0caa8d470ace0.png)

HuggingFace 上的 Mistral 7B 模型卡片

# 选择和运行模型

当然，模型选择时的约束将取决于你可用的基础设施和硬件。一个好的经验法则是，对于变换器模型，参数量超过 70 亿会使其在标准消费级 GPU 上难以良好运行。值得注意的是，你可能会发现一些模型优化方案，这些方案可以使模型在消费级硬件上运行。例如，可能会调整模型大小甚至计算精度，这可能适合你的具体任务，并允许你在自己的硬件上运行模型。

无论如何，一个好的建议是从较小的问题开始，然后逐步构建到解决你的任务的方案。一旦你弄清楚了需要什么模型和定制化，你可以查看这将对你的硬件需求意味着什么，以及是否可以实现。例如，假设对于我考虑的文本生成任务，Mistral 7B 模型在硬件需求方面符合我的需求。然后，HuggingFace 上的模型卡会给出如何下载和运行的说明。我通常使用 Google Collab ([`colab.research.google.com/?utm_source=scs-index`](https://colab.research.google.com/?utm_source=scs-index)) 来了解运行时间和资源使用情况，但你也可以使用其他平台，如 Kaggle。

![](img/47ce30e42bbf4c93e85c5852bc7050ba.png)

在标准 Google Collab 笔记本上推理 Mistral 7B

对于 Mistral 7B 模型，它在基本的 Google Collab 引擎上运行，内存为 12GB。因此，这可以让你了解简单推理模型所需的资源，以及你是否需要优化模型以便在更少资源上运行，或者你是否已经具备所需的资源。

在完成选择模型和了解所需资源的初步工作后，你可能会受到当前硬件的限制，不得不尝试优化你的模型。幸运的是，有一个 GitHub 仓库 ([`github.com/intel-analytics/BigDL`](https://github.com/intel-analytics/BigDL)) 详细介绍了大规模深度学习所需的不同优化方法。例如，它允许在标准计算机上运行 llama 语言模型。在那里，你可以找到所寻求的模型是否针对 CPU 计算（或至少单个 GPU）进行了优化，以及是否符合你的要求。

在这篇博客文章中，我描述了选择和运行模型的第一步。如果你有兴趣了解更多关于特定任务的模型修改和模型部署的内容，HuggingFace 提供了一个很棒的课程 ([`huggingface.co/learn/nlp-course/chapter1/1`](https://huggingface.co/learn/nlp-course/chapter1/1))，可以免费获取。

如果你喜欢这个故事，别犹豫，通过鼓掌或留言来表达你的欣赏！关注我在 [Medium](https://medium.com/@kevin.berlemont) 上的更多数据科学内容！你还可以通过 [LinkedIn](https://www.linkedin.com/in/kevinberlemont/) 与我联系。
