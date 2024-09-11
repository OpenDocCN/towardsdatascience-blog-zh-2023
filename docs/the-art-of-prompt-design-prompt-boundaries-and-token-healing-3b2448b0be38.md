# 《提示设计的艺术：提示边界与标记修复》

> 原文：[https://towardsdatascience.com/the-art-of-prompt-design-prompt-boundaries-and-token-healing-3b2448b0be38?source=collection_archive---------2-----------------------#2023-05-08](https://towardsdatascience.com/the-art-of-prompt-design-prompt-boundaries-and-token-healing-3b2448b0be38?source=collection_archive---------2-----------------------#2023-05-08)

[](https://medium.com/@scottmlundberg?source=post_page-----3b2448b0be38--------------------------------)[![Scott Lundberg](../Images/99f1c984f0aaabfe4e348a92fa50a1ee.png)](https://medium.com/@scottmlundberg?source=post_page-----3b2448b0be38--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3b2448b0be38--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3b2448b0be38--------------------------------) [Scott Lundberg](https://medium.com/@scottmlundberg?source=post_page-----3b2448b0be38--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3a739af9ef3a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-art-of-prompt-design-prompt-boundaries-and-token-healing-3b2448b0be38&user=Scott+Lundberg&userId=3a739af9ef3a&source=post_page-3a739af9ef3a----3b2448b0be38---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3b2448b0be38--------------------------------) ·7 分钟阅读·2023年5月8日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3b2448b0be38&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-art-of-prompt-design-prompt-boundaries-and-token-healing-3b2448b0be38&user=Scott+Lundberg&userId=3a739af9ef3a&source=-----3b2448b0be38---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3b2448b0be38&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-art-of-prompt-design-prompt-boundaries-and-token-healing-3b2448b0be38&source=-----3b2448b0be38---------------------bookmark_footer-----------)![](../Images/7da96052b147da9162ebeb9229095ca5.png)

所有图像均为原创作品。

这篇文章（与 [Marco Tulio Ribeiro](https://medium.com/@marcotcr) 共同撰写）是关于**提示设计艺术**系列的第二部分（第一部分 [这里](https://medium.com/towards-data-science/the-art-of-prompt-design-use-clear-syntax-4fc846c1ebd5)），在其中我们讨论了如何通过 `[guidance](https://github.com/microsoft/guidance)` 控制大型语言模型（LLMs）。

在这篇文章中，我们将讨论语言模型使用的贪婪/优化标记化方法如何在提示中引入微妙而强大的偏差，从而导致令人困惑的生成结果。

语言模型不是在原始文本上进行训练，而是在标记（tokens）上进行训练，标记是经常一起出现的文本块，类似于单词。这会影响语言模型如何“看待”文本，包括提示（因为提示只是标记的集合）。GPT 风格的模型使用像 [字节对编码](https://en.wikipedia.org/wiki/Byte_pair_encoding)（BPE）这样的标记化方法，它们以贪婪的方式将所有输入字节映射到标记 ID。这在训练时是可以接受的，但在推理过程中可能会导致微妙的问题，如下例所示。

# **提示边界问题的一个例子**

考虑以下示例，我们正在尝试生成 HTTP URL 字符串：

```py
import transformers

# we use StableLM as an example, but these issues impact all models to varying degrees
generator = transformers.pipeline('text-generation', model='stabilityai/stablelm-base-alpha-3b')

raw_gen('The link is <a href="http:') # helper func to call the generator
```

![](../Images/9f08089bdd4af8fb7a57582e30d16f31.png)

笔记本输出。

注意，LLM 生成的输出没有用明显的下一个字符（两个斜杠）完成 URL。它反而创建了一个中间带有空格的无效 URL 字符串。这令人惊讶，因为在 `http:` 之后 `//` 的补全是非常明显的。要理解为什么会发生这种情况，让我们改变我们的提示边界，使提示不包括冒号字符：

```py
raw_gen('The link is <a href="http')
```

![](../Images/9a83d183032d3ab835c0a1d65b13dac6.png)

现在，语言模型生成了我们预期的有效 URL 字符串。要理解 `:` 的重要性，我们需要查看提示的标记化表示。下面是以冒号结尾的提示的标记化（不包含冒号的提示有相同的标记化，除了最后一个标记）：

```py
print_tokens(generator.tokenizer.encode('The link is <a href="http:'))
```

![](../Images/06ebdcbaf90a7dc92b1e504e2829498b.png)

现在注意有效 URL 的标记化是什么样的，特别关注 `http` 后的标记 `1358`：

```py
print_tokens(generator.tokenizer.encode('The link is <a href="http://www.google.com/search?q'))
```

![](../Images/a3c2d8e91a5a4fdc91400781b851273d.png)

大多数 LLM（包括这个）使用贪婪的标记化方法，总是偏向于选择最长的标记，即 `://` 在完整文本中（例如在训练中）总是优于 `:`。

在训练中，URL 使用标记 1358（`://`）进行编码，而我们的提示使 LLM 看到标记 `27`（`:`），这会通过人为地将 `://` 拆分开来，从而影响完成。

实际上，模型可以非常确定看到标记 `27`（`:`）意味着接下来的内容极不可能是可以与冒号一起编码的“更长的标记”像 `://`，因为在模型的训练数据中，这些字符会与冒号一起编码（稍后我们将讨论的一个例外是训练期间的子词正则化）。看到一个标记意味着同时看到该标记的嵌入 **以及** 之后的内容没有被贪婪的标记化器压缩，这一点容易被忘记，但在提示边界中很重要。

让我们搜索模型词汇表中所有标记的字符串表示，看看哪些标记以冒号开头：

```py
N = generator.tokenizer.vocab_size
tokens = generator.tokenizer.convert_ids_to_tokens(range(N))
print_tokens([i for i,t in enumerate(tokens) if t.startswith(":")])
```

![](../Images/bba5fa5bd4edeccb89dafdecbea690df.png)

请注意，有**34**种不同的令牌以冒号开头，因此以冒号结尾的提示可能不会生成这些**34**种令牌字符串中的任何一种。*这种微妙而强大的偏差可能会带来各种意想不到的后果。* 这适用于**任何**可能被扩展为更长单一令牌的字符串（不仅仅是 `:`）。即使是我们以“http”结尾的“固定”提示也存在内建偏差，因为它向模型传达了“http”后面内容可能不是“s”（否则“http”不会被编码为单独的令牌）：

```py
print_tokens([i for i,t in enumerate(tokens) if t.startswith("http")])
```

![](../Images/5722e1eed5f87ab98014368cd2e2dbce.png)

以为这是只影响URL的深奥问题而不值得担忧，请记住，大多数令牌化器会根据令牌是否以空格、标点、引号等开头来处理令牌，因此**以这些内容结尾的提示可能会导致错误的令牌边界**，从而破坏内容：

```py
# Accidentally adding a space, will lead to weird generation
raw_gen('I read a book about ')
```

![](../Images/fab000c5aac694ca330aa3f9f98055a0.png)

```py
# No space, works as expected
raw_gen('I read a book about')
```

![](../Images/c4b67b763845aed5ce9f75a575cefd3c.png)

另一个例子是“[”字符。考虑以下提示和补全：

```py
raw_gen('An example ["like this"] and another example [')
```

![](../Images/2e193cde149e2a761afee84cf6cae3b6.png)

为什么第二个字符串没有被引号括起来？因为通过以“`[`”令牌结尾，我们告诉模型不生成与以下27个更长令牌匹配的补全（其中一个增加了引号字符，`15640`）：

```py
# note the Ġ is converted to a space by the tokenizer
print_tokens([i for i,t in enumerate(tokens) if t.startswith("Ġ[")])
```

![](../Images/8d0086e91c6df84c0bcdd79736f6f867.png)

令牌边界偏差无处不在。上述StableLM模型的*70%*最常见令牌是较长可能令牌的前缀，因此在提示中的最后一个令牌时会造成令牌边界偏差。*

# **通过“令牌修复”修正无意的偏差**

我们可以做些什么来避免这些无意的偏差？一种选择是始终以不能扩展为更长令牌的令牌结束提示（例如用于基于聊天的模型的角色标签），但这是一种严重限制。

相反，`guidance`具有一个叫做“令牌修复”的功能，它会在提示末尾之前自动将生成过程备份一个令牌，然后约束生成的第一个令牌具有与提示最后一个令牌匹配的前缀。在我们的URL示例中，这将意味着移除 `:` 并强制生成的第一个令牌具有 `:` 前缀。令牌修复允许用户以任何他们希望的方式表达提示，而无需担心令牌边界。

例如，让我们重新运行一些上述URL示例，并开启令牌修复功能（对于Transformer模型，默认是开启的，因此我们去掉 `token_healing=False`）：

```py
from guidance import models, gen

# load StableLM from huggingface
lm = models.Transformers("stabilityai/stablelm-base-alpha-3b", device=0)

# With token healing we generate valid URLs,
# even when the prompt ends with a colon:
lm + 'The link is <a href="http:' + gen(max_tokens=10)
```

![](../Images/076ec3d9d9243b506a86a061a6e4e78f.png)

```py
# With token healing, we will sometimes generate https URLs,
# even when the prompt ends with "http":
[str(lm + 'The link is <a href="http' + gen(max_tokens=10, temperature=1)) for i in range(10)]
```

![](../Images/143c104e36c53354f3e659a68d58613f.png)

同样，我们无需担心额外的空格：

```py
# Accidentally adding a space will not impact generation
lm + 'I read a book about ' + gen(max_tokens=5)
```

![](../Images/e217a0e6ec54dca35357504011974386.png)

```py
# This will generate the same text as above 
lm + 'I read a book about' + gen(max_tokens=6)
```

![](../Images/0f2d372e350aab9997ee5e61dbd818f9.png)

现在即使提示以“`[`”令牌结尾，我们也能获得引号括起来的字符串：

```py
lm + 'An example ["like this"] and another example [' + gen(max_tokens=10)
```

![](../Images/7aa6d716e7d702cae0a8567009775452.png)

# **那么子词正则化呢？**

如果你对语言模型的训练方式很熟悉，你可能会想知道[子词正则化](https://arxiv.org/abs/1804.10959)在这一切中如何发挥作用。子词正则化是一种技术，在训练过程中随机引入次优的分词，以提高模型的鲁棒性。这意味着模型并不总是看到最佳的贪婪分词。子词正则化有助于提高模型对标记边界的鲁棒性，但它并不能完全消除模型对标准贪婪/优化分词的偏见。这意味着虽然模型的子词正则化程度可能影响标记边界的偏见程度，但所有模型仍然存在这种偏见。如上所示，它仍然可以对模型输出产生强大而意外的影响。

# **结论**

在编写提示时，请记住贪婪分词可能对语言模型如何解释你的提示产生重大影响，尤其是当提示以一个可能扩展为更长标记的标记结束时。这种容易被忽视的偏见来源可能以令人惊讶和意想不到的方式影响你的结果。

为了解决这个问题，可以在提示的末尾加上一个不可扩展的标记，或者使用类似`guidance`的“标记修复”功能，以便你可以根据自己的意愿表达提示，而不必担心标记边界的影响。

*要自己复现本文中的结果，请查看* [*notebook*](https://github.com/microsoft/guidance/blob/main/notebooks/art_of_prompt_design/prompt_boundaries_and_token_healing.ipynb) *版本。*
