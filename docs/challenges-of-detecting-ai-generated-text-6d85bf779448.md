# 检测 AI 生成文本的挑战

> 原文：[https://towardsdatascience.com/challenges-of-detecting-ai-generated-text-6d85bf779448?source=collection_archive---------1-----------------------#2023-09-27](https://towardsdatascience.com/challenges-of-detecting-ai-generated-text-6d85bf779448?source=collection_archive---------1-----------------------#2023-09-27)

## 我们将深入探讨检测 AI 生成文本的挑战，以及在实际应用中所使用技术的有效性。

[](https://medium.com/@dhruvbird?source=post_page-----6d85bf779448--------------------------------)[![Dhruv Matani](../Images/d63bf7776c28a29c02b985b1f64abdd3.png)](https://medium.com/@dhruvbird?source=post_page-----6d85bf779448--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6d85bf779448--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6d85bf779448--------------------------------) [Dhruv Matani](https://medium.com/@dhruvbird?source=post_page-----6d85bf779448--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F63f5d5495279&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchallenges-of-detecting-ai-generated-text-6d85bf779448&user=Dhruv+Matani&userId=63f5d5495279&source=post_page-63f5d5495279----6d85bf779448---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6d85bf779448--------------------------------) ·15 min read·Sep 27, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6d85bf779448&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchallenges-of-detecting-ai-generated-text-6d85bf779448&user=Dhruv+Matani&userId=63f5d5495279&source=-----6d85bf779448---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6d85bf779448&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchallenges-of-detecting-ai-generated-text-6d85bf779448&source=-----6d85bf779448---------------------bookmark_footer-----------)![](../Images/b9dfe546990ad9a0e1f0ed38878e5470.png)

图片由 [Houcine Ncib](https://unsplash.com/@houcinencibphotography?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

与 [Naresh Singh](https://medium.com/@brocolishbroxoli) 共同撰写。

# 目录

1.  [介绍](#86be)

1.  [建立文本来源检测的直觉](#64d4)

1.  [语言模型的困惑度是什么？](#542b)

1.  [计算语言模型预测的困惑度](#dc6a)

1.  [检测 AI 生成文本](#f256)

1.  [错误信息](#b56a)

1.  [接下来是什么？](#a778)

1.  [结论](#8446)

# 介绍

现在写文章或帖子用的AI辅助技术无处不在！ChatGPT开启了基于语言的AI的众多应用，AI在任何类型内容生成中的使用已经达到了空前的高度。

然而，在诸如创意写作之类的学校作业中，学生需要创建自己的内容。然而，由于AI在这些任务中的流行和有效性，学生可能会被诱惑去使用它。在这种情况下，教师拥有能够检测AI生成内容的可靠工具就显得尤为重要。

本文旨在提供对构建此类工具的直观理解以及技术规格。它面向那些希望直观理解AI检测工作原理的读者以及希望构建此类工具的技术观众。

让我们直接进入主题！

# 建立文本来源检测的直观理解

从高层次来看，我们试图回答的问题是，*“AI语言模型（如GPT-3）生成全部或部分文本的可能性有多大？”*

如果你退一步看，你会意识到这是一种典型的日常情境。例如，你母亲对你说以下句子的可能性有多大？

1.  亲爱的，请在晚上8点之前上床睡觉。

1.  亲爱的，请在晚上11点后上床睡觉。

我们猜测前者的可能性远高于后者，因为你对周围世界已经有了一定的理解，并且对哪些事件更可能发生有了感觉。

这正是语言模型的工作原理。语言模型学习有关周围世界的知识，特别是语言。它们学习在给定不完整句子的情况下预测下一个标记或单词。

在上面的例子中，如果你被告知你母亲正在说话，而迄今为止所说的是*“亲爱的，请在睡觉”*，那么这个句子的最可能的继续就是“在晚上8点之前”，而不是“在晚上11点之后”。用技术术语来说，我们说你会对听到第二句话而非第一句话感到更多的*困惑*。

让我们深入探讨一下在语言模型的背景下困惑度的含义。

# 语言模型的困惑度是什么？

根据 [dictionary.com](https://www.dictionary.com/browse/perplexity)，困惑度被定义为

> 困惑的状态；混乱；不确定性。

在现实世界中，如果你遇到一个你没有预料到的情况，你会比遇到一个你预料到的情况感到更多的困惑。例如，当你在路上行驶时，如果你看到一个交通信号灯，那么你会比看到一只穿过街道的山羊时感到更少的困惑。

同样，对于一个试图预测句子中下一个单词的语言模型来说，我们说如果模型用一个我们没想到的单词来完成句子，它会让我们感到困惑，相比之下，如果它使用我们期待的单词。以下是一些例子。

**低困惑度的句子看起来会是这样的**

1.  外面的天气晴朗。

1.  对不起，我错过了航班，未能及时到达国家公园。

**具有高困惑度的句子可能如下所示**

1.  外面的天气很好。

1.  我错过了光线，无法到达国家公园。

接下来，让我们看看如何计算语言模型做出的预测的困惑度。

# 计算语言模型预测的困惑度

语言模型的困惑度与能够意料之外地预测句子下一个标记（单词）的概率相关。

假设我们用一个包含 6600 个标记的词汇表训练语言模型，并运行一个预测步骤以让模型预测句子中的下一个标记。假设选择该标记的概率是 5/6600（即，该标记的概率不是很高）。其困惑度是概率的倒数，即 6600/5 = 1320，这表明 *我们对这个建议感到非常困惑*。如果选择该标记的概率是 6000/6600，那么困惑度将是 6600/6000 = 1.1，这表明我们对这个建议感到仅仅是稍微困惑。

因此，模型在更可能的预测上的困惑度低于模型在不太可能的预测上的困惑度。

预测句子“x”中所有标记的困惑度形式上定义为标记概率乘积的倒数的 N 次根。

![](../Images/426c37041a0cb753755ed645420b9bae.png)

然而，为了确保数值稳定性，我们可以用对数函数来定义它。

![](../Images/525b453fe476fc340eb4bd4bd4712c9c.png)

这是 e（2.71828）到预测标记为真实标记的平均负对数似然的幂。

## 训练和验证困惑度

模型的训练和验证困惑度可以直接从批次或时期的损失计算得出。

## 预测困惑度

由于需要一组每个预测的真实标签，因此无法计算预测困惑度。

## 计算困惑度的 PyTorch 代码

假设变量 *probs* 是一个形状为 *(sequence_length,)* 的 *torch.Tensor*，它包含了语言模型在序列中该位置上预测的真实标记的概率。

可以使用以下代码计算每个标记的困惑度。

```py
token_perplexity = (probs.log() * -1.0).exp()
print(f"Token Perplexity: {token_perplexity}")
```

可以使用以下代码计算样本困惑度。

```py
# The perplexity is e^(average NLL).
sentence_perplexity = (probs.log() * -1.0).mean().exp().item()
print(f"Sentence Perplexity: {sentence_perplexity:.2f}")
```

接下来，让我们查看一下计算给定句子的每个标记概率的代码。

```py
def get_pointwise_loss(self, inputs: List[str], tok):
    self.model.eval()
    all_probs = []
    with torch.inference_mode():
        for input in inputs:
            ids_list: List[int] = tok.encode(input).ids
            # ids has shape (1, len(ids_list))
            ids: Torch.Tensor = torch.tensor(ids_list, device=self.device).unsqueeze(0)

            # probs below is the probability that the token at that location
            # completes the sentence (in ids) so far.
            y = self.model(ids)
            criterion = nn.CrossEntropyLoss(reduction='none', ignore_index=0)

            # Compute the loss starting from the 2nd token in the model's output.
            loss = criterion(y[:,:,:-1], ids[:,1:])

            # To compute the probability of each token, we need to compute the
            # negative of the log loss and exponentiate it.
            loss = loss * -1.0

            # Set the probabilities that we are not interested in to -inf.
            # This is done to make the softmax set these values to 0.0
            loss[loss == 0.0] = float("-inf")

            # probs holds the probability of each token's prediction
            # starting from the 2nd token since we don't want to include
            # the probability of the model predicting the beginning of
            # a sentence given no existing sentence context.
            #
            # To compute perplexity, we should probably ignore the first
            # handful of predictions of the model since there's insufficient
            # context. We don’t do that here, though.
            probs = loss.exp()
            all_probs.append(probs)
        #
    #
    return all_probs
#
```

现在我们对语言模型的功能以及如何计算每个标记和每个句子的困惑度有了一些了解，让我们试着把这些信息结合起来，看看如何利用这些信息构建一个可以检测文本是否由 AI 生成的工具。

# 检测 AI 生成的文本

我们已经拥有检查文本是否由 AI 生成所需的所有成分。这里是我们需要的一切：

1.  我们希望检查的文本（句子或段落）。

1.  该文本的分词版本，使用与该模型的训练数据集相同的分词器进行分词。

1.  经过训练的语言模型。

利用以上 1、2 和 3，我们可以计算以下内容：

1.  模型预测的每个 token 的概率。

1.  使用每个 token 的概率计算的困惑度。

1.  整个句子的总困惑度。

1.  模型在训练数据集上的困惑度。

要检查文本是否由 AI 生成，我们需要将句子的困惑度与模型的困惑度（经过调整的系数 alpha）进行比较。如果句子的困惑度高于调整后的模型困惑度，则可能是人类撰写的文本（即不是 AI 生成的）。否则，可能是 AI 生成的。原因在于，我们期望模型不会对它自己生成的文本感到困惑，因此如果它遇到一些自己不会生成的文本，那么就有理由相信这些文本不是 AI 生成的。如果句子的困惑度小于或等于经过调整的模型训练困惑度，那么很可能是使用该语言模型生成的，但我们不能非常确定。这是因为一个人也有可能写出这样的文本，而且这正好是模型也可能生成的内容。毕竟，模型是在大量人类写作的文本上训练的，所以在某种意义上，模型代表了一种“普通人类的写作”。

![](../Images/d515e2e4323c1603bd5fc0416815b800.png)

上述公式中的*ppx(x)*表示输入“x”的困惑度。

接下来，让我们查看一些人类撰写的文本与 AI 生成的文本的例子。

## AI 生成的文本与人类撰写的文本的例子

我们编写了一些 Python 代码，根据每个 token 相对于模型困惑度的困惑度对其着色。如果我们不考虑其困惑度，第一个 token 总是呈黑色。困惑度小于或等于模型困惑度的 token 被着色为红色，表明它们可能是 AI 生成的，而困惑度较高的 token 被着色为绿色，表明它们肯定不是 AI 生成的。

![](../Images/88d5070960ec7b91dce50880f51f986b.png)

句子前方方括号中的数字表示使用语言模型计算的句子困惑度。注意某些单词有部分红色和部分蓝色。这是因为我们使用了子词分词器。

这是生成上述 HTML 的代码。

```py
def get_html_for_token_perplexity(tok, sentence, tok_ppx, model_ppx):
    tokens = tok.encode(sentence).tokens
    ids = tok.encode(sentence).ids
    cleaned_tokens = []
    for word in tokens:
        m = list(map(ord, word))
        m = list(map(lambda x: x if x != 288 else ord(' '), m))
        m = list(map(chr, m))
        m = ''.join(m)
        cleaned_tokens.append(m)
    #
    html = [
        f"<span>{cleaned_tokens[0]}</span>",
    ]
    for ct, ppx in zip(cleaned_tokens[1:], tok_ppx):
        color = "black"
        if ppx.item() >= 0:
            if ppx.item() <= model_ppx * 1.1:
                color = "red"
            else:
                color = "green"
            #
        #
        html.append(f"<span style='color:{color};'>{ct}</span>")
    #
    return "".join(html)
#
```

从上面的例子可以看出，如果模型将某些文本检测为人类生成的，那么这些文本确实是人类生成的，但如果模型将文本检测为 AI 生成的，那么有可能并非 AI 生成的。那么为什么会发生这种情况呢？让我们接着看！

## 假阳性

我们的语言模型经过大量由人类编写的文本训练。通常很难检测到某个特定人是否（数字化）编写了某些内容。模型的训练输入包含了许多不同的写作风格，很可能是由大量不同的人编写的。这使得模型学习了许多不同的写作风格和内容。很可能你的写作风格与模型训练中某些文本的写作风格非常接近。这就是假阳性出现的原因，也是模型不能确定某些文本是否是 AI 生成的原因。然而，模型可以确定某些文本是人类生成的。

**OpenAI：** OpenAI 最近宣布将停止其用于检测 AI 生成文本的工具，理由是准确率较低（来源：[Hindustan Times](https://tech.hindustantimes.com/tech/news/openai-kills-off-its-own-ai-text-detection-tool-shocking-reason-behind-it-71690364760759.html)）。

> AI 分类器工具的原始版本从一开始就存在某些局限性和不准确性。用户需要手动输入至少 1,000 个字符的文本，然后 OpenAI 会分析这些文本以分类为 AI 或人类编写。不幸的是，该工具的表现不尽如人意，因为它仅正确识别了 26% 的 AI 生成内容，并且错误地将 9% 的人类编写文本标记为 AI。

这是 [OpenAI 的博客文章](https://openai.com/blog/new-ai-classifier-for-indicating-ai-written-text)。看起来他们使用了与本文中提到的不同的方法。

> 我们的分类器是一个语言模型，经过微调，使用的是同一主题的人工编写文本和 AI 编写文本的对照数据集。我们从多个我们认为是由人类编写的来源收集了这个数据集，比如预训练数据和提交给 InstructGPT 的人类演示。我们将每篇文本分为提示和回应。在这些提示上，我们生成了来自我们和其他组织训练的各种语言模型的回应。对于我们的网页应用，我们调整了置信度阈值，以保持低的假阳性率；换句话说，我们只有在分类器非常确信的情况下才将文本标记为可能的 AI 编写。

**GPTZero：** 另一个流行的 AI 生成文本检测工具是 [GPTZero](https://gptzero.me/)。看起来 GPTZero 使用 [困惑度和突发性](https://ecoagi.ai/topics/ChatGPT/high-perplexity-score-gpt-zero) 来检测 AI 生成的文本。*“突发性指的是某些词语或短语在文本中以突发的形式出现。换句话说，如果一个词在文本中出现了一次，它很可能会在接近的地方再次出现”*（[来源](https://ecoagi.ai/topics/ChatGPT/high-perplexity-score-gpt-zero)）。

GPTZero 宣称具有非常高的成功率。根据 [GPTZero FAQ](https://gptzero.me/faq)，*“在 0.88 的阈值下，85% 的 AI 文档被分类为 AI，99% 的人工文档被分类为人工。”*

## 这种方法的普遍性

这篇文章提到的方法不具有很好的一般化性。我们的意思是，如果你有三个语言模型，比如 GPT3、GPT3.5 和 GPT4，那么你必须将输入文本通过这三个模型，并检查它们的困惑度，以确定文本是否由其中任何一个模型生成。这是因为每个模型生成文本的方式略有不同，它们都需要独立评估文本，以确定是否有模型生成了该文本。

随着到 2023 年 8 月世界上大量语言模型的普及，似乎不太可能检查任何文本是否来自世界上的任何语言模型。

实际上，每天都有新的模型在训练，跟上这种快速进展似乎非常困难。

以下示例显示了让我们的模型预测 ChatGPT 生成的句子是否为 AI 生成的结果。如你所见，结果是混合的。

![](../Images/dfc4b3b7f610dcb435455a04baf59c02.png)

紫色框中的句子被我们的模型正确识别为 AI 生成的，而其余的则被错误识别为人工撰写的。

这可能发生的原因有很多。

1.  **训练语料库规模：** 我们的模型在非常少的文本上进行训练，而 ChatGPT 在数TB 的文本上进行训练。

1.  **数据分布：** 我们的模型在与 ChatGPT 不同的数据分布上进行训练。

1.  **微调：** 我们的模型只是一个 GPT 模型，而 ChatGPT 是针对聊天类响应进行微调的，使其生成的文本具有略微不同的语调。如果你有一个生成法律文本或医学建议的模型，那么我们的模型在这些模型生成的文本上也会表现不佳。

1.  **模型规模：** 我们的模型非常小（少于 100M 参数，而 ChatGPT 类模型有超过 200B 参数）。

很明显，如果我们希望提供合理高质量的结果以检查文本是否为 AI 生成的，我们需要一种更好的方法。

接下来，我们来看一下关于这个话题在互联网中流传的一些错误信息。

# 错误信息

一些文章对困惑度的解释不正确。例如，如果你在 google.com 搜索 [“人工撰写内容的困惑度高还是低？”](https://www.google.com/search?q=does+human+written+content+have+high+or+low+perplexity%3F)，你会在第一个位置看到 [以下结果](https://www.linkedin.com/pulse/how-write-content-using-chatgpt-outsmarts-ai-tools-99-dasun-sucharith/)。

![](../Images/51bd737f2d520f0aa9f71677734a6cf7.png)

这是不正确的，因为人工撰写的内容通常比 AI 生成的内容具有更高的困惑度。

让我们来看看研究人员在这一领域探索的技术，以期比目前的情况做得更好。

# 接下来是什么？

我们已经确定，检测AI生成文本是一个困难的问题，成功率低到不比猜测更好。让我们看看这一领域的最先进技术，研究人员如何探索以更好地处理这个问题。

**水印：** [OpenAI和Google已承诺为AI生成的文本添加水印](https://arstechnica.com/ai/2023/07/openai-google-will-watermark-ai-generated-content-to-hinder-deepfakes-misinfo/)，以便可以程序化识别。

关于这种水印如何工作的技术细节尚不清楚，两家公司都没有披露相关细节。

即使OpenAI和Google采用了水印技术，我们也不能确定所有部署的语言模型都会包含水印。人们仍然可能会部署自己的模型来生成文本并将其发布到公共平台。即使公司决定给生成的文本加水印，也不清楚这是否会成为标准，还是每家公司会有自己的专有策略和可能收费的工具来检查文本是否由其AI文本生成工具生成。如果这是一个开放标准，人们有可能绕过它，除非它像加密密码一样需要大量计算来解密。如果不是开放标准，那么人们将依赖这些公司提供开放和免费的工具及API来进行检查。此外，这些工具在长期中的有效性也是一个问题，因为甚至可能训练模型来接收带水印的AI生成文本，并返回没有水印的AI生成文本。

[这篇文章](https://www.nytimes.com/interactive/2023/02/17/business/ai-text-detection.html)讨论了一种为AI生成文本添加水印的可能技术，并提到这种方法的重大挑战。

**个性化：** 在我们看来，检测AI生成文本的问题在短期内仍将具有挑战性。我们相信，策略需要变得更加侵入性和个性化才能更有效。例如，与其询问某些文本是否由AI生成，不如询问这些文本是否由特定的人撰写。然而，这将要求系统能够访问大量该特定人撰写的文本。此外，如果某些文本由多人撰写，如本文所示，问题会变得更加复杂。

让我们看看这种个性化的系统在检测人工撰写文本方面对教育工作者和学生的影响。

如果存在这样的解决方案，**教育工作者**将更倾向于给学生布置个人作业而不是小组作业。这还将要求每个学生首先提供大量他们自己撰写的文本。这可能意味着在入学前需要在大学里亲自花费几个小时。这无疑会对教授学生团队合作以实现共同目标的重要性产生负面影响。

另一方面，在某些情况下，访问基于AI的文本生成技术可以让学生集中精力解决实际问题，例如进行研究或文献研究，而不是花时间以完善的方式撰写他们的学习成果。可以想象，学生们会在数学或科学课程中花更多时间学习概念和技术，而不是写作。那部分可以由AI处理。

# 结论

在本文中，我们建立了如何检测AI生成文本的直觉。我们可以使用的主要指标是生成文本的困惑度。我们看到了一些PyTorch代码，用于检查给定文本是否可能是AI生成的，方法是利用文本的困惑度。我们也看到了这种方法的一些缺点，包括可能出现假阳性。希望这能帮助你理解和欣赏检测AI生成文本的细节。

这是一个不断发展的领域，研究人员正努力寻找一种更高准确率检测AI生成文本的方法。这项技术对我们生活的影响承诺将是显著的，并且在许多方面仍然未知。

虽然我们讨论了检测AI生成文本的技术，但我们假设整个文本要么是人类撰写的，要么是AI生成的。实际上，文本往往是部分人类撰写的，部分AI生成的，这使得问题变得更加复杂。

如果你想了解更多检测AI生成文本的方法，比如使用突发性指标，你可以在[这里](/how-do-we-know-if-a-text-is-ai-generated-82e710ea7b51)阅读。

本文中的所有图片（除了第一张）均由作者（们）创作。
