# Self-RAG 如何革新工业 LLMs

> 原文：[`towardsdatascience.com/how-self-rag-could-revolutionize-industrial-llms-b33d9f810264`](https://towardsdatascience.com/how-self-rag-could-revolutionize-industrial-llms-b33d9f810264)

## 说实话——普通的 RAG 真的很笨。没有保证返回的响应是相关的。了解 Self-RAG 如何显著帮助

[](https://skanda-vivek.medium.com/?source=post_page-----b33d9f810264--------------------------------)![Skanda Vivek](https://skanda-vivek.medium.com/?source=post_page-----b33d9f810264--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b33d9f810264--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b33d9f810264--------------------------------) [Skanda Vivek](https://skanda-vivek.medium.com/?source=post_page-----b33d9f810264--------------------------------)

·发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b33d9f810264--------------------------------) ·7 分钟阅读·2023 年 11 月 14 日

--

![](img/69621ace143918aee012edf5e1d62323.png)

Self-RAG 演示 | Skanda Vivek

大型语言模型（LLMs）已经准备好革新各个行业。以金融行业为例，LLMs 可以用来分析大量文档，并在短时间内以低于分析师完成同样任务的成本找到趋势。但问题在于——你得到的答案很多时候只是部分的和不完整的。举个例子，假设你有一份包含公司 X 在过去 15 年的年收入的文档，但这些信息分布在不同的部分。在下面所示的标准检索增强生成（RAG）架构中，你通常会检索前 k 个文档，或选择固定上下文长度内的文档。

![](img/4041cfd00e99aed333dcc5090fda5094.png)

RAG 原型 | Skanda Vivek

然而，这可能会有几个问题。一个问题是前 k 个文档不包含所有的答案——例如可能只对应于过去的 5 年或 10 年。另一个问题是计算文档片段与提示之间的相似性并不总是能得到相关的上下文。在这种情况下，你可能会得到错误的答案。

一个实际的问题是你已经开发了一个普通的 RAG 应用程序，它在你测试的简单案例中工作良好——但当你将这个原型展示给利益相关者并且他们提出一些超出常规的问题时，它就会失败。

这就是 Self-RAG 大显身手的地方！作者开发了一种巧妙的方法，让经过微调的语言模型（Llama2–7B 和 13B）输出附加在 LM 生成的特殊标记 [Retrieval]、[No Retrieval]、[Relevant]、[Irrelevant]、[No support / Contradictory]、[Partially supported]、[Utility] 等，用于决定上下文是否相关/无关，上下文中的 LM 生成文本是否得到支持，以及生成的实用性。

![](img/92869f6ffd2ad94ea5bff625d4cf8faa.png)

[Self-RAG](https://selfrag.github.io/)

## 训练 Self-RAG

Self-RAG 通过**2 步层次化过程**进行了训练。在第一步中，训练了一个简单的语言模型（LM），用于对生成的输出（无论是仅有提示还是提示+RAG 增强输出）进行分类，并在末尾附加相关的特殊标记。这个“评判模型”由 GPT-4 注释进行训练。具体来说，GPT-4 通过特定类型的指令进行提示（“给定指令，判断从网络上查找一些外部文档是否有助于生成更好的回应。”）

在第二步中，生成模型使用标准的下一个标记预测目标，学习生成延续内容，以及检索/评估生成内容的特殊标记。与其他微调或 RLHF 方法不同，这些方法可能会影响模型输出并使未来生成的内容产生偏差，通过这种简单的方法，模型仅被训练为在适当的时候生成特殊标记，而不会改变基础 LM！这真是太棒了！

## 评估 Self-RAG

作者对公共健康事实验证、多项选择推理、问答等进行了大量评估。共有 3 种任务。封闭集任务包括事实验证和多项选择推理，准确率被用作评估指标。短文本生成任务包括开放领域的问答数据集。作者评估了模型生成中是否包含黄金答案，而不是严格要求精确匹配。

长文本生成包括传记生成和长篇问答。对于这些任务的评估，作者使用 FactScore 来评估传记——基本上是生成的各种信息及其事实正确性的度量。对于长篇问答，使用了引用精度和召回率。

![](img/2fd0180f57a97da9452323f14bd7d8af.png)

[Self-RAG Eval](https://selfrag.github.io/)

Self-RAG 在非专有模型中表现最佳，在大多数情况下，13B 参数的模型优于 7B 模型。在某些情况下，它甚至优于 ChatGPT。

## 推理

对于推理，[self-RAG 存储库](https://github.com/AkariAsai/self-rag) 建议使用 [vllm](https://github.com/vllm-project/vllm)——一个用于 LLM 推理的库。

在安装 vllm 后，你可以按如下方式加载库并进行查询：

```py
from vllm import LLM, SamplingParams
model = LLM("selfrag/selfrag_llama2_7b", download_dir="/gscratch/h2lab/akari/model_cache", dtype="half")
sampling_params = SamplingParams(temperature=0.0, top_p=1.0, max_tokens=100, skip_special_tokens=False)
def format_prompt(input, paragraph=None):
prompt = "### Instruction:\n{0}\n\n### Response:\n".format(input)
if paragraph is not None:
prompt += "[Retrieval]<paragraph>{0}</paragraph>".format(paragraph)
return prompt
query_1 = "Leave odd one out: twitter, instagram, whatsapp."
query_2 = "Can you tell me the difference between llamas and alpacas?"
queries = [query_1, query_2]
# for a query that doesn't require retrieval
preds = model.generate([format_prompt(query) for query in queries], sampling_params)
for pred in preds:
print("Model prediction: {0}".format(pred.outputs[0].text))
```

对于需要检索的查询，你可以按下面的示例提供必要的信息作为字符串。

```py
paragraph="""Llamas range from 200 to 350 lbs., while alpacas weigh in at 100 to 175 lbs."""

def format_prompt_p(input, paragraph=paragraph):
  prompt = "### Instruction:\n{0}\n\n### Response:\n".format(input)
  if paragraph is not None:
    prompt += "[Retrieval]<paragraph>{0}</paragraph>".format(paragraph)
  return prompt

query_1 = "Leave odd one out: twitter, instagram, whatsapp."
query_2 = "Can you tell me the differences between llamas and alpacas?"
queries = [query_1, query_2]

# for a query that doesn't require retrieval
preds = model.generate([format_prompt_p(query) for query in queries], sampling_params)
for pred in preds:
  print("Model prediction: {0}".format(pred.outputs[0].text))
```

```py
[Irrelevant]Whatsapp is the odd one out.
[No Retrieval]Twitter and Instagram are both social media platforms, 
while Whatsapp is a messaging app.[Utility:5]

[Relevant]Llamas are larger than alpacas, with males weighing up to 350 pounds.
[Partially supported][Utility:5]
```

在上述示例中，对于第一个查询（与社交媒体平台相关），段落上下文是不相关的，如检索开始时的 [Irrelevant] 令牌所示。然而，外部上下文与第二个查询（与美洲驼和羊驼相关）相关。正如你所见，它在生成的上下文中包含了这些信息，并用 [Relevant] 令牌标记。

但在下面的示例中，上下文 “I like Avocado.” 与提示无关。如下面所示，模型预测在两个查询中都开始为 [Irrelevant]，并仅使用内部信息来回答提示。

```py
paragraph="""I like Avocado."""
def format_prompt_p(input, paragraph=paragraph):
  prompt = "### Instruction:\n{0}\n\n### Response:\n".format(input)
  if paragraph is not None:
    prompt += "[Retrieval]<paragraph>{0}</paragraph>".format(paragraph)
  return prompt

query_1 = "Leave odd one out: twitter, instagram, whatsapp."
query_2 = "Can you tell me the differences between llamas and alpacas?"
queries = [query_1, query_2]

# for a query that doesn't require retrieval
preds = model.generate([format_prompt_p(query) for query in queries], sampling_params)
for pred in preds:
  print("Model prediction: {0}".format(pred.outputs[0].text))
```

```py
Model prediction: [Irrelevant]Twitter is the odd one out.[Utility:5]

[Irrelevant]Sure![Continue to Use Evidence]
Alpacas are a much smaller than llamas.
They are also bred specifically for their fiber.[Utility:5]
```

## 重点总结

Self-RAG 相比于普通 LLM 具有几个优势。

1.  自适应段落检索：通过这一点，LLM 可以持续检索上下文直到找到所有相关上下文（当然是在上下文窗口内）。

1.  更相关的检索：很多时候，嵌入模型在检索相关上下文方面表现不佳。Self-RAG 通过相关/不相关特殊令牌潜在地解决了这个问题。

1.  超越其他类似模型：Self-RAG 超越了其他类似模型，并且在许多任务中意外地超过了 ChatGPT。对比 ChatGPT 未经过训练的数据——即更多的专有工业数据，将会非常有趣。

1.  不改变基础 LLM：对我来说，这是一个巨大的卖点——因为我们知道微调和 RLHF 很容易导致模型偏见。Self-RAG 似乎通过添加特殊令牌来解决这个问题，并保持文本生成不变。

不过，处理固定上下文长度仍有改进的空间。这可能通过在 Self-RAG 中添加一个总结组件来实现。实际上，之前已有一些相关的工作（参见：[通过压缩和选择性增强改进检索增强型语言模型](https://arxiv.org/abs/2310.04408)）。另一个令人兴奋的方向是最近 OpenAI 发布的增加上下文窗口长度——GPT-4 128k 上下文窗口更新。然而，正如在 [论坛](https://www.reddit.com/r/OpenAI/comments/17pap5u/gpt4_turbo_128k_context_but_only_4k_output/?rdt=50609) 中提到的，这个上下文窗口表示输入长度，而输出限制仍然是 4k 令牌。

RAG 代表了工业界将 LLM 融入其数据以产生实际业务影响的最激动人心的方式之一。然而，对于语言模型的 RAG 特定调优还不多。我对这个领域未来的改进感到兴奋。

推断代码在这个 GitHub 仓库中：

[## GitHub - skandavivek/self-RAG: 关于 Self-RAG 的教程。

### 关于 Self-RAG 的教程。通过在 GitHub 上创建一个账户来为 skandavivek/self-RAG 的开发做出贡献。

[github.com](https://github.com/skandavivek/self-RAG/tree/main?source=post_page-----b33d9f810264--------------------------------)

*如果你喜欢这篇文章，请关注我——我撰写有关生成式 AI 在实际应用中的内容，以及更广泛的数据与社会之间的交集。*

*欢迎在* [*LinkedIn*](https://www.linkedin.com/in/skanda-vivek-01619311b/)*上联系我！*
