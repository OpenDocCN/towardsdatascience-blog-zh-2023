# 构建一个问答PDF聊天机器人

> 原文：[https://towardsdatascience.com/building-a-question-answering-pdf-chatbot-3e3b6372528c?source=collection_archive---------0-----------------------#2023-04-09](https://towardsdatascience.com/building-a-question-answering-pdf-chatbot-3e3b6372528c?source=collection_archive---------0-----------------------#2023-04-09)

## LangChain + OpenAI + Panel + HuggingFace

[](https://sophiamyang.medium.com/?source=post_page-----3e3b6372528c--------------------------------)[![Sophia Yang, Ph.D.](../Images/c133f918245ea4857dc46df3a07fc2b1.png)](https://sophiamyang.medium.com/?source=post_page-----3e3b6372528c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3e3b6372528c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3e3b6372528c--------------------------------) [Sophia Yang, Ph.D.](https://sophiamyang.medium.com/?source=post_page-----3e3b6372528c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fae9cae9cbcd2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-question-answering-pdf-chatbot-3e3b6372528c&user=Sophia+Yang%2C+Ph.D.&userId=ae9cae9cbcd2&source=post_page-ae9cae9cbcd2----3e3b6372528c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3e3b6372528c--------------------------------) ·6分钟阅读·2023年4月9日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3e3b6372528c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-question-answering-pdf-chatbot-3e3b6372528c&user=Sophia+Yang%2C+Ph.D.&userId=ae9cae9cbcd2&source=-----3e3b6372528c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3e3b6372528c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-question-answering-pdf-chatbot-3e3b6372528c&source=-----3e3b6372528c---------------------bookmark_footer-----------)

让我们来构建一个用于回答外部PDF文件问题的聊天机器人。通过5个简单的步骤，你应该能够构建一个像这样的问答PDF聊天机器人：

![](../Images/04e327db177f82e78b649034568bb913.png)

😊 想试用这个应用吗？我已经在Hugging Face上托管了这个应用：[https://sophiamyang-panel-pdf-qa.hf.space/LangChain_QA_Panel_App](https://sophiamyang-panel-pdf-qa.hf.space/LangChain_QA_Panel_App)

[💻](https://emojipedia.org/laptop/) 你可以在这里找到我所有的代码：

[https://huggingface.co/spaces/sophiamyang/Panel_PDF_QA/tree/main](https://huggingface.co/spaces/sophiamyang/Panel_PDF_QA/tree/main)

📒 你可以在这里找到我的笔记本文件：

[https://huggingface.co/spaces/sophiamyang/Panel_PDF_QA/blob/main/LangChain_QA_Panel_App.ipynb](https://huggingface.co/spaces/sophiamyang/Panel_PDF_QA/blob/main/LangChain_QA_Panel_App.ipynb)

好的，开始构建这个问答 PDF 聊天机器人吧！

# 第一步：设置

+   安装所需的包

```py
!pip install langchain openai chromadb pypdf panel notebook
```

+   导入所需的包

```py
import os 
from langchain.chains import RetrievalQA
from langchain.llms import OpenAI
from langchain.document_loaders import TextLoader
from langchain.document_loaders import PyPDFLoader
from langchain.indexes import VectorstoreIndexCreator
from langchain.text_splitter import CharacterTextSplitter
from langchain.embeddings import OpenAIEmbeddings
from langchain.vectorstores import Chroma
import panel as pn
```

+   定义 OpenAI API 密钥：在 OpenAI 创建一个账户并生成 API 密钥 [https://platform.openai.com/account](https://platform.openai.com/account)。请注意，OpenAI API 不是免费的。你需要在这里设置账单信息才能使用 OpenAI API。或者，你可以使用 HuggingFace Hub 或其他地方的模型。查看我之前的 [博客文章](/the-easiest-way-to-interact-with-language-models-4da158cfb5c5?sk=271c9c82a16282f93ef3df37f034babe) 和 [视频](https://www.youtube.com/watch?v=kmbS6FDQh7c) 了解如何使用其他模型。

```py
import os 
os.environ["OPENAI_API_KEY"] = "COPY AND PASTE YOUR API KEY HERE"
```

# 第一步：定义 Panel 小部件

为了制作这个应用程序，我们将需要以下小部件：

+   `file_input` 用于上传文件

+   `openaikey` 输入你的 OpenAI 密钥

+   `widgets` 结合了所有其他小部件：`prompt` 用于输入问题文本，`run_button` 用于点击并运行模型，`select_k` 用于选择相关块的数量，以及 `select_chain_type` 用于选择使用的链类型。

```py
file_input = pn.widgets.FileInput(width=300)

openaikey = pn.widgets.PasswordInput(
    value="", placeholder="Enter your OpenAI API Key here...", width=300
)
prompt = pn.widgets.TextEditor(
    value="", placeholder="Enter your questions here...", height=160, toolbar=False
)
run_button = pn.widgets.Button(name="Run!")

select_k = pn.widgets.IntSlider(
    name="Number of relevant chunks", start=1, end=5, step=1, value=2
)
select_chain_type = pn.widgets.RadioButtonGroup(
    name='Chain type', 
    options=['stuff', 'map_reduce', "refine", "map_rerank"]
)

widgets = pn.Row(
    pn.Column(prompt, run_button, margin=5),
    pn.Card(
        "Chain type:",
        pn.Column(select_chain_type, select_k),
        title="Advanced settings", margin=10
    ), width=600
)
```

![](../Images/13977854847cc63fd72ad272b5161716.png)

# 第二步：定义问答函数

我们将使用 LangChain 和 OpenAI 进行问答。在 LangChain 中至少有四种方式进行问答。查看我之前的博客文章和视频，了解 LangChain 中的四种问答方式。

在这个函数中，我们使用了 RetrievalQA 来检索相关的文本块，然后仅将这些文本块传递给语言模型。我们定义了四个参数：—

+   `file` 是你感兴趣的输入 PDF 文件

+   `query` 是你的问题

+   `chain_type` 让你将链类型定义为四种选项之一：“stuff”、“map reduce”、“refine”、“map_rerank”。查看我之前的博客文章以了解每种链类型。

+   `k` 定义了相关文本块的数量

```py
def qa(file, query, chain_type, k):
    # load document
    loader = PyPDFLoader(file)
    documents = loader.load()
    # split the documents into chunks
    text_splitter = CharacterTextSplitter(chunk_size=1000, chunk_overlap=0)
    texts = text_splitter.split_documents(documents)
    # select which embeddings we want to use
    embeddings = OpenAIEmbeddings()
    # create the vectorestore to use as the index
    db = Chroma.from_documents(texts, embeddings)
    # expose this index in a retriever interface
    retriever = db.as_retriever(search_type="similarity", search_kwargs={"k": k})
    # create a chain to answer questions 
    qa = RetrievalQA.from_chain_type(
        llm=OpenAI(), chain_type=chain_type, retriever=retriever, return_source_documents=True)
    result = qa({"query": query})
    print(result['result'])
    return result
```

下面是一个使用此函数的示例：

![](../Images/7e81186b8414586a4162c4160fc25212.png)

# 第三步：将输出显示为 Panel 对象

上述函数返回语言模型的查询、结果和源文档。为了在 Panel 应用中显示结果和源文档，我们需要将它们转换为 Panel 对象。在 `qa_result` 函数中，我将查询（`prompt_text`）、结果（`result[“result”]`）和源文档（`result["source_documents"]`）追加到一个名为 `convos` 的列表中，然后将其转换为 Panel 对象 `pn.Column(*convos)`。

```py
convos = []  # store all panel objects in a list

def qa_result(_):
    os.environ["OPENAI_API_KEY"] = openaikey.value

    # save pdf file to a temp file 
    if file_input.value is not None:
        file_input.save("/.cache/temp.pdf")

        prompt_text = prompt.value
        if prompt_text:
            result = qa(file="/.cache/temp.pdf", query=prompt_text, chain_type=select_chain_type.value, k=select_k.value)
            convos.extend([
                pn.Row(
                    pn.panel("\U0001F60A", width=10),
                    prompt_text,
                    width=600
                ),
                pn.Row(
                    pn.panel("\U0001F916", width=10),
                    pn.Column(
                        result["result"],
                        "Relevant source text:",
                        pn.pane.Markdown('\n--------------------------------------------------------------------\n'.join(doc.page_content for doc in result["source_documents"]))
                    )
                )
            ])
            #return convos
    return pn.Column(*convos, margin=15, width=575, min_height=400)
```

# 第四步：运行按钮执行函数

接下来，我想让 `qa_result` 函数具有交互性，也就是说，当我点击运行按钮时，我希望运行这个函数。这在 Panel 中非常简单。我们使用 `pn.bind` 将 `qa_result` 函数绑定到 `run_button` 小部件。请注意，在 `qa_result` 函数中，我们传入了一个参数 `_`，实际上在函数中并未使用。这只是一个占位符，让 `pn.bind` 知道我们可以将一个小部件绑定到这个函数上。

```py
qa_interactive = pn.panel(
    pn.bind(qa_result, run_button),
    loading_indicator=True,
)
```

然后我想将输出格式化到一个框中，因此我使用了 `pn.WidgetBox`：

```py
output = pn.WidgetBox('*Output will show up here:*', qa_interactive, width=630, scroll=True)
```

![](../Images/281ed418e8c7bb749853ea3c8785e738.png)

# 第 5 步：定义布局

现在我们可以使用 `pn.Column` 将所有小部件和输出组合成一列。你可以运行 `panel serve LangChain_QA_Panel_App.ipynb` 来服务这个应用程序。现在你应该有一个可以运行的应用程序！

```py
# layout
pn.Column(
    pn.pane.Markdown("""
    ## \U0001F60A! Question Answering with your PDF file

    Step 1: Upload a PDF file \n
    Step 2: Enter your OpenAI API key. This costs $$. You will need to set up billing info at [OpenAI](https://platform.openai.com/account). \n
    Step 3: Type your question at the bottom and click "Run" \n

    """),
    pn.Row(file_input,openaikey),
    output,
    widgets

).servable()
```

# 最终步骤：托管到 Hugging Face Space

如你所见，在我的视频开始时，我在 Hugging Face 上托管了我的应用：[https://sophiamyang-panel-pdf-qa.hf.space/LangChain_QA_Panel_App](https://sophiamyang-panel-pdf-qa.hf.space/LangChain_QA_Panel_App)

查看我之前的 [博客文章](/how-to-deploy-a-panel-app-to-hugging-face-using-docker-6189e3789718?sk=9c2bbd9bdbc3917e39dbd1fc9d1a5771) 和 [视频](https://www.youtube.com/watch?v=QQYq5rHgsa4)，了解如何在 Hugging Face 上托管 Panel 应用。

这是我为这个应用程序所做的：

+   我上传了我的 [Jupyter Notebook 文件](https://huggingface.co/spaces/sophiamyang/Panel_PDF_QA/blob/main/LangChain_QA_Panel_App.ipynb)

+   我创建了一个 [Dockerfile](https://huggingface.co/spaces/sophiamyang/Panel_PDF_QA/blob/main/LangChain_QA_Panel_App.ipynb)。这个应用程序的特别之处在于，它为 Chroma 向量数据库创建了一个 .chroma 目录。因此，我们需要创建这个目录并赋予它权限：`RUN mkdir .chroma` 和 `RUN chomd 777 .chroma`。

+   我在一个 `[requirements.txt](https://huggingface.co/spaces/sophiamyang/Panel_PDF_QA/blob/main/requirements.txt)` [文件](https://huggingface.co/spaces/sophiamyang/Panel_PDF_QA/blob/main/requirements.txt) 中包含了所有所需的包

# 结论

希望在这篇文章结束时，你能了解如何使用 LangChain、OpenAI 和 Panel 构建一个问答 PDF 聊天机器人，并知道如何将应用程序部署到 Hugging Face Space。

# 致谢：

感谢 Jim Bednar 和 Philipp Rudiger 的指导和反馈！

![](../Images/76672d3fd26b280ee1d8afa06e636c92.png)

图片由 [Volodymyr Hryshchenko](https://unsplash.com/de/@lunarts?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

. . .

由 [Sophia Yang](https://www.linkedin.com/in/sophiamyang/) 于 2023 年 4 月 8 日发布

Sophia Yang 是一位高级数据科学家。通过 [LinkedIn](https://www.linkedin.com/in/sophiamyang/)、[Twitter](https://twitter.com/sophiamyang) 和 [YouTube](https://www.youtube.com/SophiaYangDS) 联系我，并加入 DS/ML [书友会](https://dsbookclub.github.io/) ❤️
