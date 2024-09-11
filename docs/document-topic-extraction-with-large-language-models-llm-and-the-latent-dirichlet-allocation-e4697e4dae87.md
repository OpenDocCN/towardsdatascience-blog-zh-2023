# 使用大型语言模型（LLM）和潜在狄利克雷分配（LDA）算法的文档主题提取

> 原文：[https://towardsdatascience.com/document-topic-extraction-with-large-language-models-llm-and-the-latent-dirichlet-allocation-e4697e4dae87?source=collection_archive---------0-----------------------#2023-09-13](https://towardsdatascience.com/document-topic-extraction-with-large-language-models-llm-and-the-latent-dirichlet-allocation-e4697e4dae87?source=collection_archive---------0-----------------------#2023-09-13)

## 一份关于如何高效地从大型文档中提取主题的指南，使用大型语言模型（LLM）和潜在狄利克雷分配（LDA）算法。

[](https://medium.com/@antoniojimzc?source=post_page-----e4697e4dae87--------------------------------)[![安东尼奥·吉门内斯·卡巴列罗](../Images/978c40f181cd13666a00584cb18dc383.png)](https://medium.com/@antoniojimzc?source=post_page-----e4697e4dae87--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e4697e4dae87--------------------------------)[![数据科学进展](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e4697e4dae87--------------------------------) [安东尼奥·吉门内斯·卡巴列罗](https://medium.com/@antoniojimzc?source=post_page-----e4697e4dae87--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F990fab5876ca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdocument-topic-extraction-with-large-language-models-llm-and-the-latent-dirichlet-allocation-e4697e4dae87&user=Antonio+Jimenez+Caballero&userId=990fab5876ca&source=post_page-990fab5876ca----e4697e4dae87---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e4697e4dae87--------------------------------) · 8 分钟阅读 · 2023年9月13日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe4697e4dae87&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdocument-topic-extraction-with-large-language-models-llm-and-the-latent-dirichlet-allocation-e4697e4dae87&user=Antonio+Jimenez+Caballero&userId=990fab5876ca&source=-----e4697e4dae87---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe4697e4dae87&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdocument-topic-extraction-with-large-language-models-llm-and-the-latent-dirichlet-allocation-e4697e4dae87&source=-----e4697e4dae87---------------------bookmark_footer-----------)![](../Images/e5eba3311ca460efacf3087a66f791e0.png)

照片由 [亨利·贝](https://unsplash.com/@henry_be?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，刊登于 [Unsplash](https://unsplash.com/photos/lc7xcWebECc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## 介绍

我正在为[与PDF文件聊天](https://github.com/a-jimenezc/pdf_chat)开发一个Web应用程序，能够处理超过1000页的大型文档。但在与文档进行对话之前，我希望应用程序能够给用户提供主要主题的简要摘要，这样开始交互会更容易一些。

一种方法是使用[LangChain](https://python.langchain.com/docs/get_started/introduction.html)来总结文档，就像在其[**文档**](https://python.langchain.com/docs/use_cases/summarization)中展示的那样。然而，问题在于高计算成本，以及由此带来的金钱成本。一个千页文档包含大约250,000个单词，每个单词都需要输入到LLM中。此外，结果还必须进一步处理，如使用映射-减少方法。使用4k上下文的gpt-3.5 Turbo进行摘要的保守成本估算超过1美元每份文档。即使使用[**非官方HuggingChat API**](https://github.com/Soulter/hugging-chat-api)等免费资源，所需的API调用次数也会是一种滥用。因此，我需要另一种方法。

## LDA来拯救

潜在狄利克雷分配算法对于这个任务是一个自然选择。该算法接受一组“文档”（在这个上下文中，“文档”指的是一段文本）并返回每个“文档”的主题列表，以及与每个主题相关联的单词列表。对于我们的情况来说，重要的是与每个主题相关联的单词列表。这些单词列表编码了文件的内容，因此它们可以被输入到LLM中以提示摘要。我推荐[**这篇文章**](/end-to-end-topic-modeling-in-python-latent-dirichlet-allocation-lda-35ce4ed6b3e0)以获取关于该算法的详细解释。

在能够获得高质量结果之前，我们需要解决两个关键问题：为LDA算法选择超参数和确定输出格式。最重要的超参数是主题数量，因为它对最终结果有最显著的影响。至于输出格式，一个效果不错的选择是嵌套的项目符号列表。在这种格式中，每个主题都表示为一个带有进一步描述该主题的子条目的项目符号列表。关于为什么这样做有效，我认为通过这种格式，模型可以专注于从列表中提取内容，而不必处理用连接词和关系连接起来的段落的复杂性。

## 实施

我在[**Google Colab**](https://colab.research.google.com/drive/1_dY4EGaQfz0NIFJlsCc981rNXzmJEK8U?usp=sharing)中实现了代码。所需的库包括用于LDA的gensim，用于PDF处理的pypdf，用于单词处理的nltk，以及用于其模板和与OpenAI API接口的LangChain。

```py
import gensim
import nltk
from gensim import corpora
from gensim.models import LdaModel
from gensim.utils import simple_preprocess
from nltk.corpus import stopwords
from pypdf import PdfReader
from langchain.chains import LLMChain
from langchain.prompts import ChatPromptTemplate
from langchain.llms import OpenAI
```

接下来，我定义了一个实用函数，*preprocess*，来帮助处理输入文本。它会移除停用词和短词。

```py
def preprocess(text, stop_words):
    """
    Tokenizes and preprocesses the input text, removing stopwords and short 
    tokens.

    Parameters:
        text (str): The input text to preprocess.
        stop_words (set): A set of stopwords to be removed from the text.
    Returns:
        list: A list of preprocessed tokens.
    """
    result = []
    for token in simple_preprocess(text, deacc=True):
        if token not in stop_words and len(token) > 3:
            result.append(token)
    return result
```

第二个函数，*get_topic_lists_from_pdf*，实现了**LDA**部分的代码。它接受PDF文件的路径、主题数量和每个主题的字数，并返回一个列表。此列表中的每个元素包含与每个主题相关的单词列表。在这里，我们将PDF文件中的每一页视为一个“文档”。

```py
def get_topic_lists_from_pdf(file, num_topics, words_per_topic):
    """
    Extracts topics and their associated words from a PDF document using the 
    Latent Dirichlet Allocation (LDA) algorithm.

    Parameters:
        file (str): The path to the PDF file for topic extraction.
        num_topics (int): The number of topics to discover.
        words_per_topic (int): The number of words to include per topic.

    Returns:
        list: A list of num_topics sublists, each containing relevant words 
        for a topic.
    """
    # Load the pdf file
    loader = PdfReader(file)

    # Extract the text from each page into a list. Each page is considered a document
    documents= []
    for page in loader.pages:
        documents.append(page.extract_text())

    # Preprocess the documents
    nltk.download('stopwords')
    stop_words = set(stopwords.words(['english','spanish']))
    processed_documents = [preprocess(doc, stop_words) for doc in documents]

    # Create a dictionary and a corpus
    dictionary = corpora.Dictionary(processed_documents)
    corpus = [dictionary.doc2bow(doc) for doc in processed_documents]

    # Build the LDA model
    lda_model = LdaModel(
        corpus, 
        num_topics=num_topics, 
        id2word=dictionary, 
        passes=15
        )

    # Retrieve the topics and their corresponding words
    topics = lda_model.print_topics(num_words=words_per_topic)

    # Store each list of words from each topic into a list
    topics_ls = []
    for topic in topics:
        words = topic[1].split("+")
        topic_words = [word.split("*")[1].replace('"', '').strip() for word in words]
        topics_ls.append(topic_words)

    return topics_ls
```

下一个函数，*topics_from_pdf*，调用了LLM模型。如前所述，模型被提示将输出格式化为嵌套的项目符号列表。

```py
def topics_from_pdf(llm, file, num_topics, words_per_topic):
    """
    Generates descriptive prompts for LLM based on topic words extracted from a 
    PDF document.

    This function takes the output of `get_topic_lists_from_pdf` function, 
    which consists of a list of topic-related words for each topic, and 
    generates an output string in table of content format.

    Parameters:
        llm (LLM): An instance of the Large Language Model (LLM) for generating 
        responses.
        file (str): The path to the PDF file for extracting topic-related words.
        num_topics (int): The number of topics to consider.
        words_per_topic (int): The number of words per topic to include.

    Returns:
        str: A response generated by the language model based on the provided 
        topic words.
    """

    # Extract topics and convert to string
    list_of_topicwords = get_topic_lists_from_pdf(file, num_topics, 
                                                  words_per_topic)
    string_lda = ""
    for list in list_of_topicwords:
        string_lda += str(list) + "\n"

    # Create the template
    template_string = '''Describe the topic of each of the {num_topics} 
        double-quote delimited lists in a simple sentence and also write down 
        three possible different subthemes. The lists are the result of an 
        algorithm for topic discovery.
        Do not provide an introduction or a conclusion, only describe the 
        topics. Do not mention the word "topic" when describing the topics.
        Use the following template for the response.

        1: <<<(sentence describing the topic)>>>
        - <<<(Phrase describing the first subtheme)>>>
        - <<<(Phrase describing the second subtheme)>>>
        - <<<(Phrase describing the third subtheme)>>>

        2: <<<(sentence describing the topic)>>>
        - <<<(Phrase describing the first subtheme)>>>
        - <<<(Phrase describing the second subtheme)>>>
        - <<<(Phrase describing the third subtheme)>>>

        ...

        n: <<<(sentence describing the topic)>>>
        - <<<(Phrase describing the first subtheme)>>>
        - <<<(Phrase describing the second subtheme)>>>
        - <<<(Phrase describing the third subtheme)>>>

        Lists: """{string_lda}""" '''

    # LLM call
    prompt_template = ChatPromptTemplate.from_template(template_string)
    chain = LLMChain(llm=llm, prompt=prompt_template)
    response = chain.run({
        "string_lda" : string_lda,
        "num_topics" : num_topics
        })

    return response
```

在之前的函数中，单词列表被转换为字符串。然后，使用LangChain中的*ChatPromptTemplate*对象创建提示；请注意，提示定义了响应的结构。最后，函数调用了chatgpt-3.5 Turbo模型。返回值是LLM模型提供的响应。

现在，是时候调用这些函数了。我们首先设置API密钥。***T***[***这篇文章***](https://www.maisieai.com/help/how-to-get-an-openai-api-key-for-chatgpt)提供了获取API密钥的说明。

```py
openai_key = "sk-p..."
llm = OpenAI(openai_api_key=openai_key, max_tokens=-1)
```

接下来，我们调用了*topics_from_pdf*函数。我选择了主题数量和每个主题的字数的值。此外，我选择了一本**公有领域**的书籍，《变形记》，作者是弗朗茨·卡夫卡，作为测试。文档存储在我的个人驱动器中，并使用gdown库下载。

```py
!gdown https://drive.google.com/uc?id=1mpXUmuLGzkVEqsTicQvBPcpPJW0aPqdL

file = "./the-metamorphosis.pdf"
num_topics = 6
words_per_topic = 30

summary = topics_from_pdf(llm, file, num_topics, words_per_topic)
```

结果如下所示：

```py
1: Exploring the transformation of Gregor Samsa and the effects on his family and lodgers
- Understanding Gregor's metamorphosis
- Examining the reactions of Gregor's family and lodgers
- Analyzing the impact of Gregor's transformation on his family

2: Examining the events surrounding the discovery of Gregor's transformation
- Investigating the initial reactions of Gregor's family and lodgers
- Analyzing the behavior of Gregor's family and lodgers
- Exploring the physical changes in Gregor's environment

3: Analyzing the pressures placed on Gregor's family due to his transformation
- Examining the financial strain on Gregor's family
- Investigating the emotional and psychological effects on Gregor's family
- Examining the changes in family dynamics due to Gregor's metamorphosis

4: Examining the consequences of Gregor's transformation
- Investigating the physical changes in Gregor's environment
- Analyzing the reactions of Gregor's family and lodgers
- Investigating the emotional and psychological effects on Gregor's family

5: Exploring the impact of Gregor's transformation on his family
- Analyzing the financial strain on Gregor's family
- Examining the changes in family dynamics due to Gregor's metamorphosis
- Investigating the emotional and psychological effects on Gregor's family

6: Investigating the physical changes in Gregor's environment
- Analyzing the reactions of Gregor's family and lodgers
- Examining the consequences of Gregor's transformation
- Exploring the impact of Gregor's transformation on his family
```

输出效果相当不错，而且仅用了几秒钟！它正确地提取了书中的主要思想。

这种方法也适用于技术书籍。例如，*The Foundations of Geometry*（《几何学基础》），作者是David Hilbert（1899年）（也在公有领域）：

```py
1: Analyzing the properties of geometric shapes and their relationships
- Exploring the axioms of geometry
- Analyzing the congruence of angles and lines
- Investigating theorems of geometry

2: Studying the behavior of rational functions and algebraic equations
- Examining the straight lines and points of a problem
- Investigating the coefﬁcients of a function
- Examining the construction of a deﬁnite integral

3: Investigating the properties of a number system
- Exploring the domain of a true group
- Analyzing the theorem of equal segments
- Examining the circle of arbitrary displacement

4: Examining the area of geometric shapes
- Analyzing the parallel lines and points
- Investigating the content of a triangle
- Examining the measures of a polygon

5: Examining the theorems of algebraic geometry
- Exploring the congruence of segments
- Analyzing the system of multiplication
- Investigating the valid theorems of a call

6: Investigating the properties of a ﬁgure
- Examining the parallel lines of a triangle
- Analyzing the equation of joining sides
- Examining the intersection of segments
```

## 结论

将LDA算法与LLM结合用于大文档主题提取可以产生良好的结果，同时显著降低了成本和处理时间。我们从数百次API调用减少到仅一次，从几分钟减少到几秒钟。

输出质量很大程度上取决于其格式。在这种情况下，嵌套的项目符号列表效果很好。此外，主题的数量和每个主题的字数对结果的质量也很重要。我建议尝试不同的提示、主题数量和每个主题的字数，以找到最适合给定文档的配置。

代码可以在[**这个链接**](https://colab.research.google.com/drive/1_dY4EGaQfz0NIFJlsCc981rNXzmJEK8U?usp=sharing)中找到。

感谢阅读。请让我知道它在您的文档中的结果如何。

LinkedIn: [Antonio Jimenez Caballero](https://www.linkedin.com/in/antonio-jimnzc)

GitHub: [a-jimenezc](https://github.com/a-jimenezc)
