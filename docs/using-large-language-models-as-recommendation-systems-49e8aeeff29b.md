# 使用大型语言模型作为推荐系统

> 原文：[https://towardsdatascience.com/using-large-language-models-as-recommendation-systems-49e8aeeff29b?source=collection_archive---------0-----------------------#2023-04-10](https://towardsdatascience.com/using-large-language-models-as-recommendation-systems-49e8aeeff29b?source=collection_archive---------0-----------------------#2023-04-10)

## 最近研究的综述及自定义实现

[](https://medium.com/@mohammadhia?source=post_page-----49e8aeeff29b--------------------------------)[![Mohamad Aboufoul](../Images/af1edb6a878499e693b398c3bda7a0c5.png)](https://medium.com/@mohammadhia?source=post_page-----49e8aeeff29b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----49e8aeeff29b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----49e8aeeff29b--------------------------------) [Mohamad Aboufoul](https://medium.com/@mohammadhia?source=post_page-----49e8aeeff29b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F48bf72143c5f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-large-language-models-as-recommendation-systems-49e8aeeff29b&user=Mohamad+Aboufoul&userId=48bf72143c5f&source=post_page-48bf72143c5f----49e8aeeff29b---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----49e8aeeff29b--------------------------------) · 8分钟阅读 · 2023年4月10日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F49e8aeeff29b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-large-language-models-as-recommendation-systems-49e8aeeff29b&user=Mohamad+Aboufoul&userId=48bf72143c5f&source=-----49e8aeeff29b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F49e8aeeff29b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-large-language-models-as-recommendation-systems-49e8aeeff29b&source=-----49e8aeeff29b---------------------bookmark_footer-----------)![](../Images/0777460e6a57934a4dced98e05c6b490.png)

图片来源：[卢卡·巴吉奥](https://unsplash.com/@luca42?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

近年来，大型语言模型（LLMs）在数据科学界和新闻界引起了极大的关注。自2017年变压器架构问世以来，我们见证了这些模型在自然语言任务复杂性方面的指数级进步，包括分类、意图与情感提取，以及生成与人类相似的文本。

从应用角度来看，将 LLMs 与各种现有技术结合使用，来弥补它们的缺陷（我最喜欢的之一是[GPT + Wolfram Alpha 组合](https://writings.stephenwolfram.com/2023/03/chatgpt-gets-its-wolfram-superpowers/) 用于处理数学和符号推理问题），其可能性似乎无穷无尽。

但令我惊讶的是，LLMs 也可以作为推荐系统单独使用，而无需任何额外的特征工程或其他常见推荐系统中的手动过程。这种能力主要归功于 LLMs 的预训练方式及其操作方式。

# 目录

+   ***LLMs 和变换器如何工作的回顾***

+   ***LLMs 作为推荐系统***

+   ***使用自定义数据实现/复制 P5***

+   ***复制尝试 2 — 阿拉伯语***

+   ***LLMs 作为推荐系统的优点和缺点***

+   ***最终思考***

+   ***代码***

# LLMs 和变换器如何工作的回顾

语言模型是概率模型，试图映射一系列标记（短语、句子等）的发生概率。它们在各种文本上进行训练，并相应地推导概率分布。对于它们可以处理的各种任务（总结、问答等），它们通过条件概率迭代地选择最可能的标记/词汇来继续提示。请参见下面的示例：

![](../Images/7b434459a3c104cfeaccb6a235198ce3.png)

基于上下文的后续标记的概率示例（作者提供的图片）

LLMs 是经过大量文本训练的语言模型，这些模型具有庞大的架构，并利用了大量计算资源。它们通常由变换器架构驱动，这种架构在 Google 的著名 2017 年论文“[Attention Is All You Need](https://arxiv.org/pdf/1706.03762.pdf)”中被引入。这种架构利用了[“自注意力”机制](https://ai.googleblog.com/2017/08/transformer-novel-neural-network.html)，使得模型能够在预训练过程中学习不同标记之间的关系。

在对足够大的文本集合进行预训练后，相似的词汇将具有相似的嵌入（例如：“King”，“Monarch”），而不同的词汇将具有更大的差异。此外，利用这些嵌入，我们将看到词汇之间的代数映射，使模型能够更有效地确定序列的正确下一个标记。

+   就像经典的[“King — Man + Woman = Queen”](https://kawine.github.io/blog/nlp/2019/06/21/word-analogies.html) 示例一样

自注意力嵌入的附加好处是，它们会根据周围的词汇而有所变化，使其更贴合于特定上下文中的含义。

斯坦福大学的克里斯托弗·曼宁博士提供了一个关于 LLMs 如何工作的[高层次概述](https://www.youtube.com/watch?v=YfXc4OBDmnM&ab_channel=Databricks)。

# LLMs 作为推荐系统

2022年，Rutger’s University 的研究人员发布了论文**《推荐作为语言处理（RLP）：统一预训练、个性化提示与预测范式（P5）》**（Geng et. al）。论文中介绍了一种“灵活统一的文本到文本范式”，将多个推荐任务结合在一个系统中：P5。该系统能够通过自然语言序列执行以下操作：

+   顺序推荐

+   评分预测

+   解释生成

+   评论总结

+   直接推荐

让我们来看看论文中的一个顺序推荐任务示例。

```py
Input: "I find the purchase history list of user_15466:
4110 -> 4467 -> 4468 -> 4472
I wonder what is the next item to recommend to the user. Can you help
me decide?"
Output: "1581"
```

研究人员为用户和每个项目分配了唯一的 ID。使用包含数千名用户（及其购买历史）和唯一项目的训练集，LLM 能够学习到某些项目彼此相似以及某些用户对某些项目有倾向（由于自注意力机制的特性）。在所有这些购买序列的预训练过程中，模型本质上经历了一种[协同过滤](https://developers.google.com/machine-learning/recommendation/collaborative/basics)的形式。它查看用户购买了相同的项目以及哪些项目往往一起购买。结合 LLM 生成上下文嵌入的能力，我们突然拥有了一个非常强大的推荐系统。

在上述示例中，尽管我们不知道每个 ID 对应的项目是什么，但我们可以推断出项“**1581**”被选择的原因是其他用户购买了它，并且与“**user_15466**”已经购买的任何项一起购买了。

关于 P5 的架构，它**“利用预训练的 T5 检查点作为骨干”**（Geng et. al）。

+   [T5 是另一个 LLM](https://ai.googleblog.com/2020/02/exploring-transfer-learning-with-t5.html)，谷歌几年前发布的。它被设计来处理多种类型的序列到序列任务，因此作为这种系统的起点是合理的。

# 使用自定义数据实现/复制 P5

我对这篇论文印象深刻，想看看是否可以在较小的规模上复制顺序推荐能力。我决定利用 Hugging Face 的开源 T5 模型（[T5-large](https://huggingface.co/t5-large)），并制作了自己的自定义数据集来微调它以产生推荐。

我制作的数据集包含了超过 100 个运动设备购买的示例以及下一个要购买的项目。例如：

```py
Input: “Soccer Goal Post, Soccer Ball, Soccer Cleats, Goalie Gloves”
Target Output: “Soccer Jersey”
```

当然，为了使其更为强健，我决定使用更具体的提示。提示如下：

> ***输入:*** “购买的物品: {足球门框, 足球, 足球鞋, 守门员手套} — 推荐候选: {足球球衣, 篮球球衣, 橄榄球球衣, 棒球球衣, 网球衬衫, 曲棍球球衣, 篮球, 橄榄球, 棒球, 网球, 曲棍球, 篮球鞋, 橄榄球鞋, 棒球鞋, 网球鞋, 曲棍球头盔, 篮球臂套, 橄榄球护肩, 棒球帽, 网球拍, 曲棍球滑冰鞋, 篮球架, 橄榄球头盔, 棒球棒, 曲棍球棍, 足球标志, 篮球短裤, 棒球手套, 曲棍球护具, 足球护腿板, 足球短裤} — 推荐: ”
> 
> ***目标输出:*** “足球球衣”

上面你可以看到用户目前购买的物品规格，随后是尚未购买的推荐候选列表（这是整个库存）。

在使用 [Hugging Face 的 Trainer API](https://huggingface.co/docs/transformers/main_classes/trainer) (*Seq2SeqTrainer* 训练了约 10 个周期) 对 T5 模型进行微调后，我获得了一些令人惊讶的好结果！一些示例评估：

> ***输入:*** “购买的物品: {足球球衣, 足球门框, 足球鞋, 守门员手套} — 推荐候选: {篮球球衣, 橄榄球球衣, 棒球球衣, 网球衬衫, 曲棍球球衣, 足球, 篮球, 橄榄球, 棒球, 网球, 曲棍球, 篮球鞋, 橄榄球鞋, 棒球鞋, 网球鞋, 曲棍球头盔, 篮球臂套, 橄榄球护肩, 棒球帽, 网球拍, 曲棍球滑冰鞋, 篮球架, 橄榄球头盔, 棒球棒, 曲棍球棍, 足球标志, 篮球短裤, 棒球手套, 曲棍球护具, 足球护腿板, 足球短裤} — 推荐: ”
> 
> ***模型输出:*** “足球”
> 
> ***输入:*** “购买的物品: {篮球球衣, 篮球, 篮球臂套} — 推荐候选: {足球球衣, 橄榄球球衣, 棒球球衣, 网球衬衫, 曲棍球球衣, 足球, 橄榄球, 棒球, 网球, 曲棍球, 足球鞋, 橄榄球鞋, 棒球鞋, 网球鞋, 曲棍球头盔, 守门员手套, 橄榄球护肩, 棒球帽, 网球拍, 曲棍球滑冰鞋, 足球门框, 篮球架, 橄榄球头盔, 棒球棒, 曲棍球棍, 足球标志, 篮球短裤, 棒球手套, 曲棍球护具, 足球护腿板, 足球短裤} — 推荐: ”
> 
> ***模型输出:*** “篮球鞋”

这当然是主观的，因为推荐不一定是二元的成功/失败，但输出与迄今为止的购买项相似，确实令人印象深刻。

# 复制尝试 2 — 阿拉伯语

接下来，我想看看是否可以为阿拉伯语做这个，所以我翻译了我的数据集，并寻找了一些能够处理阿拉伯语文本的公开可用T5模型（[AraT5](https://huggingface.co/UBC-NLP/AraT5-base), [MT5](https://huggingface.co/google/mt5-large)等）。在尝试了十几个我在[Hugging Face Hub](https://huggingface.co/models)上找到的变体后，我遗憾地发现无法得到令人满意的结果。

模型（经过微调后）会推荐相同的1或2件商品，不管购买历史如何——通常是“كرة القدم”，即“soccer ball”（***不过，也许它知道阿拉伯语使用者喜欢足球，并且总是寻找新的足球***）。即使尝试了这些模型的更大版本，如MT5-xl，我也得到了相同的结果。这很可能是由于这些LLM在英语以外的语言上的数据稀缺。

对于我的最后一次尝试，我决定尝试将Google Translate API与我的英语微调T5模型结合使用。过程如下：

+   将阿拉伯语输入 → 翻译成英语 → 输入到经过英语微调的模型中 → 获取模型的英语预测 → 翻译回阿拉伯语

不幸的是，这仍然没能帮助多少，因为翻译器会犯一些错误（例如：“كرة القدم”，我们用来代替“soccer”的词直接翻译成“foot ball”），这让模型出错，导致始终推荐相同的1-2件商品。

# LLM作为推荐系统的优点和陷阱

这种技术最突出的优点在于它作为独立系统实施的简便性。由于上述LLM和预训练技术的性质，我们可以绕过繁重的手动特征工程——模型应该能够自然地学习表示和关系。此外，我们可以在一定程度上绕过新商品的冷启动问题——商品的名称/描述可以被提取并自然地与用户已经购买/选择的现有商品相关联。

然而，这种方法有一些陷阱（不要急着丢弃你当前的推荐系统！），主要是由于对推荐内容的控制缺乏。

+   因为没有对用户在检查后购买商品的不同操作/事件进行加权，所以我们完全依赖LLM预测的最可能的下一个token(s)进行推荐。我们无法考虑用户书签、查看了一段时间、放入购物车等操作。

+   此外，对于这些LLM，我们确实面临大多数推荐基于相似性的风险（即与目前购买的商品语义相似的商品），尽管我认为通过大量用户购买历史数据，我们可以通过这种方法模拟的“协同过滤”方法来改善这个问题。

+   最后，由于大型语言模型（LLMs）理论上可以生成任何文本，输出可能是一个与库存中的条目不完全匹配的字符串（尽管我认为这种情况发生的可能性较低）。

# 最终想法

根据P5论文的结果以及我尝试在T5模型上进行一些微调和提示的结果，我推测这种技术可以应用于许多语言模型。使用更强大的序列到序列模型可能会有显著帮助，尤其是当微调数据足够大且提示技术得到完善时。

然而，我不会建议（没有双关的意思）任何人单独使用这种方法。我建议将其与其他推荐系统技术结合使用，这样我们可以避免上述提到的陷阱，同时获得好处。如何实现这一点——我不确定，但我认为通过一些创造力，这种LLM技术可以以有益的方式集成（也许通过提取基于嵌入的特征用于协同过滤，或者与[“双塔”架构](https://medium.com/nvidia-merlin/scale-faster-with-less-code-using-two-tower-with-merlin-c16f32aafa9f)结合使用，可能性是无限的）。

# 代码

+   [我实现的T5推荐系统](https://github.com/Mohammadhia/t5_p5_recommendation_system)（Github仓库）

+   [我在Hugging Face上的微调T5模型](https://huggingface.co/mohammadhia/t5_recommendation_sports_equipment_english)（Hugging Face Hub）

# 参考文献

[1] S. Geng, S. Liu, Z. Fu, Y. Ge, Y. Zhang, [推荐作为语言处理（RLP）：统一的预训练、个性化提示与预测范式（P5）](https://arxiv.org/pdf/2203.13366.pdf)（2023），第16届ACM推荐系统会议
