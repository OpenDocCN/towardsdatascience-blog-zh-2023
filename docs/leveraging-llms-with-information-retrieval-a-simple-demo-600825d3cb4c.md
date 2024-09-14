# 利用信息检索增强 LLMs：一个简单的演示

> 原文：[`towardsdatascience.com/leveraging-llms-with-information-retrieval-a-simple-demo-600825d3cb4c`](https://towardsdatascience.com/leveraging-llms-with-information-retrieval-a-simple-demo-600825d3cb4c)

## 一个将问答 LLM 与检索组件集成的演示

[](https://medium.com/@vuphuongthao9611?source=post_page-----600825d3cb4c--------------------------------)![Thao Vu](https://medium.com/@vuphuongthao9611?source=post_page-----600825d3cb4c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----600825d3cb4c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----600825d3cb4c--------------------------------) [Thao Vu](https://medium.com/@vuphuongthao9611?source=post_page-----600825d3cb4c--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----600825d3cb4c--------------------------------) ·阅读时间 7 分钟·2023 年 8 月 14 日

--

![](img/309014edda2c597a2486ed5abab55028.png)

图像由作者使用 Stable Diffusion 生成

大型语言模型（LLM）可以存储大量的事实数据，但其能力受到参数数量的限制。此外，频繁更新 LLM 是昂贵的，而旧的训练数据可能使 LLM 产生过时的回答。

为了解决上述问题，我们可以使用外部工具来增强 LLM。在本文中，我将分享如何将 LLM 与检索组件集成以提高性能。

# 检索增强（RA）

检索组件可以为 LLM 提供更为最新和准确的知识。给定输入***x***，我们希望预测输出***p(y|x)***。从外部数据源***R***中，我们检索与***x***相关的上下文列表***z***=(***z_1, z_2,..,z_n)***。我们可以将**x**和***z***结合在一起，充分利用***z***的丰富信息来预测***p(y|x,z)***。此外，保持**R**的更新也要便宜得多。

![](img/3fe2cc3cad76ee8963d0c05e7be2de8f.png)

检索增强管道（图像来源于作者）

# 使用维基百科数据和 ChatGPT 的 QA 演示

在这个演示中，对于给定的问题，我们执行以下步骤：

+   检索与问题相关的维基百科文档。

+   将问题和维基百科提供给 ChatGPT。

我们希望比较并查看额外的上下文如何影响 ChatGPT 的回答。

## 数据集

对于维基百科数据集，我们可以从[这里](https://huggingface.co/datasets/wikipedia)提取。我使用了“20220301.simple”子集，其中包含超过 20 万份文档。由于上下文长度的限制，我只使用了标题和摘要部分。对于每个文档，我还添加了一个文档 ID，以便后续检索使用。因此，数据示例如下所示。

```py
{"title": "April", "doc": "April is the fourth month of the year in the Julian and Gregorian calendars, and comes between March and May. It is one of four months to have 30 days.", "id": 0}
{"title": "August", "doc": "August (Aug.) is the eighth month of the year in the Gregorian calendar, coming between July and September. It has 31 days. It is named after the Roman emperor Augustus Caesar.", "id": 1}
```

我们结合标题和摘要段落，并准备它们进行编码。

```py
with open(input_file, "r") as f:
    for line in f.readlines():
        try: 
            example = json.loads(line.strip("\n"))
            self.id2text[example["id"]] = example.get("title", "") + self.tokenizer.sep_token + example.get("doc", "")
        except Exception as _:
            continue
        if len(self.id2text) >= self.max_index_count: 
            break
```

## 编码

接下来，我们需要一个可靠的嵌入模型来构建我们的检索索引。在这个演示中，我使用了预训练的[multilingual-e5-large](https://huggingface.co/intfloat/multilingual-e5-large)，维度为 1024 来编码文档。为了更快的索引和存储效率，你可以选择其他小维度的嵌入模型。

我最初选择的嵌入模型是预训练的[ALBERT](https://huggingface.co/docs/transformers/model_doc/albert)，但结果的质量较差。在进入下一步之前，你应该进行一些测试案例，以确保你的索引工作合理。为了选择一个好的检索嵌入，你可以查看[这个排行榜](https://huggingface.co/spaces/mteb/leaderboard)。

```py
 @torch.no_grad()
  def _get_batch_embedding(self, x: List[str]):
      '''
      Get embedding of a single batch
      Parameters
      ----------
      x: List of text to encode 
      '''
      batch_dict = self.tokenizer(x, max_length=512, padding=True, truncation=True, return_tensors='pt')
      outputs = self.model(**batch_dict)
      embeddings = average_pool(outputs.last_hidden_state, batch_dict['attention_mask'])
      # normalize embeddings
      return F.normalize(embeddings, p=2, dim=1)

  def _get_all_embeddings(self):
      '''
      Get embedding of all data points
      '''
      data_loader = DataLoader(list(self.id2text.values()), batch_size=8)
      embeddings = []
      for bs in tqdm(data_loader):
          embeddings += self._get_batch_embedding(bs)
      embeddings = [e.tolist() for e in embeddings]
      return embeddings
```

## ANN 索引

我们已经准备好了文档的嵌入和 ID 列表。下一步是将它们很好地索引以进行检索。我使用[HNSW](https://arxiv.org/pdf/1603.09320.pdf)索引并使用余弦距离度量。

```py
self.index = hnswlib.Index(space = 'cosine', dim=self.dim)
self.index.init_index(max_elements =self.max_index_count, ef_construction = 200, M = 16)
self.index.add_items(embeddings, ids)
self.index.set_ef(50)
print(f"Finish building ann index, took {time()-start:.2f}s")
self.index.save_index(self.index_file) # so we don't need to do everything once again
```

给定一个问题，我们可以首先到检索索引中查找一些相关信息。为了避免错误的上下文，你可以在这里设置一个距离阈值。这样，只有相关的文档才会被使用：

```py
 def get_nn(self, text: List[str], topk:int=1):
      embeddings = self._get_batch_embedding(text)
      labels, distances = self.index.knn_query(embeddings.detach().numpy(), k=topk)
      # map id back to wiki passage
      nb_texts = [self.map_id_to_text(label) for label in labels]
      if self.debug:
          for i in range(len(text)):
              print(f"Query={text[i]}, neighbor_id={labels[i]}, neighbor={nb_texts[i]}, distances={distances[i]}")
      return nb_texts, labels, distances
```

## ChatGPT API

现在我们的检索管道已经准备好了！下一步，让我们准备一个提示来问 ChatGPT。我们准备了以下两种提示格式，一种只有问题，另一种同时包含问题和相关的维基百科文本。

这里的**‘question’**占位符是我们希望问 ChatGPT 的目标问题，而**‘info’**是从我们的 HNSW 索引中检索到的维基百科文档。

```py
NO_WIKI_PROMPT_TEMPLATE = """
Answer the following question:

Question: ```{question}```py

Your response:
"""

HAVE_WIKI_PROMPT_TEMPLATE = """
You will be provided with the following information:
1\. A question delimited with triple backticks.
2\. Addition information that is related to the question.

Perform the following tasks:
1\. Understand the provided information.
2\. Use the provided information and answer the question.

Question: ```{question}```py
Addition information: ```{info}```py

Your response:
"""
```

填充占位符与问题和相应的维基百科文档后，我们可以向[openai ChatCompletion API](https://platform.openai.com/docs/guides/gpt/chat-completions-api)发送请求。

```py
def get_chat_completion(
    messages: dict,
    model: str = "gpt-3.5-turbo",
    max_retries: int = 3,
    debug: bool = False
):
    '''
    Gets a chat completion from the OpenAI API.

    Parameters
    ----------
    messages : dict
        input messages to use, e.g: {"user", "what is the meaning of BERT"}
    model : str, optional
        The OPEN AI model to use. Here we set default value to "gpt-3.5-turbo".
    max_retries : int, optional
        The maximum number of retries to use. Defaults to 3.
    debug: bool
        If we want to debug or not
    '''
    model_dict = {"model": model}
    error_msg = None
    error_type = None
    if debug:
        logging.warning(f"Sending chat with message={messages}, model_dict={model_dict}...")
    for _ in range(max_retries):
        try:
          completion = openai.ChatCompletion.create(
              temperature=0.0, messages=messages, **model_dict
          )
          return completion
        except Exception as e:
          error_msg = str(e)
          error_type = type(e).__name__
          sleep(3)
    print(
            f"Could not obtain the completion after {max_retries} retries: `{error_type} ::"
            f" {error_msg}`" 
```

我尝试了发送一些带有和不带有维基百科文档的请求，并观察了差异。

## 示例 1

> **问题**：生活成本包括哪些内容？
> 
> **ChatGPT**：生活成本通常包括如住房、食物、交通、医疗保健、公用事业及维持特定生活水平所需的其他基本必需品等费用。它还可能包括税收、保险和其他杂项费用。
> 
> **维基百科信息**：[[‘生活成本</s>生活成本是指在某个地方生活所需的金额。它包括食物、住房等。’]]
> 
> **ChatGPT + 维基百科信息**：生活成本包括在某个地方生活所需的金额，其中涵盖了食物、住房和其他必要开支等费用。

## 示例 2

> **问题**：如何制作奶酪？
> 
> **ChatGPT**：奶酪可以通过用酸或凝乳酶使牛奶凝固，分离凝乳和乳清，然后压制和陈化凝乳以发展风味和质地来制作。具体的过程和使用的配料可以根据制作的奶酪类型而有所不同。
> 
> **维基百科信息：** [[‘奶酪</s>奶酪是一种由牛奶制成的乳制品。有很多种奶酪，比如切达奶酪、瑞士奶酪和普罗沃洛奶酪。‘]]
> 
> **ChatGPT + 维基百科信息：** 要制作奶酪，你需要以牛奶为主要成分。奶酪是一种由牛奶制成的乳制品。有很多种奶酪，比如切达奶酪、瑞士奶酪和普罗沃洛奶酪。

## 示例 3

> **问题：** 你能解释一下 seq2seq 模型吗？
> 
> **ChatGPT：** seq2seq（序列到序列）模型是一种神经网络架构，用于生成一系列输出值的任务，该任务基于一系列输入值。它由两个主要组件组成：编码器和解码器。编码器接收输入序列，并生成一个固定长度的向量表示，然后将其传递给解码器。解码器使用这个向量来逐个生成输出序列。该模型常用于自然语言处理任务，如机器翻译、文本摘要和语音识别。
> 
> **维基百科信息：** [[‘可计算性理论</s>可计算性理论是计算机科学的一部分。科学家们想知道什么是可以计算的，什么是不能计算的。‘]]
> 
> **ChatGPT + 维基百科信息：** seq2seq 模型，或称为序列到序列模型，是一种神经网络架构，用于自然语言处理任务，如机器翻译、文本摘要和对话建模。它由两个递归神经网络（RNNs）组成，一个编码器和一个解码器，它们共同作用将输入序列转换为输出序列。编码器处理输入序列并生成一个固定长度的向量表示，然后该向量被输入到解码器中以生成输出序列。该模型在许多 NLP 任务中表现出色，已成为语言生成的热门选择。提供的关于可计算性理论的额外信息与有关 seq2seq 模型的问题不直接相关。

初看起来，我们可以看到 ANN 索引效果很好，但我们的数据集覆盖面相当小。一种更好的方法是通过 Google 搜索结果扩展检索。你还可以在对特定领域进行 QA 时，用相关文档更新你的检索索引。

在示例 1 和 2 中，当维基百科页面似乎相关时，ChatGPT 的回答似乎在很大程度上依赖于额外的信息。但模型可能无法区分这些额外的信息是否真的有帮助（示例 2）。为了避免这种情况，你可以在提示中添加一些示例，让模型了解何时使用额外信息，何时不使用。

另一种情况是示例 3，其中维基百科文本完全不相关。幸运的是，答案似乎不受额外上下文的影响。

你可以在 [这里](https://github.com/thao9611/chatgpt_and_retrieval) 找到代码。希望你喜欢阅读 :-)
