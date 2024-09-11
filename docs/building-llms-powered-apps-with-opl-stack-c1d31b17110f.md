# 使用OPL堆栈构建LLMs驱动的应用程序

> 原文：[https://towardsdatascience.com/building-llms-powered-apps-with-opl-stack-c1d31b17110f?source=collection_archive---------2-----------------------#2023-04-03](https://towardsdatascience.com/building-llms-powered-apps-with-opl-stack-c1d31b17110f?source=collection_archive---------2-----------------------#2023-04-03)

## OPL：OpenAI、Pinecone 和 Langchain 用于知识驱动的AI助手

[](https://medium.com/@wen_yang?source=post_page-----c1d31b17110f--------------------------------)[![Wen Yang](../Images/5eac438762d015a0ab128757cc951967.png)](https://medium.com/@wen_yang?source=post_page-----c1d31b17110f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c1d31b17110f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c1d31b17110f--------------------------------) [Wen Yang](https://medium.com/@wen_yang?source=post_page-----c1d31b17110f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fcbb5383bd438&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-llms-powered-apps-with-opl-stack-c1d31b17110f&user=Wen+Yang&userId=cbb5383bd438&source=post_page-cbb5383bd438----c1d31b17110f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c1d31b17110f--------------------------------) ·12 min 阅读·2023年4月3日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc1d31b17110f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-llms-powered-apps-with-opl-stack-c1d31b17110f&user=Wen+Yang&userId=cbb5383bd438&source=-----c1d31b17110f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc1d31b17110f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-llms-powered-apps-with-opl-stack-c1d31b17110f&source=-----c1d31b17110f---------------------bookmark_footer-----------)![](../Images/017e9316f7ecbd4f794d51802ab23255.png)

Midjourney 提示：一个女孩用多个积木块建造乐高桥梁

我记得一个月前，Eugene Yan 在 LinkedIn 上发布了一项 [投票](https://www.linkedin.com/posts/eugeneyan_activity-7029289248209977344-oteC?utm_source=share&utm_medium=member_desktop)：

> 你是否感到因为没有参与LLMs/生成型AI而错失良机？

大多数人回答了“是”。考虑到chatGPT引发的广泛关注以及现在gpt-4的发布，很容易理解为什么会这样。人们形容大型语言模型（LLMs）的崛起就像iPhone的时刻。然而，我认为真的没有必要感到FOMO。考虑一下：错过了开发iPhones的机会并不排除创造创新iPhone应用程序的巨大潜力。LLMs也是如此。我们刚刚进入了一个新时代，现在正是利用LLMs整合构建强大应用程序的绝佳时机。

在这篇文章中，我将涵盖以下主题：

1.  什么是OPL技术栈？

1.  如何使用OPL构建具有领域知识的chatGPT？（包含代码演示的关键组件）

1.  生产考虑因素

1.  常见误解

# 1\. 什么是OPL技术栈？

![](../Images/5efe1d72069d166bbe99a2cee6c8a240.png)

作者创建的图像

**OPL代表OpenAI、Pinecone和Langchain，** 它已逐渐成为克服LLMs两个局限性的行业解决方案：

1.  **LLMs幻觉：** chatGPT有时会提供过度自信的错误回答。一个潜在的原因是这些语言模型被训练得非常有效地预测下一个词，或者更准确地说是下一个标记。给定输入文本，chatGPT会返回高概率的词，这并不意味着chatGPT具有推理能力。

1.  **知识更新不够及时：** chatGPT的训练数据仅限于2021年9月之前的互联网数据。因此，如果你的问题涉及最近的趋势或话题，它可能会产生不太理想的回答。

常见的解决方案是在LLMs上添加知识库，并使用Langchain作为构建流水线的框架。每项技术的关键组件可以总结如下：

+   **OpenAI**：

    - 提供对强大LLMs如chatGPT和gpt-4的API访问

    - 提供将文本转换为嵌入的模型。

+   **Pinecone**：提供嵌入向量存储、语义相似度比较和快速检索。

+   **Langchain**：它包含6个模块（`Models`、`Prompts`、`Indexes`、`Memory`、`Chains`和`Agents`）。

    - `Models` 提供了灵活的嵌入模型、聊天模型和LLMs，包括但不限于OpenAI的产品。你还可以使用Hugging Face上的其他模型，如BLOOM和FLAN-T5。

    - `Memory` : 有多种方式可以让聊天机器人记住过去的对话记录。根据我的经验，实体记忆效果好且高效。

    - `Chains` : 如果你是Langchain的新手，Chains是一个很好的起点。它遵循类似流水线的结构来处理用户输入，选择LLM模型，应用Prompt模板，并从知识库中搜索相关上下文。

接下来，我将介绍我使用OPL技术栈构建的应用程序。

# 2\. 如何使用OPL构建具有领域知识的chatGPT？（包含代码演示的关键组件）

我构建的应用程序称为[chatOutside](https://outsidechat.streamlit.app/)，它有两个主要部分：

+   **chatGPT**：让你直接与chatGPT聊天，格式类似于问答应用，每次接收一个输入和输出。

+   **chatOutside**：让你与具有户外活动及趋势专业知识的chatGPT版本进行对话。格式更类似于聊天机器人的风格，所有消息在对话过程中都会被记录。我还包含了一个提供源链接的部分，这可以增强用户信心，并且总是有用的。

如你所见，如果你问同样的问题：“2023年最好的跑鞋是什么？我的预算在$200左右。”chatGPT会说“作为一个AI语言模型，我无法访问未来的信息。”而chatOutside会为你提供更及时的答案，并附上源链接。

![](../Images/90480e9dfc8f81b47e0fe6bf4e2e3f4b.png)

开发过程涉及三个主要步骤：

+   第一步：在Pinecone中构建外部知识库

+   第二步：使用Langchain进行问答服务

+   第三步：在Streamlit中构建我们的应用程序

每个步骤的实施细节将在下面讨论。

## **步骤1：** 在Pinecone中构建外部知识库

+   **步骤1.1：** 我连接到我们的外部目录数据库，并选择了在2022年1月1日至2023年3月29日之间发布的文章。这为我们提供了大约20,000条记录。

![](../Images/a55bed6eacdefdd3ad7e8a6bb57cae7c.png)

外部的示例数据预览

接下来，我们需要进行两个数据转换。

+   **步骤1.2：** 将上述数据框转换为字典列表，以确保数据可以正确地插入到Pinecone中。

```py
# Convert dataframe to a list of dict for Pinecone data upsert
data = df_item.to_dict('records')
```

+   **步骤1.3：** 使用Langchain的`RecursiveCharacterTextSplitter`将`content`拆分成更小的块。将文档拆分为更小的块有两个好处：

    - 一篇典型文章可能超过1000个字符，这非常长。想象一下我们想检索前3篇文章作为提示给chatGPT，我们很容易就会超过4000个字元限制。

    - 更小的块提供更相关的信息，从而为chatGPT提供更好的提示上下文。

```py
from langchain.text_splitter import RecursiveCharacterTextSplitter

text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=400,
    chunk_overlap=20,
    length_function=tiktoken_len,
    separators=["\n\n", "\n", " ", ""]
)
```

分割后，每条记录的内容被拆分成多个部分，每个部分少于400个字元。

![](../Images/309b912e454adbd6526dcc71bbd2aab6.png)

将内容拆分为多个块

值得注意的是，使用的文本分割器称为`RecursiveCharacterTextSplitter`，这是Langchain的创建者Harrison Chase推荐使用的。基本思路是首先按段落拆分，然后按句子拆分，重叠（20字元）。这有助于保留来自周围句子的有意义信息和上下文。

+   **步骤 1.4：** 将数据插入 Pinecone。下面的代码改编自 [James Briggs](https://medium.com/u/b9d77a4ca1d1?source=post_page-----c1d31b17110f--------------------------------) 的精彩 [教程](https://github.com/pinecone-io/examples/blob/master/generation/langchain/handbook/05-langchain-retrieval-augmentation.ipynb)。

```py
import pinecone
from langchain.embeddings.openai import OpenAIEmbeddings

# 0\. Initialize Pinecone Client
with open('./credentials.yml', 'r') as file:
    cre = yaml.safe_load(file)
    # pinecone API
    pinecone_api_key = cre['pinecone']['apikey']

pinecone.init(api_key=pinecone_api_key, environment="us-west1-gcp")

# 1\. Create a new index
index_name = 'outside-chatgpt'

# 2\. Use OpenAI's ada-002 as embedding model
model_name = 'text-embedding-ada-002'
embed = OpenAIEmbeddings(
    document_model_name=model_name,
    query_model_name=model_name,
    openai_api_key=OPENAI_API_KEY
)
embed_dimension = 1536

# 3\. check if index already exists (it shouldn't if this is first time)
if index_name not in pinecone.list_indexes():
    # if does not exist, create index
    pinecone.create_index(
        name=index_name,
        metric='cosine',
        dimension=embed_dimension
    )

# 3\. Connect to index
index = pinecone.Index(index_name)
```

我们批量上传并嵌入所有文章，这花费了大约 20 分钟来插入 2 万条记录。请根据你的环境调整 `tqdm` 导入（你不需要同时导入两个！）

```py
# If using terminal
from tqdm.auto import tqdm

# If using in Jupyter notebook
from tqdm.autonotebook import tqdm

from uuid import uuid4

batch_limit = 100

texts = []
metadatas = []

for i, record in enumerate(tqdm(data)):
    # 1\. Get metadata fields for this record
    metadata = {
        'item_uuid': str(record['id']),
        'source': record['url'],
        'title': record['title']
    }
    # 2\. Create chunks from the record text
    record_texts = text_splitter.split_text(record['content'])

    # 3\. Create individual metadata dicts for each chunk
    record_metadatas = [{
        "chunk": j, "text": text, **metadata
    } for j, text in enumerate(record_texts)]

    # 4\. Append these to current batches
    texts.extend(record_texts)
    metadatas.extend(record_metadatas)

    # 5\. Special case: if we have reached the batch_limit we can add texts
    if len(texts) >= batch_limit:
        ids = [str(uuid4()) for _ in range(len(texts))]
        embeds = embed.embed_documents(texts)
        index.upsert(vectors=zip(ids, embeds, metadatas))
        texts = []
        metadatas = []
```

在将 Outside 文章数据插入后，我们可以通过使用 `index.describe_index_stats()` 检查我们的 Pinecone 索引。需要注意的统计数据之一是 `index_fullness`，在我们的案例中为 0.2。这意味着 Pinecone pod 已满 20%，暗示一个 p1 pod 大约可以存储 10 万篇文章。

![](../Images/e58bbd39a40d1f5ca391d0328c3206d7.png)

在将数据插入 Pinecone 后

## 步骤 2：使用 Langchain 进行问答服务

*注意：Langchain 最近更新非常快，下面代码使用的版本是* `*0.0.118*` *。*

![](../Images/84f252a1857dc61124514ae1c921ecfe.png)

OPL 堆栈中的数据流

上面的草图说明了推理阶段数据流动的方式：

+   用户提问：“2023 年最好的跑鞋是什么？”

+   问题使用 `ada-002` 模型转换为嵌入。

+   用户问题嵌入与 Pinecone 中存储的所有向量通过 `similarity_search` 函数进行比较，该函数检索最有可能回答问题的前三个文本块。

+   Langchain 然后将前三个文本块作为 `context`，与用户问题一起传递给 gpt-3.5（`ChatCompletion`）生成答案。

所有这些都可以通过不到 30 行代码实现：

```py
from langchain.vectorstores import Pinecone
from langchain.chains import VectorDBQAWithSourcesChain
from langchain.embeddings.openai import OpenAIEmbeddings

# 1\. Specify Pinecone as Vectorstore
# =======================================
# 1.1 get pinecone index name
index = pinecone.Index(index_name) #'outside-chatgpt'

# 1.2 specify embedding model
model_name = 'text-embedding-ada-002'
embed = OpenAIEmbeddings(
    document_model_name=model_name,
    query_model_name=model_name,
    openai_api_key=OPENAI_API_KEY
)

# 1.3 provides text_field
text_field = "text"

vectorstore = Pinecone(
    index, embed.embed_query, text_field
)

# 2\. Wrap the chain as a function
qa_with_sources = VectorDBQAWithSourcesChain.from_chain_type(
    llm=llm,
    chain_type="stuff",
    vectorstore=vectorstore
)
```

现在我们可以通过提出一个与徒步旅行相关的问题进行测试：“你能推荐一些加州湾区带水景的高级徒步旅行路线吗？”

![](../Images/20894ccdec7880545e25efa3b0beb363.png)

Langchain VectorDBQA 带来源

## 步骤 3：在 Streamlit 中构建我们的应用

在 Jupyter notebook 中验证逻辑有效后，我们可以将所有内容整合在一起，并使用 streamlit 构建前端。在我们的 streamlit 应用中，有两个 python 文件：

- `app.py`：前端的主要 python 文件，驱动应用

- `utils.py`：由 `app.py` 调用的支持函数

这就是我的 `utils.py` 看起来的样子：

```py
import pinecone
import streamlit as st
from langchain.chains import VectorDBQAWithSourcesChain
from langchain.chat_models import ChatOpenAI
from langchain.vectorstores import Pinecone
from langchain.embeddings.openai import OpenAIEmbeddings

# ------OpenAI: LLM---------------
OPENAI_API_KEY = st.secrets["OPENAI_KEY"]
llm = ChatOpenAI(
    openai_api_key=OPENAI_API_KEY,
    model_name='gpt-3.5-turbo',
    temperature=0.0
)

# ------OpenAI: Embed model-------------
model_name = 'text-embedding-ada-002'
embed = OpenAIEmbeddings(
    document_model_name=model_name,
    query_model_name=model_name,
    openai_api_key=OPENAI_API_KEY
)

# --- Pinecone ------
pinecone_api_key = st.secrets["PINECONE_API_KEY"]
pinecone.init(api_key=pinecone_api_key, environment="us-west1-gcp")
index_name = "outside-chatgpt"
index = pinecone.Index(index_name)
text_field = "text"
vectorstore = Pinecone(index, embed.embed_query, text_field)

#  ======= Langchain ChatDBQA with source chain =======
def qa_with_sources(query):
    qa = VectorDBQAWithSourcesChain.from_chain_type(
        llm=llm,
        chain_type="stuff",
        vectorstore=vectorstore
    )

    response = qa(query)
    return response 
```

最后，这就是我的 `app.py` 看起来的样子：

```py
 import os
import openai
from PIL import Image
from streamlit_chat import message
from utils import *

openai.api_key = st.secrets["OPENAI_KEY"]
# For Langchain
os.environ["OPENAI_API_KEY"] = openai.api_key

# ==== Section 1: Streamlit Settings ======
with st.sidebar:
    st.markdown("# Welcome to chatOutside 🙌")
    st.markdown(
        "**chatOutside** allows you to talk to version of **chatGPT** \n"
        "that has access to latest Outside content!  \n"
        )
    st.markdown(
        "Unlike chatGPT, chatOutside can't make stuff up\n"
        "and will answer from Outside knowledge base. \n"
    )
    st.markdown("👩‍🏫 Developer: Wen Yang")
    st.markdown("---")
    st.markdown("# Under The Hood 🎩 🐇")
    st.markdown("How to Prevent Large Language Model (LLM) hallucination?")
    st.markdown("- **Pinecone**: vector database for Outside knowledge")
    st.markdown("- **Langchain**: to remember the context of the conversation")

# Homepage title
st.title("chatOutside: Outside + ChatGPT")
# Hero Image
image = Image.open('VideoBkg_08.jpg')
st.image(image, caption='Get Outside!')

st.header("chatGPT 🤖")

# ====== Section 2: ChatGPT only ======
def chatgpt(prompt):
    res = openai.ChatCompletion.create(
        model='gpt-3.5-turbo',
        messages=[
            {"role": "system",
             "content": "You are a friendly and helpful assistant. "
                        "Answer the question as truthfully as possible. "
                        "If unsure, say you don't know."},
            {"role": "user", "content": prompt},
        ],
        temperature=0,
    )["choices"][0]["message"]["content"]

    return res

input_gpt = st.text_input(label='Chat here! 💬')
output_gpt = st.text_area(label="Answered by chatGPT:",
                          value=chatgpt(input_gpt), height=200)
# ========= End of Section 2 ===========

# ========== Section 3: chatOutside ============================
st.header("chatOutside 🏕️")

def chatoutside(query):
    # start chat with chatOutside
    try:
        response = qa_with_sources(query)
        answer = response['answer']
        source = response['sources']

    except Exception as e:
        print("I'm afraid your question failed! This is the error: ")
        print(e)
        return None

    if len(answer) > 0:
        return answer, source

    else:
        return None
# ============================================================

# ========== Section 4\. Display ChatOutside in chatbot style ===========
if 'generated' not in st.session_state:
    st.session_state['generated'] = []

if 'past' not in st.session_state:
    st.session_state['past'] = []

if 'source' not in st.session_state:
    st.session_state['source'] = []

def clear_text():
    st.session_state["input"] = ""

# We will get the user's input by calling the get_text function
def get_text():
    input_text = st.text_input('Chat here! 💬', key="input")
    return input_text

user_input = get_text()

if user_input:
    # source contain urls from Outside
    output, source = chatoutside(user_input)

    # store the output
    st.session_state.past.append(user_input)
    st.session_state.generated.append(output)
    st.session_state.source.append(source)

    # Display source urls
    st.write(source)

if st.session_state['generated']:
    for i in range(len(st.session_state['generated'])-1, -1, -1):
        message(st.session_state["generated"][i],  key=str(i))
        message(st.session_state['past'][i], is_user=True,
                avatar_style="big-ears",  key=str(i) + '_user')
```

# 3\. 生产考虑

好了，够了！

应用实际上已经很不错了。但是如果我们想进入生产环境，还有一些额外的事项需要考虑：

1.  在 Pinecone 中摄取新的和更新的数据：我们对文章数据进行了单次批量插入。实际上，每天都有新的文章添加到我们的网站中，而且一些字段可能会更新已经摄取到 Pinecone 的数据。这不是机器学习的问题，而是对媒体公司而言常常存在的问题：如何保持每个服务中的数据更新。潜在的解决方案是设置一个定时任务来定期执行插入和更新任务。有关如何并行发送插入操作的指令，如果我们可以使用 Django 和 Celery 的异步任务，这可能会非常有用。

1.  [Pinecone pod 存储](https://docs.pinecone.io/docs/limits#:~:text=Pod%20storage%20capacity,5M%20vectors%20with%20768%20dimensions.)的限制：当前应用使用的是 p1 pod，能够存储最多 1M 个 768 维向量，或者如果我们使用 OpenAI 的 `ada-002` 嵌入模型（其维度为 1536），则大约能存储 50 万个向量。

1.  流式处理功能以提高响应速度：为了减少用户感知的延迟，可能有助于向应用添加流式处理功能。这将模拟 chatGPT 按 token 返回生成的输出，而不是一次性显示整个响应。虽然这种功能在使用 LangChain 函数的 REST API 中有效，但对于我们来说，因为我们使用 GraphQL 而不是 REST，这带来了独特的挑战。

# 4\. 常见误解和问题

1.  **chatGPT 记住了截至 2021 年 9 月的互联网数据，并且基于记忆检索答案。**

    - 这并不是它的工作方式。训练之后，chatGPT 从内存中删除数据，并使用其 1750 亿个参数（权重）来预测最可能的 token（文本）。它不会基于记忆检索答案。这就是为什么如果你只是复制 chatGPT 生成的答案，你不太可能找到来自互联网的任何来源。

1.  **我们可以训练/微调/提示工程 chatGPT。**

    - 训练和微调大型语言模型意味着实际更改模型参数。你需要访问实际的模型文件，并根据你的具体使用情况指导模型。在大多数情况下，我们不会训练或微调 chatGPT。我们所需要的只是提示工程：为 chatGPT 提供额外的上下文，并让它根据这些上下文回答问题。

1.  **token 和词有什么区别？**

    -** Token 是一个词片段。100 个 token 大约等于 75 个词。例如，“Unbelievable” 是一个词，但包含 3 个 token（un, belie, able）。**

1.  **4000-token 限制意味着什么？**

    -** OpenAI gpt-3.5 的 token 限制为 4096，用于结合用户输入、上下文和响应。当使用 Langchain 的内存时，（用户问题 + 上下文 + 内存 + chatGPT 响应）使用的总词数需要少于 3000 个词（4000 个 token）。**

    - gpt-4 有更高的 token 限制，但也贵了 20 倍！（gpt-3.5：$0.002/1000 tokens；gpt-4：$0.045/1000 tokens，假设 500 个用于提示和 500 个用于完成）。

1.  **我是否必须使用像 Pinecone 这样的向量存储？**

    -** 不，Pinecone 不是唯一的向量存储选项。其他向量存储选项包括 Chroma、FAISS、Redis 等。此外，你并不总是需要向量存储。例如，如果你想为特定网站构建一个问答系统，你可以爬取网页并参考这个 [openai-cookbook-recipe](https://github.com/openai/openai-cookbook/blob/main/apps/web-crawl-q-and-a/web-qa.ipynb)。

# 告别的话

感谢你阅读这篇长文！如果你对使用 Langchain 有任何问题或建议，请随时联系我。

此外，我将参加 [LLM Bootcamp](https://fullstackdeeplearning.com/llm-bootcamp/)，希望能学习到更多关于将 LLMs 应用到生产中的最佳实践。如果你对这个话题感兴趣，请关注我未来的帖子！😃
