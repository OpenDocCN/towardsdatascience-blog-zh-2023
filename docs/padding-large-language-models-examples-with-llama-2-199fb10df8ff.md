# 填充大型语言模型 — 使用 Llama 2 的示例

> 原文：[https://towardsdatascience.com/padding-large-language-models-examples-with-llama-2-199fb10df8ff?source=collection_archive---------2-----------------------#2023-08-11](https://towardsdatascience.com/padding-large-language-models-examples-with-llama-2-199fb10df8ff?source=collection_archive---------2-----------------------#2023-08-11)

## 为因果 LLMs 填充训练示例的最佳实践

[](https://medium.com/@bnjmn_marie?source=post_page-----199fb10df8ff--------------------------------)[![Benjamin Marie](../Images/3ea1ad230cb1e67610418a8e36a5e5dd.png)](https://medium.com/@bnjmn_marie?source=post_page-----199fb10df8ff--------------------------------)[](https://towardsdatascience.com/?source=post_page-----199fb10df8ff--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----199fb10df8ff--------------------------------) [Benjamin Marie](https://medium.com/@bnjmn_marie?source=post_page-----199fb10df8ff--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fad2a414578b3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpadding-large-language-models-examples-with-llama-2-199fb10df8ff&user=Benjamin+Marie&userId=ad2a414578b3&source=post_page-ad2a414578b3----199fb10df8ff---------------------post_header-----------) 发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----199fb10df8ff--------------------------------) · 10分钟阅读·2023年8月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F199fb10df8ff&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpadding-large-language-models-examples-with-llama-2-199fb10df8ff&user=Benjamin+Marie&userId=ad2a414578b3&source=-----199fb10df8ff---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F199fb10df8ff&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpadding-large-language-models-examples-with-llama-2-199fb10df8ff&source=-----199fb10df8ff---------------------bookmark_footer-----------)![](../Images/5dee339a0ba5a939a758499baeec9333.png)

作者提供的图像 — 基于来自[Pixabay](https://pixabay.com/vectors/llama-alpaca-animal-mammal-zoo-297668/)的图像

填充是大型语言模型（LLMs）中最少被记录的方面之一。为什么？仅仅因为 LLMs 通常在没有填充的情况下进行预训练。

尽管如此，对于在自定义数据集上微调 LLMs，填充是必要的。未能正确填充训练示例可能会导致各种意想不到的行为：训练期间的空损失或无限损失、生成过多或推理期间的空输出，都是填充不正确的症状。

在这篇文章中，我首先解释什么是填充以及为什么它是必要的。然后，我展示了如何为没有填充的预训练 LLM 找到正确的填充策略。我提出了两种不同的解决方案来为 LLM 添加填充支持，使用 Hugging Face 的 Transformers。

在文章的末尾，我还提供了示例，展示了如何为 Llama 2 填充你的训练示例。

阅读完这篇文章后，你应该能够自己搞清楚如何为 LLM 填充训练示例，而无需阅读它们的文档或教程。

# 填充和批次

## 什么是填充，为什么我们需要填充？

我们来举一个例子，假设我们希望用来微调一个 LLM。

```py
example = "You are not a chatbot."
```

我们需要将这个示例转换为一个标记序列。像 Transformers 这样的库通常按以下步骤进行分词：

+   根据给定的词汇表将示例分割成子词：

```py
example = ["▁You", "▁are", "▁not", "▁a". "▁chat", "bot", "."]
```

+   用词汇表中的索引替换单词，以获得一个整数序列：

```py
example = [887, 526, 451, 263, 13563, 7451, 29889]
```

+   向序列中添加特殊标记：BOS 标记、EOS 标记、UNK 标记、PAD 标记等。

```py
example = [1, 887, 526, 451, 263, 13563, 7451, 29889]
```

*注意：在这个例子中，我使用了 Llama 2 的分词器。我们将在下面详细了解如何操作。*

在这个例子中，仅添加了 BOS（序列开始）特殊标记。

每个训练示例还会生成一个注意力掩码。这个掩码告诉变压器是否应该关注一个标记（1）或不关注（0）。这个示例的注意力掩码很简单，因为所有标记都应该被考虑。

```py
#We have as many values as tokens.
attention_mask = [1, 1, 1, 1, 1, 1, 1, 1]
```

下一步是将所有内容封装成 Pytorch 张量。这种封装是必要的，以便应用 CUDA 和 GPU 优化的矩阵操作。

```py
{'input_ids': tensor([[1, 887, 526, 451, 263, 13563, 7451, 29889]]), 
 'attention_mask': tensor([[1, 1, 1, 1, 1, 1, 1, 1]])}
```

现在，假设我们有两个训练示例。为了简单起见，我会重复我已有的一个。新的张量多出了一行：

```py
{'input_ids': tensor([[1, 887, 526, 451, 263, 13563, 7451, 29889],
                      [1, 887, 526, 451, 263, 13563, 7451, 29889]]), 
 'attention_mask': tensor([[1, 1, 1, 1, 1, 1, 1, 1],
                           [1, 1, 1, 1, 1, 1, 1, 1]])}
```

两个示例具有相同的长度（当然，因为它们是相同的）。这两个张量的维度都是 2x8（N x M）。

示例被放入张量中以创建批次，以便神经网络在看到 N 个示例后可以更新其值。批次对于计算效率和模型性能至关重要。

现在，让我们引入一个更短的第三个示例：

```py
example = "You are not."
```

在分词后，我们得到：

```py
example = [1, 887, 526, 451, 29889]
attention_mask = [1, 1, 1, 1, 1]
```

如果你尝试将其添加到我们的示例列表中并创建张量，你将会遇到错误。但想象一下，如果没有错误，我们将得到：

```py
{'input_ids': tensor([[1, 887, 526, 451, 263, 13563, 7451, 29889],
                      [1, 887, 526, 451, 263, 13563, 7451, 29889],
                      [1, 887, 526, 451, 29889]]), 
 'attention_mask': tensor([[1, 1, 1, 1, 1, 1, 1, 1],
                           [1, 1, 1, 1, 1, 1, 1, 1],
                           [1, 1, 1, 1, 1]])}
```

*你能看到这里的问题吗，为什么不能创建这样的张量？*

我们有一行不同长度的数据。我们无法对其应用矩阵操作。

在大多数数据集中，示例的长度不相同。我们必须修改它们，以确保同一批次中的示例具有相同的长度。

这就是我们需要“填充”的原因。

## 填充标记和填充侧

你可以将填充视为通过重复一个虚拟标记来扩展序列直到给定长度。

这个虚拟标记是一个“填充标记”。

例如，上面第一个示例的长度为 8 个标记（包括 BOS 标记）。假设在我们的批处理中，我们不会有超过 8 个标记的序列。所有序列必须为 8 个标记长。

我们的第二个示例仅包含 5 个标记。所以我们必须添加 3 个填充标记。

```py
example = "You are not. [PAD] [PAD] [PAD]"
```

实际上，我们不会手动将“[PAD]”标记添加到序列中。大多数分词器会将“[PAD]”拆分成子词。填充标记通常是分词器内部定义的特殊标记，并且在必要时会与其他特殊标记一起自动添加到序列中。

如果填充标记在词汇表中的 ID 是 32000，我们将得到：

```py
example = [1, 887, 526, 451, 29889, 32000, 32000, 32000]
```

现在，我们有了一个具有预期长度的序列。但还有一个问题：我们还需要修改注意力掩码。

记住，填充标记是虚拟标记，我们不希望 LLM 对它们给予任何关注。我们引入这些标记只是为了填充序列并创建正确的张量。

为了将其指示给模型，我们只需在注意力掩码中设置“0”，这样模型将忽略它们。

```py
attention_mask = [1, 1, 1, 1, 1, 0, 0, 0]
```

最后，我们可以使用填充的示例创建正确的张量：

```py
{'input_ids': tensor([[1, 887, 526, 451, 263, 13563, 7451, 29889],
                      [1, 887, 526, 451, 263, 13563, 7451, 29889],
                      [1, 887, 526, 451, 29889, 32000, 32000, 32000]]), 
 'attention_mask': tensor([[1, 1, 1, 1, 1, 1, 1, 1],
                           [1, 1, 1, 1, 1, 1, 1, 1],
                           [1, 1, 1, 1, 1, 0, 0, 0]])}
```

*注意：填充是在序列长度小于最大长度时执行的。但在某些情况下，序列可能会太长。在这种情况下，我们必须截断序列，以使其大小匹配最大长度。*

填充的另一个重要参数是填充侧面。在上面的示例中，我进行了右填充。如果模型有 EOS 标记，填充标记将添加到它之后。

我们也可以进行左填充。在这种情况下，张量看起来像这样：

```py
{'input_ids': tensor([[1, 887, 526, 451, 263, 13563, 7451, 29889],
                      [1, 887, 526, 451, 263, 13563, 7451, 29889],
                      [32000, 32000, 32000, 1, 887, 526, 451, 29889]]), 
 'attention_mask': tensor([[1, 1, 1, 1, 1, 1, 1, 1],
                           [1, 1, 1, 1, 1, 1, 1, 1],
                           [0, 0, 0, 1, 1, 1, 1, 1]])}
```

填充标记是在 BOS 标记之前添加的。

选择哪个侧面主要取决于你想使用的 LLM 和你的下游任务。这就是为什么在做出任何决定之前，研究模型及其分词器是很重要的。下面，我们将看到如何为 Llama 2 做出这个决定。

# 为因果 LLM 添加填充支持

如我们所见，填充（几乎）总是对于微调是必要的。然而，许多 LLM 默认不支持填充。这意味着它们的词汇表中没有特殊的填充标记。

在这里，我展示了两种添加填充标记的解决方案。

## 简单的解决方案

这个解决方案是你在大多数教程中会找到的。

它只是将现有标记分配给填充标记。例如，你可以声明你的填充标记将是 EOS 标记。我们将得到像这样（右填充，其中“2”是 EOS 标记的 ID）的张量：

```py
{'input_ids': tensor([[1, 887, 526, 451, 263, 13563, 7451, 29889],
                      [1, 887, 526, 451, 263, 13563, 7451, 29889],
                      [1, 887, 526, 451, 29889, 2, 2, 2]]), 
 'attention_mask': tensor([[1, 1, 1, 1, 1, 1, 1, 1],
                           [1, 1, 1, 1, 1, 1, 1, 1],
                           [1, 1, 1, 1, 1, 0, 0, 0]])}
```

这个解决方案的问题是 LLM 现在感到困惑：大多数情况下，EOS 标记在注意力掩码中将是“0”。这会促使 LLM 忽略原始的 EOS 标记。这并不理想，因为 EOS 标记会指示 LLM 停止生成。

使用这种解决方案，我们也必须进行右填充。如果你进行左填充，你会得到以 EOS 标记开头的序列，从而过早停止生成。

*注意：我阅读了几个用于微调 Llama 2 的教程，这些教程使用 EOS token 进行左填充。如果这样做，你将得到 0.0 的损失，训练将会发散。试试看吧！观察这个现象很有趣。*

在我看来，更好的替代方案是使用 UNK token 作为 pad token。这个 token 很少被使用。只有当 token 不在词汇表中时，它才会出现在序列中。将它用于其他目的不应产生重大影响。

Meta 在其“[Llama recipes](https://github.com/facebookresearch/llama-recipes/blob/main/utils/train_utils.py)”中使用了这一替代方案。使用此解决方案，填充侧别不那么重要。

## 另一种解决方案：从头创建一个 pad token

UNK token 在模型中已经有其角色。理想情况下，我们希望有一个仅用于填充的 pad token。

如果词汇表中不存在，我们必须从头创建一个 pad token。[这是 Hugging Face 推荐的 Llama 2 解决方案](https://huggingface.co/docs/transformers/main/model_doc/llama2)。

使用如 transformers 等库，扩展词汇表非常简单。

如果你想创建一个 pad token，你需要按照以下步骤进行：

+   将 pad token 作为特殊 token 添加到 LLM 的词汇中。

+   调整 token embeddings 的大小

+   重新训练 token embeddings（可选）

如果你有预算限制并使用 LoRa 进行微调，你可能会想跳过最后一步，因为 token embeddings 可能会有数亿参数。此外，在我的实验中，重新训练 embeddings 总是导致更糟的结果，这表明我的微调数据集可能不够大，或者好的超参数很难找到。

# 案例研究：用 Hugging Face 的 Transformers 填充 Llama 2

在本节中，我们将为 Llama 2 启用填充。要复制每个步骤，你需要访问 Hugging Face 上的 Llama 2。我在[这篇文章中解释了如何获取 Llama 2](https://kaitchup.substack.com/p/fine-tune-llama-2-on-your-computer)。

*注意：我分享了* [*一个笔记本 (#8) 在 The Kaitchup，我的 substack 新闻通讯上复制所有这些步骤。*](https://kaitchup.substack.com/publish/post/134749542)

首先，安装 Transformers 库：

```py
pip install transformers
```

然后，我们导入 transformers 并加载 tokenizer。确保你在其中输入了你的 Hugging Face 访问 token：

```py
from transformers import AutoTokenizer

#Replace the following with your own Hugging Face access token.
access_token = "hf_token"

#The model we want to quantize
pretrained_model_dir = "meta-llama/Llama-2-7b-hf"
tokenizer = AutoTokenizer.from_pretrained(pretrained_model_dir, use_fast=True,  use_auth_token=access_token)
```

我们定义了两个训练示例：

```py
prompt1 = "You are not a chatbot."
prompt2 = "You are not."
```

如果我们将 prompt1 放在同一批次中两次，一切顺利：

```py
prompts = [prompt1, prompt1]
input = tokenizer(prompts, return_tensors="pt");
print(input)
```

输出：

```py
{'input_ids': tensor([[    1,   887,   526,   451,   263, 13563,  7451, 29889],
        [    1,   887,   526,   451,   263, 13563,  7451, 29889]]), 'attention_mask': tensor([[1, 1, 1, 1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1, 1, 1, 1]])}
```

但如果你添加了 prompt2，你将如预期那样遇到错误：

```py
prompts = [prompt1, prompt1, prompt2]
input = tokenizer(prompts, return_tensors="pt");
print(input)
```

输出：

```py
ValueError: Unable to create tensor, you should probably activate truncation and/or padding with 'padding=True' 'truncation=True' to have batched tensors with the same length. Perhaps your features (`input_ids` in this case) have excessive nesting (inputs type `list` where type `int` is expected).
```

很明显，tokenizer 并没有填充示例。

我们可以通过简单地使用 UNK token 作为 pad token 来解决这个问题，具体如下：

```py
tokenizer.padding_side = "left"
tokenizer.pad_token = tokenizer.unk_token
input = tokenizer(prompts, padding='max_length', max_length=20, return_tensors="pt");
print(input)
```

在这个例子中，我要求 tokenizer 填充到 max_length。我将 max_length 设置为 20。如果你的示例包含 10 个 tokens，tokenizer 将添加 10 个 pad tokens。

```py
{'input_ids': tensor([[    0,     0,     0,     0,     0,     0,     0,     0,     0,     0,
             0,     0,     1,   887,   526,   451,   263, 13563,  7451, 29889],
        [    0,     0,     0,     0,     0,     0,     0,     0,     0,     0,
             0,     0,     1,   887,   526,   451,   263, 13563,  7451, 29889],
        [    0,     0,     0,     0,     0,     0,     0,     0,     0,     0,
             0,     0,     0,     0,     0,     1,   887,   526,   451, 29889]]), 'attention_mask': tensor([[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1]])}
```

另一种方法是从头创建一个 pad token。使用 Hugging Face 的 transformers，我们可以通过方法“add_special_tokens”来实现这一点。

```py
tokenizer.add_special_tokens({'pad_token': '[PAD]'})
input = tokenizer(prompts, padding='max_length', max_length=20, return_tensors="pt");
print(input)
```

输出：

```py
{'input_ids': tensor([[32000, 32000, 32000, 32000, 32000, 32000, 32000, 32000, 32000, 32000,
         32000, 32000,     1,   887,   526,   451,   263, 13563,  7451, 29889],
        [32000, 32000, 32000, 32000, 32000, 32000, 32000, 32000, 32000, 32000,
         32000, 32000,     1,   887,   526,   451,   263, 13563,  7451, 29889],
        [32000, 32000, 32000, 32000, 32000, 32000, 32000, 32000, 32000, 32000,
         32000, 32000, 32000, 32000, 32000,     1,   887,   526,   451, 29889]]), 'attention_mask': tensor([[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1]])}
```

添加了填充标记到Llama 2的词汇表后，不要忘记调整Llama 2的令牌嵌入。我在这篇文章中解释了如何做到这一点：

[## 在你的计算机上使用QLoRa和TRL对Llama 2进行微调](https://kaitchup.substack.com/p/fine-tune-llama-2-on-your-computer?source=post_page-----199fb10df8ff--------------------------------)

### 在Guanaco和正确的填充设置下

[kaitchup.substack.com](https://kaitchup.substack.com/p/fine-tune-llama-2-on-your-computer?source=post_page-----199fb10df8ff--------------------------------)

# 结论

一旦你理解了，填充就非常简单。

使用UNK标记进行填充，或者从头创建填充标记，是非常安全的解决方案，几乎适用于所有因果语言模型。但你应该始终查看分词器的工作方式。至少你应该了解它已经支持的特殊标记。例如，并非所有语言模型都有UNK标记，有些语言模型的填充标记在词汇表中没有明确定义为填充标记，等等。

像往常一样，如果你有任何问题，请留下评论。我会尽量回答。
