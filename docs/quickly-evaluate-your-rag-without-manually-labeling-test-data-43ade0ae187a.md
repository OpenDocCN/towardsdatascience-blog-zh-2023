# 快速评估你的 RAG，无需手动标注测试数据

> 原文：[`towardsdatascience.com/quickly-evaluate-your-rag-without-manually-labeling-test-data-43ade0ae187a`](https://towardsdatascience.com/quickly-evaluate-your-rag-without-manually-labeling-test-data-43ade0ae187a)

## 自动化评估你的检索增强生成应用程序的过程，无需人工干预

[](https://ahmedbesbes.medium.com/?source=post_page-----43ade0ae187a--------------------------------)![Ahmed Besbes](https://ahmedbesbes.medium.com/?source=post_page-----43ade0ae187a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----43ade0ae187a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----43ade0ae187a--------------------------------) [Ahmed Besbes](https://ahmedbesbes.medium.com/?source=post_page-----43ade0ae187a--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----43ade0ae187a--------------------------------) ·阅读时间 12 分钟·2023 年 12 月 21 日

--

![](img/d058a92e9619f69812d1ccfc71bcb537.png)

用户生成的图像

今天的话题是如何在不手动标注测试数据的情况下评估你的 RAG。

测量你的 RAG 的性能是你应该关注的，尤其是当你正在构建这些系统并在生产环境中提供服务时。

除了让你大致了解应用的表现外，评估你的 RAG 还提供了量化反馈，这些反馈指导实验和参数的适当选择（如 LLMs、嵌入模型、分块大小、top K 等）。

评估你的 RAG 对你的客户或利益相关者也很重要，因为他们***总是***期望有性能指标来验证你的项目。

不再卖关子，这个问题涵盖了以下内容：

1.  自动生成一个合成测试集，从你的 RAG 数据中

1.  流行的 RAG 指标概述

1.  使用 Ragas 包在合成数据集上计算 RAG 指标

*PS: 这个问题的某些部分是稍微动手的。它们包括实现数据集生成和评估 RAG 所需的编码材料。*

所有内容也会在这个* [*notebook*](https://colab.research.google.com/drive/1Qr3fUfBvJbDLZ0k6al_axlmFrArlm9-u?usp=drive_link)*中提供。*

一起来看看 🔎

# 1 — 生成合成测试集 🧪

假设你刚刚构建了一个 RAG，现在想要评估其性能。

为此，你需要一个具有以下列的评估数据集：

+   **question** (str)：用来评估 RAG 的问题

+   **ground_truths** (列表)：问题的参考（即真实）答案

+   **answer** (str)：RAG 预测的答案

+   **contexts** (列表)：RAG 用于每个问题生成答案的相关上下文列表

> ***→ 前两列表示真实数据，后两列表示 RAG 预测。***

![](img/103ca833aac503a2d0c58bce1f8c30b8.png)

作者提供的截图

要构建这样的数据集，我们首先需要生成问题和相应答案的元组。

然后，在下一步中，我们需要对这些问题运行 RAG 以进行预测。

## 👉 生成问题和真实答案（理论）

为了生成 (问题, 答案) 的元组，我们首先需要准备 RAG 数据，将其拆分成块，并嵌入到向量数据库中。

一旦拆分完成并嵌入，我们将指示一个 LLM 从 `N_c` 个主题中生成 `N_q` 个问题，最终得到 `N_q x N_c` 个问题和答案的元组。

为了从给定上下文中生成问题和答案，我们需要经过以下步骤：

1.  随机抽取一个拆分，并将其用作根上下文

1.  从向量数据库中提取 K 个相似上下文

1.  将根上下文的文本与其 K 个邻居连接起来，以构建更大的上下文

1.  使用下列提示模板中的大 `context` 和 `num_questions` 生成问题和答案

```py
"""\
Your task is to formulate exactly {num_questions} questions from given context and provide the answer to each one.

End each question with a '?' character and then in a newline write the answer to that question using only 
the context provided.
Separate each question/answer pair by "XXX"
Each question must start with "question:".
Each answer must start with "answer:".

The question must satisfy the rules given below:
1.The question should make sense to humans even when read without the given context.
2.The question should be fully answered from the given context.
3.The question should be framed from a part of context that contains important information. It can also be from tables,code,etc.
4.The answer to the question should not contain any links.
5.The question should be of moderate difficulty.
6.The question must be reasonable and must be understood and responded by humans.
7.Do no use phrases like 'provided context',etc in the question
8.Avoid framing question using word "and" that can be decomposed into more than one question.
9.The question should not contain more than 10 words, make of use of abbreviation wherever possible.

context: {context}
"""
```

然后，重复步骤 1 到 4 `N_c` 次，以每次变化上下文并生成不同的问题

我已经使用这个工作流来生成 Python 编程相关的问题和答案，以下是我得到的一些结果示例。

```py
|    | question                                           | ground_truths                                      |
|---:|:---------------------------------------------------|:---------------------------------------------------|
|  8 | What is the difference between lists and tuples in | ['Lists are mutable and cannot be used as          |
|    | Python?                                            | dictionary keys, while tuples are immutable and    |
|    |                                                    | can be used as dictionary keys if all elements are |
|    |                                                    | immutable.']                                       |
|  4 | What is the name of the Python variant optimized   | ['MicroPython and CircuitPython']                  |
|    | for microcontrollers?                              |                                                    |
| 13 | What is the name of the programming language that  | ['ABC programming language']                       |
|    | Python was designed to replace?                    |                                                    |
| 17 | How often do bugfix releases occur?                | ['Bugfix releases occur about every 3 months.']    |
|  3 | What is the significance of Python's release       | ['Python 2.0 was released in 2000, while Python    |
|    | history?                                           | 3.0, a major revision with limited backward        |
|    |                                                    | compatibility, was released in 2008.']             |
```

## 👉 现在是编码部分 💻

我们首先从构建一个包含 RAG 使用数据的 vectorstore 开始。

我们可以从维基百科加载这些（但如果你有任何有趣的 Python 编程 PDF，你也可以加载）

```py
from langchain.document_loaders import WikipediaLoader
topic = "python programming"
wikipedia_loader = WikipediaLoader(
    query=topic,
    load_max_docs=1,
    doc_content_chars_max=100000,
)
docs = wikipedia_loader.load()
doc = docs[0]
```

加载数据后，我们将其拆分成块。

```py
from langchain.text_splitter import RecursiveCharacterTextSplitter

CHUNK_SIZE = 512
CHUNK_OVERLAP = 128
splitter = RecursiveCharacterTextSplitter(
    chunk_size=CHUNK_SIZE,
    chunk_overlap=CHUNK_OVERLAP,
    separators=[". "],
)
splits = splitter.split_documents([doc])
```

然后，我们在 Pinecone 中创建一个索引：

```py
import pinecone

pinecone.init(
    api_key=os.environ.get("PINECONE_API_KEY"),
    environment=os.environ.get("PINECONE_ENV"),
)
index_name = topic.replace(" ", "-")
pinecone.init(
    api_key=os.environ.get("PINECONE_API_KEY"),
    environment=os.environ.get("PINECONE_ENV"),
)
if index_name in pinecone.list_indexes():
    pinecone.delete_index(index_name)
pinecone.create_index(index_name, dimension=768)
```

并使用 LangChain 包装器来对拆分的嵌入进行索引。

```py
from langchain.vectorstores import Pinecone

docsearch = Pinecone.from_documents(
    splits,
    embedding_model,
    index_name=index_name,
)
```

现在进入有趣的部分：生成合成数据集。

为此，我们使用 LLM、文档拆分、嵌入模型和 Pinecone 索引名称初始化一个 `TestsetGenerator` 类的对象。

```py
from langchain.embeddings import VertexAIEmbeddings
from langchain.llms import VertexAI
from testset_generator import TestsetGenerator

generator_llm = VertexAI(
    location="europe-west3",
    max_output_tokens=256,
    max_retries=20,
)
embedding_model = VertexAIEmbeddings()
testset_generator = TestsetGenerator(
    generator_llm=generator_llm,
    documents=splits,
    embedding_model=embedding_model,
    index_name=index_name,
    key="text",
)
```

然后，我们通过传递两个参数来调用 `generate` 方法：

```py
synthetic_dataset = testset_generator.generate(
    num_contexts=10,
    num_questions_per_context=2,
)
```

这会生成以下数据框：

```py
|    | question                                           | ground_truths                                      |
|---:|:---------------------------------------------------|:---------------------------------------------------|
|  8 | What is the difference between lists and tuples in | ['Lists are mutable and cannot be used as          |
|    | Python?                                            | dictionary keys, while tuples are immutable and    |
|    |                                                    | can be used as dictionary keys if all elements are |
|    |                                                    | immutable.']                                       |
|  4 | What is the name of the Python variant optimized   | ['MicroPython and CircuitPython']                  |
|    | for microcontrollers?                              |                                                    |
| 13 | What is the name of the programming language that  | ['ABC programming language']                       |
|    | Python was designed to replace?                    |                                                    |
| 17 | How often do bugfix releases occur?                | ['Bugfix releases occur about every 3 months.']    |
|  3 | What is the significance of Python's release       | ['Python 2.0 was released in 2000, while Python    |
|    | history?                                           | 3.0, a major revision with limited backward        |
|    |                                                    | compatibility, was released in 2008.']             |
```

很简单，对吧？

**如果你对实现细节感兴趣，可以在** [**notebook**](https://colab.research.google.com/drive/1Qr3fUfBvJbDLZ0k6al_axlmFrArlm9-u?usp=drive_link)** 中找到它们。**

这只是其中的一半。现在，我们需要使用 RAG 来预测每个问题的答案，并提供用于生成响应的上下文列表。

RAG 定义在 RAG 类中，所以我们首先需要初始化它：

```py
from rag import RAG

rag = RAG(
    index_name,
    "text-bison",
    embedding_model,
    "text",
)
```

然后，我们通过对每个问题调用 `predict` 方法来迭代合成数据集，并收集预测结果。

```py
rag_answers = []
contexts = []

for i, row in synthetic_dataset.iterrows():
    question = row["question"]
    prediction = rag.predict(question)
    rag_answer = prediction["answer"]
    rag_answers.append(rag_answer)
    source_documents = prediction["source_documents"]
    contexts.append([s.page_content for s in source_documents])
synthetic_dataset_rag = synthetic_dataset.copy()
synthetic_dataset_rag["answer"] = rag_answers
synthetic_dataset_rag["contexts"] = contexts
```

下面是最终结果的样子：

```py
|    | question                                                                    | ground_truths               | answer                                                                                                     | contexts                                           |
|---:|:----------------------------------------------------------------------------|:----------------------------|:-----------------------------------------------------------------------------------------------------------|:---------------------------------------------------|
|  7 | What are the two types of classes that Python supported before version 3.0? | ['old-style and new-style'] | Before version 3.0, Python had two kinds of classes (both using the same syntax): old-style and new-style. | ['. New instances of classes are constructed by    |
|    |                                                                             |                             |                                                                                                            | calling the class (for example, SpamClass() or     |
|    |                                                                             |                             |                                                                                                            | EggsClass()), and the classes are instances of the |
|    |                                                                             |                             |                                                                                                            | metaclass type (itself an instance of itself),     |
|    |                                                                             |                             |                                                                                                            | allowing metaprogramming and reflection.\nBefore   |
|    |                                                                             |                             |                                                                                                            | version 3.0, Python had two kinds of classes (both |
|    |                                                                             |                             |                                                                                                            | using the same syntax): old-style and new-style,   |
|    |                                                                             |                             |                                                                                                            | current Python versions only support the semantics |
|    |                                                                             |                             |                                                                                                            | new style.\nPython supports optio     ..........   |
```

🏆 恭喜你完成到这一步，现在你准备好评估你的 RAG 了。

# 2 — 流行的 RAG 指标 📊

在跳入代码之前，让我们介绍一下用于评估 RAG 的 **四** 个基本指标。

每个指标考察了不同的方面。因此，在评估你的应用时，考虑多个指标以获得全面的视角至关重要。

**1 — 答案相关性**：

答案相关性指标旨在**评估生成答案与提供的提示的相关性**。缺乏完整性或包含冗余信息的答案会获得较低的分数。该指标利用问题和答案，生成 0 到 1 之间的值。得分越高，相关性越好。

**示例**

❓*问题：健康饮食的关键特征是什么？*

⬇️ *低相关性答案*：健康饮食对整体健康非常重要。

⬆️ *高相关性答案*：健康饮食应包括多种水果、蔬菜、全谷物、瘦蛋白和乳制品，提供必需的营养素以促进最佳健康。

**2 — 准确性**

该指标评估**生成答案在提供的上下文中的事实一致性**。计算涉及答案和检索到的上下文，答案的评分范围在 0 到 1 之间，得分越高表示一致性越好。

为了使答案被认为是准确的，回答中提出的所有主张必须可以从给定的上下文中推断出来。

**示例：**

❓*问题：玛丽·居里的主要成就是什么？*

📑 *上下文*：玛丽·居里（1867–1934）是开创性的物理学家和化学家，第一位获得诺贝尔奖的女性，也是唯一一位在两个不同领域获得诺贝尔奖的女性。

⬆️ *高准确性答案*：玛丽·居里获得了物理学和化学两项诺贝尔奖，使她成为首位实现这一成就的女性。

⬇️ *低准确性答案*：玛丽·居里仅获得了物理学方面的诺贝尔奖。

**3 — 上下文精确度**

上下文精确度是一个指标，评估`上下文`中所有真实相关条目是否都被排在更高的位置。理想情况下，所有相关片段应出现在排名靠前的位置。该指标使用`问题`和`上下文`进行计算，得分范围在 0 到 1 之间，得分越高表示精确度越好。

**4 — 答案准确性**

该指标衡量生成答案与真实答案之间的准确性。这项评估利用真实答案和生成答案，并在 0 到 1 的范围内进行评分。得分越高，表明生成的答案与真实答案之间的对齐越准确，表示更高的正确性。

**示例：**

🟢 *真实答案：埃菲尔铁塔于 1889 年在法国巴黎竣工。*

⬆️ *高准确性答案*：埃菲尔铁塔的建设于 1889 年在法国巴黎完成。

⬇️ *低准确性答案*：埃菲尔铁塔于 1889 年竣工，位于英国伦敦。

# 3 — 使用 RAGAS 评估 RAG 📏

为了评估 RAG 并计算四个指标，我们可以使用[Ragas](https://github.com/explodinggradients/ragas)。

Ragas（用于 Rag 评估）是一个帮助你评估检索增强生成（RAG）管道的框架。

它还提供了一些[指标](https://docs.ragas.io/en/latest/concepts/metrics/index.html)和实用的[函数](https://docs.ragas.io/en/latest/concepts/testset_generation.html)来生成合成数据集。

要在我们的数据集上运行 Ragas，你首先需要导入指标并将合成数据的数据框转换为数据集对象。

```py
from datasets import Dataset
from ragas.llms import LangchainLLM
from ragas.metrics import (
    answer_correctness,
    answer_relevancy,
    answer_similarity,
    context_precision,
    context_recall,
    context_relevancy,
    faithfulness,
)

synthetic_ds_rag = Dataset.from_pandas(synthetic_dataset_rag)
```

然后，我们需要配置 Ragas 以使用 VertexAI LLMs 和嵌入。

这一步很重要，因为 Ragas 默认配置为使用 OpenAI。

```py
metrics = [
    answer_relevancy,
    context_precision,
    faithfulness,
    answer_correctness,
    answer_similarity,
]

for m in metrics:
    m.__setattr__("llm", ragas_vertexai_llm)
    if hasattr(m, "embeddings"):
        m.__setattr__("embeddings", vertexai_embeddings)
answer_correctness.faithfulness = faithfulness
answer_correctness.answer_similarity = answer_similarity
```

最后，我们在合成数据集上调用`evaluate`函数，并指定我们想要计算的指标：

```py
from ragas import evaluate

results_rag = evaluate(
    synthetic_ds_rag,
    metrics=[
        answer_relevancy,
        context_precision,
        faithfulness,
        answer_correctness,
    ],
)
```

评估完成后，你可以直接打印结果。

```py
{
  'answer_correctness': 0.86875,
  'answer_relevancy': 0.9709101875947284,
  'context_precision': 0.8541666666143055,
  'faithfulness': 0.9375
}
```

或者你可以将其转换为数据框，以检查每个问题的这些指标。

```py
|    | question                                                                          | contexts   | answer                                            | ground_truths                                     |   answer_relevancy |   context_precision |   faithfulness |   answer_correctness |
|---:|:----------------------------------------------------------------------------------|:-----------|:--------------------------------------------------|:--------------------------------------------------|-------------------:|--------------------:|---------------:|---------------------:|
|  1 | What is the difference between lists and tuples in Python?                        | ...        | Lists are mutable, while tuples are immutable.    | ['Lists and tuples are both ordered sequences of  |           0.987162 |            1        |              1 |             0.75     |
|    |                                                                                   |            | This means that the elements of a list can be     | elements in Python. However, lists are mutable,   |                    |                     |                |                      |
|    |                                                                                   |            | changed, while the elements of a tuple cannot.    | meaning their elements can be changed, while      |                    |                     |                |                      |
|    |                                                                                   |            | Additionally, tuples can be used as keys in       | tuples are immutable, meaning their elements      |                    |                     |                |                      |
|    |                                                                                   |            | dictionaries, while lists cannot.                 | cannot be changed.']                              |                    |                     |                |                      |
|  5 | What is the name of the Python runtime that uses just-in-time compilation?        | ...        | The name of the Python runtime that uses just-in- | ['Pyston']                                        |           1        |            1        |              1 |             1        |
|    |                                                                                   |            | time compilation is Pyston.                       |                                                   |                    |                     |                |                      |
| 13 | What is the name of the programming language that Python was designed to replace? | ...        | The programming language that Python was designed | ['ABC programming language']                      |           0.982582 |            0.416667 |              1 |             0.5      |
|    |                                                                                   |            | to replace is called ABC.                         |                                                   |                    |                     |                |                      |
|  2 | What is Python's approach to type checking?                                       | ...        | Python uses a combination of dynamic typing and   | ["Python follows dynamic typing, where type       |           0.890692 |            0.916667 |              1 |             0.666667 |
|    |                                                                                   |            | duck typing, with optional static type checking   | constraints are not checked at compile time but   |                    |                     |                |                      |
|    |                                                                                   |            | available through the use of type annotations and | may result in operational failures if an object's |                    |                     |                |                      |
|    |                                                                                   |            | the mypy type checker.                            | type is unsuitable."]                             |                    |                     |                |                      |
| 12 | Which programming language has been the most popular since 2003?                  | ...        | According to the TIOBE Programming Community      | ['Python']                                        |           0.899585 |            0.5      |              1 |             0.75     |
|    |                                                                                   |            | Index, Python has consistently ranked in the top  |                                                   |                    |                     |                |                      |
|    |                                                                                   |            | ten most popular programming languages since 2003 |                                                   |                    |                     |                |                      |
|    |                                                                                   |            | and as of December 2022, it was the most popular  |                                                   |                    |                     |                |                      |
|    |                                                                                   |            | language.                                         |                                                   |                    |                     |                |                      |
```

# 结论

生成一个合成数据集以评估你的 RAG 是一个好的开始，特别是当你没有访问标注数据时。

然而，这个解决方案也有其问题。

一些生成的答案：

+   可能缺乏多样性

+   是冗余的

+   只是对原始文本的简单改述，需要更多的复杂性来反映需要推理的实际问题

+   可能过于笼统（尤其是在非常技术性的领域）

为了应对这些问题，你可以调整和调整你的提示，过滤无关的问题，创建特定主题的合成问题，并使用 Ragas 进行数据集生成。
