# 基于LLM+RAG的问题回答

> 原文：[https://towardsdatascience.com/llm-rag-based-question-answering-6a405c8ad38a?source=collection_archive---------0-----------------------#2023-12-25](https://towardsdatascience.com/llm-rag-based-question-answering-6a405c8ad38a?source=collection_archive---------0-----------------------#2023-12-25)

## 如何在Kaggle上表现不佳，并从中学习RAG+LLM

[](https://teemukanstren.medium.com/?source=post_page-----6a405c8ad38a--------------------------------)[![Teemu Kanstrén](../Images/8ad278d60d1fa3f794fccb4c61d607ce.png)](https://teemukanstren.medium.com/?source=post_page-----6a405c8ad38a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6a405c8ad38a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6a405c8ad38a--------------------------------) [Teemu Kanstrén](https://teemukanstren.medium.com/?source=post_page-----6a405c8ad38a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9fc0679190dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fllm-rag-based-question-answering-6a405c8ad38a&user=Teemu+Kanstr%C3%A9n&userId=9fc0679190dc&source=post_page-9fc0679190dc----6a405c8ad38a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6a405c8ad38a--------------------------------) ·23分钟阅读·2023年12月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6a405c8ad38a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fllm-rag-based-question-answering-6a405c8ad38a&user=Teemu+Kanstr%C3%A9n&userId=9fc0679190dc&source=-----6a405c8ad38a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6a405c8ad38a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fllm-rag-based-question-answering-6a405c8ad38a&source=-----6a405c8ad38a---------------------bookmark_footer-----------)![](../Images/f497014da435709dae04a493366a7919.png)

图像由ChatGPT+/DALL-E3生成，展示了关于RAG的文章的插图。

检索增强生成（RAG）似乎现在相当受欢迎。随着大语言模型（LLM）的兴起，它成为了使LLM在特定任务上表现更好的热门技术之一，比如对内部文档进行问答。前段时间，我参加了一个 [Kaggle比赛](https://www.kaggle.com/competitions/kaggle-llm-science-exam)，这让我能够尝试它，并比自己随意实验学到更多一些。以下是从这些实验中获得的一些经验教训。

除非另有说明，否则所有图片均由作者提供。生成工具为 ChatGPT+/DALL-E3（如有注明），或取自我个人的 Jupyter 笔记本。

# RAG 概述

RAG 有两个主要部分：检索和生成。在第一部分中，检索用于获取与查询相关的（块）文档。生成则使用这些检索到的块作为额外输入，即 *context*，传递给第二部分的答案生成模型。这个附加的上下文旨在为生成器提供比基本训练数据更及时、更好的信息，以生成答案。

# 构建 RAG 输入，或文本分块

LLM 的最大上下文或序列窗口长度是它们可以处理的范围，RAG 生成的输入上下文需要足够短以适应这个序列窗口。我们希望将尽可能多的相关信息纳入这个上下文，因此从潜在的输入文档中获取最佳的“块”文本非常重要。这些块应当是生成正确答案所需的最相关的内容。

作为第一步，输入文本通常会被分块成更小的片段。RAG 的一个基本预处理步骤是使用特定的嵌入模型将这些块转换为嵌入。一个典型的嵌入模型的序列窗口为 512 个 tokens，这也使其成为实际的分块目标。一旦文档被分块并编码为嵌入，就可以使用这些嵌入进行相似性搜索，以构建生成答案的上下文。

我发现 [Langchain](https://github.com/langchain-ai/langchain) 提供了有用的工具用于输入加载和文本分块。例如，使用 Langchain 对文档进行分块（在此情况下，使用 [Flan-T5-Large](https://huggingface.co/google/flan-t5-large) 模型的分词器）是非常简单的：

```py
from transformers import AutoTokenizer 
from langchain.text_splitter import RecursiveCharacterTextSplitter 

#This is the Flan-T5-Large model I used for the Kaggle competition 
llm = "/mystuff/llm/flan-t5-large/flan-t5-large" 
tokenizer = AutoTokenizer.from_pretrained(llm, local_files_only=True) 
text_splitter = RecursiveCharacterTextSplitter
                 .from_huggingface_tokenizer(tokenizer, chunk_size=12,
                                             chunk_overlap=2,                        
                                             separators=["\n\n", "\n", ". "]) 
section_text="Hello. This is some text to split. With a few "\ 
             "uncharacteristic words to chunk, expecting 2 chunks." 
texts = text_splitter.split_text(section_text) 
print(texts)
```

这将生成以下两个块：

```py
['Hello. This is some text to split',
 '. With a few uncharacteristic words to chunk, expecting 2 chunks.']
```

在上面的代码中，*chunk_size* 12 告诉 LangChain 旨在每个块最多包含 12 个 token。根据文本结构，[这可能并不总是 100% 精确](https://stackoverflow.com/questions/76633836/what-does-langchain-charactertextsplitters-chunk-size-param-even-do)。然而，根据我的经验，这通常效果很好。需要记住的是 tokens 和单词之间的区别。下面是对上述 *section_text* 进行分词的一个示例：

```py
section_text="Hello. This is some text to split. With a few "\ 
             "uncharacteristic words to chunk, expecting 2 chunks." 
encoded_text = tokenizer(section_text) 
tokens = tokenizer.convert_ids_to_tokens(encoded_text['input_ids']) 
print(tokens)
```

生成的输出 tokens：

```py
['▁Hello', '.', '▁This', '▁is', '▁some', '▁text', '▁to', '▁split', '.', 
 '▁With', '▁', 'a', '▁few', '▁un', 'character', 'istic', '▁words', 
 '▁to', '▁chunk', ',', '▁expecting', '▁2', '▁chunk', 's', '.', '</s>']
```

大多数 *section_text* 中的单词本身形成一个 token，因为它们是[文本中的常见单词](https://huggingface.co/docs/transformers/tokenizer_summary)。然而，对于特殊形式的单词或领域词汇，这可能会更复杂。例如，在这里，“uncharacteristic” 这个词变成了三个 tokens [“ *un*”， “ *character*”， “ *istic*”]。这是因为模型的分词器知道这三个部分词汇，但不知道整个单词（“ *uncharacteristic*”）。每个模型都有自己的分词器来匹配输入和模型训练中的这些规则。

在分块中，来自Langchain的[RecursiveCharacterTextSplitter](https://python.langchain.com/docs/modules/data_connection/document_transformers/text_splitters/recursive_text_splitter)用于上述代码中，计算这些令牌，并寻找给定的分隔符将文本拆分成请求的块。不同块大小的试验可能会有用。在我的Kaggle实验中，我从嵌入模型的最大大小开始，即512个令牌。然后尝试了256、128和64个令牌的块大小。

# 示例RAG查询

我提到的[Kaggle比赛](https://www.kaggle.com/competitions/kaggle-llm-science-exam)是基于维基百科数据的多项选择题回答。任务是从多个选项中选择每个问题的正确答案。显而易见的方法是使用RAG从维基百科数据中找到所需的信息，并用它来生成正确答案。以下是比赛数据中的第一个问题及其答案选项，用于说明：

![](../Images/35ff5c28169adc2755e25364da4b542b.png)

示例问题和答案选项A-E。

多项选择题是尝试RAG的一个有趣话题。但我相信，最常见的RAG用例是根据源文档回答问题。有点像聊天机器人，但通常是针对特定领域或（公司）内部文档的问答。我在本文中使用这个基本的问答用例来展示RAG。

作为本文的RAG示例问题，我需要一个LLM无法仅凭其训练数据直接回答的问题。我使用了维基百科数据，因为它可能是LLM训练数据的一部分，所以我需要一个与模型训练后相关的问题。我为本文使用的模型是[Zephyr 7B beta](https://huggingface.co/HuggingFaceH4/zephyr-7b-beta)，于2023年初训练完成。最后，我决定询问[Google Bard AI聊天机器人](https://bard.google.com/)。它在Zephyr训练日期之后的一年里有很多发展。我对Bard也有一定了解，以评估LLM的答案。因此，我使用“*what is google bard?*”作为本文的示例问题。

# 嵌入向量

RAG的第一阶段检索基于嵌入向量，这些向量实际上只是多维空间中的点。它们看起来像这样（这里只列出了前10个值）：

```py
q_embeddings[:10]
array([-0.45518905, -0.6450379, 0.3097812, -0.4861114 , -0.08480848,
 -0.1664767 , 0.1875889, 0.3513346, -0.04495572, 0.12551129],
```

这些嵌入向量可以用来比较单词/句子及其相互关系。这些向量可以通过嵌入模型构建。可以在[MTEB排行榜](https://huggingface.co/spaces/mteb/leaderboard)找到各种统计数据的模型集。使用这些模型之一就像这样简单：

```py
from sentence_transformers import SentenceTransformer, util

embedding_model_path = "/mystuff/llm/bge-small-en" 
embedding_model = SentenceTransformer(embedding_model_path, device='cuda')
```

HuggingFace上的模型页面通常显示示例代码。上述代码从本地磁盘加载模型“ [bge-small-en](https://huggingface.co/BAAI/bge-small-en-v1.5) ”。使用此模型创建嵌入只是：

```py
question = "what is google bard?" 
q_embeddings = embedding_model.encode(question)
```

在这种情况下，嵌入模型用于将给定的问题编码为嵌入向量。该向量与上面的示例相同：

```py
q_embeddings.shape
(, 384)

q_embeddings[:10]
array([-0.45518905, -0.6450379, 0.3097812, -0.4861114 , -0.08480848,
       -0.1664767 , 0.1875889, 0.3513346, -0.04495572, 0.12551129],
       dtype=float32)
```

形状 (, 384) 表示 *q_embeddings* 是一个长度为 384 个浮点数的单一向量（而不是一次嵌入多个文本的列表）。上面的切片显示了这 384 个值中的前 10 个。某些模型使用更长的向量以获得更准确的关系，而其他模型，如本例所示，则使用较短的向量（此处为 384）。再次，[MTEB 排行榜](https://huggingface.co/spaces/mteb/leaderboard) 提供了很好的示例。较小的向量需要更少的空间和计算，而较大的向量在表示块之间的关系以及有时的序列长度方面提供了一些改进。

对于我的 RAG 相似性搜索，我首先需要问题的嵌入。这就是上面的 *q_embeddings*。需要将其与所有被搜索文章（或其块）的嵌入向量进行比较。在这种情况下，所有被分块的维基百科文章。要为所有这些构建嵌入：

```py
article_embeddings = embedding_model.encode(article_chunks)
```

这里 *article_chunks* 是来自英文维基百科数据转储的所有文章的所有块的列表。这样它们可以批量编码。

# 向量数据库

在大规模文档/文档块上实现相似性搜索，在基本层面上并不复杂。一个常见的方法是计算查询和文档向量之间的 [余弦相似性](https://www.geeksforgeeks.org/how-to-calculate-cosine-similarity-in-python/)，并进行排序。然而，在大规模时，这有时会变得有些复杂。向量数据库是使这种管理和搜索在规模上变得更简单/更高效的工具。

例如，[Weaviate](https://weaviate.io/) 是一个向量数据库，曾用于 [StackOverflow 的基于 AI 的搜索](https://resources.stackoverflow.co/topic/thought-leadership/stack-overflows-ai-journey-webinar)。在其最新版本中，它也可以以 [嵌入模式](https://weaviate.io/developers/weaviate/installation/embedded) 使用，这使得它甚至可以在 Kaggle 笔记本中使用。它也被用于一些 [Deeplearning.AI LLM 短期课程](https://learn.deeplearning.ai/vector-databases-embeddings-applications/lesson/1/introduction)，所以至少似乎有些受欢迎。当然，还有许多其他工具，进行比较是很好的，这个领域也在快速发展。

在我的试验中，我使用了来自 Facebook/Meta 研究的 [FAISS](https://github.com/facebookresearch/faiss) 作为向量数据库。FAISS 更像是一个库，而不是一个客户端-服务器数据库，因此在 Kaggle 笔记本中使用非常简单。它的表现也很不错。

# 分块数据和嵌入

一旦所有文章的分块和嵌入完成后，我构建了一个包含所有相关信息的 Pandas DataFrame。以下是我使用的维基百科数据转储前 5 个块的示例，文档标题为 *无政府主义*：

![](../Images/b0cad38abdd821b3e885baf39e3bcafb.png)

我使用的维基百科数据转储中的第一篇文章的前 5 个块。

这个表格中的每一行（一个 Pandas DataFrame）包含块化过程后单个块的数据。它有 5 列：

+   *chunk_id*：允许我稍后将块嵌入映射到块文本。

+   *doc_id*：允许将块映射回其文档。

+   *doc_title*：用于尝试一些方法，例如将文档标题添加到每个块中。

+   *chunk_title*：块的文章子部分标题，与 *doc_title* 目的相同。

+   *chunk*：实际的块文本

这里是前五个无政府主义块的嵌入，顺序与上面的 DataFrame 相同：

```py
[[ 0.042624 -0.131264 -0.266858 ... -0.329627 0.178211 0.248001]
 [-0.120318 -0.110153 -0.059611 ... -0.297150 -0.043165 0.558150]
 [ 0.116761 -0.066759 -0.498548 ... -0.330301 0.019448 0.326484]
 [-0.517585 0.183634 0.186501 ... 0.134235 -0.033262 0.498731]
 [-0.245819 -0.189427 0.159848 ... -0.077107 -0.111901 0.483461]]
```

每一行在这里部分展示，但说明了概念。

# 搜索相似的查询嵌入与块嵌入

早些时候，我对查询“ *what is google bard?* ”进行了编码，然后编码了所有文章块。通过这两组嵌入，RAG 搜索的第一部分很简单：找到“语义上”最接近查询的文档。实际上，只需计算查询嵌入向量与所有块向量之间的余弦相似度，并按相似度得分排序。

这里是与 *q_embeddings* 语义上最接近的前 10 个块：

![](../Images/fa786f4b953c673500fa0eeb4ed0bf31.png)

按照与问题的余弦相似度排序的前 10 个块。

这个表格中的每一行（DataFrame）表示一个块。这里的 *sim_score* 是计算的余弦相似度得分，行从最高余弦相似度到最低排序。表格显示了前 10 个最高的 *sim_score* 行。

# Re-ranking

纯粹基于嵌入的相似性搜索在计算上非常快速且低成本。然而，它不如其他一些方法准确。*Re-ranking* 是一个术语，用于描述使用另一个模型更准确地排序这些初始文档列表的过程，这种模型通常计算成本更高。这个模型通常在所有文档和块上运行的成本太高，但在初始相似性搜索后的前几个块上运行就可行得多。Re-ranking 帮助获得更好的最终块列表，以便为 RAG 的生成部分建立输入上下文。

同样的 [MTEB 排行榜](https://huggingface.co/spaces/mteb/leaderboard) 也托管了许多模型的 re-ranking 得分。在这种情况下，我使用了 [bge-reranker-base](https://huggingface.co/BAAI/bge-reranker-base) 模型进行 re-ranking：

```py
import torch 
from transformers import AutoModelForSequenceClassification, AutoTokenizer 

rerank_model_path = "/mystuff/llm/bge-reranker-base"
rerank_tokenizer = AutoTokenizer.from_pretrained(rerank_model_path) 
rerank_model = AutoModelForSequenceClassification 
                  .from_pretrained(rerank_model_path) 
rerank_model.eval() 

def calculate_rerank_scores(pairs): 
    with torch.no_grad(): inputs = rerank_tokenizer(pairs, padding=True, 
                                          truncation=True, return_tensors='pt',
                                          max_length=512) 
    scores = rerank_model(**inputs, return_dict=True) 
                         .logits.view(-1, ).float() 
    return scores 

question = questions[idx]
pairs = [(question, chunk) for chunk in doc_chunks_all[idx]] 
rerank_scores = calculate_rerank_scores(pairs) 
df["rerank_score"] = rerank_scores
```

在将 *rerank_score* 添加到块 DataFrame 并用它进行排序后：

![](../Images/4fe80155ef3b8e0c33a065708d942c8d.png)

按照与问题的重新排序得分排序的前 10 个块。

比较上面的两个表格（第一个按*sim_score*排序，现按*rerank_score*排序），可以看到一些明显的差异。按嵌入生成的普通相似性得分（*sim_score*）排序， [Tenor页面](https://en.wikipedia.org/wiki/Tenor_(website)) 是第5个最相似的片段。由于Tenor似乎是一个由Google托管的GIF搜索引擎，我想看到它的嵌入与问题“*what is google bard?*”接近是有道理的。但它实际上与Bard本身没有什么关系，只是Tenor是一个在类似领域的Google产品。

然而，在按*rerank_score*排序后，结果更有意义。Tenor从前10名中消失了，前10名列表中的最后两个片段似乎不相关。这些片段关于“Bard”和“Bård”的名字。可能是因为有关Google Bard的最佳信息来源似乎是 [Google Bard页面](https://en.wikipedia.org/wiki/Bard_(chatbot))，在上述表格中这是id为6026776的文档。之后，我猜RAG用完了好的文章匹配，并有些偏离了正轨（Bård）。这也可以从表格最后两行/片段的负面重新排序得分中看到。

通常会有许多相关文档和文档中的片段，而不仅仅是上面提到的1份文档和8个片段。但是在这种情况下，这种限制有助于说明基于基本嵌入的相似性搜索和重新排序之间的区别，以及重新排序如何积极地影响最终结果。

# 构建上下文

当我们收集了RAG输入的顶级片段后，我们该怎么做？我们需要从这些片段中为生成模型构建上下文。最简单的方法就是将选择的顶级片段连接成一个长文本序列。该序列的最大长度受所用模型的限制。由于我使用了 [Zephyr 7B模型](https://huggingface.co/HuggingFaceH4/zephyr-7b-beta)，所以我将4096个标记作为最大长度。 [Zephyr页面](https://huggingface.co/HuggingFaceH4/zephyr-7b-beta)将此作为一个灵活的序列限制（带有滑动注意窗口）。更长的上下文似乎更好，但 [这并不总是如此](https://www-cs.stanford.edu/~nfliu/papers/lost-in-the-middle.arxiv2023.pdf)。最好尝试一下。

以下是我用来生成具有此上下文的答案的基本代码：

```py
from transformers import AutoTokenizer, AutoModelForCausalLM 
import torch 

llm_answer_path = "/mystuff/llm/zephyr-7b-beta" 
torch_device = "cuda:0" 
tokenizer = AutoTokenizer.from_pretrained(llm_answer_path, 
                                          local_files_only=True) 
llm_answer = AutoModelForCausalLM.from_pretrained(llm_answer_path, 
                           device_map=torch_device, local_files_only=True, 
                           torch_dtype=torch.float16) 
# assuming here that "context" contains the pre-built context 
query = "answer the following question, "\ 
        "based on your knowledge and the provided context. "\n 
        "Keep the answer concise.\n\nquestion:" + question + 
        "\n\ncontext:"+context 

input_ids = tokenizer.encode(query+"\n\nANSWER:", return_tensors='pt', 
                             return_attention_mask=False).to(torch_device) 
greedy_output = llm_answer.generate(input_ids, max_new_tokens=1024, 
                                    do_sample=True) 
answer = tokenizer.decode(greedy_output[0], skip_special_tokens=True) 
print(answer[len(query):])
```

如前所述，在这种情况下，上下文只是将排名最高的片段连接起来。

# 生成答案

进行比较，首先让我们尝试模型在没有任何额外上下文的情况下的回答，即仅基于其训练数据：

```py
query = "what is google bard?" 
input_ids = tokenizer.encode(query+"\n\nANSWER:", return_tensors='pt', 
                             return_attention_mask=False).to(torch_device) 
greedy_output = llm_answer.generate(input_ids, max_new_tokens=1024, 
                                    do_sample=True) 
answer = tokenizer.decode(greedy_output[0], skip_special_tokens=True) 
print(answer[len(query):])
```

这给出了（多次运行之一，略有变化但通常相似）：

```py
ANSWER:
Google Bard is an experimental, AI-based language model developed by 
Google's sister company, DeepMind. Its primary use is to generate 
human-like text responses to prompts, which can help in tasks such as 
content creation, idea generation, and text summarization. Bard is 
trained on a vast amount of textual data and can provide highly 
relevant and contextually accurate responses, making it a useful tool 
in various applications where text generation is required. However, as 
an experimental feature, Bard's accuracy and effectiveness may still be 
improving, and it is not yet publicly available for use.
```

通常来说，虽然准确，但缺乏最新的发展。相比之下，我们可以尝试将生成的上下文提供给问题：

```py
query = "answer the following question, "\
        "based on your knowledge and the provided context. "\
        "Keep the answer concise.\n\n"\
        "question:" + question + "\n\ncontext:"+context
input_ids = tokenizer.encode(query+"\n\nANSWER:", return_tensors='pt',
                             return_attention_mask=False).to(torch_device)
greedy_output = llm_answer.generate(input_ids, max_new_tokens=1024, 
                                    do_sample=True)
answer = tokenizer.decode(greedy_output[0], skip_special_tokens=True)
print(answer[len(query):])
```

以下是一个示例答案，使用了按*sim_score*排序的片段作为上下文（包括Tenor和Bård页面片段）：

```py
ANSWER: 
Bard is a conversational generative artificial intelligence (AI) 
chatbot developed by Google based initially on the LaMDA family of 
large language models (LLMs) and later PaLM. It was launched in a 
limited capacity in March 2023 to mixed reviews, and expanded to other 
countries in May, following the launch of Microsoft's Bing Chat. Google 
researcher Jacob Devlin resigned from the company after alleging Bard 
had surreptitiously leveraged data from ChatGPT. Tenor is an online GIF 
search engine and database owned by Google, available on Android, iOS, 
and macOS. Its main product is the GIF Keyboard, which is integrated 
into numerous apps and messaging services. Bård is a Norwegian 
masculine given name of Old Norse origin. It is sometimes used as a 
surname. It may refer to several people. Tenor and Bård are not related.
```

这不是一个很好的答案，因为它开始谈论完全无关的话题，*Tenor*和*Bård*。部分原因是因为在这种情况下，Tenor块被包含在上下文中，块的顺序也通常较差，因为没有重新排序。

相比之下，使用*rerank_score*排序的上下文块（更好的块排序和Tenor消失）：

```py
ANSWER:
Google Bard is a conversational generative artificial intelligence (AI) 
chatbot, based initially on the LaMDA family of large language models 
(LLMs) and later on PaLM, developed by Google to compete with OpenAI's 
ChatGPT. It was rolled out in a limited capacity in March 2023 and 
expanded to more countries in May, prompting a mixed reception from 
critics, who raised safety and ethical concerns about its accuracy and 
usefulness. Google has promised that Bard will be tightly integrated 
with other Google AI products and services, leading to claims that a 
new AI-powered version of the Google Assistant, dubbed "Assistant with 
Bard", is being prepared for launch. Google has also stressed that Bard 
is still in its early stages and being continuously refined, with plans 
to upgrade it with new personalization and productivity features, while 
stressing that it remains distinct from Google Search.
```

现在不相关的主题已经消失，答案总体上更好，更切题。

这突出了不仅要找到适当的上下文以提供给模型，而且还要去除无关的上下文。在这种情况下，Zephyr模型似乎无法直接识别哪个部分的上下文是相关的，而是似乎对所有内容进行了总结。不能真正责怪模型，因为我提供了这些上下文并要求它使用这些内容。

查看块的重新排序分数，基于诸如负面重新排序分数等指标的一般过滤方法也可以解决上述问题，因为在这种情况下“坏”块具有负面重新排序分数。

值得注意的是，Google在我写这篇文章时发布了一个全新的、显著改进的*Gemini*模型系列。由于维基百科的内容生成有一些延迟，因此这里生成的答案没有提到这个模型。因此，如人们所想，尝试保持上下文的信息是最新的，并保持其相关性和重点是很重要的。

# 可视化嵌入检查

嵌入是一个很好的工具，但有时确实很难真正理解它们是如何工作的，以及相似度搜索发生了什么。一个基本的方法是将嵌入彼此绘制，以获得一些关于它们关系的见解。

构建这样的可视化使用[PCA](https://en.wikipedia.org/wiki/Principal_component_analysis)和可视化库是相当简单的。它涉及将嵌入向量映射到2维或3维，并绘制结果。在这里，我将这384维映射到2维，并绘制了结果：

```py
import seaborn as sns 
import numpy as np 

fp_embeddings = embedding_model.encode(first_chunks) 
q_embeddings_reshaped = q_embeddings.reshape(1, -1) 
combined_embeddings = np.concatenate((fp_embeddings, q_embeddings_reshaped)) 

df_embedded_pca = pd.DataFrame(X_pca, columns=["x", "y"]) 
# text is short version of chunk text (plot title) 
df_embedded_pca["text"] = titles 
# row_type = article or question per each embedding 
df_embedded_pca["row_type"] = row_types 

X = combined_embeddings pca = PCA(n_components=2).fit(X) 
X_pca = pca.transform(X) 

plt.figure(figsize=(16,10)) 
sns.scatterplot(x="x", y="y", hue="row_type", 
                palette={"article": "blue", "question": "red"}, 
                data=df_embedded_pca, #legend="full", 
                alpha=0.8, s=100 ) 
for i in range(df_embedded_pca.shape[0]): 
   plt.annotate(df_embedded_pca["text"].iloc[i], 
                (df_embedded_pca["x"].iloc[i], df_embedded_pca["y"].iloc[i]), 
                fontsize=20 ) 
plt.legend(fontsize='20') 
# Change the font size for x and y axis ticks plt.xticks(fontsize=16) 
plt.yticks(fontsize=16) 
# Change the font size for x and y axis labels 
plt.xlabel('X', fontsize=16) 
plt.ylabel('Y', fontsize=16)
```

对于“*what is google bard?*”问题的前10篇文章，这里给出了以下可视化：

![](../Images/67b91c3adabe96243a9d5e60010483b7.png)

基于PCA的2D绘图，比较问题嵌入与文章第一个块嵌入。

在这个图中，红点是问题“*what is google bard?*”的嵌入。蓝点是根据*sim_score*找到的最接近的维基百科文章匹配项。

[Bard文章](https://en.wikipedia.org/wiki/Bard_(chatbot))显然是与问题最接近的，而其他的则稍远一些。[Tenor文章](https://en.wikipedia.org/wiki/Tenor_(website))似乎是第二接近的，而[Bård文章](https://en.wikipedia.org/wiki/B%C3%A5rd)则稍远一些，可能是因为从384维映射到2维时信息的丢失。由于这一点，可视化并不是完全准确的，但对快速人工概览是有帮助的。

下图展示了我在Kaggle代码中发现的实际错误，使用了类似的PCA图。为了获取一些见解，我对维基百科转储中的第一篇文章（“ *无政府主义*”）提出了一个简单的问题：“ *无政府主义的定义是什么？*”。下面是PCA可视化的结果，标记的离群点可能是最有趣的部分：

![](../Images/200d00bcdabbf0f98d58bdd7cf0f5802.png)

我在PCA基于2D图的Kaggle嵌入中显示的失败，针对所选的顶级文档。

左下角的红点再次表示问题。旁边的蓝点簇是所有与无政府主义相关的文章。然后右上角有两个离群点。我删除了图表中的标题以保持其可读性。查看时，这两个离群文章似乎与问题无关。

为什么会这样？由于我使用了512、256、128和64的各种块大小来索引文章，在处理256块大小的所有文章时遇到了一些问题，并在中途重新启动了分块。这导致某些嵌入与我存储的块文本的索引有所不同。在注意到这些奇怪的结果后，我重新计算了256个令牌块大小的嵌入，并将结果与512大小进行比较，注意到这个差异。可惜那时比赛已经结束🙂

# 更高级的上下文选择

上述内容讨论了将文档分块并使用相似度搜索+重新排序作为找到相关块和构建问题回答上下文的方法。我发现有时也有必要考虑初始文档的选择方式，而不仅仅是块本身。

作为示例方法，[高级RAG](https://www.deeplearning.ai/short-courses/building-evaluating-advanced-rag/)课程在[DeepLearning.AI](https://www.deeplearning.ai/)上介绍了两种方法：句子窗口化和层次块合并。总结来说，这种方法查看附近的块，如果多个块的分数很高，则将它们作为一个大的块。所谓“层次结构”是通过考虑越来越大的块组合来共同相关。旨在提供更连贯的上下文，而不是随机排序的小块，给生成LLM更好的输入。

作为一个简单的示例，这是我上面Bard示例的重新排序的前几个块：

![](../Images/fa17b950e30b9f3b12c8cb936393317c.png)

我在Bard示例中的前10个块，按重新排序分数排序。

这里最左侧的列是块的索引。在我的生成中，我只是按表中的排序顺序取了顶级块。如果我们想使上下文更连贯，我们可以按文档中的顺序对最终选择的块进行排序。如果在高度排名的块之间有小片段缺失，添加缺失的部分（例如这里的块 ID 7）可能有助于填补空白，类似于层次合并。这可能是作为最终步骤进行尝试的内容，以获得最终的改进。

在我的 Kaggle 实验中，我仅基于第一个块进行初步文档选择。这部分是由于 Kaggle 的资源限制，但它似乎也有一些其他的优势。通常，一篇文章的开头部分充当了总结（引言或摘要）。从这些排名文章中进行初步块选择可能有助于选择具有更相关整体上下文的块。

在我上面的 Bard 示例中可以看到，无论是 *rerank_score* 还是 *sim_score*，对于最佳文章的第一个块都是最高的。为了改进这一点，我还尝试使用更大的块大小进行初始文档选择，以包括更多的引言以提高相关性。然后将顶级选择的文档按较小的块大小进行切分，以实验每种大小的上下文效果。

由于资源限制，我无法在 Kaggle 上对所有文档的所有块进行初始搜索，但我在 Kaggle 外部尝试了一下。在这些试验中，我发现有时单个不相关的文章块会被排名较高，而实际上对答案生成存在误导。例如，与相关电影的演员传记。初步的文档相关性选择可能有助于避免这种情况。不幸的是，我没有时间用不同的配置进一步研究这个问题，好的重排名可能已经有帮助。

最后，在上下文中重复相同的信息在多个块中不是很有用。块的最高排名并不保证它们彼此最好地补充，或最佳块多样性。例如，LangChain 为 [最大边际相关性](https://python.langchain.com/docs/modules/model_io/prompts/example_selectors/mmr) 提供了一个特殊的块选择器。它通过对新块进行惩罚，惩罚依据是它们与已添加块的相似度来实现这一点。

# 扩展 RAG 查询

我在这里使用了一个非常简单的问题/查询作为我的 RAG 示例（“ *what is google bard?*”），简单的查询有助于说明基本的 RAG 概念。考虑到我使用的嵌入模型具有 512 令牌的最大序列长度，这个查询输入相当简短。如果我使用嵌入模型的分词器（*bge-small-en*）将这个问题编码成令牌，我会得到以下令牌：

```py
['[CLS]', 'what', 'is', 'google', 'bard', '?', '[SEP]']
```

这总共是 7 个标记。最大序列长度为 512，这为我使用更长的查询句子留出了足够的空间。有时这很有用，特别是当我们想要检索的信息不是简单的查询，或者领域较复杂时。对于非常简单的查询，语义搜索可能效果不好，正如在[Stack Overflows AI Journey 文章](https://stackoverflow.blog/2023/07/31/ask-like-a-human-implementing-semantic-search-on-stack-overflow/)中提到的那样。

例如，Kaggle 比赛有一组问题，每个问题有 5 个答案选项可以选择。我最初尝试了仅将问题作为嵌入模型的输入的 RAG。搜索结果并不理想，因此我再次尝试了将问题 + 所有答案选项作为查询。这产生了更好的结果。

作为一个例子，比赛训练数据集中的第一个问题：

```py
Which of the following statements accurately describes the impact of 
Modified Newtonian Dynamics (MOND) on the observed "missing baryonic mass" 
discrepancy in galaxy clusters?
```

对于 *bge-small-en* 模型，这需要 32 个标记。因此，最大 512 个标记的序列长度还剩大约 480 个标记可以填充。

这里是第一个问题及其给出的 5 个答案选项：

![](../Images/52216d4403a7ac4827efa1264b76eb88.png)

示例问题和答案选项 A-E。将所有这些文本合并形成了查询。

将问题和给定的选项合并成一个 RAG 查询，得到的长度为 235 个标记，并且仍然有超过 50% 的嵌入模型序列长度剩余。就我而言，这种方法产生了更好的结果，无论是通过人工检查，还是比赛分数。因此，尝试不同的方式使 RAG 查询本身更具表现力是值得的。

# 幻觉

最后是关于[幻觉](https://machinelearningmastery.com/a-gentle-introduction-to-hallucinations-in-large-language-models/)的话题，即模型生成的文本不正确或虚构。我的 *sim_score* 排序中的 Tenor 示例就是一个例子，即使生成器确实基于实际的给定上下文。因此，我想最好还是保持上下文良好 :).

为了解决幻觉问题，大型 AI 公司（[Google Bard](https://bard.google.com/chat)，[ChatGPT](https://chat.openai.com/)，[Bing Chat](https://www.bing.com/chat)）的聊天机器人都提供了将其生成的回答部分链接到可验证来源的方法。[Bard](https://bard.google.com/chat) 有一个特定的 “G” 按钮，可以执行 Google 搜索，并突出显示与搜索结果匹配的生成回答部分。可惜我们并不总是拥有世界级的搜索引擎来帮助处理我们的数据。

[Bing Chat](https://www.bing.com/chat)采用了类似的方法，突出显示答案的部分并添加对源网站的引用。[ChatGPT](https://chat.openai.com/)的方法略有不同；我必须明确要求它验证其答案并更新最新进展，告诉它使用其浏览器工具。之后，它进行了互联网搜索并链接到特定网站作为来源。源的质量似乎有很大的变化，就像任何互联网搜索一样。当然，对于内部文档，这种类型的网络搜索是不可能的。然而，即使在内部，也应该始终可以链接到来源。

我还询问了Bard、ChatGPT+和Bing关于检测幻觉的想法。结果包括一个LLM幻觉[排名指数](https://www.rungalileo.io/blog/hallucination-index)，以及[RAG幻觉](https://www.rungalileo.io/hallucinationindex)。在调优LLM时，将[温度参数设为零](https://txt.cohere.com/llm-parameters-best-outputs-language-ai/)可能有助于LLM生成确定性的、最可能的输出令牌。

最后，由于这是一个非常常见的问题，似乎有各种方法正在被构建以更好地解决这一挑战。例如，特定的[LLM来帮助检测幻觉](https://huggingface.co/blog/dhuynh95/automatic-hallucination-detection)似乎是一个有前途的领域。我没有时间尝试它们，但在更大的项目中肯定是相关的。

# 评估结果

除了实现一个有效的RAG解决方案之外，能够评估它的效果也是很有价值的。在Kaggle比赛中，这相当简单。我只是运行解决方案以尝试回答训练数据集中的给定问题，并将其与训练数据中提供的正确答案进行比较。或者将模型提交到Kaggle比赛测试集进行评分。答案分数越高，RAG解决方案就越好，即使分数背后还有更多内容。

在许多情况下，可能没有适用于领域特定RAG的合适评估数据集。对于这种情况，可以考虑从一些通用NLP评估数据集开始，例如[这个列表](https://paperswithcode.com/task/question-answering#:~:text=Popular%20benchmark%20datasets%20for%20evaluation%20question%20answering%20systems%20include%20SQuAD,models%20are%20T5%20and%20XLNet.)。像LangChain这样的工具还提供了[自动生成问题和答案](https://blog.langchain.dev/auto-eval-of-question-answering-tasks/)并进行评估的支持。在这种情况下，使用一个LLM为给定文档集创建示例问题和答案，另一个LLM用于评估RAG是否能够提供这些问题的正确答案。也许可以在这个[LangChain的RAG评估教程](https://learn.deeplearning.ai/langchain/lesson/6/evaluation)中更好地解释。

虽然通用解决方案在开始时可能很好，但在实际项目中，我会尝试收集来自领域专家和目标用户的真实问题和答案数据集。由于大型语言模型（LLM）通常被期望生成自然语言响应，这些响应可能在正确的前提下变化很大。因此，评估答案是否正确不像正则表达式或类似的模式匹配那么直接。在这种情况下，我发现使用另一种LLM来评估给定的响应是否匹配参考响应是一个非常有用的工具。这些模型能够更好地处理文本变异。

# 结论

RAG 是一个非常不错的工具，随着对LLM的高度关注，它现在也是一个相当热门的话题。虽然RAG和嵌入技术已经存在了很长时间，但最新的强大LLM及其快速演变可能使它们在许多高级应用场景中更具吸引力。我预计这一领域将持续以良好的速度发展，有时很难跟上所有最新动态。为此，像[RAG 发展综述](https://arxiv.org/pdf/2312.10997.pdf)这样的总结可以提供至少保持主要发展方向的参考。

一般来说，RAG 方法相当简单：找到与给定查询类似的一组文本块，将它们拼接成上下文，然后向LLM请求答案。然而，正如我在这里尝试展示的那样，在如何使这一过程对不同需求有效且高效方面，可能会有各种问题需要考虑。从良好的上下文检索，到排名和选择最佳结果，最后能够将结果链接回实际的源文档。还要评估生成的查询上下文和答案。正如[Stack Overflow 的人们指出的](https://stackoverflow.blog/2023/07/31/ask-like-a-human-implementing-semantic-search-on-stack-overflow/)，有时更传统的词汇搜索或混合搜索也非常有用，即使语义搜索也很酷。

今天就到这里。RAG继续...

![](../Images/852104944e2f6138ecb58522d8e91819.png)

ChatGPT+/DALL-E3 对 RAG 的理解..

*最初发布于* [*http://teemukanstren.com*](https://teemukanstren.com/2023/12/25/llmrag-based-question-answering/) *于2023年12月25日。*
