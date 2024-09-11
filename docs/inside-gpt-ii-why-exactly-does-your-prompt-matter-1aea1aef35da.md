# GPT内部 — II：提示工程的核心机制

> 原文：[https://towardsdatascience.com/inside-gpt-ii-why-exactly-does-your-prompt-matter-1aea1aef35da?source=collection_archive---------8-----------------------#2023-12-21](https://towardsdatascience.com/inside-gpt-ii-why-exactly-does-your-prompt-matter-1aea1aef35da?source=collection_archive---------8-----------------------#2023-12-21)

## 提示工程背后的简单推理

[](https://medium.com/@fatih-demirci?source=post_page-----1aea1aef35da--------------------------------)[![Fatih Demirci](../Images/f60108429c4fac601a511f38954982bf.png)](https://medium.com/@fatih-demirci?source=post_page-----1aea1aef35da--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1aea1aef35da--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1aea1aef35da--------------------------------) [Fatih Demirci](https://medium.com/@fatih-demirci?source=post_page-----1aea1aef35da--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe4aaee0b8cc3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Finside-gpt-ii-why-exactly-does-your-prompt-matter-1aea1aef35da&user=Fatih+Demirci&userId=e4aaee0b8cc3&source=post_page-e4aaee0b8cc3----1aea1aef35da---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1aea1aef35da--------------------------------) ·8分钟阅读·2023年12月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1aea1aef35da&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Finside-gpt-ii-why-exactly-does-your-prompt-matter-1aea1aef35da&user=Fatih+Demirci&userId=e4aaee0b8cc3&source=-----1aea1aef35da---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1aea1aef35da&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Finside-gpt-ii-why-exactly-does-your-prompt-matter-1aea1aef35da&source=-----1aea1aef35da---------------------bookmark_footer-----------)![](../Images/c8268cc53ce4b66c6f6365fc2b262ae8.png)

解密与描绘恩igma机（通过midjourney创建）

大型语言模型是通过人类文本的视角对世界的压缩*。在[第一部分](/inside-gpt-i-1e8840ca8093)中，我们已经解构了GPT的模型架构，接下来，让我们探讨如何解码这种压缩并操控已经训练好的模型的输出。

在模型训练过程中，我们的模型通过人类文本的投射了解世界。在训练之后，每次我们提示模型并生成文本（推理）时，它通过训练过程中学到的参数形成一个涉及词汇表中每个词元的概率分布。

预测的标记一次成为输入的最后一个标记。将最后预测的标记附加到输入中，然后用来预测下一个标记。因此，文本生成简单来说，就是在给定输入（提示）中标记出现的情况下，对下一个标记预测的概率表达。

这个概率是由什么形成的？模型训练。模型在训练过程中看到的文本数据。

让我们查看一个示例提示句，并使我们上面的解释更清晰：

“德国以其” 

你如何完成这个句子？你已经了解了“德国”是什么，并且你基于你一生中所见/所闻/所读的内容（训练数据）形成了一个概念。这类似于模型在训练阶段的情况。我们从互联网/书籍中收集大量文本数据。然后我们筛选并删除有害内容。通过模型训练，最终我们的模型通过人类文本的视角理解世界。

那么，完成我们句子的下一个概率最高的标记是什么？让我们看一下GPT-2模型，以获取模型的实际概率。

```py
Input: "Germany is known for its"
NEXT TOKEN: 
Choice rank, token (probability)
Choice 1, high (3.30%)
Choice 2, strong (2.28%)
Choice 3, liberal (1.18%)
Choice 4, love (1.14%)
Choice 5, beautiful (0.96%)
Choice 6, generous (0.79%)
Choice 7, history (0.76%)
Choice 8," "" (0.75%)"
Choice 9, open (0.74%)
Choice 10, great (0.72%)
Choice 11, rich (0.67%)
Choice 12, beer (0.64%)
Choice 13, low (0.63%)
Choice 14, strict (0.61%)
Choice 15, unique (0.56%)
Choice 16, long (0.55%)
Choice 17, innovative (0.55%)
Choice 18, quality (0.55%)
Choice 19, many (0.52%)
Choice 20, large (0.50%)
Choice 21, heavy (0.50%)
.
.
.
Choice 89, harsh (0.17%)
Choice 90, wide (0.17%)
Choice 91, colorful (0.17%)
Choice 92, historic (0.17%)
Choice 93, ability (0.16%)
Choice 94, lack (0.16%)
Choice 95, aggressive (0.16%)
Choice 96, military (0.16%)
Choice 97, small (0.16%)
Choice 98, state (0.16%)
Choice 99, legendary (0.16%)
Choice 100, powerful (0.16%)
.
.
.
Choice 50158 phis (0.00000001097%)
Choice 50159 Florida (0.00000001094%)
Choice 50160 rez (0.00000001088%)
Choice 50161 etus (0.00000001066%)
Choice 50162 chapter (0.00000001045%)
Choice 50163 obin (0.00000001023%)
Choice 50164 Hong (0.00000000928%)
Choice 50165 assetsadobe (0.00000000894%)
Choice 50166 teasp (0.00000000862%)
Choice 50167 earthqu (0.00000000716%)
.
.
Choice 50255 � (0.0000000000000774%)
Choice 50256 サーティ (0.0000000000000749%)
Choice 50257 (0.0000000000000743%)
```

*(注：本文中的示例使用了GPT-2模型生成，因为它是一个公开模型，且足够小，可以通过实际示例来说明这些概念。)*

上面展示了模型在给定提示下的概率分布。对于语料库中的每个标记（模型训练时使用的文本数据集），我们都有一个相应的计算概率。模型的词汇总数（在GPT-2中为50,257）也是概率分布的大小。这些概率计算背后的技术在本博客文章系列的第一篇文章中进行了详细解释。 ([link](/inside-gpt-i-1e8840ca8093))

从预训练语言模型生成的输出可以通过几种解码策略进行控制。因此，无论我们是尝试提取事实信息还是生成创意故事，我们都可以影响输出，并调整其真实性和创造性。

解码生成文本的预测概率最简单的方法是直接在每一步预测时选择具有最高概率的标记。这种策略也被称为“贪婪”解码。

让我们提取给定初始输入句子（提示）的预测概率最高的前五个标记。每个预测标记成为输入句子的最后一个标记，直到我们达到最大标记限制，这是我们定义的一个参数。在这个例子中，让我们生成接下来的10个标记。

![](../Images/086d2983380c2697a8aef591efe5fc28.png)

前5个概率选择（图片由作者提供）

正如你在上面所见，通过贪婪策略，我们将具有最高概率的标记附加到输入序列中，并预测下一个标记。

![](../Images/f4da8be65cd5f68aecbb911ccda86659.png)

贪婪搜索决策路径（图片由作者提供）

使用这种策略，让我们用贪婪搜索解码生成128个后续标记的较长文本。

```py
"""
Germany is known for its high-quality beer, but the country's beer culture is also a reflection of its history. 
The country's beer culture is a reflection of its history. Beer is a German tradition. 
The country's beer culture is a reflection of its history. Beer is a German tradition. 
The country's beer culture is a reflection of its history. Beer is a German tradition. 
The country's beer culture is a reflection of its history. Beer is a German tradition. 
The
"""
```

正如你从上面的文本中看到的，尽管这是最简单的逻辑，但这种方法的缺点是生成了重复的序列。由于它未能捕捉到序列的概率，也就是说，多个单词一个接一个的整体概率被忽视。贪婪搜索一次只预测并考虑一步的概率。

重复的文本是一个问题。我们希望生成的输出简洁，我们该如何实现呢？

我们不是在每一步选择概率最高的标记，而是考虑未来的 x 步，计算联合概率（简单地是连续概率的乘积），并选择*下一个标记序列*，即最可能的序列。这里的 x 指的是 beams 的数量，也就是我们查看未来步骤的序列深度。这种策略被称为*beam 搜索*。

让我们回到 GPT-2 的示例，探索 beam 搜索与贪婪搜索的情境。

根据提示，查看概率最高的两个标记及其在树状图中的延续（4 个 beams）。

![](../Images/f1935a0dbf35b7155cd4669c9b0511e3.png)

贪婪搜索 vs beam 搜索（图像由作者提供）

让我们计算上述绿色序列的联合概率。

德国以其 -> 高质量的啤酒而闻名……

具有联合概率 3.30%*24.24%*31.26%*6.54% = 0.0016353

而下方路径的序列；

德国以其 -> 强大的生活传统而闻名……

2.28%*2.54%*87.02%*38.26% = 0.0019281。

尽管顶部序列中的第一个下一个标记预测步骤具有更高的概率，但底部序列总体上具有更高的联合概率。

尽管贪婪搜索在每个预测步骤中优先考虑绝对最大概率，但它忽略了序列中标记的概率。Beam 搜索解码使我们能够深入序列，并帮助我们以更广泛的方式解码文本。那么，beam 搜索是最终解决方案吗？

让我们进一步探索，并以 5 个 beam 的深度解码接下来的 128 个标记。

```py
"""
Germany is known for its high-speed rail network, but the country is also home to some of the world's most beautiful natural landscapes.

Here are 10 of the most beautiful places in Germany.

1\. Lake Constance

Lake Constance is one of the largest lakes in Germany. 

It is located in the state of North Rhine-Westphalia and is the second largest lake in Germany after Lake Constance in Bavaria.

Lake Constance is located in the state of North Rhine-Westphalia and is the second largest lake in Germany after Lake Constance in Bavaria.

"""
```

尽管与贪婪搜索相比较少，beam 搜索也会遭遇重复输出的问题。然而，通过 beam 搜索解码，我们可以通过惩罚重复的词序列对来解决这个问题。换句话说，如果序列之前已经被解码过，那么标记序列的概率将被赋值为零。重复标记序列的这种惩罚也被称为 n-gram 惩罚。

> 当“n”表示序列的长度时，“gram”是一个在计算语言学中指“单位”的术语，通常对应于我们案例中的“标记”。

其背后的原因是为了避免生成包含连续重复 n-gram 的序列。解码算法将惩罚包含重复词对的生成序列。

知道这一点后，让我们应用 n-gram 惩罚，其中 n = 2。

```py
"""
Germany is known for its high-speed rail network, but the country is also home to some of the world's most beautiful natural landscapes, including the Alps, the Baltic Sea, and Lake Constance.

The country's capital, Berlin, is the largest city in Europe, with a population of more than 8.5 million people. 

The city is located in the former East Germany, which was divided into East and West Germany after World War II. 

Today, Germany is a member of both the European Union and NATO, as well as the World Trade Organization and the Organization for Economic Cooperation and Development (OECD).<|endoftext|>

""" 
```

这是我们从模型中提取的关于输入提示的最佳完成文本，体现了连贯性和紧凑性。通过 n-gram 惩罚，使用 beam-search 解码的输出变得更加类似人类。

什么时候应该使用 beam-search，什么时候使用 greedy-search？当事实准确性至关重要时，比如解决数学问题、关键信息提取、总结或翻译时，应该优先选择 greedy-search。然而，当我们希望实现创意输出而事实准确性不是优先考虑的问题时（比如故事生成），beam-search 通常是更合适的方法。

为什么你的提示内容如此重要？因为你选择的每个词、句子结构、指令布局都会在大型语言模型的深层激活不同的参数序列，每个不同的提示都会形成不同的概率。问题的本质在于，文本生成是对你的提示的概率表达。

还有其他替代方法可以防止重复并影响生成文本的事实准确性/创造性，例如截断词汇分布或采样方法。如果你对深入探讨这一主题感兴趣，我强烈推荐 Patrick von Platen 在 HuggingFace 博客中的 [文章](https://huggingface.co/blog/how-to-generate)。

本系列的下一篇也是最后一篇文章将探讨通过人类反馈的微调和强化学习，这在为什么预训练模型能够在多个基准测试中超越 SOTA 模型中起到了重要作用。希望在这篇博客文章中，我能够帮助你更好地理解提示工程的推理。感谢阅读。下次再见。

**参考文献：**

+   *— 伊利亚·苏茨克维，No Priors 第39期 | 与 OpenAI 联合创始人兼首席科学家伊利亚·苏茨克维对话 [https://www.youtube.com/watch?v=Ft0gTO2K85A](https://www.youtube.com/watch?v=Ft0gTO2K85A)*

+   L. Tunstall, L. von Werra, 和 T. Wolf，《使用变换器的自然语言处理（修订版）》，O’Reilly Media, Inc.，2022年5月发布，ISBN: 9781098136796。

+   **相关链接：**

+   Inside GPT — I : 理解文本生成 [https://towardsdatascience.com/inside-gpt-i-1e8840ca8093](/inside-gpt-i-1e8840ca8093)

+   如何生成文本：使用不同的解码方法生成变换器语言文本

    [https://huggingface.co/blog/how-to-generate](https://huggingface.co/blog/how-to-generate)
