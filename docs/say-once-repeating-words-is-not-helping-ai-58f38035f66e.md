# 说一遍！重复的话语并未帮助 AI

> 原文：[`towardsdatascience.com/say-once-repeating-words-is-not-helping-ai-58f38035f66e`](https://towardsdatascience.com/say-once-repeating-words-is-not-helping-ai-58f38035f66e)

## | 人工智能 | 自然语言处理 | 大语言模型

## 重复使用标记如何以及为什么会对 LLM 造成伤害？这是什么问题？

[](https://salvatore-raieli.medium.com/?source=post_page-----58f38035f66e--------------------------------)![Salvatore Raieli](https://salvatore-raieli.medium.com/?source=post_page-----58f38035f66e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----58f38035f66e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----58f38035f66e--------------------------------) [Salvatore Raieli](https://salvatore-raieli.medium.com/?source=post_page-----58f38035f66e--------------------------------)

·发布在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----58f38035f66e--------------------------------) ·14 分钟阅读·2023 年 6 月 20 日

--

![](img/40a23dee61acd2fe912d9497b6a07d5f.png)

图片由[Kristina Flour](https://unsplash.com/it/@tinaflour)提供，来源于 Unsplash

[大语言模型（LLMs）](https://en.wikipedia.org/wiki/Large_language_model)已经展示了它们的能力，并且在全球引起了轰动。每个大公司现在都有一个名字花哨的模型。但实际上，它们都是[变换器](https://en.wikipedia.org/wiki/Transformer_(machine_learning_model))。**每个人都梦想拥有万亿参数，但难道没有限制吗？**

在这篇文章中，我们讨论了以下内容：

+   更大的模型是否保证比小模型性能更好？

+   我们是否有关于巨大模型的数据？

+   如果不收集新数据而是重复使用已有数据，会发生什么？

# 在天空中扩展：是什么伤害了机翼？

![](img/4c30898f3c8683fc888bf2424f1a20f8.png)

图片由[Sean Pollock](https://unsplash.com/it/@seanpollock)提供，来源于 Unsplash

[OpenAI 定义了规模定律](https://arxiv.org/abs/2001.08361)，指出模型性能遵循一个幂律，取决于使用了多少参数和数据点。这与对新兴属性的探索一起，催生了参数竞赛：**模型越大，性能越好。**

> 这是真的吗？更大的模型是否会提供更好的性能？

最近，新兴属性面临危机。斯坦福研究人员表明，新兴属性的概念可能并不存在。

[](/emergent-abilities-in-ai-are-we-chasing-a-myth-fead754a1bf9?source=post_page-----58f38035f66e--------------------------------) ## 人工智能的新兴能力：我们是在追逐一个神话吗？

### 对大语言模型（LLMs）新兴属性的观点改变

towardsdatascience.com

缩放法则可能赋予数据集的价值远低于实际认为的价值。[DeepMind 通过 Chinchilla](https://www.deepmind.com/publications/an-empirical-analysis-of-compute-optimal-large-language-model-training)表明，人们不仅要考虑参数的规模，还要考虑数据的规模。事实上，Chinchilla 显示出它在容量上优于[Gopher](https://arxiv.org/abs/2112.11446)（70 B 与 280 B 参数）

![](img/b7da5b5a3d95d2a77649d3981baacb1d.png)

“叠加预测。我们叠加了三种不同方法的预测，以及 Kaplan 等（2020 年）的预测。我们发现所有三种方法都预测当前的大型模型应该小得多，因此训练时间也应该比现在的时间长。” 图片来源：[这里](https://arxiv.org/pdf/2203.15556.pdf)

最近，机器学习社区对 LLaMA 感到兴奋，不仅因为它是开源的，还因为 65 B 版本的参数超越了[OPT 175 B](https://ai.facebook.com/blog/democratizing-access-to-large-scale-language-models-with-opt-175b/)。

[](https://medium.com/mlearning-ai/metas-llama-a-small-language-model-beating-giants-5065948e0b7f?source=post_page-----58f38035f66e--------------------------------) [## META 的 LLaMA：一个小型语言模型战胜巨头

### META 开源模型将帮助我们理解语言模型偏差的产生

[medium.com](https://medium.com/mlearning-ai/metas-llama-a-small-language-model-beating-giants-5065948e0b7f?source=post_page-----58f38035f66e--------------------------------)

正如 DeepMind 在 Chinchilla 文章中所述，可以估计完全训练一个最先进的 LLM 所需的 tokens 数量。另一方面，也可以估计存在多少高质量的 tokens。最近的研究对此话题产生了疑问。[他们得出结论](https://arxiv.org/pdf/2211.04325.pdf)：

+   语言数据集呈指数增长，语言数据集出版的年增长率达到 50%（到 2022 年底达到 2e12 个单词）。这表明新语言数据集的研究和出版是一个非常活跃的领域。

+   另一方面，互联网上的单词数量（单词库存）在增长（作者估计在 7e13 到 7e16 个单词之间，因此是 1.5 到 4.5 个数量级）。

+   然而，由于他们尝试使用高质量的单词库存，实际上作者估计高质量库存在 4.6e12 到 1.7e13 个单词之间。**作者表示，在 2023 年至 2027 年间，我们将耗尽质量单词的数量，而在 2030 年至 2050 年之间将耗尽全部库存。**

+   图像库存的情况也没有好多少（三到四个数量级）

![](img/b40e2e8fedec8ae3213af29a06693e90.png)

数据使用的预测。图片来源：[这里](https://arxiv.org/pdf/2211.04325.pdf)

> 为什么会发生这种情况？

好吧，因为我们人类并非无限制地生成文本，不能像 ChatGPT 那样大量生产。事实上，互联网用户数量的预测（真实与预测）说明了一切：

![](img/02944a3a9c4df3a096c2b764ee797c18.png)

互联网用户的真实和预测演变。图片来源：[这里](https://arxiv.org/pdf/2211.04325.pdf)

事实上，并非所有人都对用文本、代码和其他来源来训练人工智能模型感到满意。实际上，维基百科、Reddit 和其他用于训练模型的来源希望公司付费使用他们的数据。相比之下，公司则援引公平使用条款，目前的法规环境尚不明确。

将数据整合在一起，可以清晰地看到一个趋势。为了最佳训练 LLM 所需的令牌数量增长速度超过了现有的令牌库存。

![](img/7a7d8cfc715fd75d345833f62d899c07.png)

图片来源：[这里](https://arxiv.org/pdf/2305.13230.pdf)

根据 Chinchilla 定义的扩展法则（用于最佳 LLM 训练所需的令牌数量），我们已经超过了限制。从图表中可以看出，根据这些估计，使用[PaLM-540 B](https://ai.googleblog.com/2022/04/pathways-language-model-palm-scaling-to.html)，我们已达到极限（需要 10.8 万亿个令牌，而库存为 9 万亿）。

**一些作者称这个问题为“令牌危机”。** 此外，到目前为止，我们仅考虑了英语令牌，但还有七千种其他语言。[整个网络的 56%是英语](https://w3techs.com/technologies/overview/content_language)，剩下的 44%则属于仅 100 种其他语言。这也反映在其他语言模型的表现中。

# 我们能获取更多的数据吗？

![](img/9f98ef16a2dbf8efd323a627e748fd11.png)

图片由[Karen Vardazaryan](https://unsplash.com/it/@bright)提供，来源于 Unsplash

正如我们所见，更多的参数并不等于更好的性能。为了获得更好的性能，我们需要优质的令牌（文本），但这些资源稀缺。**我们如何获得这些资源？我们能依靠人工智能来帮助自己吗？**

> 为什么我们不使用 Chat-GPT 来生成文本？

**如果我们人类生成的文本不足，为什么不自动化这个过程呢？** 最近的研究显示了[这个过程如何不尽如人意](https://arxiv.org/pdf/2305.15717.pdf)。斯坦福 Alpaca 使用 52,000 个从[GPT-3](https://en.wikipedia.org/wiki/GPT-3)中衍生的示例进行训练，但显然只达到了类似的性能。[**实际上，该模型学习了目标模型的风格，但未能掌握其知识。**](https://levelup.gitconnected.com/the-imitation-game-taming-the-gap-between-open-source-and-proprietary-models-627374b390e5)

> 为什么不进行更长时间的训练？

对于 PaLM、Gopher 和 LLaMA（以及其他 LLMs），清楚地写明了这些模型训练了几个时期（一个或几个）。这不是[Transformer](https://en.wikipedia.org/wiki/Transformer_(machine_learning_model))的限制，因为例如，[视觉 Transformer（ViT）](https://en.wikipedia.org/wiki/Vision_transformer)在 ImageNet（100 万张图片）上训练了 300 个时期，如下表所示：

![](img/bc5ebefbcba1ba2a15990ccedb965a4a.png)

图片来源：[这里](https://arxiv.org/pdf/2010.11929.pdf)

因为这实在太昂贵了。在[LLaMA 文章](https://medium.com/mlearning-ai/metas-llama-a-small-language-model-beating-giants-5065948e0b7f)中，作者只训练了一个时期（而数据集的一部分训练了两个时期）。尽管如此，作者报告称：

> 当训练一个 65B 参数的模型时，我们的代码在 2048 张 80GB RAM 的 A100 GPU 上处理约 380 个令牌/秒。这意味着在包含 1.4T 令牌的数据集上训练大约需要 21 天。 ([source](https://arxiv.org/pdf/2302.13971.pdf))

训练一个大型语言模型（LLM）即使只训练几个时期也极其昂贵。如[德米特罗·尼古拉耶夫（Dimid）](https://medium.com/u/97b5279dad26?source=post_page-----58f38035f66e--------------------------------)计算，这相当于 400 万美元，如果你在谷歌云平台上训练一个类似于 META 的 LLaMA 的模型。

所以训练其他的时期将导致成本的指数增加。**此外，我们不知道这些额外的训练是否真的有用：我们还没有测试过。**

最近，新加坡大学的一组研究人员研究了如果我们训练一个 LLM 多个时期会发生什么：

[](https://arxiv.org/abs/2305.13230?source=post_page-----58f38035f66e--------------------------------) [## 是否重复：从扩展 LLM 中的令牌危机中获得的见解

### 最近的研究突显了数据集规模在扩展语言模型中的重要性。然而，大型语言模型...

arxiv.org](https://arxiv.org/abs/2305.13230?source=post_page-----58f38035f66e--------------------------------)

# Repetita iuvant aut continuata secant

![](img/bad195651655fa3252688bea96be3549.png)

图片由[Unseen Studio](https://unsplash.com/it/@craftedbygc)提供，来自 Unsplash

直到现在，我们知道模型的表现不仅由参数数量决定，还由用于训练的优质令牌数量决定。另一方面，这些优质令牌不是无限的，我们正接近极限。**如果我们找不到足够的优质令牌，而生成它们是一个选项，我们该怎么办？**

> 我们可以使用相同的训练集并延长训练时间吗？

有一句拉丁语说，重复有益（*repetita iuvant*），但随着时间的推移，有人加上了“但持续的无聊”（*continuata secant*）。

神经网络也是如此：增加训练轮数会提高网络性能（减少损失）；然而，在某个时刻，当训练集中的损失继续下降时，验证集中的损失开始上升。神经网络进入了[过拟合](https://en.wikipedia.org/wiki/Overfitting)状态，开始考虑仅存在于训练集中的模式，失去了泛化能力。

![](img/66cd1d33ca987237ac5987a905023782.png)

监督学习中的过拟合/过度训练。图片来源：[here](https://en.wikipedia.org/wiki/Overfitting)

> 好的，这在小型神经网络中已经进行了广泛研究，但在大型变压器中情况如何呢？

本研究的作者在 C4 数据集上使用了[T5 模型](https://ai.googleblog.com/2020/02/exploring-transfer-learning-with-t5.html)（编码器-解码器模型）。作者训练了几个版本的模型，增加了参数数量，直到较大的模型超过了较小的模型（表明较大的模型获得了足够的 tokens，如 Chinchilla 定律所示）。作者指出，所需 tokens 的数量与模型的大小之间存在线性关系（证实了 DeepMind 对 Chinchilla 的观察）。

![](img/00720fa825e60895d6743c561eab6428.png)

图片来源：[here](https://arxiv.org/pdf/2305.13230.pdf)

C4 数据集是有限的（没有无限的 tokens），因此为了增加参数数量，作者发现自己处于 tokens 短缺的条件下。因此，他们决定模拟 LLM 看到重复数据的情况。他们抽取了一定数量的 tokens，因此模型发现自己在 tokens 训练中再次看到它们。这表明：

+   重复的 tokens 导致性能下降。

+   在 tokens 短缺条件下，大型模型更容易发生过拟合（因此尽管理论上它消耗了更多的计算资源，但这会导致性能下降）。

![](img/940085b94c885d0037fba10b6bf27663.png)

图片来源：[here](https://arxiv.org/pdf/2305.13230.pdf)

此外，这些模型还用于下游任务。通常，一个大语言模型（LLM）在大量文本上进行无监督训练，然后在较小的数据集上进行微调以完成下游任务。或者，它可能会经历称为对齐的过程（如 ChatGPT 的情况）。

当一个 LLM 在重复的数据上训练，即使之后在另一个数据集上进行微调，性能也会下降。因此，下游任务也会受到影响。

![](img/ac9c68a16e51f162ca1bc64c95b3d321.png)

图片来源：[here](https://arxiv.org/pdf/2305.13230.pdf)

# 为什么重复的 tokens 不是一个好主意

![](img/2c4a3f3cca1e4bbf67e2653d55867c12.png)

图片由[Brett Jordan](https://unsplash.com/it/@brett_jordan)在 Unsplash 提供

> 我们刚刚看到重复的 tokens 会损害训练。但是为什么会发生这种情况呢？

作者决定通过固定重复标记的数量并增加数据集中总标记的数量来进行调查。结果表明，更大的数据集缓解了多轮训练降级的问题。

![](img/41965687e7b13654ad2741d3ed1689db.png)

图片来源：[这里](https://arxiv.org/pdf/2305.13230.pdf)

[去年，Galactica](https://arxiv.org/pdf/2211.09085.pdf) 发布了（一个原本旨在帮助科学家的模型，但[仅存活了三天](https://www.technologyreview.com/2022/11/18/1063487/meta-large-language-model-ai-only-survived-three-days-gpt-3-science/)）。除了那次惊人的失败之外，文章还指出，他们的部分结果来源于数据的质量。根据作者的说法，数据质量降低了过拟合的风险：

> 我们能够在其上进行多轮训练而不会过拟合，其中上游和下游性能随着重复标记的使用而提高。 ([来源](https://arxiv.org/pdf/2211.09085.pdf))

![](img/973eb580593c47877e6f117fff7207e2.png)

图片来源：[这里](https://arxiv.org/pdf/2211.09085.pdf)

对于作者来说，重复标记实际上不仅没有损害模型训练，反而提高了下游性能。

在这项新研究中，作者使用了被认为质量高于 C4 的维基百科数据集，并添加了重复标记。结果显示，降级水平相似，这与 Galactica 文章中的说法相反。

![](img/c6635e936469851fb038cd398a69c00b.png)

图片来源：[这里](https://arxiv.org/pdf/2305.13230.pdf)

作者还尝试调查是否也由于模型扩展。在模型扩展过程中，参数数量和计算成本都会增加。作者决定分别研究这两个因素：

+   [**专家混合模型（MoE）**](https://ai.googleblog.com/2022/11/mixture-of-experts-with-expert-choice.html) 因为虽然它增加了参数数量，但保持了类似的计算成本。

+   [**ParamShare**](https://ai.googleblog.com/2022/11/mixture-of-experts-with-expert-choice.html) 则减少了参数数量，但保持了相同的计算成本。

![](img/a1c97c42cba2c96288f2c14b2d4e632d.png)

图片来源：[这里](https://arxiv.org/pdf/2305.13230.pdf)

结果表明，参数较少的模型受重复标记的影响较小。相比之下，MoE 模型（参数较多）更容易过拟合。这个结果很有趣，因为 MoE 在许多 AI 模型中已经成功使用，所以作者建议，虽然 MoE 是一个在数据充足时有用的技术，但在标记不足时可能会损害性能。

作者还探讨了目标训练是否影响性能降级。通常，有两个训练目标：

+   [**下一个标记预测**](https://arxiv.org/abs/2212.11281)（给定一系列标记，预测序列中的下一个标记）。

+   [**掩码语言建模**](https://huggingface.co/docs/transformers/main/tasks/masked_language_modeling)，即掩盖一个或多个标记并需要预测它们。

最近，谷歌推出了 [PaLM2–2](https://ai.google/discover/palm2/)，并引入了 UL2，这是一种这两种训练目标的混合。虽然 UL2 显示出加速模型训练的效果，但有趣的是，UL2 更容易过拟合，并且有更大的多轮次退化。

![](img/714c09520e9e558a53f6cf1acd00586a.png)

图片来源：[这里](https://arxiv.org/pdf/2305.13230.pdf)

作者接着探索了如何尝试缓解多轮次退化。由于正则化技术的使用正是为了防止过拟合，作者测试了这些技术是否在这里也有有益的效果。

Dropout 被证明是缓解这个问题的最有效技术之一。这并不令人惊讶，因为作为一种最有效的正则化技术之一，它容易并行化，并被大多数模型使用。

![](img/819f3826fd343df6ba93c90da456a499.png)

图片来源：[这里](https://arxiv.org/pdf/2305.13230.pdf)

此外，作者发现最好从不使用 dropout 开始，并仅在训练的较晚阶段添加 dropout。

![](img/f0cf3be64f13de793f9c2298a8097df7.png)

图片来源：[这里](https://arxiv.org/pdf/2305.13230.pdf)

另一方面，作者指出，在某些模型，尤其是较大的模型中，使用 Dropout 可能会导致性能轻微下降。因此，尽管它可能在防止过拟合方面有益，但在其他环境中可能会导致意外的行为。因此，GPT-3、PaLM、LLaMA、Chinchilla 和 Gopher 等模型在其架构中不使用它。

![](img/a005e0d3460b7fb717c34dbbf3e6c013.png)

图片来源：[这里](https://arxiv.org/pdf/2305.13230.pdf)

如下表所述，作者在实验中使用的模型现在被认为几乎是小型模型。因此，在设计大型语言模型（LLM）时，测试不同的超参数非常昂贵：

> 例如，在我们特定的场景中，训练 T5-XL 五次大约需要 $37,000 USD 来租用 Google Cloud TPUs。考虑到更大的模型如 PaLM 和 GPT-4，在更大的数据集上训练，这个成本变得不可控（[来源](https://arxiv.org/pdf/2305.13230.pdf)）

![](img/eac5745319c497f6992c6d1f88ec8d78.png)

图片来源：[这里](https://arxiv.org/pdf/2305.13230.pdf)

由于在他们的实验中，稀疏 MoE 模型近似于密集模型（后者计算开销更大）的行为，因此可以使用它来搜索最佳超参数。

例如，作者展示了可以测试 MoE 模型的不同学习率，并且它展现出与等效的密集模型相同的性能。因此，对作者来说，可以用 MoE 模型测试不同的超参数，然后用选择的参数训练密集模型，从而节省成本：

> 对 MoE 大型模型的全面调整在 Google Cloud Platform 上花费了大约 10.6K USD。相比之下，只训练一次 Dense XL 模型只需 7.4K USD。因此，整个开发过程，包括调整，总成本达到了 18K USD，这仅为直接调整 Dense XL 模型的费用的 0.48 倍 ([source](https://arxiv.org/pdf/2305.13230.pdf))

![](img/1d7936a9edc9c0c727a734e00b06da96.png)

图像来源：[here](https://arxiv.org/pdf/2305.13230.pdf)

# 思考总结

近年来，出现了争夺最大模型的竞赛。一方面，这场竞赛的动机在于在某一规模下，会出现一些无法用更小模型预测的特性。另一方面，OpenAI 的缩放定律指出，性能是模型参数数量的函数。

> 在过去一年中，这一范式陷入了危机。

最近，LlaMA 显示了数据质量的重要性。同时，Chinchilla 展示了一个用于计算训练模型所需的标记数量的新规则。实际上，具有一定数量参数的模型需要相应的数据量才能达到最佳性能。

随后的研究表明，优质标记的数量不是无限的。另一方面，模型参数的数量增长快于我们人类能够生成的标记数量。

这引出了如何解决标记危机的问题。最近的研究表明，使用 LLM 生成标记并不是一个可行的方法。这项新工作显示了在多个周期内使用相同标记实际上会降低性能。

这样的工作很重要，因为尽管我们越来越多地训练和使用 LLM，但仍有许多基本方面我们不了解。这项工作回答了一个看似基本的问题，但作者通过实验数据给出了答案：**训练 LLM 多个时期会发生什么？**

此外，本文是不断增长的文献的一部分，这些文献展示了不加批判地增加参数数量是多么不必要。另一方面，越来越大的模型变得越来越昂贵，同时也消耗越来越多的电力。考虑到我们需要优化资源，本文建议，在没有足够数据的情况下训练一个巨大的模型只是浪费。

本文仍然展示了我们需要新的架构来替代 transformer。因此，是时候将研究重点放在新想法上，而不是继续扩大模型规模。

# 如果你觉得这很有趣：

*你可以查看我的其他文章，你还可以* [***订阅***](https://salvatore-raieli.medium.com/subscribe) *以便在我发布新文章时获得通知，你还可以* [***成为 Medium 会员***](https://medium.com/@salvatore-raieli/membership) *访问所有故事（这些是平台的推广链接，我从中获得少量收入，不会对你产生额外费用），你还可以通过*[***LinkedIn***](https://www.linkedin.com/in/salvatore-raieli/)***与我联系或找到我。***

*这是我 GitHub 仓库的链接，我计划在这里收集与机器学习、人工智能等相关的代码和许多资源。*

[## GitHub - SalvatoreRa/tutorial：机器学习、人工智能、数据科学的教程…](https://github.com/SalvatoreRa/tutorial?source=post_page-----58f38035f66e--------------------------------) [## GitHub - SalvatoreRa/tutorial：关于机器学习、人工智能、数据科学的教程…

### 关于机器学习、人工智能、数据科学的教程，包括数学解释和可重用代码（用 Python 编写…

[## GitHub - SalvatoreRa/tutorial：机器学习、人工智能、数据科学的教程…](https://github.com/SalvatoreRa/tutorial?source=post_page-----58f38035f66e--------------------------------)

*或者你可能对我最近的一篇文章感兴趣：*

[## 扩展并非一切：更大的模型为何失败得更惨](https://salvatore-raieli.medium.com/scaling-isnt-everything-how-bigger-models-fail-harder-d64589be4f04?source=post_page-----58f38035f66e--------------------------------) [## 扩展并非一切：更大的模型为何失败得更惨

### 大型语言模型真的能理解编程语言吗？

[## META 的 LIMA：玛丽亚·近藤的 LLM 训练方式](https://salvatore-raieli.medium.com/scaling-isnt-everything-how-bigger-models-fail-harder-d64589be4f04?source=post_page-----58f38035f66e--------------------------------) [](https://levelup.gitconnected.com/metas-lima-maria-kondo-s-way-for-llms-training-8411e3907fed?source=post_page-----58f38035f66e--------------------------------) [## META’S LIMA：玛丽亚·近藤的 LLM 训练方式

### 更少而整洁的数据来创建一个能够与 ChatGPT 竞争的模型

[## META 的 LIMA：玛丽亚·近藤的 LLM 训练方式](https://levelup.gitconnected.com/metas-lima-maria-kondo-s-way-for-llms-training-8411e3907fed?source=post_page-----58f38035f66e--------------------------------) [](https://levelup.gitconnected.com/google-med-palm-2-is-ai-ready-for-medical-residency-e37907115bd0?source=post_page-----58f38035f66e--------------------------------) [## 谷歌 Med-PaLM 2：AI 是否准备好进入医学住院医生培训？

### 谷歌的新模型在医学领域取得了令人印象深刻的成果

[## 谷歌 Med-PaLM 2：AI 是否准备好进入医学住院医生培训？](https://levelup.gitconnected.com/google-med-palm-2-is-ai-ready-for-medical-residency-e37907115bd0?source=post_page-----58f38035f66e--------------------------------) [](https://levelup.gitconnected.com/to-ai-or-not-to-ai-how-to-survive-f5e853aebd5b?source=post_page-----58f38035f66e--------------------------------) [## AI 还是非 AI：如何生存？

### 随着生成性 AI 对企业和副业的威胁，你如何找到自己的空间？

[levelup.gitconnected.com](https://levelup.gitconnected.com/to-ai-or-not-to-ai-how-to-survive-f5e853aebd5b?source=post_page-----58f38035f66e--------------------------------)

# 参考文献

本文参考的主要文献列表：

1.  Fuzhao Xue 等，2023，《重复还是不重复：在令牌危机下扩展 LLM 的见解》，[链接](https://arxiv.org/abs/2305.13230)

1.  Hugo Touvron 等，2023，《LLaMA：开放且高效的基础语言模型》。 [链接](https://arxiv.org/abs/2302.13971)

1.  Arnav Gudibande 等，2023，《模仿专有 LLM 的虚假承诺》。 [链接](https://arxiv.org/abs/2305.15717)

1.  PaLM 2，谷歌博客，[链接](https://ai.google/discover/palm2/)

1.  Pathways Language Model (PaLM)：扩展至 540 亿参数以实现突破性性能。谷歌博客，[链接](https://ai.googleblog.com/2022/04/pathways-language-model-palm-scaling-to.html)

1.  Buck Shlegeris 等，2022，《语言模型在下一个令牌预测上优于人类》，[链接](https://arxiv.org/abs/2212.11281)

1.  Pablo Villalobos 等，2022，《我们会用完数据吗？对机器学习中数据集扩展极限的分析》。 [链接](https://arxiv.org/abs/2211.04325)

1.  Susan Zhang 等，2022，《OPT：开放预训练变换器语言模型》。 [链接](https://arxiv.org/abs/2205.01068)

1.  Jordan Hoffmann 等，2022，《计算最优大型语言模型训练的实证分析》。 [链接](https://arxiv.org/abs/2203.15556)

1.  Ross Taylor 等，2022，《Galactica：一种用于科学的大型语言模型》，[链接](https://arxiv.org/abs/2211.09085)

1.  Zixiang Chen 等，2022，《朝着理解深度学习中的专家混合模型前进》，[链接](https://arxiv.org/abs/2208.02813)

1.  Jared Kaplan 等，2020，《神经语言模型的规模定律》。 [链接](https://arxiv.org/abs/2001.08361)

1.  人工智能如何助长全球变暖，TDS，链接

1.  掩码语言建模，HuggingFace 博客，[链接](https://huggingface.co/docs/transformers/main/tasks/masked_language_modeling)

1.  专家混合模型与专家选择路由，谷歌博客，[链接](https://ai.googleblog.com/2022/11/mixture-of-experts-with-expert-choice.html)

1.  为什么 Meta 最新的大型语言模型在线仅存活了三天，MIT 评审，[链接](https://www.technologyreview.com/2022/11/18/1063487/meta-large-language-model-ai-only-survived-three-days-gpt-3-science/)

1.  探索使用 T5 的迁移学习：文本到文本的迁移变换器，谷歌博客，[链接](https://ai.googleblog.com/2020/02/exploring-transfer-learning-with-t5.html)

1.  奖励模型过度优化的规模定律，OpenAI 博客，[链接](https://openai.com/research/scaling-laws-for-reward-model-overoptimization)

1.  计算最优大型语言模型训练的实证分析，DeepMind 博客，[链接](https://www.deepmind.com/blog/an-empirical-analysis-of-compute-optimal-large-language-model-training)

1.  Xiaonan Nie 等, 2022, **EvoMoE: 一种通过稠密到稀疏门控的进化专家混合模型训练框架**。 [link](https://arxiv.org/abs/2112.14397)

1.  Tianyu Chen 等, 2022, **任务特定专家剪枝用于稀疏专家混合模型**, [link](https://arxiv.org/abs/2206.00277)

1.  Bo Li 等, 2022, **稀疏专家混合模型是领域通用的学习者**, [link](https://arxiv.org/abs/2206.04046)
