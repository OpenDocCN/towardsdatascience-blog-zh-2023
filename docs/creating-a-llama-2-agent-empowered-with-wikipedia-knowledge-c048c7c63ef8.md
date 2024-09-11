# 创建一个具备维基百科知识的 LLaMa 2 代理

> 原文：[https://towardsdatascience.com/creating-a-llama-2-agent-empowered-with-wikipedia-knowledge-c048c7c63ef8?source=collection_archive---------2-----------------------#2023-09-26](https://towardsdatascience.com/creating-a-llama-2-agent-empowered-with-wikipedia-knowledge-c048c7c63ef8?source=collection_archive---------2-----------------------#2023-09-26)

## 利用检索增强生成技术为 LLaMa 2 提供支持，从维基百科中查找和使用信息。

[](https://medium.com/@gabrielesgroi94?source=post_page-----c048c7c63ef8--------------------------------)[![Gabriele Sgroi, PhD](../Images/b81978d35e6238d160457de2affc2b0e.png)](https://medium.com/@gabrielesgroi94?source=post_page-----c048c7c63ef8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c048c7c63ef8--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c048c7c63ef8--------------------------------) [Gabriele Sgroi, PhD](https://medium.com/@gabrielesgroi94?source=post_page-----c048c7c63ef8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F97ea0c34751b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-a-llama-2-agent-empowered-with-wikipedia-knowledge-c048c7c63ef8&user=Gabriele+Sgroi%2C+PhD&userId=97ea0c34751b&source=post_page-97ea0c34751b----c048c7c63ef8---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c048c7c63ef8--------------------------------) ·11 min 阅读·2023年9月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc048c7c63ef8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-a-llama-2-agent-empowered-with-wikipedia-knowledge-c048c7c63ef8&user=Gabriele+Sgroi%2C+PhD&userId=97ea0c34751b&source=-----c048c7c63ef8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc048c7c63ef8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-a-llama-2-agent-empowered-with-wikipedia-knowledge-c048c7c63ef8&source=-----c048c7c63ef8---------------------bookmark_footer-----------)![](../Images/4fe5a16d530a8b089a1c6ed5cd5ef8ce.png)

图片由 [Lysander Yuen](https://unsplash.com/@lysanderyuen?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

# 引言

大语言模型（LLMs）是人工智能领域的热门趋势之一。它们展示了令人印象深刻的文本生成能力，从与人类用户对话的能力到编写代码。开源LLMs的兴起，如LLama、Falcon、Stable Beluga等，使得它们的潜力对广泛的AI社区可用，同时也得益于开发更小、更高效模型的重点，这些模型可以在消费级硬件上运行。

促成大语言模型（LLMs）成功的关键因素之一是革命性论文[Attention Is All You Need](https://arxiv.org/abs/1706.03762)中引入的著名变换器架构。最先进的LLMs之所以表现出色，是因为将这一架构扩展到数十亿参数，并在包含万亿个标记的数据集上进行训练。这种预训练产生了强大的基础模型，能够理解人类语言，并可以进一步调整以适应特定的应用场景。

大语言模型的预训练是在自监督的方式下进行的。它需要大量的文本语料库，但不需要人工标注。这使得可以将训练扩展到以自动化方式创建的大规模数据集。将输入文本转化为标记序列后，预训练的目标是预测序列中每个标记的概率，条件是所有先前的标记。通过这种方式，训练完成后，模型能够通过自回归地生成文本，每次根据到目前为止采样的所有标记来采样一个标记。大语言模型仅凭借这种预训练已经展示了令人印象深刻的语言能力。然而，通过根据从训练数据中学到的条件概率采样标记，生成的文本通常与人类偏好不一致，LLMs在遵循特定指令或人类意图方面存在困难。

对齐大语言模型（LLMs）生成的文本与人类偏好的一个重要进展是通过人类反馈强化学习（RLHF）实现的。这项技术是最先进的聊天模型，如ChatGPT的核心。通过RLHF，在初始的自我监督预训练阶段之后，大语言模型会进一步通过强化学习算法进行微调，以最大化一个基于人类偏好的奖励。为了获得奖励函数，通常会训练一个辅助模型来学习一个反映人类偏好的标量奖励。这样，可以将实际需要的人类标注数据量保持在最低。奖励模型的训练数据包括由人类根据其偏好对生成的文本进行排名的文本。模型的目标是对排名较高的文本预测更高的奖励。通过最大化反映人类偏好的奖励来训练LLM应能生成更符合人类意图的文本。实际上，通过人类反馈强化学习微调的大语言模型已被证明能够更好地遵循用户指令，同时也更少有毒，更真实。

## 检索增强生成

大语言模型的一个典型缺点是它们离线训练，因此没有关于训练数据收集后发生的事件的信息。同样，它们不能使用训练数据中未包含的特定知识。这对于特定领域可能是个问题，因为用于训练LLMs的数据通常来自一般领域的语料库。绕过这些问题的一种方法是检索增强生成（RAG），而无需昂贵的微调。RAG通过将外部文本信息增强到输入的提示中来工作。这通常是通过与当前提示相关的外部数据源进行检索来实现的。在实践中，第一步涉及将提示和外部文本转换为向量嵌入。后者可以通过汇聚一个训练过的转换器编码器模型（如BERT）的输出获得，该模型将具有相似意义的文本映射到根据适当度量接近的嵌入中。在长文本的情况下，可以将其拆分为单独嵌入的块，从而检索出最相关的段落。接下来，检索到的文本的嵌入与提示嵌入最接近。经过适当格式化的提示和检索文本的连接将作为输入提供给语言模型。

通过检索增强生成（RAG），模型可以访问训练期间不可用的信息，并基于选定的文本语料库来回答问题。RAG还使得检查模型用于回答的来源成为可能，从而使人类用户对模型输出的评估更加直接。

在这篇博客文章中，我将解释如何创建一个简单的代理，能够基于从维基百科检索到的内容来回答问题，以展示LLMs寻求和使用外部信息的能力。给定用户的提示，模型将搜索适当的维基百科页面，并基于其内容进行回答。我在[这个GitHub仓库](https://github.com/GabrieleSgroi/wiki_llama)中提供了完整的代码。

# 增强了维基百科内容的Llama 2代理

在本节中，我将描述创建一个简单的Llama 2代理的步骤，该代理基于从维基百科检索到的信息来回答问题。具体来说，代理将…

+   创建适当的查询以搜索与用户问题相关的维基百科页面。

+   从维基百科找到的页面中检索出内容与用户问题最相关的页面。

+   从检索到的页面中提取与用户提示最相关的段落。

+   根据页面摘录回答用户的问题。

请注意，更一般来说，模型可能会收到一个增强的提示，包含最相关页面的完整内容或来自不同顶级页面的多个摘录，这些页面按照与用户提示的相关性进行排名。虽然这可以提高模型响应的质量，但它会增加所需的内存，因为它将不可避免地导致更长的提示。为了简单起见，并且为了在免费的Google Colab GPU上运行一个最小的示例，我限制了代理只使用来自最相关文章的几个摘录。

现在让我们深入探讨各个步骤的详细信息。代理需要执行的第一步是创建一个合适的搜索查询，以从维基百科中检索包含回答用户提示信息的内容。为此，我们将提示一个Llama 2聊天模型，要求它返回表示用户提示的关键词。在讨论具体的提示之前，我将简要回顾Llama 2聊天模型的通用提示格式。

在Llama 2聊天模型的训练过程中使用的模板具有以下结构：

```py
<s>[INST] <<SYS>>
{{ system_prompt }}
<</SYS>>

{{ user_message }} [/INST]
```

{{ system prompt}}指定了聊天模型对后续提示的行为，并可以用于将模型响应适应于不同任务。{{user_message}}是模型需要回答的用户提示。

回到获取维基百科搜索查询的问题，我们的代理将使用带有以下提示的Llama 2模型：

```py
<s>[INST] <<SYS>>
You are an assistant returning text queries to search Wikipedia articles containing relevant information about the prompt. Write the queries and nothing else.
Example: [prompt] Tell me about the heatwave in Europe in summer 2023 [query] heatwave, weather, temperatures, europe, summer 2023.
<</SYS>>

[prompt] {prompt} [/INST] [query]
```

{prompt} 在生成之前会被用户的输入替换。系统提示中提供的示例旨在利用模型的上下文学习能力。上下文学习指的是模型基于作为提示一部分提供的几个演示示例解决新任务的能力。通过这种方式，模型可以了解到我们希望它在文本 *[query]* 后提供与提供的提示相关的关键词，并用逗号分隔。后者用于作为分隔符，以区分示例中的提示和答案，并且还用于从模型输出中提取查询。它已作为输入的一部分提供，因此模型只需要生成它之后的内容。

一旦从模型输出中获得查询，就可以用来搜索维基百科，检索返回页面的元数据和文本。在文章中附带的代码中，我使用了 [wikipediapackage](https://pypi.org/project/wikipedia/)，这是一个简单的 Python 包，它封装了 [MediaWiki API](https://www.mediawiki.org/wiki/API)，用于从维基百科搜索和检索数据。

在从搜索结果中提取文本后，选择与原始用户提示最相关的页面。这将重新对齐检索到的信息与原始用户的提示，可能消除由模型生成的搜索查询引起的偏差。为此，用户的提示和搜索结果页面的摘要都会被嵌入并存储在向量数据库中。然后检索与用户提示嵌入最接近的文章。我使用了句子变换器 *all-MiniLM-L6-v2* 模型作为嵌入模型，并使用了 *langchain* 包提供的 FAISS 向量数据库集成。

在找到相关的维基百科页面后，由于将其整个文本添加到提示中可能需要大量内存（或超过模型的上下文长度令牌限制），我们的代理将找到最相关的摘录以补充提示。这是通过首先将页面的文本拆分成块，然后像之前一样，将它们嵌入到向量空间中，并检索与提示嵌入最接近的块来完成的。我再次使用了 *all-MiniLM-L6-v2* 模型来嵌入这些块，并使用 FAISS 向量数据库来存储和检索它们。

现在我们已经从文章中获得了检索到的段落，我们可以将它们与用户的提示结合起来，输入到 Llama 2 模型中以获得答案。输入的模板如下：

```py
<s>[INST] <<SYS>>
You are a helpful and honest assistant. Your answers should not include any harmful, unethical, racist, sexist, toxic, dangerous, or illegal content.
You have retrieved the following extracts from the Wikipedia page {title}:
{extracts}.
You are expected to give truthful answers based on the previous extracts. If it doesn't include relevant information for the request just say so and don't make up false information.
<</SYS>>

{prompt} [/INST]
```

在生成之前，{prompt} 被替换为用户提示，{title} 被替换为维基百科页面的标题，{extracts} 被替换为提取的段落。也可以提供几个示例，以再次利用模型的上下文学习能力，但这会显著增加提示的长度，从而增加内存需求。

现在让我们检查模型是否能够使用在训练过程中未包含的最新信息来回答问题，这些信息是从维基百科中提取的。以下示例是使用在Google Colab免费层GPU上运行的最小LLaMa 2聊天版本（拥有70亿参数）获得的。

我给模型提供了以下提示：

```py
prompt: "Tell me about the earthquake in Marrakesh"
```

模型创建了以下查询以在维基百科上搜索：

```py
"earthquake, Marrakesh, Morocco, recent earthquakes, seismic activity."
```

在提取了来自最相关页面的段落后，模型返回了以下回答：

```py
The earthquake that occurred in Marrakesh on September 8, 2023, was a
devastating event that caused widespread destruction and loss of life in
the region. According to the information provided on Wikipedia, the
earthquake had a moment magnitude of 6.8-6.9 and a maximum Mercalli
intensity of VIII (Severe), making it the strongest instrumentally recorded
earthquake in Morocco and the deadliest since the 1960 Agadir earthquake.
The earthquake's epicentre was located approximately 45 miles (73.4 km)
southwest of Marrakesh, near the town of Ighil and the Oukaïmeden ski
resort in the Atlas Mountains. The earthquake occurred as a result of
shallow oblique-thrust faulting beneath the mountain range. According to
the World Health Organization, around 300,000 people from Marrakesh and the
surrounding areas were affected by the earthquake, including 100,000
children. The earthquake resulted in at least 2,946 deaths, with most
occurring outside of Marrakesh. It is important to note that the
information provided on Wikipedia is based on available data and may not be
exhaustive or up-to-date. If you have any specific questions 
```

该代理还返回了生成答案所用页面的元数据和摘录，允许用户检查其准确性，并通过阅读原始页面深入了解细节。以下是之前回答的元数据

```py
RETRIEVED WIKIPEDIA PAGE: 
title: 2023 Marrakesh–Safi earthquake 
url: https://en.wikipedia.org/wiki/2023_Marrakesh%E2%80%93Safi_earthquake
Retrieved extracts: 
Extract_0:Earthquake aftermath in Marrakesh and Moulay Brahim  Extract_1:.
Damage was widespread, and historic landmarks in Marrakesh were destroyed.
The earthquake was also felt in Spain, Portugal, and Algeria.It is the
strongest instrumentally recorded earthquake in Morocco, the deadliest in
the country since the 1960 Agadir earthquake and the second-deadliest
earthquake of 2023 after the Turkey–Syria earthquake. The World Health
Organization estimated about 300,000 people from Marrakesh and the
surrounding areas were affected, including 100,000 children  Extract_2:On 8
September 2023 at 23:11 DST (22:11 UTC), an earthquake with a moment
magnitude of 6.8–6.9 and maximum Mercalli intensity of VIII (Severe) struck
Morocco's Marrakesh–Safi region. The earthquake's epicentre was located
73.4 km (45.6 mi) southwest of Marrakesh, near the town of Ighil and the
Oukaïmeden ski resort in the Atlas Mountains. It occurred as a result of
shallow oblique-thrust faulting beneath the mountain range. At least 2,946
deaths were reported, with most occurring outside Marrakesh
```

# 结论

在这篇文章中，我解释了如何创建一个简单的代理，它可以通过在维基百科上搜索并基于检索到的页面来回应用户的提示。尽管它很简单，但即使是最小的Llama 2 7B模型也能提供最新且准确的回答。该代理还返回了生成回答所用页面的摘录，允许用户检查模型提供信息的准确性，并通过阅读完整的原始页面深入了解细节。

维基百科是展示LLM代理寻找和使用训练数据中不存在的外部信息能力的有趣场所，但同样的方法也可以应用于其他需要外部知识的场景。例如，这适用于需要最新答案的应用、需要训练数据中不存在的特定知识的领域，或从私有文档中提取信息的情况。这种方法还突显了LLM与人类合作的潜力。模型可以快速返回有意义的回答，通过从非常大的外部知识库中搜索相关信息，而人类用户可以检查模型答案的有效性，并通过检查原始来源深入了解问题。

对本文中描述的智能体的直接改进可以通过结合来自不同页面的多个摘录来获得，以便向模型提供更多的信息。实际上，对于复杂的提示，提取多个维基百科页面的信息可能会很有用。由于上下文的增加导致内存需求的增加，可以通过实现量化技术来部分抵消这种增加，例如[GPTQ](https://arxiv.org/abs/2210.17323)。通过让模型在给出最终答案之前，能够在搜索结果和检索到的内容上进行推理，结果可能会进一步改善，例如可以遵循论文中描述的ReAct框架[ReAct: Synergizing Reasoning and Acting in Language Models](https://arxiv.org/abs/2210.03629)。这样，例如，可以构建一个模型，迭代地从不同页面中收集最相关的片段，丢弃不需要的片段，并结合来自不同主题的信息。

谢谢阅读！
