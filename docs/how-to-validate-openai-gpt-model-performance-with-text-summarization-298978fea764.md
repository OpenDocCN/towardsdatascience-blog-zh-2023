# 如何验证 OpenAI GPT 模型的文本摘要性能

> 原文：[https://towardsdatascience.com/how-to-validate-openai-gpt-model-performance-with-text-summarization-298978fea764?source=collection_archive---------6-----------------------#2023-04-04](https://towardsdatascience.com/how-to-validate-openai-gpt-model-performance-with-text-summarization-298978fea764?source=collection_archive---------6-----------------------#2023-04-04)

## 生成性 AI 使用与测试研究的第一部分

[](https://markopolocheno.medium.com/?source=post_page-----298978fea764--------------------------------)[![Mark Chen](../Images/2d51d4e7ab451b55733a018a3d10a0a7.png)](https://markopolocheno.medium.com/?source=post_page-----298978fea764--------------------------------)[](https://towardsdatascience.com/?source=post_page-----298978fea764--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----298978fea764--------------------------------) [Mark Chen](https://markopolocheno.medium.com/?source=post_page-----298978fea764--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F377682c0f342&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-validate-openai-gpt-model-performance-with-text-summarization-298978fea764&user=Mark+Chen&userId=377682c0f342&source=post_page-377682c0f342----298978fea764---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----298978fea764--------------------------------) ·9 分钟阅读·2023年4月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F298978fea764&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-validate-openai-gpt-model-performance-with-text-summarization-298978fea764&user=Mark+Chen&userId=377682c0f342&source=-----298978fea764---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F298978fea764&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-validate-openai-gpt-model-performance-with-text-summarization-298978fea764&source=-----298978fea764---------------------bookmark_footer-----------)![](../Images/3d19cbe8fc67655d0ad5525170d590da.png)

照片由 [Patrick Tomasso](https://unsplash.com/@impatrickt?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

无论你从事什么职业或年龄多大，你都可能在 LinkedIn、YouTube 或新闻中听说过 OpenAI 的生成性预训练变换器（GPT）技术。这些强大的人工智能模型/聊天机器人似乎能够处理任何任务，从创作诗歌到解决 leetcode 问题，再到连贯地总结长篇文章。

![](../Images/30787bc6542582b06ffd10096457ba8c.png)

[OpenAI的GPT Playground](https://platform.openai.com/playground) 的截图，总结了Jupiter Notes，由作者拍摄

GPT模型在不断扩展的NLP行业中似乎有无尽的应用前景。但是随着模型规模的不断增加，构建大型语言模型（LLM）的团队必须 **了解每个模型的性能和行为**。由于AI，如GPT，是一个日益增长的伦理问题，开发者应确保他们的模型是公平、负责任和可解释的。然而，在许多不同的环境中对人工通用智能进行适当的测试既繁琐又昂贵，并且耗时。

从 [Kolena](https://www.kolena.io/) 的机器学习工程师的角度来看，本文提供了使用GPT模型的全面指南，并比较了它们在 [抽象文本摘要](https://paperswithcode.com/task/abstractive-text-summarization#:~:text=Abstractive%20Text%20Summarization%20is%20the,appear%20in%20the%20source%20text.) 任务上的表现。通过这项积极研究的NLP问题，我们将能够 **审查模型行为、性能差异、投资回报率** 等等。

在本文结束时，你将了解到GPT-3.5的Turbo模型在4.8倍的成本和4.5倍的平均推理时间下，比GPT-3的Ada模型在抽象文本摘要任务上具有22%的更高BERT-F1得分和15%的更低失败率。

# 有效使用GPT

假设你想在自然语言处理应用中使用GPT来快速解决问题，比如翻译文本或解释代码。你从哪里开始？幸运的是，使用GPT处理任何独特任务时，只有三个主要步骤：

1.  选择合适的模型

1.  创建一个适当的 [提示](https://platform.openai.com/docs/guides/completion/prompt-design)

1.  使用 [GPT的API](https://platform.openai.com/docs/api-reference/completions/create) 来获取响应（我们的代码在本文末尾）

在选择模型之前，我们必须先考虑几个问题：每个模型的表现如何？哪个模型提供了最佳的投资回报率？哪个模型整体表现最佳？哪个模型在你的数据上表现最好？

为了在选择GPT模型时缩小范围，我们使用了 [CNN-DailyMail](https://paperswithcode.com/dataset/cnn-daily-mail-1) 文本摘要数据集来基准测试和比较五种 [**GPT模型**](https://platform.openai.com/docs/models/gpt-3)**：Ada、Babbage、Curie、Davinci 和 Turbo** 的性能。数据集的测试部分包含11,490篇新闻文章及其相应摘要。

第二步，我们使用一致的提示生成每个模型的新摘要，格式如下：

> *“像记者一样专业地总结这篇新闻文章，字数约为 {word_count_limit} 到 {word_count_limit+50} 字：\n {full_text}”*

实际操作中，需要一些实验来优化提示，从而获得主观上最优的结果。通过使用相同的提示，我们可以准确比较模型行为，减少模型差异的一个变量。

![](../Images/d77b3f0f4dacf41d0af9a3b69ae09c7a.png)

在这篇文章中，我们重点关注第一步，即选择合适的模型。

# 验证 GPT 模型性能

让我们了解一下感兴趣的 GPT 模型，它们来自 [GPT-3](https://platform.openai.com/docs/models/gpt-3) 和 [GPT-3.5](https://platform.openai.com/docs/models/gpt-3-5) 系列。每个模型都有一个令牌限制，定义了输入和输出的最大尺寸。因此，例如，如果你的 Turbo 模型的提示包含 2,000 个令牌，你将收到的最大输出是 2,096 个令牌。对于英文文本，75 个单词通常会分成大约 100 个令牌。

![](../Images/e5bbc5007d1d6b97dc1487ec2767c673.png)

我们目前在 [GPT-4](https://platform.openai.com/docs/models/gpt-4) 的等待名单上，所以将来会包含这些模型。现在，GPT-4 和 GPT-3.5 之间的主要区别对基本任务并不显著，但 GPT-4 提供了比 Davinci 更大的令牌限制，但价格要高得多。

## 抽象文本摘要的性能指标

众所周知，指标帮助我们衡量性能。下面的表格突出了我们用来评估模型文本摘要性能的标准和自定义指标：

![](../Images/119111b34611f75076a75019912c94c0.png)

*我们使用 [SacreBLEU](https://huggingface.co/spaces/evaluate-metric/sacrebleu) 计算 BLEU 分数，并用微软的 [deberta-xlarge-mnli](https://huggingface.co/microsoft/deberta-xlarge-mnli) 模型计算 BERT 分数。*

![](../Images/3171ffddf0fda1b31e10bfaa94d80d75.png)

ROUGE 和 BLEU 通过对比地面真实数据和推断结果中的单词匹配来测量相似度，而 BERT 分数则考虑语义相似度。值越高，相似度越接近：

![](../Images/2e553be150410d278ef069348edc5459.png)

## 标准指标结果

在每个模型上生成新的摘要（推断）后，我们可以通过任何类型的指标将模型性能与地面真实数据进行比较。让我们查看摘要比较和指标图，忽略 Babbage 以提高可读性。

**ROUGE_L 和 BLEU**‍

在以下示例中，原始的350字新闻文章有以下总结：

> *来自Suncorp银行的一份新报告发现，过去一年澳大利亚人花费了200亿澳元用于科技。男性在计算机、数字配件、移动应用程序和流媒体服务上的花费是女性的两倍。有孩子的家庭在数字化方面的支出比单身人士、没有孩子的情侣和空巢老人多出50%。三分之一的家庭没有为科技预算，或者大大低估了他们的支出。*

我们使用 Davinci 和 Ada 得到了以下 ROUGE_L、BLEU 和生成的摘要：

![](../Images/c3504e652170776af801823853e54562.png)

你会发现，通过阅读生成的摘要，Davinci 能很好地总结较大文本的内容。然而，Ada 并没有提供相同质量的摘要，ROUGE_L 和 BLEU 较低的值反映了输出质量的差距。

![](../Images/45808dfcdc595a9f4deb97d5a7466913.png)

ROUGE_L 的分布

当我们检查每个模型的 ROUGE_L 和 BLEU 分布时，我们看到 Ada 的指标值较低，而 Turbo 的指标值最高。Davinci 在这些指标方面略逊于 Turbo。随着 GPT 模型的**规模增加**，我们看到**ROUGE 和 BLEU 分数也有普遍的增加**。这些指标的值越大，生成文本中与真实摘要匹配的单词数量就越多。此外，这些**较大的模型生成的摘要信息更丰富，语法问题更少**。

![](../Images/e9fd69ab2b5deccbe2f2537027173e24.png)

BLEU 的分布

‍**BERT_F1**‍

对于 BERT 分数，趋势一致：较大的模型在匹配关键字和语义意义方面表现更好。这在较大模型的分布向右移动的趋势中可以明显看出，这表明 F1 分数更高。

![](../Images/3844e97e53995f55fc429e5f482a0166.png)

BERT_F1 的分布

![](../Images/616b44804e4e036d6dd7aa0ea8eccc64.png)

BERT_F1 与 word_count

从上图中，我们看到较大的模型在文本大小增加时能更好地保持性能。较大的模型在各种文本长度范围内表现一致，而较小的模型随着文本变长，性能波动较大。

## 自定义指标结果

让我们检查一下自定义指标，看看是否有理由不使用 Turbo 或 Davinci。

![](../Images/0684aa06a3d1ddacd2e24130db3e5aaf.png)

API 请求成本分布

从模型的成本分布中，我们了解到**Davinci 的成本远高于**其他任何模型。尽管 Davinci 和 Turbo 的性能相似，**Davinci 的成本大约是 Turbo 的十倍**。

![](../Images/eea86402e480a9f7dff0afdd07bef8a1.png)

inf_to_gt_word_count 的分布

在上图中，对于相同真实内容生成的单词数量差异非常大。Turbo 和 Davinci 始终提供长度为真实摘要两倍的总结，而**其他模型非常不一致**。具体来说，某些较小模型生成的摘要要短得多，而有些则超过了真实摘要长度的四倍！请记住，我们对每个模型使用了相同的请求和每篇文章的字数目标，但**某些模型遵守了这一限制**，而其他模型则完全忽视了它。

![](../Images/5a1ca507ffc3155e4c62d565beac0d6d.png)

总结长度的差异对用户来说是一个问题，因为这种不平衡表明模型可能存在问题或性能不佳。在上面的例子中，Curie至少重复了“两次过去的慈善事业，特别是他在圣犹大儿童研究医院的工作”。与Turbo相比，**Curie的总结冗余且不够优化**，同时成本**在十分之一美分的范围内相同**。在这个小差异中，我们应该注意到，使用Curie生成这个特定总结的成本是Turbo的两倍，因为输出中的token数量极高。

# 结果分析

在Kolena上运行模型评估一个小时后，我们可以概述和总结每个模型的性能和特征，如下所示。

![](../Images/1fd40e6d32caa34c54642e8622a5f42c.png)

我们现在了解到，模型规模越大：

+   提供的总结与生成的总结在语义上越相似

+   计算成本越高，除了Turbo之外

+   空总结的数量越少

+   生成总结的速度越慢

+   **模型表现越一致**

最终，**Turbo模型是GPT-3/3.5系列中表现最佳的模型**，提供了最一致的文本相似性评分，同时也非常具有成本效益。

## 进一步研究的注意事项

有趣的是，给定一个需要总结的文本，**一些模型根本拒绝生成输出**，即使提示在token限制内。Turbo没有在任何文章上失败，这是一项了不起的成就。然而，这可能是因为Turbo在标记敏感内容方面不如其他模型那么响应，或者在考虑这些因素时关注较少。Ada可能表现较差，但我们应该询问OpenAI是否因为道德考虑或技术限制而拒绝生成总结。下面是**Ada未能提供任何总结的前十六篇新闻文章的样本，其中Turbo生成了不错的总结**。看起来Ada在生成涉及敏感内容的总结时更不宽容：

![](../Images/bc14c077db7059eb345df4c3adeb8071.png)

Ada失败而Turbo表现良好的文章 — 来自Kolena

数据集中的真实总结**在内容或长度上不一定理想**。然而，我们假设真实总结对于直接的性能计算是理想的，因此模型评估指标可能会表明一个出色的模型实际上表现不佳，即使它生成了完全有效且详细的总结。也许**一些生成的总结甚至比其真实总结更好**，如下所示：

![](../Images/ee7e65ec15f1cf870d4c9f76fdd00de7.png)

# 结论

NLP的世界正迅速发展，随着像GPT这样的LLM的引入。这些模型变得越来越大、复杂且昂贵，因此开发者和用户都必须了解它们在特定使用场景下的预期性能水平。

‍

‍**不同的模型可能更适合你的业务需求**，这取决于你的问题、期望和可用资源。在为你的NLP任务选择单一GPT模型时，有很多因素需要考虑。在LLM快速发展的时代，希望本文中的发现能为你提供有关OpenAI模型差异的新视角。

‍

敬请关注未来的[更多帖子](https://www.kolena.io/blog)，我们可能会涉及提示工程、GPT-4性能，或不同内容类型对模型行为的影响等话题！

‍

正如本文早前承诺的，我们提供了参考代码以及本文所有例子的五个模型的摘要，都在这个[页面](https://docs.google.com/document/d/1FrbfHgEJL90Nnwr3mr3De4gJJtntnN2ZsB98f_9-mZg/edit?usp=sharing)上。你可以在[OpenAI的文档](https://platform.openai.com/docs/introduction)中了解更多关于OpenAI API或模型的信息。

所有图表的图片均为[Kolena](https://www.kolena.io/)的截图，除非另有说明。注意，相似的图表可以在如mathplotlib等常见框架中手动生成。
