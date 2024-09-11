# 如何分块文本数据 — 一项比较分析

> 原文：[https://towardsdatascience.com/how-to-chunk-text-data-a-comparative-analysis-3858c4a0997a?source=collection_archive---------0-----------------------#2023-07-20](https://towardsdatascience.com/how-to-chunk-text-data-a-comparative-analysis-3858c4a0997a?source=collection_archive---------0-----------------------#2023-07-20)

## 探索文本分块的不同方法。

[](https://solano-todeschini.medium.com/?source=post_page-----3858c4a0997a--------------------------------)[![Solano Todeschini](../Images/75e871340659c8df37f558b74c9d73c5.png)](https://solano-todeschini.medium.com/?source=post_page-----3858c4a0997a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3858c4a0997a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3858c4a0997a--------------------------------) [Solano Todeschini](https://solano-todeschini.medium.com/?source=post_page-----3858c4a0997a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F618a52c38c0c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-chunk-text-data-a-comparative-analysis-3858c4a0997a&user=Solano+Todeschini&userId=618a52c38c0c&source=post_page-618a52c38c0c----3858c4a0997a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3858c4a0997a--------------------------------) ·17分钟阅读·2023年7月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3858c4a0997a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-chunk-text-data-a-comparative-analysis-3858c4a0997a&user=Solano+Todeschini&userId=618a52c38c0c&source=-----3858c4a0997a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3858c4a0997a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-chunk-text-data-a-comparative-analysis-3858c4a0997a&source=-----3858c4a0997a---------------------bookmark_footer-----------)![](../Images/dfd6dd3b7af559b26e33e94122605cf6.png)

图片由作者整理。菠萝图片来自Canva。

# 介绍

自然语言处理（NLP）中的“文本分块”过程涉及将非结构化文本数据转换为有意义的单位。这一看似简单的任务掩盖了实现这一目标的各种方法的复杂性，每种方法都有其优缺点。

从高层次来看，这些方法通常分为两类。第一类是基于规则的方法，依赖于使用明确的分隔符，如标点符号或空格字符，或应用复杂的系统如正则表达式，将文本分成块。第二类是语义聚类方法，利用文本中固有的意义来指导分块过程。这些方法可能利用机器学习算法来辨别上下文，并推断文本中的自然分界。

在本文中，我们将探讨并比较这两种不同的文本分块方法。我们将用NLTK、Spacy和Langchain来表示基于规则的方法，并将其与两种不同的语义聚类技术进行对比：KMeans和一种自定义的邻近句子聚类技术。

目标是让从业者清楚理解每种方法的优缺点和理想的使用场景，以便在他们的NLP项目中做出更好的决策。

> 在巴西俚语中，“abacaxi”翻译为“菠萝”，意指“某事未能产生良好结果、混乱的局面或毫无价值的事物。”

## 文本分块的使用案例

文本分块可以被多种不同的应用程序使用：

1.  **文本摘要**：通过将大段文本拆分为可管理的块，我们可以对每个部分进行单独总结，从而得到更准确的总体摘要。

1.  **情感分析**：分析较短且连贯的文本块通常能比分析整个文档更精确地得出结果。

1.  **信息提取**：分块有助于定位文本中的特定实体或短语，增强信息检索的过程。

1.  **文本分类**：将文本分解成块允许分类器专注于较小且有上下文意义的单元，而不是整个文档，这可以提高性能。

1.  **机器翻译**：翻译系统通常在文本块上操作，而不是单个单词或整个文档。分块有助于保持翻译文本的一致性。

理解这些使用场景可以帮助选择最适合您特定项目的分块技术。

# 语义分块的不同方法比较

在本文的这一部分，我们将比较流行的非结构化文本语义分块方法：NLTK句子分割器、Langchain文本分割器、KMeans聚类以及基于相似性的邻近句子聚类。

在下面的例子中，我们将使用从PDF中提取的文本评估这一技术，将其处理为句子及其聚类。

我们使用的数据是从[巴西的维基百科页面](https://en.wikipedia.org/wiki/Brazil)导出的PDF。

为了从PDF中提取文本并用NLTK将其分割成句子，我们使用以下函数：

```py
from PyPDF2 import PdfReader
import nltk
nltk.download('punkt')

# Extracting Text from PDF
def extract_text_from_pdf(file_path):
    with open(file_path, 'rb') as file:
        pdf = PdfReader(file)
        text = " ".join(page.extract_text() for page in pdf.pages)
    return text

# Extract text from the PDF and split it into sentences
text = extract_text_from_pdf(file_path)
```

如此，我们得到了一个长度为210964个字符的字符串`text`。

这是维基文本的一个样本：

```py
sample = text[1015:3037]
print(sample)

"""
=======
Output:
=======

Brazil is the world's fifth-largest country by area and the seventh most popul ous. Its capital
is Brasília, and its most popul ous city is São Paulo. The federation is composed of the union of the 26
states and the Federal District. It is the only country in the Americas to have Portugue se as an official
langua ge.[11][12] It is one of the most multicultural and ethnically diverse nations, due to over a century of
mass immigration from around t he world,[13] and the most popul ous Roman Catholic-majority country.
Bounde d by the Atlantic Ocean on the east, Brazil has a coastline of 7,491 kilometers (4,655 mi).[14] It
borders all other countries and territories in South America except Ecuador and Chile and covers roughl y
half of the continent's land area.[15] Its Amazon basin includes a vast tropical forest, home to diverse
wildlife, a variety of ecological systems, and extensive natural resources spanning numerous protected
habitats.[14] This unique environmental heritage positions Brazil at number one of 17 megadiverse
countries, and is the subject of significant global interest, as environmental degradation through processes
like deforestation has direct impacts on gl obal issues like climate change and biodiversity loss.
The territory which would become know n as Brazil was inhabited by numerous tribal nations prior to the
landing in 1500 of explorer Pedro Álvares Cabral, who claimed the discovered land for the Portugue se
Empire. Brazil remained a Portugue se colony until 1808 when the capital of the empire was transferred
from Lisbon to Rio de Janeiro. In 1815, the colony was elevated to the rank of kingdom  upon the
formation of the United Kingdom  of Portugal, Brazil and the Algarves. Independence was achieved in
1822 with the creation of the Empire of Brazil, a unitary state gove rned unde r a constitutional monarchy
and a parliamentary system. The ratification of the first constitution in 1824  led to the formation of a
bicameral legislature, now called the National Congress.
"""
```

# NLTK句子分割器

自然语言工具包 (NLTK) 提供了一个将文本划分为句子的有用功能。这个句子分词器将给定的文本块划分为其组成句子，然后可以用于进一步处理。

## 实现

这是使用 NLTK 句子分词器的一个示例：

```py
import nltk
nltk.download('punkt')

# Splitting Text into Sentences
def split_text_into_sentences(text):
    sentences = nltk.sent_tokenize(text)
    return sentences

sentences = split_text_into_sentences(text)
```

这会返回从输入文本中提取的 2670 个 `句子`，每个句子的平均字符数为 78。

## 评估 NLTK 句子分词器

虽然 NLTK 句子分词器是一种将大量文本划分为单独句子的直接有效方法，但它确实存在某些局限性：

1.  **语言依赖性**：NLTK 句子分词器在很大程度上依赖于文本的语言。它在英语中表现良好，但对于其他语言，可能需要额外配置才能提供准确结果。

1.  **缩写和标点**：分词器有时会误解句子末尾的缩写或其他标点符号。这可能导致句子片段被当作独立的句子处理。

1.  **语义理解的缺乏**：像大多数分词器一样，NLTK 句子分词器不考虑句子之间的语义关系。因此，跨越多个句子的上下文可能在分词过程中丢失。

# Spacy 句子分割器

Spacy 另一个强大的 NLP 库，提供了一种依赖语言规则的句子分词功能。这与 NLTK 的方法类似。

## 实现

实现 Spacy 的句子分割器相当简单。以下是在 Python 中的实现方法：

```py
import spacy

nlp = spacy.load('en_core_web_sm')
doc = nlp(text)
sentences = list(doc.sents)
```

这会返回从输入文本中提取的 2336 个 `句子`，每个句子的平均字符数为 89。

## 评估 Spacy 句子分割器

与 Langchain 字符文本分割器相比，Spacy 的句子分割器倾向于创建较小的块，因为它严格遵守句子边界。当分析需要较小的文本单元时，这可能是一个优势。

然而，与 NLTK 一样，Spacy 的性能取决于输入文本的质量。对于标点或结构较差的文本，识别的句子边界可能并不总是准确。

现在，我们将查看 Langchain 如何提供文本数据分块的框架，并进一步与 NLTK 和 Spacy 进行比较。

# Langchain 字符文本分割器

Langchain 字符文本分割器通过在特定字符处递归地划分文本来工作。它对通用文本特别有用。

该分割器由一系列字符定义。它尝试根据这些字符划分文本，直到生成的块符合所需的大小标准。默认列表是[“\n\n”， “\n”， “ ”， “”]，旨在尽可能保持段落、句子和词汇的连贯，以维持语义一致性。

## 实现

考虑以下示例，我们使用这种方法分割了从 PDF 中提取的示例文本。

```py
# Initialize the text splitter with custom parameters
custom_text_splitter = RecursiveCharacterTextSplitter(
    # Set custom chunk size
    chunk_size = 100,
    chunk_overlap  = 20,
    # Use length of the text as the size measure
    length_function = len,

)

# Create the chunks
texts = custom_text_splitter.create_documents([sample])

# Print the first two chunks
print(f'### Chunk 1: \n\n{texts[0].page_content}\n\n=====\n')
print(f'### Chunk 2: \n\n{texts[1].page_content}\n\n=====')

"""
=======
Output:
=======

### Chunk 1: 

Brazil is the world's fifth-largest country by area and the seventh most popul ous. Its capital

=====

### Chunk 2: 

is Brasília, and its most popul ous city is São Paulo. The federation is composed of the union of

=====

"""
```

最后，我们得到 3205 个文本块，由 `texts` 列表表示。每个块的平均长度为 65.8 个字符——比 NLTK 的平均长度（79 个字符）稍少。

## **更改参数和使用 '\n' 分隔符：**

对于 Langchain Splitter 的更自定义方法，我们可以根据需要更改 `chunk_size` 和 `chunk_overlap` 参数。此外，我们还可以仅指定一个字符（或字符集）作为分隔操作，例如 `\n`。这将指导分隔器仅在新行字符处将文本拆分成块。

让我们考虑一个例子，其中我们将 `chunk_size` 设置为 300，`chunk_overlap` 设置为 30，并且只使用 `\n` 作为分隔符。

```py
# Initialize the text splitter with custom parameters
custom_text_splitter = RecursiveCharacterTextSplitter(
    # Set custom chunk size
    chunk_size = 300,
    chunk_overlap  = 30,
    # Use length of the text as the size measure
    length_function = len,
    # Use only "\n\n" as the separator
    separators = ['\n']
)

# Create the chunks
custom_texts = custom_text_splitter.create_documents([sample])

# Print the first two chunks
print(f'### Chunk 1: \n\n{custom_texts[0].page_content}\n\n=====\n')
print(f'### Chunk 2: \n\n{custom_texts[1].page_content}\n\n=====')
```

现在，让我们比较一些标准参数集与自定义参数的输出：

```py
# Print the sampled chunks
print("====   Sample chunks from 'Standard Parameters':   ====\n\n")
for i, chunk in enumerate(texts):
  if i < 4:
    print(f"### Chunk {i+1}: \n{chunk.page_content}\n")

print("====   Sample chunks from 'Custom Parameters':   ====\n\n")
for i, chunk in enumerate(custom_texts):
  if i < 4:
    print(f"### Chunk {i+1}: \n{chunk.page_content}\n")

"""
=======
Output:
=======

====   Sample chunks from 'Standard Parameters':   ====

### Chunk 1: 
Brazil is the world's fifth-largest country by area and the seventh most popul ous. Its capital

### Chunk 2: 
is Brasília, and its most popul ous city is São Paulo. The federation is composed of the union of

### Chunk 3: 
of the union of the 26

### Chunk 4: 
states and the Federal District. It is the only country in the Americas to have Portugue se as an

====   Sample chunks from 'Custom Parameters':   ====

### Chunk 1: 
Brazil is the world's fifth-largest country by area and the seventh most popul ous. Its capital
is Brasília, and its most popul ous city is São Paulo. The federation is composed of the union of the 26

### Chunk 2: 
states and the Federal District. It is the only country in the Americas to have Portugue se as an official
langua ge.[11][12] It is one of the most multicultural and ethnically diverse nations, due to over a century of

### Chunk 3: 
mass immigration from around t he world,[13] and the most popul ous Roman Catholic-majority country.
Bounde d by the Atlantic Ocean on the east, Brazil has a coastline of 7,491 kilometers (4,655 mi).[14] It

### Chunk 4: 
borders all other countries and territories in South America except Ecuador and Chile and covers roughl y
half of the continent's land area.[15] Its Amazon basin includes a vast tropical forest, home to diverse
"""
```

我们已经可以看到，这些自定义参数产生了更大的块，从而保留了比默认参数集更多的内容。

## 评估 Langchain 字符文本拆分器

在使用不同参数将文本拆分成块后，我们获得了两个块列表：`texts` 和 `custom_texts`，分别包含 3205 和 1404 个文本块。现在，让我们绘制这两种情况下块长度的分布图，以更好地理解更改参数的影响。

![](../Images/6bc0fe338ed1534c308f134cf80fa36b.png)

**图1**：Langchain 分隔器不同参数的块长度分布图（图像由作者提供）

在这个直方图中，x 轴表示块长度，而 y 轴表示每个长度的频率。蓝色条表示原始参数的块长度分布，橙色条表示自定义参数的分布。通过比较这两种分布，我们可以看到参数变化如何影响生成的块长度。

> *请记住，理想的分布取决于您文本处理任务的具体要求。如果您处理的是精细的分析，可能需要更小、更众多的块；如果是更广泛的语义分析，则可能需要更大、更少的块。*

## Langchain 字符文本拆分器与 NLTK 和 Spacy

早些时候，我们使用 Langchain 分隔器及其默认参数生成了 3205 个块。而 NLTK 句子分词器将相同的文本拆分成总共 2670 个句子。

为了更直观地理解这些方法之间的差异，我们可以可视化块长度的分布。下面的图显示了每种方法的块长度密度，让我们可以看到长度的分布情况及其大多数长度的位置。

![](../Images/55d688776a0dcc8cd5b779ba04b3d185.png)

**图2**：使用自定义参数的 Langchain Splitter 与 NLTK 和 Spacy 的块长度分布图（图像由作者提供）

从图 1 中，我们可以看到 Langchain 分割器结果具有更简洁的聚类长度密度，并且更倾向于生成较长的聚类，而 NLTK 和 Spacy 似乎在聚类长度方面产生非常相似的输出，倾向于较小的句子，同时存在许多长度可达 1400 个字符的离群点，并且长度有减少的趋势。

# KMeans 聚类

句子聚类是一种根据句子的语义相似性进行分组的技术。通过使用句子嵌入和诸如 K-means 的聚类算法，我们可以实现句子聚类。

## 实现

这是一个使用 Python 库 `sentence-transformers` 生成句子嵌入和 `scikit-learn` 进行 K-means 聚类的简单示例代码片段：

```py
from sentence_transformers import SentenceTransformer
from sklearn.cluster import KMeans

# Load the Sentence Transformer model
model = SentenceTransformer('all-MiniLM-L6-v2')

# Define a list of sentences (your text data)
sentences = ["This is an example sentence.", "Another sentence goes here.", "..."]

# Generate embeddings for the sentences
embeddings = model.encode(sentences)

# Choose an appropriate number of clusters (here we choose 5 as an example)
num_clusters = 3

# Perform K-means clustering
kmeans = KMeans(n_clusters=num_clusters)
clusters = kmeans.fit_predict(embeddings)
```

你可以在这里看到对句子列表进行聚类的步骤：

1.  加载一个句子转换模型。在这种情况下，我们使用的是来自 [HuggingFace](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2) 的`all-MiniLM-L6-v2`模型。

1.  使用模型的`encode()`方法定义你的句子并生成它们的嵌入。

1.  然后你定义你的聚类技术和聚类数量（这里我们使用的是 3 个聚类的 KMeans），并最终将其适配到数据集。

## 评估 KMeans 聚类

最后，我们为每个聚类绘制一个词云。

```py
from wordcloud import WordCloud
import matplotlib.pyplot as plt
from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize
import string

nltk.download('stopwords')

# Define a list of stop words
stop_words = set(stopwords.words('english'))

# Define a function to clean sentences
def clean_sentence(sentence):
    # Tokenize the sentence
    tokens = word_tokenize(sentence)
    # Convert to lower case
    tokens = [w.lower() for w in tokens]
    # Remove punctuation
    table = str.maketrans('', '', string.punctuation)
    stripped = [w.translate(table) for w in tokens]
    # Remove non-alphabetic tokens
    words = [word for word in stripped if word.isalpha()]
    # Filter out stop words
    words = [w for w in words if not w in stop_words]
    return words

# Compute and print Word Clouds for each cluster
for i in range(num_clusters):
    cluster_sentences = [sentences[j] for j in range(len(sentences)) if clusters[j] == i]
    cleaned_sentences = [' '.join(clean_sentence(s)) for s in cluster_sentences]
    text = ' '.join(cleaned_sentences)

    wordcloud = WordCloud(max_font_size=50, max_words=100, background_color="white").generate(text)
    plt.figure()
    plt.imshow(wordcloud, interpolation="bilinear")
    plt.axis("off")
    plt.title(f"Cluster {i}")
    plt.show()
```

下面是生成的聚类的词云图：

![](../Images/d315eac1d84c9a0a389376dedeab4441.png)

**图 3**：KMeans 聚类的词云图 — 聚类 0（图片由作者提供）

![](../Images/2e9fcb7f2635eb6512d863ef5f8aa009.png)

**图 4**：KMeans 聚类的词云图 — 聚类 1（图片由作者提供）

![](../Images/aa981bfdb20ca0356275a6cbc557f8a6.png)

**图 5**：KMeans 聚类的词云图 — 聚类 2（图片由作者提供）

在我们对 KMeans 聚类的词云分析中，很明显每个聚类在其最常见词汇的语义上具有显著的差异。这显示了聚类之间的强语义区分。此外，观察到聚类大小的明显变化，表明每个聚类所包含的序列数量存在显著差异。

## KMeans 聚类的局限性

尽管句子聚类有益，但也存在一些显著的缺点。主要的局限性包括：

1.  **句子顺序丧失**：句子聚类不会保留原始句子的顺序，这可能会扭曲叙事的自然流动。**这点非常重要**

1.  **计算效率**：KMeans 可能会计算密集且较慢，特别是在处理大型文本语料库或较多聚类时。这对于实时应用或处理大数据来说可能是一个显著的缺点。

# 聚类相邻句子

为了克服 KMeans 聚类的一些局限性，尤其是句子顺序的丢失，另一种方法可以是基于语义相似性对相邻句子进行聚类。该方法的基本前提是，文本中连续出现的两个句子比相距较远的两个句子更有可能在语义上相关。

## 实施

下面是使用 Spacy 句子作为输入的启发式扩展实现：

```py
import numpy as np
import spacy

# Load the Spacy model
nlp = spacy.load('en_core_web_sm')

def process(text):
    doc = nlp(text)
    sents = list(doc.sents)
    vecs = np.stack([sent.vector / sent.vector_norm for sent in sents])

    return sents, vecs

def cluster_text(sents, vecs, threshold):
    clusters = [[0]]
    for i in range(1, len(sents)):
        if np.dot(vecs[i], vecs[i-1]) < threshold:
            clusters.append([])
        clusters[-1].append(i)

    return clusters

def clean_text(text):
    # Add your text cleaning process here
    return text

# Initialize the clusters lengths list and final texts list
clusters_lens = []
final_texts = []

# Process the chunk
threshold = 0.3
sents, vecs = process(text)

# Cluster the sentences
clusters = cluster_text(sents, vecs, threshold)

for cluster in clusters:
    cluster_txt = clean_text(' '.join([sents[i].text for i in cluster]))
    cluster_len = len(cluster_txt)

    # Check if the cluster is too short
    if cluster_len < 60:
        continue

    # Check if the cluster is too long
    elif cluster_len > 3000:
        threshold = 0.6
        sents_div, vecs_div = process(cluster_txt)
        reclusters = cluster_text(sents_div, vecs_div, threshold)

        for subcluster in reclusters:
            div_txt = clean_text(' '.join([sents_div[i].text for i in subcluster]))
            div_len = len(div_txt)

            if div_len < 60 or div_len > 3000:
                continue

            clusters_lens.append(div_len)
            final_texts.append(div_txt)

    else:
        clusters_lens.append(cluster_len)
        final_texts.append(cluster_txt)
```

*代码中的主要收获：*

1.  **文本处理**：每个文本片段都传递给`process`函数。该函数使用 SpaCy 库创建句子嵌入，这些嵌入用于表示文本片段中每个句子的语义意义。

1.  **簇创建**：`cluster_text`函数根据句子嵌入的余弦相似性形成句子簇。如果余弦相似性低于指定的阈值，则开始一个新的簇。

1.  **长度检查**：代码接着检查每个簇的长度。如果一个簇太短（少于 60 个字符）或太长（超过 3000 个字符），则调整阈值，并对该簇重复处理，直到达到可接受的长度。

我们来看一下这种方法的一些输出片段，并将它们与 Langchain 分割器进行比较：

```py
====   Sample chunks from 'Langchain Splitter with Custom Parameters':   ====

### Chunk 1: 
Brazil is the world's fifth-largest country by area and the seventh most popul ous. Its capital
is Brasília, and its most popul ous city is São Paulo. The federation is composed of the union of the 26

### Chunk 2: 
states and the Federal District. It is the only country in the Americas to have Portugue se as an official
langua ge.[11][12] It is one of the most multicultural and ethnically diverse nations, due to over a century of

====   Sample chunks from 'Adjacent Sentences Clustering':   ====

### Chunk 1: 
Brazil is the world's fifth-largest country by area and the seventh most popul ous. Its capital
is Brasília, and its most popul ous city is São Paulo.

### Chunk 2: 
The federation is composed of the union of the 26
states and the Federal District. It is the only country in the Americas to have Portugue se as an official
langua ge.[11][12]
```

很好，现在让我们将`final_texts`（来自相邻序列聚类方法）的片段长度分布与 Langchain 字符文本分割器和 NLTK 句子分词器的分布进行比较。为此，我们首先需要计算`final_texts`中片段的长度：

```py
final_texts_lengths = [len(chunk) for chunk in final_texts]
```

现在我们可以绘制三种方法的分布图：

![](../Images/296e3efe4e40f3ca545e546ceb143638.png)

**图 3**：测试的所有不同方法生成的片段长度分布图（图像由作者提供）

从图 6 中，我们可以推导出 Langchain 分割器使用其预定义的片段大小，创建了一个均匀的分布，意味着片段长度一致。

另一方面，Spacy 句子分割器和 NLTK 句子分词器似乎更倾向于较短的句子，虽然存在许多较大的离群值，表明它们依赖语言线索来确定分割，可能会产生不规则大小的片段。

最后，自定义的相邻序列聚类方法，根据语义相似性进行聚类，展现出更为多样化的分布。这可能表明一种更具上下文敏感的方法，在保持片段内容连贯的同时，允许更大的灵活性。

## 评估相邻序列聚类方法

相邻序列聚类方法带来了**独特的好处**：

1.  **上下文一致性**：通过考虑语义和上下文一致性生成主题一致的片段。

1.  **灵活性**：平衡上下文保持和计算效率，提供可调的片段大小。

1.  **阈值调整**：允许用户根据需求微调分块过程，通过调整相似度阈值。

1.  **序列保留**：保留文本中句子的原始顺序，对于需要文本顺序的序列语言模型和任务至关重要。

# 文本分块方法比较：见解总结

## Langchain 字符文本分割器

这种方法提供了均匀的块长度，产生均匀的分布。当下游处理或分析需要标准大小时，这可能是有益的。这种方法对文本的具体语言结构不那么敏感，更侧重于产生预定义字符长度的块。

## NLTK 句子分词器和 Spacy 句子分割器

这些方法表现出对较小句子的偏好，但包括许多较大的离群点。虽然这可以导致更具语言一致性的块，但也可能导致块大小的高度可变性。

这些方法也能产生良好的结果，可以作为下游任务的输入。

## 相邻序列聚类

这种方法生成了更多样的分布，表明了其上下文敏感的方法。通过基于语义相似性进行聚类，确保每个块中的内容是连贯的，同时允许块大小的灵活性。当保持文本数据的语义连续性很重要时，这种方法可能是有利的。

为了更直观和抽象（或有趣）的表示，让我们看看下面的图7，试着找出哪种菠萝“切块”更好地代表所讨论的方法：

![](../Images/89004ea38220a7f04c73cac9d5616f0f.png)

**图7**：不同的文本分块方法以菠萝切块形式展示（图像由作者整理，菠萝图像来自 Canva）

按顺序列出：

1.  切割方法1 代表了基于规则的方法，你可以根据过滤器或正则表达式“剥离”你想要的“垃圾”文本。不过要处理整个菠萝可要花不少功夫，因为它还保留了许多上下文较大的离群点。*这确实需要很多工作*。

1.  Langchain 类似于切割方法2。大小非常相似，但未能包含整个所需的上下文（它是一个三角形，所以也可能是一个西瓜）。

1.  切割方法3 绝对是 KMeans。你甚至可以只分组那些对你有意义的部分——最有汁的部分——但你无法得到其核心。没有它，块会失去所有结构和意义。我认为这也需要很多工作…… *特别是对于更大的菠萝*。

1.  最后，切割方法4展示了相邻句子聚类方法。块的大小可能有所不同，但通常保持上下文信息，类似于不规则的菠萝块，仍然能显示出水果的整体结构。

***总结***：在这篇文章中，我们比较了三种文本分块方法及其独特的好处。Langchain 提供了一致的分块大小，但语言结构却被忽视。NLTK 和 Spacy 提供了语言上连贯的分块，但大小差异较大。相邻序列聚类基于语义相似性进行聚类，提供内容连贯性和灵活的分块大小。*最终，最佳选择取决于你的具体需求，包括语言连贯性、分块大小的一致性和可用的计算能力。*

# 感谢阅读！

+   关注我 [Linkedin](https://www.linkedin.com/in/solano-todeschini/)!
