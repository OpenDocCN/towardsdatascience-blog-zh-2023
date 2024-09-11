# ChatGPT 能推荐带有机器学习的电影吗

> 原文：[`towardsdatascience.com/can-chatgpt-recommend-movies-with-machine-learning-a5bc6c8fe90?source=collection_archive---------14-----------------------#2023-04-17`](https://towardsdatascience.com/can-chatgpt-recommend-movies-with-machine-learning-a5bc6c8fe90?source=collection_archive---------14-----------------------#2023-04-17)

## 在推荐的背景下测试 ChatGPT 的极限的有趣旅程

[](https://thushv89.medium.com/?source=post_page-----a5bc6c8fe90--------------------------------)![Thushan Ganegedara](https://thushv89.medium.com/?source=post_page-----a5bc6c8fe90--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a5bc6c8fe90--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a5bc6c8fe90--------------------------------) [Thushan Ganegedara](https://thushv89.medium.com/?source=post_page-----a5bc6c8fe90--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6f0b045d5681&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-chatgpt-recommend-movies-with-machine-learning-a5bc6c8fe90&user=Thushan+Ganegedara&userId=6f0b045d5681&source=post_page-6f0b045d5681----a5bc6c8fe90---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a5bc6c8fe90--------------------------------) ·6 min read·2023 年 4 月 17 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa5bc6c8fe90&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-chatgpt-recommend-movies-with-machine-learning-a5bc6c8fe90&user=Thushan+Ganegedara&userId=6f0b045d5681&source=-----a5bc6c8fe90---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa5bc6c8fe90&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-chatgpt-recommend-movies-with-machine-learning-a5bc6c8fe90&source=-----a5bc6c8fe90---------------------bookmark_footer-----------)![](img/1bdca70947a7ce0b4d891d683a9c415e.png)

照片由[Tech Daily](https://unsplash.com/@techdailyca?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供，来源于[Unsplash](https://unsplash.com/photos/PGuCnUzsRSM?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

最近我花了一些时间与我们亲爱的 AI 统治者 ChatGPT（开个玩笑！）一起探讨这个模型并推动其极限。我测试了电影推荐的用例。你可以在[这里](https://www.youtube.com/watch?v=ks6qe3f3Fv8)找到视频演示。

[由数十亿参数驱动的单体 LLM](https://arxiv.org/pdf/2005.14165.pdf)，经过 [RLHF](https://arxiv.org/pdf/2203.02155.pdf) 微调，彻底改变了我们对 AGI 的认知。[ChatGPT 的崛起](https://openai.com/blog/chatgpt)，GPT-3.5 和 [GPT-4](https://openai.com/product/gpt-4) 证明了语言模型的能力和技能在过去几个月中扩展了多少。[ChatGPT 在发布后仅两个月内达到 1 亿用户](https://www.reuters.com/technology/chatgpt-sets-record-fastest-growing-user-base-analyst-note-2023-02-01/) 是 AI 取得惊人进步的见证。

# 使用 ChatGPT 的电影推荐

许多人正在以创意方式使用 ChatGPT，从 [从零开始制作 Flappy Bird](https://www.youtube.com/watch?v=8y7GRYaYYQg) 到 [构建网站](https://www.youtube.com/watch?v=8I3NTE4cn5s)。跟随这一趋势，我决定查看 ChatGPT 是否可以根据数据集计算一个未见过的电影的用户评分。我首先要求 ChatGPT 生成一个数据集。

![](img/7c1808336834721093f234d37d427b98.png)

响应迅速，并生成了如上下文中解释的数据集。

![](img/cc0502ec2a8a66467fc6e0663fe75bd0.png)

d

我将会要求 ChatGPT 进行以下操作，

> 预测 Jack 对电影《复仇者联盟》的评分

我的希望是 ChatGPT 使用协作过滤方法来完成此任务。可以首先创建一个评分矩阵，利用评分矩阵计算与 Jack 的用户相似度。最后，

![](img/a56449843370777af4a3e1138e5216bc.png)

请注意，我在计算评分时忽略了对《复仇者联盟》的评分为 0 的用户。以下的 Excel 表展示了这些计算。我们正在寻找的最终答案是 9。

![](img/5747bbc4f77e3d1860edd315c1ab4737.png)

接下来，我提出了如下问题。

![](img/bbffab957015265f4030c5e3924c91ad.png)

看起来 ChatGPT 认为这应该是一个数据点，当前在数据集中缺失。我还尝试了 “[一步步思考](https://github.com/openai/openai-cookbook/blob/main/techniques_to_improve_reliability.md#model-capabilities-depend-on-context)” 的技巧，但这并没有让 ChatGPT 进展太远。

接下来，我尝试使用 [思路链推理](https://lilianweng.github.io/posts/2023-03-15-prompt-engineering/#chain-of-thought-cot) 来阐述计算最终结果所需遵循的方法。

![](img/3af83f80ff587a8bfb472e42a1247e33.png)

成功了！这次，ChatGPT 能够按照计划执行，生成中间结果并计算最终答案。

![](img/daad76302b608214c252faf213cd0d30.png)

但稍等一下！最终结果是错误的。

## 问题 1：由于任务复杂性，ChatGPT 可能在数学上失败了

看起来 ChatGPT 得出的最终结果是错的。如果你把最后一步第 2 行的方程式复制到计算器中，你会得到 9，而不是 8.95。此外，余弦距离也毫不意外地错误。但是，考虑到 ChatGPT 只是一个语言模型，它能做到这一点仍然令人印象深刻。让我们给予它怀疑的余地，尝试找出 ChatGPT 出错的地方。

![](img/e4d7d60fe9c94fa97abef352b071f133.png)

不幸的是，ChatGPT 无法解决这个问题。这是新响应的一部分内容。

![](img/fe6470e2b50fef43305ed841d2b79bad.png)

我无法让 ChatGPT 纠正错误。但它一直承认自己犯了错误，这有点矛盾。这引出了第二个问题。

## 问题 #2：ChatGPT 过于奉承

ChatGPT 非常过于奉承，每次你指出它错了，它都会认为自己错了。有趣的是，即使它手头上有正确的解决方案，它也会认为自己错了 😅。

![](img/74aba971ec1122479a22e0b05a2ad263.png)

在这里，`[0, 10, 0, 8]` 是实际向量。但是 ChatGPT 认为这是错误的，并产生了其他幻觉，以摆脱它当前的困境。这几乎像是 [Bing 聊天是 ChatGPT 的邪恶兄弟](https://www.gizmodo.com.au/2023/02/bing-ai-early-access/)。

经过一番对话后，我想测试一下 ChatGPT 的记忆/注意力跨度。所以我问，

![](img/8aaf2dbff37c0b1cf519d8d8e9945295.png)

ChatGPT 说，

![](img/0a0960f4700f564353ff7335a08474f4.png)

哦哦！如果你回顾 ChatGPT 的第一个有意义的回应，评分矩阵已经发生了变化。这就进入了大型语言模型（LLMs）中最棘手的问题之一。

## 问题 #3：ChatGPT 会产生幻觉

ChatGPT 的出现激励了科学界，引发了围绕 ChatGPT 的各种理念；从提高生产力到征服世界。一个观点是 ChatGPT 作为计算机程序的范式转变。在历史上，我们所熟知和喜爱的计算机程序是一组确定性的特定指令，通过这些指令我们可以达到预期的结果。ChatGPT 就像一个计算机程序，但它使用户可以使用自然语言进行交流，而不是用语法化的指令。

然而，在计算机程序中，如果一个变量脱离了上下文，那就是一个明显的错误。但对于 LLMs，它们只是凭空想象一些东西来填补空白。这在某些情况下可能是致命的。想象一下你尝试用 ChatGPT 解决账单错误，而 ChatGPT 却产生了一个登录错误。这将是一个非常混乱的用户体验。

你可以在下面找到我冒险的录像。

# 新的前沿

即使 ChatGPT 存在一些问题，这也不是世界末日！我仍然对 ChatGPT 比预训练的 GPT-3 更加出色感到印象深刻。所以这些模型只会变得更好。

我们已经有了[GPT-4 的发布](https://openai.com/product/gpt-4)和等待列表。[技术报告](https://cdn.openai.com/papers/gpt-4.pdf)已经显示出令人瞩目的性能提升。例如，在小学数学问题上，GPT-3.5 达到了 57.1%，而 GPT-4 则设定了 92% 的标准。此外，GPT-4 在事实检索能力和比 ChatGPT 更少的幻觉方面表现得更好。

如果你对从定性角度比较 GPT-4 和 ChatGPT 感兴趣，我推荐[这个视频](https://www.youtube.com/watch?v=qbIk7-JPB2c)。

另一个进展是[最近推出的能够使用自然语言进行推荐的模型](https://arxiv.org/pdf/2203.13366.pdf)。这个模型被称为 P5，并在与最先进的模型对比时表现出色。例如，P5 在顺序推荐上超越了 Bert4Rec 和 SASRec。

# 结论

ChatGPT 当然也不是没有缺陷。例如，ChatGPT 在简单的算术操作上失败，表现出谄媚行为，并在这个过程中产生了幻觉。但这只是开始。ChatGPT 的继任者 GPT-4 在许多方面都展现了显著的改进。此外，研究人员正在寻找利用自然语言的新方法来解决如推荐系统等新问题。

> *除非另有说明，所有图片均为作者提供*
