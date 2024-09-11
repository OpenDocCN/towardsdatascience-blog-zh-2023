# 使用 LangChain 在 CPU 上的检索增强生成（RAG）推理引擎

> 原文：[https://towardsdatascience.com/retrieval-augmented-generation-rag-inference-engines-with-langchain-on-cpus-d5d55f398502?source=collection_archive---------1-----------------------#2023-12-05](https://towardsdatascience.com/retrieval-augmented-generation-rag-inference-engines-with-langchain-on-cpus-d5d55f398502?source=collection_archive---------1-----------------------#2023-12-05)

![](../Images/1e2477ae7cf2785a1000e1cf8f7cdf54.png)

使用 Nightcafe 创建 — 作者所有

## 探索 AI 应用中的规模、保真度和延迟，利用 RAG

[](https://eduand-alvarez.medium.com/?source=post_page-----d5d55f398502--------------------------------)[![Eduardo Alvarez](../Images/afa0ad855c8ec2e977ebbe60dc3e77a4.png)](https://eduand-alvarez.medium.com/?source=post_page-----d5d55f398502--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d5d55f398502--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d5d55f398502--------------------------------) [Eduardo Alvarez](https://eduand-alvarez.medium.com/?source=post_page-----d5d55f398502--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe49cc416a8ef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fretrieval-augmented-generation-rag-inference-engines-with-langchain-on-cpus-d5d55f398502&user=Eduardo+Alvarez&userId=e49cc416a8ef&source=post_page-e49cc416a8ef----d5d55f398502---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d5d55f398502--------------------------------) ·13 min read·2023年12月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd5d55f398502&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fretrieval-augmented-generation-rag-inference-engines-with-langchain-on-cpus-d5d55f398502&user=Eduardo+Alvarez&userId=e49cc416a8ef&source=-----d5d55f398502---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd5d55f398502&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fretrieval-augmented-generation-rag-inference-engines-with-langchain-on-cpus-d5d55f398502&source=-----d5d55f398502---------------------bookmark_footer-----------)

尽管检索增强生成（RAG）在其应用于基于聊天的LLM方面得到了广泛的覆盖，但在本文中，我们旨在从不同的角度来审视它，并分析它作为强大操作工具的能力。我们还将提供一个实用的动手示例，以获得RAG基础应用的实践经验。文章结束时，你将对RAG有一个独特的视角——理解它在生产规模LLM部署中的可扩展推理中的角色和潜力。

# 但首先，让我们重新理解推理。

推理是将数据转化为预测的过程。机器学习生命周期的这一部分通常由数据管道承载，负责处理预处理和后处理任务。让我们评估一个实际的例子，考虑一个音乐流媒体服务的推荐系统，如图1所示。当用户访问流媒体平台时，应用程序界面上会展示一个精心策划的前10首歌曲列表。负责这个列表的推荐系统依赖于一个经过训练的模型和强大的数据管道，以确保高质量的结果。

![](../Images/75837d8a56126276ae978236c03b5275.png)

图1\. 简单的示意图，展示了支持“前10推荐歌曲列表”功能的推荐系统 — 图片来源于作者

预处理阶段，由图中的黄色框表示，对于确保模型的预测与用户的独特品味紧密对齐至关重要。从用户最近播放的250首歌曲开始，数据管道处理数据并生成一组上下文特征，然后将其传递给经过训练的模型进行推理。推理步骤预测该用户可能喜欢什么，并生成一个输出，传递到后处理阶段（橙色表示）。在最后一步中，模型的顶级推荐会通过附加的元数据进行丰富——专辑封面、歌曲标题、艺术家名字以及排名。这些信息随后在用户界面上显示供用户使用。

在上述工作流程中，推理步骤在应用程序拓扑中与用户的接近程度是显而易见的。与数据收集和模型优化等在后台运行的其他AI生命周期组件不同，推理引擎位于前线，与用户界面（UI）和用户体验（UX）紧密互动。

![](../Images/ebe160f4764522808d7b8d4905ab88e4.png)

图2\. AI系统的各个组件及其与用户体验的接近程度的示意图。 — 图片来源于作者

我们可以使用上述图（图2）来说明AI生命周期的各个组件与用户的接近程度。虽然许多组件如数据收集和标注都在“幕后”，但推理引擎则是AI内部过程与最终用户之间的关键桥梁。它不仅仅是另一个后台机制，它是用户与应用程序之间切实体验的核心部分。

鉴于推理引擎在塑造用户体验中的关键作用，推理引擎 — 包括推理过程及其外围组件，如预处理/后处理、API管理和计算管理 — 必须无缝运行。为了建立推理引擎质量的边界条件，我引入了“推理质量（IQ）三角形”，如图3所示。这个定性图形突出了在提升推理工作负载性能时需要关注的三个关键方面：

+   **延迟：** 如果推理引擎响应所需的时间较短，它将减少应用程序的开销，从而改善用户体验。

+   **保真度：** 推理需要提供用户可以信任并感到自信的答案。这包括但不限于确保响应的高准确性和减少虚假信息。

+   **可扩展性：** 随着AI系统负载的波动，扩展基础设施的能力是优化成本和实现计算资源正确配置的关键。

![](../Images/c7167e48daf98f92fa5db84d6f8758ac.png)

图3\. 一个简单的模型，用于对齐高质量推理引擎的三个关键要素。重点关注操作卓越，AI系统需要在保持低延迟的同时进行扩展，并提供高保真度的见解。 — 作者提供的图像

随着文章的深入，我们将参考IQ三角形深入探讨这三个组件——延迟、保真度和可扩展性——如何与RAG工作负载很好地对齐。

# 检索增强生成简介

检索增强生成，也称为RAG，是由Piktus等人（2021年）在*检索增强生成用于知识密集型NLP任务*中首次引入的技术，之后在各种框架和应用中得到了适应。RAG属于上下文学习技术的范畴，重点是为预训练模型提供额外的知识，以提高其响应的质量。

RAG的特点在于从相关数据源中智能检索额外的信息，通常使用相似性搜索等算法的向量数据库。检索到的数据与用户的查询结合，丰富了提供给生成模型的输入。标准RAG工作流程如图4所示。

![](../Images/fdf37eb0af1e796fd7cf4098a76f8286.png)

图4\. 简单的RAG工作流程图 — 作者提供的图像

为了理解 RAG 的真正价值，让我们考虑一个实际场景：一家大型公司的金融分析师（图 5）正在进行季度收益报告的编制任务。以传统方式执行此任务将是一项耗时的工作。基于 LLM 的应用程序提供了显著的效率提升，但有一个问题——需要最新的专有信息，而在训练基础开源模型时这些信息并不可用。这可以通过微调部分解决，但商业操作的快速节奏意味着这个过程需要持续的微调以保持模型的最新。

![](../Images/1ae0610f4167cb96bd81fe0f231940fe.png)

图 5\. 图 6 的改编版本。在这一改编中，涉及金融分析师使用 RAG 的场景被展示出来，以说明基于专有数据构建 RAG 系统的工作流程的有效性。—— 作者提供的图像

RAG 通过检索相关的实时数据来应对这些挑战，使模型能够动态更新最新信息。这对底层数据库的质量提出了压力，但至少数据管理比 LLM 幻觉和神经网络推断更具经验性和可预测性。作为额外的好处，这种方法可以保护组织数据基础设施中的敏感数据。

许多应用 AI 工程师认为，应该转向一种混合策略，专注于周期性微调和强大的 RAG 管道。在实践中，这种策略与特定领域任务的对齐得到了改善，并增加了在数据环境快速演变的应用中的模型相关性。

*在实践示例中，你可以切换 RAG 的开/关，以查看提供和不提供智能检索机制上下文时，预训练模型响应质量的影响。见图 10。*

# 支持高质量推理引擎的操作性 RAG 系统

现在我们对 RAG 及其在基于 LLM 的应用中的作用有了基础理解，我们将专注于实施这些系统的实际和操作方面。

如承诺的，让我们重新审视 IQ 三角形（图 3），它突显了高质量操作推理引擎的三个关键方面。我们将使用下图所示的技术栈（图 6）来分析解决这三个方面——可扩展性、延迟和保真度——的机会，该技术栈专注于由 RAG 管道组成并在 CPU 上进行高度优化的模型的推理引擎。

![](../Images/449e2b8fab0d5a6410796b6ab8cc9162.png)

图 6\. 提议的推理引擎技术栈 —— 作者提供的图像

## RAG 的架构优势

基于 RAG 的应用带来了显著的架构优势。从**可扩展性**的角度来看，管道中所有以数据为中心的组件都会汇聚到一个（或几个）向量数据库（见图 7），允许 RAG 的新数据优势随着用户请求的增加/减少而良好地扩展。这种统一的方法可以显著提高对特定领域任务的**准确度**响应，同时大大简化数据治理。

![](../Images/ea79e05dd5e7c389c2453a539c83a288.png)

图 7\. 简单数据流图，展示了基于 RAG 的 AI 系统中数据的流动 — 作者提供的图像

## 优化模型：效率与性能

模型可以通过模型压缩和参数高效微调技术实现更小的计算和环境足迹。虽然微调可以帮助将模型调整到特定任务，从而提升其预测准确性（**准确度**），但像量化这样的压缩方法可以缩小模型的大小，显著提高推理**延迟**。这些精简和调整过的模型在数据中心部署更为便捷，并且能够在边缘计算中实现 AI 应用，开辟了各种创新用例的可能性。

![](../Images/4062887d1a54b6abaab8b38f88894567.png)

图 8 — 作者提供的图像

## 支持 RAG 的 CPU

对于涉及复杂逻辑的工作流程，例如 RAG，CPU 因其普及性和成本效益而脱颖而出。这提高了规模，因为几乎任何组织都可以在云中访问企业级 CPU，这与更难获得的专用加速器不同。

现代 CPU 还配备了低级别的优化——例如，Intel 第四代 Xeon 处理器中的 Intel 高级矩阵扩展——这些优化改善了深度学习训练和推理阶段的内存管理和矩阵运算。它们对较低精度数据类型（如 bf16 和 int8）的支持使其非常适合在推理过程中实现低**延迟**。

此外，CPU 与 RAG 管道（见图 9）中多个组件的兼容性，包括向量数据库和智能搜索（例如，相似度搜索），简化了基础设施管理，使得**规模化**部署更加直接和高效。

![](../Images/063e1262526782e9e5f47c365f685326.png)

图 9\. 图 7 的适配。展示了 CPU 支持 RAG 系统各部分的能力。 — 作者提供的图像

*在继续之前，我必须披露我与 Intel 的关系以及下面使用的产品。作为 Intel 的高级 AI 工程师，以下实践示例在 Intel 开发者云（IDC）上运行。我们将使用 IDC 作为* 访问计算的免费且便利的方式，以便获得*与本文所述概念的实际经验。*

# 实践示例：在 Intel 开发者云（IDC）上实施 RAG 与 LangChain

要跟随以下的动手示例，请在[Intel Developer Cloud](https://bit.ly/3sTXnHt)上创建一个免费账户，并导航到“Training and Workshops”页面。在*Gen AI Essentials*部分中，选择**Retrieval Augmented Generation (RAG) with LangChain**选项。按照网页上的说明启动JupyterLab窗口，并自动加载包含所有示例代码的笔记本。

该笔记本包含详细的文档字符串和代码描述。本文将讨论高级机制，并为特定功能提供背景。

## 设置依赖项

我们首先将所有必需的软件包安装到基础环境中。你可以创建自己的conda环境，但这是一个快速简便的开始方法。

```py
import sys
import os
!{sys.executable} -m pip install langchain==0.0.335 --no-warn-script-location > /dev/null
!{sys.executable} -m pip install pygpt4all==1.1.0 --no-warn-script-location > /dev/null
!{sys.executable} -m pip install gpt4all==1.0.12 --no-warn-script-location > /dev/null
!{sys.executable} -m pip install transformers==4.35.1 --no-warn-script-location > /dev/null
!{sys.executable} -m pip install datasets==2.14.6 --no-warn-script-location > /dev/null
!{sys.executable} -m pip install tiktoken==0.4.0 --no-warn-script-location > /dev/null
!{sys.executable} -m pip install chromadb==0.4.15 --no-warn-script-location > /dev/null
!{sys.executable} -m pip install sentence_transformers==2.2.2 --no-warn-script-location > /dev/null
```

这些命令将把所有必要的软件包安装到你的基础环境中。

## 数据和模型

我们将使用GPT4All项目中的Falcon 7B（gpt4all-falcon-q4_0）的量化版本。你可以在[GPT4ALL页面](https://gpt4all.io/index.html)的“Model Explorer”部分了解更多关于此模型的信息。该模型已存储在磁盘上，以简化模型访问过程。

以下逻辑从一个名为[FunDialogues](https://huggingface.co/FunDialogues)的Hugging Face项目下载可用的数据集。选择的数据将通过嵌入模型处理，并在后续步骤中放入我们的向量数据库。

```py
def download_dataset(self, dataset):
        """
        Downloads the specified dataset and saves it to the data path.

        Parameters
        ----------
        dataset : str
            The name of the dataset to be downloaded.
        """
        self.data_path = dataset + '_dialogues.txt'

        if not os.path.isfile(self.data_path):

            datasets = {"robot maintenance": "FunDialogues/customer-service-robot-support", 
                        "basketball coach": "FunDialogues/sports-basketball-coach", 
                        "physics professor": "FunDialogues/academia-physics-office-hours",
                        "grocery cashier" : "FunDialogues/customer-service-grocery-cashier"}

            # Download the dialogue from hugging face
            dataset = load_dataset(f"{datasets[dataset]}")
            # Convert the dataset to a pandas dataframe
            dialogues = dataset['train']
            df = pd.DataFrame(dialogues, columns=['id', 'description', 'dialogue'])
            # Print the first 5 rows of the dataframe
            df.head()
            # only keep the dialogue column
            dialog_df = df['dialogue']

            # save the data to txt file
            dialog_df.to_csv(self.data_path, sep=' ', index=False)
        else:
            print('data already exists in path.')
```

在上面的代码片段中，你可以从4种不同的合成数据集中进行选择：

+   **机器人维护**：技术人员和客户支持代理在排除机器人手臂故障时的对话。

+   **篮球教练**：篮球教练和球员在比赛中的对话。

+   **物理学教授**：学生和物理学教授在办公时间的对话。

+   **杂货店收银员**：杂货店收银员和顾客的对话

## 配置模型

LangChain API中的GPT4ALL扩展负责将模型加载到内存中，并建立各种参数，如：

+   **model_path**：这一行指定了预训练模型的文件路径。

+   **n_threads**：设置要使用的线程数，这可能会影响并行处理或推理速度。这对于多核系统特别相关。

+   **max_tokens**：限制输入或输出序列中的令牌（单词或子词）数量，确保输入到模型中的数据或模型生成的数据不会超出此长度。

+   **repeat_penalty**：该参数可能会惩罚模型输出中的重复内容。大于1.0的值可以防止模型生成重复的序列。

+   **n_batch**：指定处理数据的批量大小。这有助于优化处理速度和内存使用。

+   **top_k**：定义模型生成过程中的“top-k”采样策略。生成文本时，模型将仅考虑最可能的前k个下一个令牌。

```py
def load_model(self, n_threads, max_tokens, repeat_penalty, n_batch, top_k, temp):
        """
        Loads the model with specified parameters for parallel processing.

        Parameters
        ----------
        n_threads : int
            The number of threads for parallel processing.
        max_tokens : int
            The maximum number of tokens for model prediction.
        repeat_penalty : float
            The penalty for repeated tokens in generation.
        n_batch : int
            The number of batches for processing.
        top_k : int
            The number of top k tokens to be considered in sampling.
        """
        # Callbacks support token-wise streaming
        callbacks = [StreamingStdOutCallbackHandler()]
        # Verbose is required to pass to the callback manager

        self.llm = GPT4All(model=self.model_path, callbacks=callbacks, verbose=False,
                           n_threads=n_threads, n_predict=max_tokens, repeat_penalty=repeat_penalty, 
                           n_batch=n_batch, top_k=top_k, temp=temp)
```

## 使用ChromaDB构建向量数据库

Chroma向量数据库是我们RAG设置的核心部分，我们在这里高效地存储和管理数据。以下是我们构建它的方法：

```py
def build_vectordb(self, chunk_size, overlap):
        """
        Builds a vector database from the dataset for retrieval purposes.

        Parameters
        ----------
        chunk_size : int
            The size of text chunks for vectorization.
        overlap : int
            The overlap size between chunks.
        """
        loader = TextLoader(self.data_path)
        # Text Splitter
        text_splitter = RecursiveCharacterTextSplitter(chunk_size=chunk_size, chunk_overlap=overlap)
        # Embed the document and store into chroma DB
        self.index = VectorstoreIndexCreator(embedding= HuggingFaceEmbeddings(), text_splitter=text_splitter).from_loaders([loader])
```

## 执行检索机制

在接收到用户查询后，我们使用相似性搜索在向量数据库中查找相似数据。一旦找到k个匹配结果，它们会被检索并用于为用户的查询添加上下文。我们使用`PromptTemplate`函数构建模板，并将用户的查询与检索到的上下文嵌入其中。一旦模板填充完毕，我们继续进行推理组件。

```py
def retrieval_mechanism(self, user_input, top_k=1, context_verbosity = False, rag_off= False):
        """
        Retrieves relevant document snippets based on the user's query.

        Parameters
        ----------
        user_input : str
            The user's input or query.
        top_k : int, optional
            The number of top results to return, by default 1.
        context_verbosity : bool, optional
            If True, additional context information is printed, by default False.
        rag_off : bool, optional
            If True, disables the retrieval-augmented generation, by default False.
        """

        self.user_input = user_input
        self.context_verbosity = context_verbosity

        # perform a similarity search and retrieve the context from our documents
        results = self.index.vectorstore.similarity_search(self.user_input, k=top_k)
        # join all context information into one string 
        context = "\n".join([document.page_content for document in results])
        if self.context_verbosity:
            print(f"Retrieving information related to your question...")
            print(f"Found this content which is most similar to your question: {context}")

        if rag_off:
            template = """Question: {question}
            Answer: This is the response: """
            self.prompt = PromptTemplate(template=template, input_variables=["question"])
        else:     
            template = """ Don't just repeat the following context, use it in combination with your knowledge to improve your answer to the question:{context}

            Question: {question}
            """
            self.prompt = PromptTemplate(template=template, input_variables=["context", "question"]).partial(context=context)
```

LangChain `LLMChain`工具用于根据用户传递的查询和配置的模板执行推理。结果将返回给用户。

```py
 def inference(self):
        """
        Performs inference to generate a response based on the user's query.

        Returns
        -------
        str
            The generated response.
        """

        if self.context_verbosity:
            print(f"Your Query: {self.prompt}")

        llm_chain = LLMChain(prompt=self.prompt, llm=self.llm)
        print("Processing the information with gpt4all...\n")
        response = llm_chain.run(self.user_input)

        return  response 
```

## 互动实验

为了帮助你快速入门，笔记本包含了集成的ipywidget组件。你必须运行笔记本中的所有单元格以启用这些组件。我们鼓励你调整参数，评估对系统响应的延迟和保真度的影响。记住，这只是一个起点和RAG能力的基本演示。

![](../Images/6ff90f892d94b23b4608c11f563e7e04.png)

图10。在这个示例中，我们快速体验了RAG的强大，清楚地看到RAG提供的额外上下文的好处，这有助于为用户的问题——“我的机器人没有开启，你能帮我吗？”——提供有用的答案。启用了RAG的输出提供了有效的建议，而没有RAG的原始模型仅仅提供了一个礼貌的询问，对用户帮助不大。

# 总结与讨论

没有人愿意与响应虚假信息的慢速、不稳定聊天机器人互动。技术栈组合的选择繁多，帮助开发者避免构建产生糟糕用户体验的系统。本文从一个能够提供规模、保真度和延迟效益的技术栈角度，解读了推理引擎质量对用户体验的重要性。RAG、CPU和模型优化技术的组合涵盖了IQ三角形的各个方面（图3），很好地满足了基于LLM的AI聊天应用程序的需求。

**一些令人兴奋的尝试包括：**

+   编辑`retrieval_mechanism`方法中的提示模板，以便更好地设计提示，与检索到的上下文配合。

+   调整各种模型和RAG特定参数，评估对推理延迟和响应质量的影响。

+   添加对你的领域有意义的新数据集，并测试使用RAG构建AI聊天应用程序的可行性。

+   这个示例中的模型（gpt4all-falcon-q4_0）未针对Xeon处理器进行优化。探索使用优化了CPU平台的模型，并评估推理延迟的益处。

***感谢你的阅读！别忘了关注*** [***我的个人资料以获取更多类似的文章***](https://eduand-alvarez.medium.com/) ***！***
