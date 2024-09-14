# 探索大型语言模型中的创造力：从 GPT-2 到 GPT-4

> 原文：[`towardsdatascience.com/exploring-creativity-in-large-language-models-from-gpt-2-to-gpt-4-1c2d1779be57`](https://towardsdatascience.com/exploring-creativity-in-large-language-models-from-gpt-2-to-gpt-4-1c2d1779be57)

## 通过创造力测试分析大型语言模型中创意过程的演变

[](https://medium.com/@artfish?source=post_page-----1c2d1779be57--------------------------------)![Yennie Jun](https://medium.com/@artfish?source=post_page-----1c2d1779be57--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1c2d1779be57--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1c2d1779be57--------------------------------) [Yennie Jun](https://medium.com/@artfish?source=post_page-----1c2d1779be57--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1c2d1779be57--------------------------------) ·阅读时间 21 分钟 ·2023 年 4 月 11 日

--

![](img/7c92d68bd2a35c36d6c1f1499d438759.png)

Midjourney 设想中的创造力。由作者生成。

*本博客最初发布于* [`www.artfish.ai/p/exploring-creativity-in-large-language`](https://www.artfish.ai/p/exploring-creativity-in-large-language)

最近几周，人们使用大型语言模型（LLMs）生成了各种创意内容，如 [书籍](https://www.reuters.com/technology/chatgpt-launches-boom-ai-written-e-books-amazon-2023-02-21/)、[闪小说](https://blog.yenniejun.com/p/creative-writing-with-gpt-3-from)、[说唱对决](https://twitter.com/mehran__jalali/status/1639846978850021377?lang=en) 和 音乐和弦。但是否有可能更广泛地衡量这些模型中的创意过程水平？

人类创造力长期以来一直吸引着心理学家和研究人员。[自 1950 年代起](https://www.ideatovalue.com/podc/nickskillicorn/2021/04/the-1950-speech-that-started-creativity-research/)，研究人员创建了一系列测试，以比较个人的创造力表现和潜力。虽然没有单一测试能完全捕捉创造力，但这些测试尝试测量和量化其不同方面。

在这篇文章中，我分析了 2019 年到 2023 年间 GPT 模型在测量两种创造力的测试中的表现：收敛性（存在单一正确解决方案）和发散性（开放式；可能存在多种解决方案）[1]。这些测试包括：

+   用一个第四个词将三个看似不相关的词连接起来（[远程联想测试](https://en.wikipedia.org/wiki/Remote_Associates_Test)）

+   生成尽可能多的日常物品的替代用途（[替代用途任务](https://en.wikipedia.org/wiki/Guilford%27s_Alternate_Uses)）

+   命名十个尽可能不同的名词（[发散联想任务](https://en.wikipedia.org/wiki/Divergent_Association_Task)）

通过这些分析，我专注于从早期的 GPT-2 模型到最新的 GPT-4 模型的性能变化。我使用了主观和客观的视角来理解这些模型的行为。通常，较新的模型在所有创造性任务上表现更好，尽管我发现了一些例外情况，其中在测试有“正确”答案时，较简单的 GPT-2 模型超越了更新的 GPT-4 模型。在可能的情况下，我将 GPT 模型与人类在相同创造性测试上的表现进行比较。

*注意：这些实验的目的是测量模型生成中的创造性过程水平，而不是评估模型的创造力。我并不声称 GPT 模型或一般的 LLM 具有与人类相同的创造性思维。我旨在展示模型如何响应特定的创造性过程测量。*

# GPT 模型的发展历程

生成预训练变换器（GPT）模型是训练在大量互联网数据上的语言模型，以根据前面的词预测下一个词。虽然最近的 ChatGPT 在过去几个月引起了公众的关注，但 OpenAI 的 GPT 模型（以及这些模型背后的技术）已经存在几年了。

+   GPT-1（2018 年，117M 参数）。原始 GPT 模型。我使用了[HuggingFace 上的开源版本](https://huggingface.co/openai-gpt)。然而，输出结果非常无意义，所以我没有将它们包含在本文中。

    论文：[通过生成预训练提升语言理解](https://cdn.openai.com/research-covers/language-unsupervised/language_understanding_paper.pdf)

+   [GPT-2](https://en.wikipedia.org/wiki/GPT-2#:~:text=Generative%20Pre%2Dtrained%20Transformer%202,by%20OpenAI%20in%20February%202019.)（2019 年，1.5B 参数）。我使用了[HuggingFace 上的开源版本](https://huggingface.co/gpt2)。对于一些实验，GPT-2 的输出是无意义的，不利于理解其行为，所以我没有将这些结果包含在相应的分析中。论文：[语言模型是无监督多任务学习者](https://d4mucfpksywv.cloudfront.net/better-language-models/language_models_are_unsupervised_multitask_learners.pdf)。

+   [GPT-3](https://en.wikipedia.org/wiki/GPT-3)（2020 年，175B 参数）。比 GPT-2 大超过 100 倍。我使用了来自[OpenAI APIs](https://platform.openai.com/docs/models/gpt-3-5)的*text-davinci-002*。

    论文：[语言模型是少样本学习者](https://arxiv.org/pdf/2005.14165.pdf)

+   GPT-3.5（2022）。虽然 GPT-3.5 有多个版本，我使用的是与原版 [ChatGPT](https://en.wikipedia.org/wiki/ChatGPT) 相同的模型，该模型通过带有人工反馈的强化学习进行训练。我使用了来自 [OpenAI APIs](https://platform.openai.com/docs/models/gpt-3-5) 的 *gpt-3.5-turbo*。

+   [GPT-4](https://arxiv.org/abs/2303.08774)（2023，约 100T 参数）。我使用了来自 [OpenAI APIs](https://platform.openai.com/docs/models/gpt-3-5) 的 *gpt-4*。

    论文: [GPT-4 技术报告](https://arxiv.org/pdf/2303.08774.pdf)。

在本文的所有实验中，除非另有说明，我对每个独特的提示调用了每个 GPT 模型五次，以考虑其生成的随机性。

# 1\. 远程联想测试：连接无关词

远程联想测试（RAT）是由玛莎·梅德尼克（Martha Mednick）于 1962 年开发的一种创造力测试 [2]。该测试衡量的是聚合思维，这意味着**每个问题都有一个“正确”的答案**。

给定三个看似无关的词，测试者必须找到一个第四个词来连接这三个词。例如，“dream”、“break”和“light”可以通过“day”（daydream, daybreak, daylight）来联系。词汇“sense”、“courtesy”和“place”可以通过“common”来联系。

我从 [远程联想测试网站](https://www.remote-associates-test.com/) 收集了词汇三元组。**总共有 312 道问题**。每个问题的难度按从非常容易到非常困难的等级进行评分。

![](img/2f2d6ab51eb2f16a24735f7e563ff4a4.png)

从 [测试网站](https://www.remote-associates-test.com/) 获取的随机联想测试题目分解。由作者创建。

对于每个模型和每组三个词，我使用了以下提示模板。我调用了每个模型 5 次，以考虑随机性。

给定三个词。想出一个与前三个词都有某种关联的第四个词。首先给出答案（“Answer”），然后解释你的推理（“Reason”）。

```py
You are given three words. Think of a fourth word that is somehow related to each of the first three words. First give the answer ("Answer") then explain your reasoning ("Reason").Words: {Word1}, {Word2}, {Word3}
```

# 较新的 GPT 模型在远程联想上得分更高

我计算了每个 GPT 模型生成正确答案的总体百分比。可能并不奇怪，**模型越新，生成的正确答案越多**。

![](img/bdaad2048a6c8c37702328e4b74d82bd.png)

每个 GPT 模型在远程联想测试中正确回答问题的百分比。由作者创建。

按照问题难度级别分解模型表现显示了类似的趋势：较新的 GPT 模型在每个难度级别上的表现均优于旧模型。虽然较新的 GPT 模型在难题上的表现优于旧模型，但这种改进比在简单问题上的改进要少。这意味着**即使是 GPT-4 在困难问题上也感到吃力**。

![](img/a0449267a411f9f1f2c16850305c1c9e.png)

每个 GPT 模型在远程联想测试中按问题难度级别的正确回答百分比。由作者创建。

根据远程联想测试网站，“典型的人可以解决大多数标记为简单的问题，大约一半的中等难度问题，以及很少的难问题。” 网站上没有分享官方统计数据，但从快速浏览来看，GPT-4 确实解决了大约一半的中等难度问题和一些难问题，使其成为唯一一个大致可与人类水平相媲美的模型。其他 GPT 模型的表现则逊色于人类水平。

# 一些例外：当 GPT-2 回答正确而 GPT-4 回答错误

尽管不常发生，但确实有一些问题，2019 年的更基础的 GPT-2 模型能够生成正确答案，而 2023 年的 GPT-4 模型却不能。实际上，有 8 个这样的例子。（完整列表见文章末尾的附录）。

**一个简单的问题**

+   单词：小屋 / 砖块 / 蛋糕

+   GPT-2：“奶酪” [正确答案]

+   GPT-4：“层”

**一个中等难度的问题**

+   单词：猫 / 数字 / 电话

+   GPT-2：“呼叫”

+   GPT-4：“拨号”

**一个难的问题**

+   单词：通过 / 磁盘 / 路径

+   GPT-2：“驾驶” [正确]

+   GPT-4：“圆圈”

**一个非常难的问题**

+   单词：土地 / 手 / 房子

+   GPT-2：“农场” [正确]

+   GPT-4：“财产”

请注意，对于所有这些问题，GPT-4 的回答是 *不* 不合理的。实际上，如果事先不知道正确答案，你可能会认为 GPT-4 的答案同样可以将这三个词联系起来。如果你这样想，你并没有错。我将在下一部分讨论这个问题。

# GPT-4 是错误的，还是只是思维方式不同？

GPT-4 错误地回答了超过一半的问题，但可能有充分的理由。我让 GPT-4 解释了每一个答案。然后，我查看了它对几个“非常难”问题的解释。

**单词：艺术家 / 孵化 / 路线**

+   正确答案：“逃脱”

+   GPT-4 答案：“地图”

> “艺术家”这个词可能会让人想到创造某些东西，甚至是某种视觉表现形式。地图是一种常见的视觉表现形式，需要一定的艺术技能来制作。
> 
> “孵化”这个词可以指打开某物或使某物存在的行为。地图是一种蓝图或计划，通常用于导航新领土或规划行动路线。
> 
> “路线”这个词与方向和运动相关，这些都可以在地图上绘制和追踪。地图可以帮助你确定旅行时最佳的路线，并且可以提供关于其他人过去所走过路线的信息。
> 
> 总的来说，“地图”似乎是一个与最初的三个词相符的词，因为它包含了艺术创作、规划和方向，以及运动和导航的概念。

GPT-4 对于为什么“地图”是一个与“艺术家 / 孵化 / 路线”相关的好词的解释很有说服力。

这是另一个例子：

**词汇：fork / dark / man**

+   正确答案：“pitch”

+   GPT-4 的答案：“shadow”

> “fork”这个词可以与分裂或分支相关联，比如道路上的分叉。阴影也可以被视为一种分裂，因为它们代表了光的缺失。
> 
> “dark”这个词指的是光的缺失。当光被物体阻挡时，阴影会形成，且往往会比周围区域显得更暗。
> 
> “man”这个词可以与身份或个体性概念相关联。阴影也可以用来表示一个人的身份，因为它们可以提供有关他们形状和大小的线索。
> 
> 因此，“阴影”似乎是一个很好地适应最初三个词的词汇，因为它包含了分裂、缺失和身份的概念。

这些例子展示了远程联想测试的陷阱，以及一般收敛创造力测试的陷阱——即“正确”答案可能并不是唯一的“对”的答案。在这些情况下，GPT-4 生成了一个“错误”的答案，但仅仅是因为它没有符合传统的“正确”答案。

实际上，如果我让 GPT-4 为一组三个词中的第四个词进行头脑风暴，它能够给出多个选项，并提供令人信服的答案。

![](img/efe8aafae763d1ba30351195a4efd50b.png)

GPT-4 生成多个“正确”答案以回应一组三个词。截图由作者拍摄。

# GPT-4 答案任意生成的三个词组合

GPT 模型可能已经在其训练数据中看到了一些上述远程联想测试问题。GPT-4 会如何回应任意生成的三个词组合？在这一部分，我构建了我自己的“远程联想”测试，并主观地评估了输出结果。

我从[10,000 个最常见的英语单词](https://github.com/first20hours/google-10000-english) [3]中随机抽取了三个单词，并让 GPT-4 提供一个将这些单词联系在一起的第四个单词。这些三词组合中的一些相当奇怪且不相关，但 GPT-4 总是能找到将它们联系在一起的方式。我鼓励读者在阅读 GPT-4 的生成结果之前先尝试这些！

在查看接下来的几个例子后，你可以做出决定——GPT-4 是在跳出框框思考吗？还是只是将答案勉强套入框框？（文章末尾的附录中展示了更长的例子列表）。

⛑️ **健康**

这个回应显得相当有说服力。我发现“健康监测”的定义几乎是从[CDC 公共健康监测介绍](https://www.cdc.gov/training/publichealth101/surveillance.html#anchor_available_materials)网站上逐字复制的，这一点很有趣。

> *公共健康监测是“对与健康相关的数据进行持续、系统的收集、分析和解释，这些数据对于规划、实施和评估公共健康实践至关重要。” —* 领域流行病学
> 
> （来自 CDC 网站）

```py
Words: dietary / surveillance / bornAnswer: HealthReason: 
1\. Dietary - A person's diet is directly related to their health, as it can affect their overall well-being, energy levels, and risk for certain diseases.
2\. Surveillance - Health surveillance is the ongoing, systematic collection, analysis, and interpretation of data essential to the planning, implementation, and evaluation of health practice.
3\. Born - When a person is born, their health is of utmost importance, as it can have long-lasting effects on their development, growth, and overall quality of life.
```

**🍀 三叶草**

我最初对 GPT-4 的回答感到信服——事实上，这个回答感觉相当聪明。直到我做了一个快速的 Google 搜索，发现*Trifolium duodecim* 并不存在。这是一个[幻觉](https://en.wikipedia.org/wiki/Hallucination_(artificial_intelligence))的例子，幻觉是指大型语言模型编造出不准确的答案的常见现象。

```py
Words: lucky / field / twelveAnswer: CloverReason: 
1\. Lucky - A four-leaf clover is considered a symbol of good luck.
2\. Field - Clovers are plants that can be commonly found growing in fields.
3\. Twelve - There is a variety of clover known as "Trifolium duodecim" (Twelve-leaf clover), which has 12 leaflets.
```

**☕️ 星巴克**

这个例子是我最喜欢的，虽然它跳出了常规思维，但感觉有点牵强。而且这有点搞笑（尤其是关于 Patrick 的部分）。

```py
Words: grande / cognitive / patrickAnswer: StarbucksReason: 
1\. Grande: Starbucks uses the term "grande" to refer to one of its medium-sized drinks.
2\. Cognitive: Starbucks is a popular place for people to go to relax, study, and work, which are all activities that require cognitive function.
3\. Patrick: Patrick Dempsey, the actor who played Dr. Derek Shepherd on the television show Grey's Anatomy, is known for his love of coffee and was nicknamed "McDreamy." Starbucks is a well-known coffee shop, so it's possible that his character would have frequented Starbucks.
```

这些例子和分析说明了具有单一正确答案的创造力测试可能存在局限性。这一局限性在本文下一部分探讨的其他测试中得到了补充。

# 2\. 替代用途任务：在普通物品中寻找意义

另一种常见的测量创造力的测试是替代用途任务（AUT），由 J. P. Guilford 于 1967 年创建。与远程联想测试不同，这个测试测量发散性思维，这意味着**没有正确答案**。

测试者被要求为一个日常物品提供尽可能多的原创用途。例如，一个“笔记本电脑”可以用作“盘子”、“护体盾牌”或“墙面艺术”[4]。对这个测试的回答通过原创性（用途的不同程度）和灵活性（创意的多样性）等指标进行评估。此测试的目的是查看一个人能为普通物品想出多少种非常规用途。

然而，由于资源有限，我没有能力在没有人工标注者的情况下生成这些测量值。因此，我使用了**创造力的代理**，这也是阿姆斯特丹大学的研究人员在[《对 GPT-3 创造力的（替代用途）测试》](https://arxiv.org/pdf/2206.08932.pdf)中使用的方法。

在这篇论文中，研究人员要求人类和 GPT-3（davinci-002）为以下常见物品提出替代用途：叉子、铁罐和书籍。作为衡量创造力的代理，他们测量了*语义距离*——即生成的用途与原始物品之间的差异有多大？

我从[论文项目网站](https://osf.io/vmk3c/?view_only=)获取了人工回答，以便与人类表现进行比较。我使用了以下提示供 GPT 模型使用，基于论文中使用的类似提示。

> 为以下物品想出 10 种可能让人觉得聪明、不同寻常、有趣、不常见、幽默、创新或与众不同的创造性用途：{item}

# 嵌入向量之间的余弦距离作为创造力的代理

在自然语言处理领域，语义距离（或[语义相似性](https://en.wikipedia.org/wiki/Semantic_similarity)）是基于词汇在相似上下文中一起使用的频率来衡量两个词汇相关程度的代理。例如，“计算机”和“屏幕”这两个词可能有较短的语义距离，因为它们常常在相同的上下文中使用，而“计算机”和“扁豆”这两个词的语义距离可能较大，因为它们很少在相同的上下文中出现。

为了计算两个词的语义距离，我使用了与研究人员在[上述论文](https://arxiv.org/pdf/2206.08932.pdf)中使用的相同方法。我使用了[Spacy](https://spacy.io/models/en) (`en_core_web_lg`)，一个自然语言处理库，来计算物体（例如“笔记本电脑”）和使用案例词汇（例如“盘子”）的词嵌入。然后，我使用这两个嵌入之间的余弦距离来计算语义距离。

对于替代用途任务，**较大的语义距离与“更具创造性”**的物体使用案例相关。结果显示，对于三个物体（叉子、锡罐和书），人类的表现通常导致更高的余弦距离（因此更高的语义差异）相比于 GPT-3。然而，也有一些情况下，GPT-3.5 和/或 GPT-4 达到了或超越了人类创造力的语义距离得分。

![](img/39de38d8fc97ba32e9443e9288a67949.png)

原始词汇与替代使用案例的语义距离（使用余弦距离测量）。由作者创建。

虽然人类倾向于提出比 GPT-3 更具创造性的使用案例（使用余弦距离作为创造力的代理），GPT-4 的表现更接近于人类水平。也就是说，GPT-4 的回应生成了与原始项目相比具有较大语义距离的项目，其水平与人类表现相似。然而，由于没有实际的人工标注和决定更主观的类别如原创性、灵活性和实用性，这种使用余弦距离的量化方法是有限的，因为它只测量了 GPT 模型创造性输出的一小部分。

# 你能分辨出人工生成和 AI 生成的使用案例吗？

列表之一展示了人工生成的锡罐替代使用案例，另一个则展示了由 GPT-4 生成的替代使用案例。你能分辨出区别吗？答案在最后。

列表 1：锡罐的用途

+   作为镜子

+   用来制作玩具

+   作为艺术品

+   作为对安迪·沃荷的提醒

+   作为带有罐子夹的钥匙圈

+   作为珠宝

列表 2：锡罐的用途

+   微型鼓套件

+   古怪的花盆

+   临时饼干切割器

+   自制营炉

+   异想天开的风铃

+   微型草药花园

下一部分的测试也测量了发散性思维。

# 3\. 发散联想任务：十个词，没有连接

第三个创造力测试是发散性联想任务（DAT），由一组研究人员在 2021 年最近创建[5]。此测试要求命名十个尽可能不同的名词。显而易见，这项测试是发散性的，而且**没有正确答案**。

有一个[在线测试版本](https://www.datcreativity.com/)。完成测试只需几分钟，因为它涉及列出十个尽可能不相关的名词。以下是我从网站上获得的结果：

![](img/aa3a465a40574d28ff017b98685d1feb.png)

从发散性联想任务中截取的屏幕截图。由作者创建。

## DAT 分数

如何计算分数？作者在[这里](https://www.datcreativity.com/faq)描述了他们的过程。DAT 分数是通过嵌入和余弦距离计算的。我对他们的计算分数过程进行了调整[6]：

1.  计算每个单词的[GloVe](https://nlp.stanford.edu/projects/glove/)嵌入

1.  计算每对单词之间的余弦距离

1.  取该值的平均值

这个过程类似于在替代用途任务中计算创造力代理分数的方法，唯一的区别是使用了不同的单词嵌入模型。

## 比较不同模型之间的差异

我用来提示 GPT 模型的以下提示语取自[DAT 网站](https://www.datcreativity.com/task)。

```py
Please enter 10 words that are as different from each other as possible, in all meanings and uses of the words.Rules
- Only single words in English.
- Only nouns (e.g., things, objects, concepts).
- No proper nouns (e.g., no specific people or places).
- No specialized vocabulary (e.g., no technical terms).
- Think of the words on your own (e.g., do not just look at objects in your surroundings).
```

我没有在下面的结果中包括 GPT-2，因为 GPT-2 生成的输出是乱码。（你可以在文章末尾的附录中看到一个示例）。

结果显示，每个更新的模型 DAT 分数都提高了。这意味着后来的模型在生成尽可能语义不同的 10 个单词方面表现更好。

![](img/47b6d5b8fda43230d69db455aa67f200.png)

按照余弦距离测量的不同模型的发散性联想测试分数。由作者生成。

例如，以下是 GPT-3 生成的列表，其分数为 0.36：

```py
dusk, night, day, dawn, morning, afternoon, evening, midnight, twilight
```

另一方面，以下是 GPT-4 生成的列表，其分数为 0.95：

```py
elephant, microscope, sandcastle, galaxy, harmony, spatula, glacier, philosophy, tambourine, drought
```

根据[介绍 DAT 测试的论文](https://www.pnas.org/doi/10.1073/pnas.2022340118)，分数通常在 65 到 90 之间，平均在 75 到 80 之间。95 的分数被认为非常高。

上图中需要注意的是，GPT-4 的误差条远小于 GPT-3 的。这一点在下一个实验中会更加清晰，我在实验中通过调整“温度”参数来改变每个 GPT 模型生成内容的随机性。

温度是一个介于 0 和 1 之间的值，接近 0 的值最具可预测性，接近 1 的值则最为随机。我尝试了介于 0.5 和 0.9 之间的温度值，每次递增 0.1（在之前的实验中，我将温度保持在 0.7，这是默认值）。我期望更高温度值会带来更多的变化，而较低的温度则会更一致。

然而，结果显示温度对语义距离变异性的影响不如模型本身重要。对于 GPT-3 而言，尽管有温度的影响，每组 10 个词的 DAT 得分变化很大。而对于 GPT-4 而言，尽管有温度的影响，DAT 得分在一个小范围内保持了高度一致，变化非常小。

![](img/b7c569d6c2a668b897761c671eca6ea5.png)

按模型类型和温度分数分开的词嵌入的平均余弦距离。作者创建。

一旦我查看了三种 GPT 模型生成的实际文本，这一点变得更加清晰。**GPT-4 在生成词汇时的变异性远低于 GPT-3。** 从某种程度上说，感觉 GPT-4 似乎已经知道了一个应该没有正确答案的任务的“正确答案”。

汇总所有温度分数的响应，我让每个模型生成 10 个词汇的列表 50 次。然后，我查看了不同词汇在所有 50 次运行中的出现频率。值得注意的是，GPT-3 最常生成的词是 book，出现了 50 次中的 19 次 = 38%。另一方面，GPT-4 最常生成的词是显微镜，出现了**48/50=96%**。

![](img/323fb7c8bfe263f62d4408741add0e58.png)

每个模型为 DAT 测试生成的顶级词汇（根据温度分数汇总）。由词汇创建。

这验证了 GPT-3 在生成词汇时确实比 GPT-4 更具变异性，而 GPT-4 则采取了更为保守的策略，预测了许多它知道会在“多样化词汇”任务中得高分的词汇——如显微镜、火山、大象和银河。

这一发现展示了 GPT-4 似乎已经“破解了生成多样化词汇的密码”。如果是这样，我不确定是否可以自信地说 GPT-4 的创造力更高，还是它已经找到了一个“如何最好地玩这个特定游戏”的局部最优解。

# 结论

> *“我们可以设想一个未来，在这个未来中，GPT-3 和其他生成型 LLMs 的响应无法与人类区分，尽管创造力过程将会不同。”*
> 
> — 来自* [*GPT-3 创造力的另类用途测试*](https://arxiv.org/abs/2206.08932)

使用有限的文本基础测试准确衡量如此复杂多面的创造力是具有挑战性的。虽然像[视觉远程联想测试](https://www.frontiersin.org/articles/10.3389/fpsyg.2020.00026/full)这样的测试通过加入视觉元素来尝试解决这个问题，但这些评估仍然存在固有的局限性。具有“正确答案”的聚合测试（例如远程联想测试）可能不会总是奖励最具创造性或正确的解决方案。另一方面，没有特定正确答案的发散测试（例如发散联想测试）依赖于不完美的词嵌入，并且可能包含偏差。

此外，GPT 模型在这些测试中的表现可能会受到训练过程中对任务的暴露影响，可能导致数据重叠。例如，GPT-4 在发散联想测试中的表现展示了单词的特殊重复，表明它可能提前了解了“正确”的答案。开发替代测试方法至关重要，因为有[关于 GPT-4 接受测试数据训练的传闻](https://www.reddit.com/r/MachineLearning/comments/124eyso/n_openai_may_have_benchmarked_gpt4s_coding/)，特别是在编码任务中。

尽管有这些局限性，创造力测试仍然可以作为比较和跟踪大型语言模型表现的有价值基准。未来的研究实验可以在其他语言中重复本文中的实验，以评估 GPT 模型在语言领域的创造力。这样，我们可以获得对 AI 生成内容的更全面理解，并进一步探索这些先进语言模型的能力和局限性。

*感谢阅读这篇文章！*

# 附录

# 远程联想测试：GPT 模型正确回答的问题比较

我们可以查看模型正确回答的每个问题的难度级别分解。我们可以看到 GPT3、GPT3.5 和 GPT4 在回答难题的能力上相似。

![](img/a500fe60944887f82bd908a91ed6fbbe.png)

# GPT-2 答对的远程联想和 GPT-4 答错的部分

```py
Words:  cat / number / phone
Level:  Medium
Answer: call
GPT2:   call 
GPT4:   dialWords:  land / hand / house
Level:  Very Hard
Answer: farm
GPT2:   farm 
GPT4:   propertyWords:  water / pen / soda
Level:  Unknown
Answer: fountain
GPT2:   fountain 
GPT4:   liquidWords:  measure / worm / video
Level:  Very Easy
Answer: tape
GPT2:   tape 
GPT4:   lengthWords:  cottage / brick / cake
Level:  Very Easy
Answer: cheese
GPT2:   cheese 
GPT4:   layerWords:  through / disk / way
Level:  Hard
Answer: drive
GPT2:   drive 
GPT4:   circleWords:  skunk / kings / boiled
Level:  Very Easy
Answer: cabbage
GPT2:   cabbage 
GPT4:   smellWords:  jump / kill / bliss
Level:  Hard
Answer: joy
GPT2:   joy 
GPT4:   height
```

# GPT-4 对随机生成的远程联想测试问题的回答

这里是 GPT-4 为几个随机生成的远程联想测试问题生成的输出。

**戒指**

这个感觉有点勉强。如果你对你的感情生活感到非常厌倦，我可以接受关于戒指是“复杂性象征，尤其是在关系中的”那部分。但最后的部分感觉有点生硬。

```py
Words: complications | saturn | drops
Answer: Rings
Reason: 
1\. Complications: Rings can be a symbol of complications, especially in relationships, as they represent commitment and the complexities that come with it.
2\. Saturn: Saturn is famously known for its beautiful and prominent rings, made primarily of ice particles and some rocky debris.
3\. Drops: A ring can be seen as a drop or loop of material, such as metal or plastic, that forms a complete circle.
```

**革命。** [蒙特卡西诺](https://en.wikipedia.org/wiki/Monte_Cassino)确实是一个真实的地方，二战期间确实遭到轰炸。

```py
Words: belfast | movement | monte
Answer: Revolution
Reason: 
1\. Belfast is a city in Northern Ireland that has experienced significant political and social changes due to the Irish Republican movement. 
2\. Movement is a general term that can describe political and social changes, often caused by revolutions. 
3\. Monte is likely referring to Monte Cassino, a historic site in Italy where a significant battle occurred during World War II, which was a time of global revolution and change. 
```

```py
Therefore, the word "revolution" is related to each of the first three words as it represents significant change and upheaval.
```

**帮助。** 通过帮助中心将“南安普顿”这座城市与“帮助”连接起来的有趣方式……

```py
Words: ensures | assistant | southampton
Answer: Help
Reason: 
1\. Ensures: "Help" is related to "ensures" as it is an action that can ensure the success or completion of a task.
2\. Assistant: "Help" is related to "assistant" because an assistant provides help in various tasks and activities.
3\. Southampton: "Help" is related to "Southampton" as Southampton is a city in England which has various help centers and services, such as the Southampton Voluntary Services, providing assistance to the community.
```

**互联网。** 我认为它的论点是相当正确的。

```py
Words: uruguay | accessible | requirement
Answer: Internet
Reason: Uruguay has a government initiative called Plan Ceibal, which aims to make the Internet accessible to all its citizens, fulfilling a requirement for digital inclusion and reducing the digital divide.
```

**语言。** 可能有点通用，但我想它还是有效的。

```py
Words: compensation | atlas | prefix
Answer: Language
Reason: 
1\. Compensation - In linguistics, compensation is a phenomenon where a sound change in one part of a word is "compensated" by a change in another part of the word to maintain the overall structure of the word. 
2\. Atlas - An atlas is a collection of maps, which often include labels and descriptions in a specific language.
3\. Prefix - A prefix is a morpheme or a linguistic element that is added to the beginning of a word to create a new word with a different meaning, which is a concept in language and grammar.
```

**健康。** 健康监测部分有点阴暗，但我想它还是有效的。

```py
Words: dietary | surveillance | born
Answer: Health
Reason: 
1\. Dietary - A person's diet is directly related to their health, as it can affect their overall well-being, energy levels, and risk for certain diseases.
2\. Surveillance - Health surveillance is the ongoing, systematic collection, analysis, and interpretation of data essential to the planning, implementation, and evaluation of health practice.
3\. Born - When a person is born, their health is of utmost importance, as it can have long-lasting effects on their development, growth, and overall quality of life.
```

**ERIC！** 这个其实还挺有趣的。

```py
Words: eric | figures | purchasing
Answer: accountant
Reason: An accountant is a professional who can be named Eric, works with figures (numbers), and is often involved in purchasing decisions for businesses.
```

# GPT-2 在发散联想测试中的输出

我排除了 GPT-2 在发散测试分析中的结果，因为它的输出完全是胡言乱语。即使我尽可能简化提示，它的输出也不太有用。我在这里包含了一个示例，因为我实际上觉得它们非常有趣。

```py
List 10 words that are as different from each other as possible:"Merry Christmas""I'm not a robot, we're the robots""It's a good thing we're all robots""I'm just a robot""I'm just a robot""I'm just a robot""I'm just a robot""I'm not a robot""I'm just a robot""I'm just a robot"
```

[1] [吉尔福德, J. P.](https://en.wikipedia.org/wiki/J._P._Guilford) (1967). *人类智慧的本质*。

[2] Mednick, S. A., & Mednick, M.T. (1959,1962). 远程联想测试，大学及成人版。

[3] 通过对[Google’s Trillion Word Corpus](https://books.google.com/ngrams/info)的[n-gram](https://en.wikipedia.org/wiki/N-gram) [频率分析](https://en.wikipedia.org/wiki/Frequency_analysis)，确定了最常见的 10,000 个英语单词。我进一步筛选了这个列表，去除了停用词（例如，“the”、“and”和“you”这样的常见词）。更多信息见此[仓库](https://github.com/first20hours/google-10000-english)。

[4] 我曾在一次约会应用上发布了这个，但很遗憾，没有人觉得好笑。

[5] Olson, Jay A.；Nahas, Johnny；Chmoulevitch, Denis；Cropper, Simon J.；Webb, Margaret E.（2021 年 6 月 22 日）。[“命名无关的词预测创造力”](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8237676)。*《国家科学院学报》*。**118**（25）：e2022340118。 [doi](https://en.wikipedia.org/wiki/Doi_(identifier))：[10.1073/pnas.2022340118](https://doi.org/10.1073%2Fpnas.2022340118)。 [PMC](https://en.wikipedia.org/wiki/PMC_(identifier)) [8237676](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8237676)。 [PMID](https://en.wikipedia.org/wiki/PMID_(identifier)) [34140408](https://pubmed.ncbi.nlm.nih.gov/34140408)

[6] 作者从 10 个词中取了 7 个子集来计算分数。我保留了所有 10 个词。此外，作者使用了 840B-GloVe 嵌入，而我使用了较小的 6B-GloVe 嵌入。我将我的 GloVe 嵌入结果与官方 DAT 分数进行了比较，发现尽管存在这些差异，结果是相当的。
