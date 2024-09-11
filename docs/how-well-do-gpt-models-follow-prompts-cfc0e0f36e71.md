# GPT模型对提示的响应效果如何?

> 原文：[https://towardsdatascience.com/how-well-do-gpt-models-follow-prompts-cfc0e0f36e71?source=collection_archive---------15-----------------------#2023-05-24](https://towardsdatascience.com/how-well-do-gpt-models-follow-prompts-cfc0e0f36e71?source=collection_archive---------15-----------------------#2023-05-24)

## 这是关于生成性AI使用与测试的研究第2部分

[](https://markopolocheno.medium.com/?source=post_page-----cfc0e0f36e71--------------------------------)[![Mark Chen](../Images/2d51d4e7ab451b55733a018a3d10a0a7.png)](https://markopolocheno.medium.com/?source=post_page-----cfc0e0f36e71--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cfc0e0f36e71--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----cfc0e0f36e71--------------------------------) [Mark Chen](https://markopolocheno.medium.com/?source=post_page-----cfc0e0f36e71--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F377682c0f342&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-well-do-gpt-models-follow-prompts-cfc0e0f36e71&user=Mark+Chen&userId=377682c0f342&source=post_page-377682c0f342----cfc0e0f36e71---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cfc0e0f36e71--------------------------------) ·10 min 阅读·2023年5月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcfc0e0f36e71&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-well-do-gpt-models-follow-prompts-cfc0e0f36e71&user=Mark+Chen&userId=377682c0f342&source=-----cfc0e0f36e71---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcfc0e0f36e71&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-well-do-gpt-models-follow-prompts-cfc0e0f36e71&source=-----cfc0e0f36e71---------------------bookmark_footer-----------)![](../Images/821def3c457ab2a1a20227cb000c02d0.png)

图片来源于 [Debby Hudson](https://unsplash.com/@hudsoncrafted?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

阅读第1部分: [验证OpenAI GPT模型性能](/how-to-validate-openai-gpt-model-performance-with-text-summarization-298978fea764)

**提示工程** 是来自自然语言处理行业的一个专有术语，用来描述为大型语言模型（LLMs）制定更好问题的过程，以获得更好的回答。换句话说，它是有效提示设计的艺术。

如果你仔细想想，我们在日常生活中都在进行提示工程。有些人把好奇心直接表达成问题。其他人则反复在 Google 搜索信息，直到找到答案。坚持不懈的人，特别是孩子，会不断问“为什么？为什么？但为什么？”或改变最初的问题，直到得到令人满意的答案。

对于大型语言模型（LLMs），提示工程的工作方式非常相似。用户期望 LLMs 根据给定的输入，即提示，产生输出。当输出不符合用户的期望时，他们可能会**修改提示以期获得更好的结果**。提示工程是解锁 LLMs 的关键，使其能够发挥最大潜力。它是引导一艘拥有 4000 亿参数的大船向正确方向前进的小舵。

从 [Kolena](https://www.kolena.io/) 的一位机器学习工程师的角度来看，本文提供了**评估提示有效性**的指南。我们将使用 OpenAI 的 GPT-3.5 Turbo 模型继续探索新闻文章的文本摘要问题。

# 必要的下一步

[之前的博客文章](/how-to-validate-openai-gpt-model-performance-with-text-summarization-298978fea764) 探讨了一个文本摘要问题，即通过使用生成型 AI 来总结新闻文章。我们使用了 [CNN-DailyMail](https://paperswithcode.com/dataset/cnn-daily-mail-1) 数据集（11,490 篇新闻文章，每篇都有人工编写的真实摘要），并比较了五种不同 GPT 模型的性能。最终，Turbo ([gpt-3.5-turbo](https://platform.openai.com/docs/models/gpt-3-5)) 生成了最佳摘要，并在最低成本下获得了最高的整体性能。

我们的下一步是**确定不同提示是否会导致显著的性能差异**和**定量评估 Turbo 对其给定提示的遵循程度**。

## 评估指标

我们希望避免定性的性能评估，因为我们声明为“更好”的摘要可能与其他人的意见不一致。定性比较总是引入**人为偏见和主观性**的问题。相反，我们将再次专注于定量性能评估。

[之前的博客文章](/how-to-validate-openai-gpt-model-performance-with-text-summarization-298978fea764) 已经定义了基于文本的性能指标，如 `BERT_rec`（BERTScore 的召回率）、`wc_inf`（推理词数）和 `ROUGE_L`。在这里，我们将增加三个新的指标：

1.  [METEOR](https://huggingface.co/spaces/evaluate-metric/meteor): 一个更好的 BLEU，声称与人工判断的相关性更高（0 到 1）。

1.  [formality](https://huggingface.co/s-nlp/roberta-base-formality-ranker?text=Is+this+a+formal+text%3F+I+think+it+is+quite+formal.): 一个表示文本非正式或正式程度的分数（0 到 1）。

1.  [pii_count](https://huggingface.co/ArunaSaraswathy/bert-finetuned-ner-pii?text=My+name+is+Wolfgang+and+I+live+in+Berlin)：一种计算文本中专有名词或个人数据的指标。

我们可以使用这些指标来识别模型生成冗长或非正式摘要时的表现不佳情况。总的来说，生成的摘要应该**保持简短并保留原始新闻文章的正式语气**。

## 提示词

我们对Turbo模型使用了八个提示词生成了八组不同的摘要。我们在下面列出它们，附上一个/两个单词的别名（加粗），随后是原始提示词：

```py
**1\. Basic
**  Summarize: \n{TEXT}
**2\. Professional
**  Professionally summarize this article: \n{TEXT}
**3\. Uppercase
**  Summarize this news article USING {MIN} TO {MAX} WORDS: \n{TEXT}
**4\. Detailed
**  Summarize this news article: \n\n{TEXT}\n\nUse {MIN} to {MAX} words like a reporter
**5\. DetailedV2
**  Professionally summarize this news article like a reporter with about {MIN} to {MAX} words :\n{TEXT}
**6\. 60 Words
**  Summarize this news article:\n\n{TEXT}\n\nUse about 60 words
**7\. No PII
**  Summarize this news article:\n\n{TEXT}\n\nUse about 60 words. Do not use any proper nouns.
**8\. Point Form
**  Summarize this news article in short point form sentences:\n{TEXT}

Note that MIN and MAX are bounds for a flexible word count range based on the provided ground truths.
```

## 性能差异

要查看哪些提示词在基于文本相似度生成摘要时表现更好，我们可以按表现顺序（最好的在前）列出每个指标的前五个提示词：

```py
**BERT_rec:** DetailedV2, Detailed, Uppercase, Professional, Basic
**ROUGE_L:** 60 Words, Detailed, DetailedV2, Uppercase, Basic
**METEOR:** DetailedV2, Uppercase, Detailed, Basic, Professional
```

对于`BERT_rec`、`ROUGE_L`和`METEOR`，前五个提示词始终包括`DetailedV2`、`Detailed`、`Uppercase`和`Basic`。不深入细节，`DetailedV2`在`BERT_rec`上比`Basic`好0.4%，在`ROUGE_L`上好4%，在`METEOR`上好3%。

![](../Images/d0b07f1d6435c5e239279a99a4f814a1.png)

ROUGE_L分布：DetailedV2 > Basic

从上面的图表中，我们可以观察到蓝色直方图的值高于紫色直方图。这表明`**DetailedV2**` **在** `**ROUGE_L**` **上表现优于** `**Basic**` **，这是一个改进。总的来说，这四个提示词（`DetailedV2`、`Detailed`、`Uppercase`和`Basic`）在文本相似度指标上表现优于`No PII`或`Point Form`等其他提示词。这并不令人惊讶，因为`No PII`和`Point Form`是具有不同输出要求的提示词，这可能导致生成的摘要中保留的信息量下降。**

![](../Images/6cd8f3c673a37f2343cc87d621223d06.png)

ROUGE_L分布：60 Words > DetailedV2

一个有趣的观察是`**60 Words**` **是产生最高** `**ROUGE_L**` **得分的最佳提示词**，尽管`60 Words`在其他文本相似度指标中不在前五名。我们可以从`60 Words`分布的紫色条形图中看到它向右突出在蓝色分布的右侧。如果你最看重`ROUGE_L`指标，你会想使用`60 Words`作为提示词，这可能不是最明显的选择，因为`DetailedV2`似乎表现一直很好。这就是提示词工程的重要性。

## 遵守提示词

为了衡量GPT Turbo在多大程度上遵守我们的提示词，我们将从三个不同的方面进行考察：**PII计数、词数和正式性**。

> **GPT Turbo在多大程度上遵守限制PII的要求？**

对于一些GPT用户，生成输出的一个潜在要求是避免个人身份信息（PII）。许多人重视个人信息的安全和隐私，一些企业可能需要保持数据匿名性——这也可能有法律要求。

让我们看看Turbo对`No PII`的遵守情况，该提示要求“不要使用任何专有名词”，并将其与`Basic`和`60 Words`作为基准进行比较。我们应该**期望`No PII`中出现零PII**，或者至少比基准中的PII数量显著更少。

![](../Images/2b0f11bc61194cd2fe0b2aef17fddb0d.png)

PII数量分布：Basic > 60 Words > No PII

从上述情况来看，Turbo模型**确实减少了`No PII`（紫色）生成摘要中的PII数量**，但远未达到零PII的平均水平。尽管大多数生成的摘要都有非零数量的PII，但`No PII`的摘要平均比其他提示少十个PII。

让我们看一个例子，并将`No PII`和`60 Words`生成的摘要与提供的事实真相摘要进行比较：

> *事实真相：* 威尔士魔术师加雷斯·贝尔在周日的惊人胜利中首开纪录。皇家马德里在对阵格拉纳达的比赛中打入九球，给巴塞罗那施加了压力。贝尔在胜利后享受了西班牙阳光下的烧烤。克里斯蒂亚诺·罗纳尔多在这场令人振奋的团队表现中打入了五球。
> 
> *No PII:* 加雷斯·贝尔在皇家马德里以9–1战胜格拉纳达的比赛中打入一球。他还在比赛后发了推文，并在当晚享受了一次烧烤。克里斯蒂亚诺·罗纳尔多在同一场比赛中打入了五球。皇家马德里在之前的失利后有所反弹。
> 
> *60 Words:* 加雷斯·贝尔在比赛中首开纪录，并在皇家马德里以9–1战胜格拉纳达后庆祝，还享受了一次烧烤。克里斯蒂亚诺·罗纳尔多在比赛中打入了五球，包括一个八分钟的帽子戏法，给西甲领头羊巴塞罗那施加了压力。这场胜利发生在皇家马德里在经典德比中输给巴塞罗那之后。

`No PII`的摘要中有16个PII，而`60 Words`的摘要中有27个PII。这是有道理的，因为很难在不提及贝尔和罗纳尔多名字的情况下为关于他们的文章写摘要。

事实上，即使是最好的创意作家，**创造一个没有专有名词的直接摘要也是非常困难的**。作为对自己的挑战，试着在不使用任何名字或地点的情况下准确描述罗纳尔多和贝尔。GPT Turbo决定创建一个包含专有名词的摘要，以**保持生成摘要的实用性**，因此Turbo并没有完全避免使用专有名词，但确实**努力减少PII的使用**。

> **GPT对字数限制请求的遵守情况如何？**

字数限制在提示中很重要，以控制潜在的[令牌](https://learn.microsoft.com/en-us/semantic-kernel/prompt-engineering/tokens)数量并满足自定义要求。当提示中要求使用大约60个单词（`No PII`，`60 Words`）时，我们应该期望输出的平均字数为60个单词。对于有字数范围的`Detailed`，我们不希望摘要超出或低于该范围。

![](../Images/3fc217bd784807750406fca7107dfbb2.png)

总结字数的分布：要点式 > 基础 > 详细 > 60 字 > 无个人信息

这个图表显示，当提示定义了 60 个词的目标字数时，**Turbo 生成了更短的总结**（`No PII` 以红色和 `60 Words` 以黄色表示）。然而，大多数生成的总结长度超过 60 个词，介于 80 到 100 个词之间。这并不理想，但也不算差距过大。在 `No PII` 和 `60 Words` 之间，`No PII` 更好地遵循了提示的指示。

对于 `Detailed`，Turbo 一贯生成 75 到 125 个词的总结。由于真实总结的平均字数约为 55 个词，大多数生成总结的预期范围应该是 50 到 100 个词。对于较长的文章和较长的真实总结，预期的总结字数范围不应扩展到 200 个词。看起来 Turbo 一直在接近定义范围的上限。也许它**希望生成“最佳”质量的总结，而实现这一点的方法是尽可能使用更多的词语**。

额外说明：当人们编写要点式笔记时，通常会简洁明了，因此令人惊讶的是，`Point Form` 让 Turbo 生成的总结平均为 120 个词。虽然我们可能期待简洁，Turbo 可能没有理解这一隐含的假设。有时，**在设计提示时你需要添加额外的细节**。

关于字数，GPT Turbo 确实会考虑提示中的要求，但并不会完全遵守它们。我们只能推测**Turbo 选择使用更多的词语，而不是仅仅满足于较短但“质量较差”的总结**。生成的总结词数越多，其包含所有文章重要信息的潜力也越大。

> **GPT 在回应要求提高专业性方面表现如何？**

专业性很难衡量，因此我们使用正式性评分来评估文本是否比其他文本更正式/专业。

从总体正式性评分来看，排名前三的提示（依次是）是 `No PII`、`60 Words` 和 `Basic`，但没有一个要求“专业”书写的总结。让我们将 `Basic` 和 `Uppercase` 作为 `Professional` 和 `DetailedV2` 的基准。

![](../Images/886865773a402cc209687c3da0a4e17f.png)

正式性评分的分布：基础 > 详细V2 > 专业 > 大写

从这个直方图来看，我们可以看到`Basic`、`DetailedV2` 和 `Professional` 生成的总结在正式性上**没有明显差异**。最简单且装饰最少的 `Basic` 提示生成了最多的非常专业/正式的总结。有趣的是，`Uppercase` 似乎生成了最不正式的总结。难道大写锁定的要求对 GPT 有负面影响吗？GPT 现在是否有了人工感情？

**有时，最简单的提示会产生最佳效果**。在这种情况下，很明显要求GPT Turbo“专业”并不会提高正式性评分。

# 学习要点

对于GPT的Turbo模型，**不同的提示确实会导致轻微的性能差异**。就我们的问题而言，文本相似性度量的差异不像我们定制的度量那样明显。对于不同大小的LLM，文本相似性度量的性能差异可能更为明显，但也以独特的方式遵循提示。

就服从性而言，**Turbo显然努力遵循每个提示的具体指示**，如明确的字数范围和最小化PII。不幸的是，当提示要求更专业或正式的写作风格时，它的性能或行为没有显著不同。也许GPT的默认设置是始终保持专业。

# 结论

设计一个周到的提示与微调模型的方式类似。虽然提示工程的主题不断增长和演变，但了解提示如何指导LLM不仅仅是定性检查时是重要的。当提示缺乏详细说明，或者输出要求突然变化时，定性测试效果不佳。

要了解提示在整个数据集中的价值，将定量测试尽可能结合起来是重要的。我们对Turbo模型在文本摘要任务中的定量评估方法包括以下步骤：

```py
1\. Build a meaningful test dataset
2\. Define text summarization performance metrics and custom metrics
3\. Compute all metrics for all data points in the dataset
4\. Compare resultant metrics of baseline prompts with the results of other prompts
   a. Compare each average metric values at a surface level
   b. Compare the difference in performance distributions for relevant metrics
```

通过这些步骤，任何人都可以**自动测量他们的LLM如何遵循提示规格**。此外，这些步骤概述了一种测试范式，在这种范式中，定性分析逐渐失去相关性。那些希望**以最少的人力投入实现自动化和可扩展性**的人应转向定量测试实践。

请继续关注未来的帖子，我们将讨论定量测试，以揭示LLM的**隐藏和不同的行为**！剧透：可能会出现性能意外下降的情况。

如果你渴望了解更多关于这篇博客的详细信息，这里有一个来自[Kolena](https://www.kolena.io/)的[CSV文件链接](https://drive.google.com/file/d/1fKzohW2XKFkAPniDYCmoeUxkbhxiQ6Uh/view?usp=sharing)，展示了**八个提示的所有汇总性能指标**（还有更多内容）。

所有图片均来自[Kolena](https://www.kolena.io/)，除非另有说明。请注意，类似的图表可以在常见的框架中手动生成，例如Matplotlib。
