# GPT-4 的去污染评估

> 原文：[https://towardsdatascience.com/the-decontaminated-evaluation-of-gpt-4-38a27fc45c30?source=collection_archive---------4-----------------------#2023-03-27](https://towardsdatascience.com/the-decontaminated-evaluation-of-gpt-4-38a27fc45c30?source=collection_archive---------4-----------------------#2023-03-27)

## GPT-4 不会很快成为你的律师

[](https://medium.com/@bnjmn_marie?source=post_page-----38a27fc45c30--------------------------------)[![本杰明·玛丽](../Images/3ea1ad230cb1e67610418a8e36a5e5dd.png)](https://medium.com/@bnjmn_marie?source=post_page-----38a27fc45c30--------------------------------)[](https://towardsdatascience.com/?source=post_page-----38a27fc45c30--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----38a27fc45c30--------------------------------) [本杰明·玛丽](https://medium.com/@bnjmn_marie?source=post_page-----38a27fc45c30--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fad2a414578b3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-decontaminated-evaluation-of-gpt-4-38a27fc45c30&user=Benjamin+Marie&userId=ad2a414578b3&source=post_page-ad2a414578b3----38a27fc45c30---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----38a27fc45c30--------------------------------) ·7 分钟阅读·2023年3月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F38a27fc45c30&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-decontaminated-evaluation-of-gpt-4-38a27fc45c30&user=Benjamin+Marie&userId=ad2a414578b3&source=-----38a27fc45c30---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F38a27fc45c30&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-decontaminated-evaluation-of-gpt-4-38a27fc45c30&source=-----38a27fc45c30---------------------bookmark_footer-----------)![](../Images/d402e6d2653cbbb19e0b8a91f3899e1c.png)

图片来源：[Pixabay](https://pixabay.com/vectors/monster-alien-chasing-hunting-149013/)

GPT-4 在三月由 OpenAI 宣布，伴随令人印象深刻的展示和突出的声明。

这些声明大多数来自他们对 GPT-4 的自我评估。

OpenAI 使用了许多现有的专业和学术考试进行评估。

**但在公共基准测试中评估大型语言模型是极具挑战性的。**

像 GPT-4 这样的模型可能会受到“数据污染”，即**它们可能在评估数据上进行了训练。**

*为什么这是个问题？*

让我们举个例子。

GPT-4在LSAT考试中进行了评估。为了进行科学可信的评估，OpenAI必须检查用于评估的LSAT问题是否不在GPT-4的训练数据中。如果在其中，GPT-4可能已经记住了这些问题，那么在评估时显然会在这些特定问题上表现更好。

就像一个在考试前已经知道考试题目的人。

**你可以说这就像是作弊。**

在[GPT-4技术报告](https://cdn.openai.com/papers/gpt-4.pdf)中，OpenAI透露了关于GPT-4的少数几件事，其中之一是他们评估中的数据污染。他们公开了量化和评估这种污染的策略，并从观察中得出了几个结论。

在这篇文章中，我回顾和讨论了OpenAI如何处理GPT-4的数据污染。我揭示了他们方法中的几个陷阱。

我无法同意他们的几个结论。

# 评估数据的去污染

为了检查训练数据和评估数据之间是否存在交集，OpenAI使用了一种依赖于子字符串匹配算法的非常简单的技术（在技术报告第28页描述）。

首先，他们删除了训练数据和评估数据（考试）中的所有空格和符号。他们保留了数字。

然后，他们**随机**挑选了考试中每个问题的3个50字符的子字符串（或等效的）。如果这些子字符串中的任何一个恰好出现在GPT-4的训练数据中，则该问题会从**评估数据**中删除。

使用这种方法，他们做出了两个关键选择。

首先，这种方法是随机的。

对于问题非常长的考试来说，选择3个随机子字符串尤其具有问题。

例如，统一律师资格考试中的一个问题可能包含1,500个50字符的序列。*注意：它们是非常长的问题，* [*查看一些示例*](https://www.ncbex.org/pdfviewer/?file=%2Fdmsdocument%2F310)*。*

在1,500个子字符串中随机选择3个，这意味着该去污染策略完全忽略了每个问题的大部分内容。

这种策略无法可靠地检测出问题的大部分是否存在于训练数据中。

我们可以想象这些考试问题中的一些可能已经在GPT-4训练数据中被研究或讨论过，但只是部分而非全部，因为它们是非常长的问题。因此，在这种情况下，部分但重要的匹配将不会被检测到。

统一律师资格考试有400道题目。但是通过随机检查每个问题的3个子字符串，OpenAI并未发现这些问题出现在训练数据中。

第二个关键选择是他们去污染了评估数据，而不是训练数据。

从训练数据中删除问题，重新训练GPT-4，然后再在考试中评估它显然会花费过高。

然而，如果他们在开发过程中早些时候评估了这种污染，即在训练之前，他们本可以从训练数据中移除所有的考试示例。

还要注意，他们在去污染过程中没有包括RLHF的数据。如果一个考试的问题在RLHF中，它将保留在评估数据中。

> **定义**
> 
> RLHF代表来自人类反馈的强化学习。GPT-4在预训练后，通过在人工反馈上进行强化学习进一步微调，以提高其性能。这个“反馈”数据集没有被检查以进行去污染。

不包括RLHF训练数据的主要原因是利用RLHF进行微调并未显著提高GPT-4的性能。它们仅观察到在RLHF后训练的平均得分提高了+0.3%。

# 它是被污染的

![](../Images/6ed4c65c02b829ce16c5b95190b01ab7.png)

图片来自 [Pixabay](https://pixabay.com/illustrations/smiley-scared-surprised-fear-shock-1958283/)

每个考试的污染细节在报告的第30页中给出。

在用于评估的49个考试中，发现有12个完全没有出现在训练数据中。它们是：所有Leetcode数据集、Uniform Bar Exam、SAT EBRW考试和一些AP考试。

总体而言，用于评估的考试包含4,123个问题。其中545**.5**个问题已在训练数据中找到。*注意：为什么会有“****.5****”？据我了解，如果有匹配，OpenAI会完全移除该问题。但对于考试“USA Biolympiad Semifinal Exam 2020”，包含150个问题，他们指出他们移除了3.00%的问题（见论文的表10）。150的3%是4.5。这些数字中的一个可能是错误的。*

这是13.2%的评估数据被污染。

有趣的是，对于几个考试，去污染似乎改善了GPT-4的结果。

这与直觉相反。

我们可能认为，如果被移除的问题在训练数据中，GPT-4应该擅长回答它们，因为它有机会记住它们。

但我们对这些被排除的问题一无所知。

对于一些考试来说，它们可能是最困难的，因此在从评估中排除这些问题后正确答案的百分比更高。

OpenAI声称污染没有显著影响。他们指出：

> 总体来看，大多数考试中，污染和视力的影响相对较小。 （表9的标题）
> 
> 退化通常很小，正面和负面效果一样多 […] （表10的标题）

这是“总体”结论。如果我们更仔细地查看结果，这并不那么明显。让我们看看一些细节。

在技术报告的表10中，OpenAI还在每个考试中对两个独立的问题集合评估了GPT-4：

+   “contaminated”：这个集合仅包含在训练数据中找到的问题。

+   “non-contaminated”：这个集合包含了所有剩余的问题。

这是一个有趣的实验。GPT-4 在这两种数据集（第5和第6列）上的表现对某些考试变化极大，例如AMC 12的表现从41.67%到0%。

对于其他一些考试，GPT-4 在没有使用的评估数据（未受污染）上的表现更好。

*这是否意味着 GPT-4 在训练期间未见过的问题上表现更好？*

不，“受污染”和“未受污染”只是两种不同的评估数据。

GPT-4 可能因多种原因在两个数据集中的表现不同，例如问题的主题、长度、难度等。

# GPT-4 对这些考试表现如何？

让我们具体看看 LSAT 考试。假设 160 以上的分数在此考试中是一个好分数。

GPT-4 获得了163的分数。在去污染后，移除39%的问题，GPT-4获得了更高的167分。

*我们可以得出结论，GPT-4 能在 LSAT 考试中取得好分数吗？*

是的，我们可以。但前提是允许作弊。

一方面，我们有完整的考试，GPT-4 的分数为163。这是一个好分数，但 GPT-4 在考试前见过一些问题。

另一方面，如果我们移除39%的问题进行去污染，这就不再是 LSAT 考试了。没有人能通过61%的 LSAT。这种考试并不存在。

此外，移除的 39% 问题可能包含最难的问题。我们不知道在这61%的LSAT中，167的分数好坏如何。

对于所有其他用于评估的“受污染”考试，我们可以类似地推理。

一些考试没有“受污染”，例如统一律师资格考试和Leet代码问题，但还有其他问题。

我不会在这里讨论这些问题。[Arvind Narayanan](https://substack.com/profile/19265909-arvind-narayanan) 和 [Sayash Kapoor](https://substack.com/profile/891603-sayash-kapoor) 已经在他们的权威文章中讨论了这些问题的结果，你可以在这里阅读：

[](https://aisnakeoil.substack.com/p/gpt-4-and-professional-benchmarks?source=post_page-----38a27fc45c30--------------------------------) [## GPT-4 和专业基准：错误的问题的错误答案

### 我们不知道答案，但我们希望能将一些现实注入对话中。OpenAI 可能违反了…

aisnakeoil.substack.com](https://aisnakeoil.substack.com/p/gpt-4-and-professional-benchmarks?source=post_page-----38a27fc45c30--------------------------------)

# 结论

正如我在介绍中写的，评估大型语言模型的数据污染是一个极其困难的任务。

在收集和预处理训练数据时，理想情况下我们应该已经确定了一份需要从训练数据中排除的公共相关考试和基准的清单。

尽管如此，我的观点是，OpenAI 训练 GPT-4 时包含所有这些考试实际上是非常有意义的。

目标也是让GPT-4尽可能地适应这些考试提出的问题。我可以看到GPT-4在这个领域的许多潜在应用，比如帮助学生和老师准备考试。

然而，这个选择是有代价的：**我们不能用这些考试来以科学的可信度评估GPT-4。**

*如果你喜欢这篇文章并且有兴趣阅读接下来的文章，支持我的工作的最佳方式是通过这个链接成为Medium会员：*

[](https://medium.com/@bnjmn_marie/membership?source=post_page-----38a27fc45c30--------------------------------) [## 通过我的推荐链接加入Medium - Benjamin Marie

### 阅读Benjamin Marie的每一个故事（以及Medium上的其他成千上万的作家）。你的会员费直接支持…

medium.com](https://medium.com/@bnjmn_marie/membership?source=post_page-----38a27fc45c30--------------------------------)

*如果你已经是会员并且想要支持这项工作，* [*只需在Medium上关注我*](https://medium.com/@bnjmn_marie)*。*
