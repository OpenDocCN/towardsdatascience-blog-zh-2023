# 提取式与生成式问答——哪种更适合您的业务？

> 原文：[`towardsdatascience.com/extractive-vs-generative-q-a-which-is-better-for-your-business-5a8a1faab59a`](https://towardsdatascience.com/extractive-vs-generative-q-a-which-is-better-for-your-business-5a8a1faab59a)

## ChatGPT 的出现暗示着一个搜索引擎的新纪元，本教程深入探讨了两种基本的 AI 问答类型

[](https://skanda-vivek.medium.com/?source=post_page-----5a8a1faab59a--------------------------------)![Skanda Vivek](https://skanda-vivek.medium.com/?source=post_page-----5a8a1faab59a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5a8a1faab59a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5a8a1faab59a--------------------------------) [Skanda Vivek](https://skanda-vivek.medium.com/?source=post_page-----5a8a1faab59a--------------------------------)

·发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5a8a1faab59a--------------------------------) ·阅读时间 6 分钟·2023 年 2 月 6 日

--

![](img/17ef8827814e6d88ceadb0b0aa8cbbdb.png)

提取式与生成式问答 | Skanda Vivek

变换模型 [于 2017 年引入](https://arxiv.org/abs/1706.03762) 已经在解决难度大的语言相关任务上取得了突破。像 BERT、GPT 等模型在大量文本数据上训练的原始变换架构变体，在语言相关任务上产生了最先进的结果。

AI 的一个重大好处在于其执行任务的能力，这些任务之前需要领域专业知识和仔细审阅——现在可以更快地完成，并且成本仅为以前的一小部分。我相信这将在未来十年内彻底改变各个行业。

一项典型的任务是从文本中提取信息。问答系统是一个强大的信息提取工具，通过复杂的查询，模型可以被训练来提取特定的信息。想象一下，如果用 AI 模型回答法律文件中的难题，可以节省多少时间和金钱，而不是请经验丰富的律师或聘请实习生花费数小时细读文件。让我们深入了解两种基本的基于 AI 的问答类型：提取式与生成式。

# 提取式问答

BERT 变换模型由 Google 语言团队于 2019 年发布。BERT 通过遮蔽单词并训练模型基于上下文预测遮蔽的单词，利用未标记的文本数据进行训练。这种遮蔽单词预测是一个常见的测试，用于评估语言能力。

在训练模型后，BERT 后来在多个任务上进行了微调。特别是，BERT 在来自 SQUAD 数据集的数十万个问答对上进行了微调，该数据集包含在 Wikipedia 文章上提出的问题，每个问题的答案是对应段落中的一段文本或 *跨度*。

![](img/b8216f58e2743b33f3e2a89ddd41ddb8.png)

BERT Transformer 架构来自 [`arxiv.org/abs/1810.04805`](https://arxiv.org/abs/1810.04805)

BERT 和类似 BERT 的模型架构构成了 2017 年论文中提出的原始 Transformer 架构的一半，被称为编码器。在这个模型中，***E***表示令牌嵌入，其中原始句子长度为 *M* 被转换为长度 *M’*（BERT 使用了 WordPiece 嵌入）。最终隐藏向量 *T* 可以用来预测文本中表示答案开始和结束的部分，通过 softmax 实现。

RoBERTa 是 BERT 的一个变体，通过在训练过程中修改关键超参数来提高整体性能。让我们看看 [huggingface 发布的经过微调的 RoBERTa 模型的输出。](https://huggingface.co/deepset/roberta-base-squad2) 如下所示，在抽取式问答中，你只能回答原始上下文中的文本：

![](img/5521944641c87a3397c4e828283621e5.png)

[RoBERTa 微调问答模型输出](https://huggingface.co/deepset/roberta-base-squad2)

然而，答案并不总是最佳的。如下面所示，对于电影评论，我选择的答案会是“*在一个未来的世界中，人类仍然非常活跃但不再掌控一切的地球上生活会是什么样子*”

![](img/1a89d4e9255183918352814ca29bea6d.png)

[RoBERTa 微调问答模型输出](https://huggingface.co/deepset/roberta-base-squad2)

获得更相关结果的解决方案是微调。在下面的文章中，我讨论了如何使用自定义数据在 HuggingFace hub 上微调抽取式问答模型。仅通过几千个示例进行微调可以大幅提高性能，有时提高幅度达到 **超过 50%**。

[](/fine-tune-transformer-models-for-question-answering-on-custom-data-513eaac37a80?source=post_page-----5a8a1faab59a--------------------------------) ## 对自定义数据进行微调的 Transformer 模型以进行问答

### 关于如何在自定义数据上微调 Hugging Face RoBERTa QA 模型并获得显著性能提升的教程

towardsdatascience.com

然而，抽取式问答在答案没有明确存在于上下文中的情况下效果不佳，如下所示。

![](img/c1df25fccb77b442930dabf652d6ccda.png)

当答案没有明确存在时，模型产生无用结果

可以通过附加“ANSWERNOTFOUND”并在这些案例上进行微调来规避这个问题，以确保模型在不确定时不提供答案。

# 抽象问答

虽然 ChatGPT 最近在全球范围内引起了轰动，[原始的 GPT 模型](https://s3-us-west-2.amazonaws.com/openai-assets/research-covers/language-unsupervised/language_understanding_paper.pdf) 在 BERT 之前发布。GPT 模型使用了 2017 年 Transformer 的解码器层。GPT 模型被训练来以无监督的方式预测序列中的下一个词。然后，它们在监督的方式下进行微调。对于问答，GPT 模型在微调时会接触到多个答案选项的众多示例，并被训练选择正确的选项。一个重要的推断区别是，GPT 模型一次输出一个 token，因此是生成式的，而不是提取式的。

目前，OpenAI 提供了 4 个主要的语言模型 API 访问：

1.  Ada ($0.0004 / 1K tokens — 最快)

1.  Babbage ($0.0005 / 1K tokens)

1.  Curie ($0.0020 / 1K tokens)

1.  Davinci ($0.0200 / 1K tokens — 最强大)

作为参考，1K tokens 基本上是你发送给 API 处理的 750 个单词。那我们来看看这个模型对类似问题的表现如何：

![](img/191f90cc5451722d0fdbd5f006f9c87d.png)

基于 GPT3 的 Davinci OpenAI 模型用于问答

![](img/2dad6116cd83bcff8cb601ee5abc85ef.png)

基于 GPT3 的 Davinci OpenAI 模型用于问答

正如你所见，Davinci 模型在总结电影情节方面表现很好，同时在答案不明确的情况下会说“我不知道”。

# 哪种模型更好 — 抽象还是提取式？

你可能会倾向于说 OpenAI 的抽象问答明显优于提取式问答模型。然而，这就是商业案例发挥作用的地方。我将在下面详细说明：

## 成本

Davinci 模型在大规模时明显更贵。每 1K tokens 需 $0.02，这可能也适用于 1–10 个查询。而在 AWS 上托管 Hugging Face 模型可能成本更低，运行数千次或更多查询每小时只需 0.5 美分到 1 美元。

## 输出

如果你有兴趣构建一个聊天机器人类型的界面并期望自由响应的答案，那么抽象问答是最佳选择。用户可能不会满意仅仅是将文本改写的干巴巴的提取式答案。然而，如果你在对获得的答案进行后处理——比如将数字存入数据库，那么抽象问答可能会成为障碍，因为你需要使用额外的逻辑来剥离多余的词汇。

## 自定义

使用 OpenAI API 需要依赖 OpenAI 服务器。虽然他们确实允许在自定义数据上微调模型，但无法在 AWS 等独立基础设施上托管这些模型。不过，你可以使用 Hugging Face 上的开源模型，在 AWS 上创建 API，从而不再依赖 Hugging Face 进行模型服务。这一做法的强大之处在于，它允许公司将所有基础设施保留在内部，只依赖像 AWS 这样的云服务提供商。

我想指出的一点是，Hugging Face 也支持抽象型 QA 模型。事实上，他们最近在模型中心发布了一个 [text2text 生成模型 Flan T5](https://huggingface.co/docs/transformers/model_doc/flan-t5)。但我发现这个模型在 QA 任务上的表现不如 Davinci GPT-3 模型。很快，我期望 Hugging Face 也会托管像 Davinci GPT-3 这样的开源微调模型。

我希望这篇文章对使用 AI 进行问题回答提供了有用的指导。结合现有的信息检索方法和通过大量数据进行搜索，基于 AI 的信息提取可以帮助从大海捞针中提取有用信息，大大提高从大量数据中提取关键细节的效率，这在过去只能通过人工理解来实现。

***更新：*** [***https://www.answerchatai.com/***](https://www.answerchatai.com/) ***— 我们的 QA 引擎使用生成式 AI 回答问题并从自定义文本中提取关键信息现已上线！只需 3 个简单步骤即可回答领域特定的问题！***

1.  ***上传一个 URL 或粘贴文本并点击搜索按钮***

1.  ***提出一个与上下文相关的问题并点击查询***

1.  ***获取你的答案！***

***随意使用，并告诉我你的反馈！***

*如果你还不是 Medium 会员并想支持像我这样的写作者，可以通过我的推荐链接注册：* [*https://skanda-vivek.medium.com/membership*](https://skanda-vivek.medium.com/membership)

*想要获取基于数据的每周视角* [*请在这里订阅*](https://skandavivek.substack.com/)*！*
