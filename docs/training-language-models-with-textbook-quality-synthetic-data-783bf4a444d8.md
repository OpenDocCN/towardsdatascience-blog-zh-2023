# 使用教科书质量的合成数据训练语言模型

> 原文：[https://towardsdatascience.com/training-language-models-with-textbook-quality-synthetic-data-783bf4a444d8?source=collection_archive---------5-----------------------#2023-06-30](https://towardsdatascience.com/training-language-models-with-textbook-quality-synthetic-data-783bf4a444d8?source=collection_archive---------5-----------------------#2023-06-30)

## 对微软研究院的论文《教科书就是你所需要的一切》的探索

[](https://medium.com/@vatter?source=post_page-----783bf4a444d8--------------------------------)[![Vincent Vatter](../Images/9bed202f6057ab352b68ff281e165026.png)](https://medium.com/@vatter?source=post_page-----783bf4a444d8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----783bf4a444d8--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----783bf4a444d8--------------------------------) [Vincent Vatter](https://medium.com/@vatter?source=post_page-----783bf4a444d8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe9a7aeeab29f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftraining-language-models-with-textbook-quality-synthetic-data-783bf4a444d8&user=Vincent+Vatter&userId=e9a7aeeab29f&source=post_page-e9a7aeeab29f----783bf4a444d8---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----783bf4a444d8--------------------------------) ·12 min read·2023年6月30日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F783bf4a444d8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftraining-language-models-with-textbook-quality-synthetic-data-783bf4a444d8&user=Vincent+Vatter&userId=e9a7aeeab29f&source=-----783bf4a444d8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F783bf4a444d8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftraining-language-models-with-textbook-quality-synthetic-data-783bf4a444d8&source=-----783bf4a444d8---------------------bookmark_footer-----------)![](../Images/4942fce548c702ee6ca932b139d07d2a.png)

图片来源于 [DALL·E 2](https://openai.com/dall-e-2)。

微软研究院刚刚发布了一篇论文，为有关数据在模型训练中的作用的持续辩论注入了新的动力，特别是涉及到**数据质量**和**合成数据**的角色。虽然论文的重点是训练模型编写Python代码，但其影响远远超出了编程领域。这项工作的洞察可以作为语言模型项目在各种背景下的宝贵案例研究。

在[《教科书就是你所需的一切》](https://arxiv.org/abs/2306.11644)中，模型的成功并不依赖于任何突破性的设计或训练方法。实际上，作者指出“我们的模型架构和训练方法相当常规。”相反，创新在于训练数据。引用论文中的话：

> “我们假设这种高质量的数据显著提高了语言模型对代码的学习效率，因为它们提供了清晰、自包含、具有指导性的以及平衡的编码概念和技能示例。”

**数据质量**的价值在某种程度上是显而易见的——很难想象有人会主张在手头有相等量的更高质量数据时，用较低质量的数据进行训练。然而，关于数据质量相对重要性的观点在过去几年中发生了显著变化。

在2020年，OpenAI的论文[《神经语言模型的扩展法则》](https://arxiv.org/abs/2001.08361)将**模型大小**定位为最重要的因素：“最佳计算效率的训练涉及在相对适中的数据量上训练非常大的模型。”然后在2022年，DeepMind的Chinchilla论文[《训练计算最优的大型语言模型》](https://arxiv.org/abs/2203.15556)则认为**数据大小**同样重要：“当前的大型语言模型显著欠训练。”但现在，在2023年，焦点正转向数据质量。这种转变在最近泄露的谷歌备忘录[《我们没有护城河》](https://www.semianalysis.com/p/google-we-have-no-moat-and-neither)中的一部分得到了强调，备忘录声明：“数据质量的扩展效果优于数据大小。”

*《教科书就是你所需的一切》*论文的分析只是这一更大趋势的一个亮点。另一个值得注意的例子是[LIMA: 对齐的少即是多](https://arxiv.org/abs/2305.11206)，它展示了如何使用一个小而高质量的数据集来实现模型对齐中的令人印象深刻的结果。

**合成数据**的实用性——即由模型自身生成的数据——一直是一个备受争议的话题。尝试在大型模型的输出上训练较小模型，例如在创建[Alpaca](https://crfm.stanford.edu/2023/03/13/alpaca.html)和[Vicuna](https://lmsys.org/blog/2023-03-30-vicuna/)时，遇到了怀疑。批评者常常指出像伯克利论文[《模仿专有LLMs的虚假承诺》](https://arxiv.org/abs/2305.15717)中提出的论点，该论文称“模型模仿是一种虚假的承诺：开放和封闭的语言模型之间存在显著的能力差距，现有的方法只能通过大量的模仿数据或使用更强大的基础语言模型来弥合。”

然而，*《教科书就是你需要的一切》*挑战了这一观点，展示了大型模型的输出可以用于超越单纯模仿的目的。值得注意的是，论文中的小型模型甚至成功地超越了生成其训练数据的大型模型。这一观察引发了一个令人兴奋的问题：是否可以通过在*自身*的输出上训练来提升大型模型的表现？

# 结果

在深入探讨用于训练模型的训练数据之前，让我们先看看它们所取得的结果。论文中的三种模型是**phi-1-base**、**phi-1**和**phi-1-small**。值得注意的是，这些模型不仅在参数上是紧凑的，而且它们也仅在有限的数据上进行训练。鉴于此，它们的表现令人惊叹。

![](../Images/713694d365ae94632d3338c39f5fd492.png)

选定模型在HumanEval基准上的评估。来源：改编自[*《教科书就是你需要的一切》*](https://arxiv.org/abs/2306.11644)。

这里的分数基于OpenAI的HumanEval基准，该基准在他们的论文[评估基于代码训练的大型语言模型](https://arxiv.org/abs/2107.03374)中介绍。在这个基准中的问题中，模型被告知一个函数签名和文档字符串，并要求编写函数的主体。举例来说，请考虑以下从HumanEval论文中提取的例子，其中模型被给定以下签名和文档字符串。

![](../Images/2574f0fe5b0222a6ca3c968d733d2190.png)

来源：[评估基于代码训练的大型语言模型](https://arxiv.org/abs/2107.03374)。

对于这个问题，我们希望模型能够生成类似这样的内容：

![](../Images/e7c0b550f31228f06c2be0c3f16a5f58.png)

来源：[评估基于代码训练的大型语言模型](https://arxiv.org/abs/2107.03374)。

然而，该模型并不是基于生成这一确切字符串来进行评估（这将要求模型以相同的方式和相同的变量名称解决问题），而是对模型生成的任何内容进行多个单元测试评估（平均而言，每个问题7.7个单元测试，每个测试包括函数的参数选择和生成代码需要匹配的预期输出）。如果代码通过了所有单元测试，则认为其正确。上表中的pass@1指标仅仅是所有生成函数体中通过所有单元测试的百分比。更通用的pass@k指标允许模型生成*k*个样本，并认为如果其中任何一个样本通过所有单元测试，则视为成功。

论文中的模型训练于来自三个不同来源的数据。第一个，*The Stack+*，是一个 35B 令牌的去重版本的 [The Stack](https://arxiv.org/abs/2211.15533)，以及来自 StackOverflow 的代码，并限制为 Python。然而，重要的是要注意 **phi-1** 及其变体并未在这个来源上进行训练。相反，这些模型是在 *CodeTextbook* 上进行训练的，它是从 The Stack+ 中筛选出的 6B 令牌的教材质量选择，外加一个 10 亿令牌的合成部分，以及 *CodeExercises*，一个 180M 令牌的合成练习和解决方案集合，模拟了 HumanEval 数据集中发现的问题风格。效果见下图。

![](../Images/5f879b265fc30da2961e394b1d03214a.png)

HumanEval 结果来自于对各种来源的训练。图片来自于 [教科书就是你所需的一切](https://arxiv.org/abs/2306.11644)。

在这里，我们看到 9 个具有不同参数的模型在数据的不同子集上进行训练。图表中浅绿色的模型仅在 CodeTextbook 上进行训练，而没有在 The Stack+ 上训练，因此显然 CodeTextbook 是更好的来源。图表中深绿色的模型在 CodeExercises 上的微调带来了更大的差异。

图表中的三个模型名称为：

+   **phi-1-base** 是一个 13 亿参数的模型（预）训练了“约 8 次”对 7B 令牌的 CodeTextbook 进行训练。这相当于约 50B 令牌的训练数据，并且在 8 个 A100 上花费了 4 天。

+   **phi-1** 是在 180M 令牌的 CodeExercises 上对 **phi-1-base** 进行微调的结果。这次微调在 8 个 A100 上花费了 7 小时。

+   **phi-1-small** 是使用类似 **phi-1** 的过程制作的，但参数模型设计为 3.5 亿，并且显然对 CodeTextbook 进行了约 11 次的训练。它在 8 个 A100 上训练大约需要 2 天。

# 筛选后的 CodeTextbook 部分（6B 令牌）

对于这部分 CodeTextbook，他们开始时使用了 35B 令牌的去重和 Python 限制版的 The Stack，以及图表中所称的 Stack+ 的 StackOverflow 代码。然后他们筛选出一个 6B 令牌的教材质量子集。

为了进行这项筛选，首先使用 GPT-4 确定大约 35B 令牌数据集（100M 令牌）中约 0.3% 的教育价值。使用的提示是“确定其对目标是学习基础编码概念的学生的教育价值”。

没有明确说明为什么选择 GPT-4 而不是 GPT-3.5 来进行这一步骤，因为 GPT-3.5 用于过程中的其他所有阶段。然而，考虑到任务是分类“仅” 100M 令牌，使用 GPT-4 并不会过于昂贵，并且肯定会产生更准确的结果。

接下来，这些注释用于训练另一个模型（一个 [随机森林分类器](/random-forests-algorithm-explained-with-a-real-life-example-and-some-python-code-affbfa5a942c)）以将其余的数据集分类为高或低教育价值。随后，这个分类器用于将原始数据集筛选成一个 6B 令牌的高教育质量数据集。

# CodeTextbook 的合成部分（1B tokens）

在这里事情变得更有趣，因为作者使用 GPT-3.5 生成合成高质量的“Python 教科书”。

使用 LLMs 生成用于训练较小模型的合成数据已有一些先例。在早期的微软研究论文中，[TinyStories: How Small Can Language Models Be and Still Speak Coherent English?](https://arxiv.org/abs/2305.07759)，目标是训练小型语言模型（1M 到 33M 参数）写出适合幼儿理解的故事，该数据集完全由 GPT-3.5 和 GPT-4 编写的故事组成。引用自 TinyStories 论文：

> “使用大型语言模型生成训练数据的主要挑战是生成一个足够**多样化**的数据集：即使将生成的温度设置为较高值，提示这些模型生成的故事仍然会生成一个非常重复的数据集，其多样性远未达到训练具有与儿童相当的语言‘理解’的语言模型所需的水平。”

TinyStories 用来多样化合成数据的技巧是选择三个随机词（一个名词，一个动词和一个形容词）以及每个提示的小数量的“故事特征”。例如，他们的一个提示是如下所示。

![](../Images/9af012416590b6bbedc5dc9edaf6741c.png)

来源：[TinyStories: How Small Can Language Models Be and Still Speak Coherent English?](https://arxiv.org/abs/2305.07759)

不幸的是，微软研究没有给我们提供有关生成多样化的教科书质量文本的技巧的详细信息，该项目似乎也没有发布任何代码或数据供我们调查。他们确实表示，他们的目标是将内容定位为“促使推理和基本算法技能的主题”，并且他们对主题和教科书的受众设置了限制。以下是他们对一个提示的典型回应示例，引用自论文。

![](../Images/f16f425540f923b05d298f3ff61d1ea0.png)

来源：[*Textbooks Are All You Need*](https://arxiv.org/abs/2306.11644)*.*

不用说，了解这一过程的更多细节将会很有趣。具体的提示是什么？主题是如何选择的？GPT-3.5 被告知要为哪些受众写作？检查 *CodeTextbook* 也会很有趣，但数据尚未发布。

# CodeExercises（180M tokens）

**phi-1** 和 **phi-1-small** 的最终训练数据（但不包括 **phi-1-base**）是一组模拟 HumanEval 基准问题格式的练习和解答。再次强调，这些数据完全是合成的，由 GPT-3.5 生成。作者表示，通过限制函数名称实现了输出的多样性。虽然我不清楚这具体意味着什么，但这可能涉及到另一个模型首先生成函数名称和签名列表，然后再提示 GPT-3.5 生成相应的文档字符串和正文。作者提供了一个典型输出的例子，如下所示。

![](../Images/300dd69c90e67884ceeadf54b18aee86.png)

来源：[*教科书是你所需要的一切*](https://arxiv.org/abs/2306.11644)*.*

作者将这个数据集称为小型，因为它仅包含 180M 个标记。然而，如果上述例子具有代表性，则 CodeExercises 包含大约一百万个练习和解答。

对于 CodeExercises 是否只是偶然碰到 HumanEval 基准中的相同函数，从而导致 **phi-1** 在测试用例上进行了微调，值得怀疑。作者在第5节中花费了大量篇幅来反驳这一担忧。他们首先主张 CodeExercises 与 HumanEval 之间的相似性有限。其次，他们认为，即使在 CodeExercises 中略微类似于 HumanEval 的练习被修剪掉（相似性以嵌入距离衡量），在修剪后的数据集上训练的模型仍然表现出色。

# 成本

论文的重点，以及对论文的深入探讨，都集中在数据质量上。然而，考虑一下今天复制实验的成本，至少要考虑其各个组成部分的相对成本，是很有启发性的。

+   **过滤。** 过滤 The Stack+ 的过程涉及使用 GPT-4 确定 100,000 个文件的教育价值，或大约 100M 输入标记。忽略输出标记（这些标记会很少）并以今天的价格 $0.03 / 1K 输入标记计算，这将花费大约 $3,000。

+   **合成。** CodeTextbook 和 CodeExercises 一共包含大约 1280M 个由 GPT-3.5 生成的文本标记。以今天的价格 $0.002 / 1K 输出标记计算，创建这些数据的成本略高于 $2,500。

+   **训练。** **phi-1** 模型训练了1090小时。以今天约 $1/小时 的 A100 价格来计算，这将花费大约 $1,000。350M 参数的 **phi-1-small** 可以用 $400 进行训练。

创建 **phi-1** 花费了大约 $6,500 的计算成本。

作者推测，使用 GPT-4 进行合成会好得多：“我们还认为，使用 GPT-4 生成合成数据会取得显著的进步，而不是 GPT-3.5，因为我们注意到 GPT-3.5 数据的错误率很高。”但是，这些成本显示了他们没有这样做的原因。由于 GPT-4 的价格是 GPT-3.5 的 30 倍，生成 CodeTextbook 和 CodeExercises 的合成部分大约需要 75,000 美元。

# 结论

*Textbooks Are All You Need* 的结果非常令人印象深刻，尤其是考虑到模型的较小尺寸和所给的有限训练数据。这篇论文提供了更多证据表明，**数据质量**可以弥补数据量和模型大小的不足。

关于合成数据的讨论无疑会持续存在。这个概念很有吸引力——如果我们没有高质量的数据可以随时获得，我们是否可以合成这些数据？*Textbooks Are All You Need* 在这一领域展示了一些有希望的可能性。尽管如此，考虑到 CodeTextbook 中只有大约 10 亿个 token 是合成创建的，这还不是我们可能梦想中的完美实验。但值得指出的是，其他 60 亿个 token 是经过合成筛选的。

在完全合成数据上的训练在图像处理领域已经显示出一些令人兴奋的结果。谷歌研究的 [StableRep: 从文本到图像模型生成的合成图像使得视觉表征学习者变得更强](https://arxiv.org/abs/2306.00984) 研究，使用文本到图像模型并完全基于由 Stable Diffusion 生成的合成数据进行训练。他们报告的结果匹配或超越了 Stable Diffusion 自身的表现。

TinyStories 论文采用了类似的方法，该方法仅依赖合成数据进行训练。但它所使用的模型非常小。如果更大的语言模型以相同的方式进行训练会怎样？这一潜力令人兴奋，毫无疑问，它将成为未来众多研究的重点。

# 参考文献

Chen, M., Tworek, J., Jun, H., Yuan, Q., de Oliveira Pinto, H. P., Kaplan, J., Edwards, H., Burda, Y., Joseph, N., Brockman, G., Ray, A., Puri, R., Krueger, G., Petrov, M., Khlaaf, H., Sastry, G., Mishkin, P., Chan, B., Gray, S., Ryder, N., Pavlov, M., Power, A., Kaiser, L., Bavarian, M., Winter, C., Tillet, P., Such, F. P., Cummings, D., Plappert, M., Chantzis, F., Barnes, E., Herbert-Voss, A., Guss, W. H., Nichol, A., Paino, A., Tezak, N., Tang, J., Babuschkin, I., Balaji, S., Jain, S., Saunders, W., Hesse, C., Carr, A. N., Leike, J., Achiam, J., Misra, V., Morikawa, E., Radford, A., Knight, M., Brundage, M., Murati, M., Mayer, K., Welinder, P., McGrew, B., Amodei, D., McCandlish, S., Sutskever, I., 和 Zaremba, W. (2021). [评估训练于代码上的大型语言模型](https://arxiv.org/abs/2107.03374). arXiv:2107.03374。

Eldan, R. 和 Li, Y. (2023). [TinyStories: 语言模型可以小到多小仍然能讲出连贯的英语？](https://arxiv.org/abs/2305.07759) arXiv:2305.07759。

Gudibande, A., Wallace, E., Snell, C., Geng, X., Liu, H., Abbeel, P., Levine, S., 和 Song, D. (2023). [模仿专有大型语言模型的虚假承诺](https://arxiv.org/abs/2305.15717)。arXiv:2305.15717。

Gunasekar, S., Zhang, Y., Aneja, J., Mendes, C. C. T., Giorno, A. D., Gopi, S., Javaheripi, M., Kaufmann, P., de Rosa, G., Saarikivi, O., Salim, A., Shah, S., Behl, H. S., Wang, X., Bubeck, S., Eldan, R., Kalai, A. T., Lee, Y. T., 和 Li, Y. (2023). [教科书就是你所需的一切](https://arxiv.org/abs/2306.11644)。arXiv:2306.11644。

Hoffmann, J., Borgeaud, S., Mensch, A., Buchatskaya, E., Cai, T., Rutherford, E., de Las Casas, D., Hendricks, L. A., Welbl, J., Clark, A., Hennigan, T., Noland, E., Millican, K., van den Driessche, G., Damoc, B., Guy, A., Osinder, S., Simonyan, K., Elsen, E., Rae, J. W., Vinyals, O., 和 Sifre, L. (2022). [训练计算最优的大型语言模型](https://arxiv.org/abs/2203.15556)。arXiv:2203.15556。

Kaplan, J., McCandlish, S., Henighan, T., Brown, T. B., Chess, B., Child, R., Gray, S., Radford, A., Wu, J., 和 Amodei, D. (2020). 神经语言模型的扩展规律。arXiv:2001.08361。Tian, Y., Fan, L., Isola, P., Chang, H., 和 Krishnan, D. (2023). StableRep: 从文本到图像模型生成的合成图像成为强大的视觉表示学习者。arXiv:2306.00984。Zhou, C., Liu, P., Xu, P., Iyer, S., Sun, J., Mao, Y., Ma, X., Efrat, A., Yu, P., Yu, L., Zhang, S., Ghosh, G., Lewis, M., Zettlemoyer, L., 和 Levy, O. (2023). [LIMA: 对齐的极简主义](https://arxiv.org/abs/2305.11206)。arXiv:2305.11206。
