# 清理Confluence混乱：一个Python和BERTopic的探索

> 原文：[https://towardsdatascience.com/cleaning-up-confluence-chaos-a-python-and-bertopic-quest-d3aafc2ed736?source=collection_archive---------1-----------------------#2023-04-29](https://towardsdatascience.com/cleaning-up-confluence-chaos-a-python-and-bertopic-quest-d3aafc2ed736?source=collection_archive---------1-----------------------#2023-04-29)

## 一个驯服混乱文档的故事，旨在创建最终的GPT聊天机器人

[](https://medium.com/@massi.costacurta?source=post_page-----d3aafc2ed736--------------------------------)[![Massimiliano Costacurta](../Images/599c3469021c53f116cc67c390db6695.png)](https://medium.com/@massi.costacurta?source=post_page-----d3aafc2ed736--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d3aafc2ed736--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d3aafc2ed736--------------------------------) [Massimiliano Costacurta](https://medium.com/@massi.costacurta?source=post_page-----d3aafc2ed736--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F233cb43234c3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcleaning-up-confluence-chaos-a-python-and-bertopic-quest-d3aafc2ed736&user=Massimiliano+Costacurta&userId=233cb43234c3&source=post_page-233cb43234c3----d3aafc2ed736---------------------post_header-----------) 发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----d3aafc2ed736--------------------------------) ·8 min read·2023年4月29日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd3aafc2ed736&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcleaning-up-confluence-chaos-a-python-and-bertopic-quest-d3aafc2ed736&user=Massimiliano+Costacurta&userId=233cb43234c3&source=-----d3aafc2ed736---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd3aafc2ed736&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcleaning-up-confluence-chaos-a-python-and-bertopic-quest-d3aafc2ed736&source=-----d3aafc2ed736---------------------bookmark_footer-----------)![](../Images/b1dec38e9c54230b6b25f899c6b43db0.png)

[Rick Mason](https://unsplash.com/@egnaro?utm_source=medium&utm_medium=referral)的照片，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍：

想象一下：你在一家迅速成长的科技公司，接受了创建一个最先进的聊天机器人任务，使用令人惊叹的 GPT 技术。这个聊天机器人注定将成为公司的瑰宝，一个虚拟的神谕，回答基于你 Confluence 空间中储存的知识宝藏的问题。听起来像是一个梦寐以求的工作，对吧？

但，当你仔细审视 Confluence 知识库时，现实会袭来。这是一片充满空白/不完整页面、无关文档和重复内容的狂野丛林。就像有人将一千块拼图倒入一个巨大的搅拌机并按下“启动”按钮。现在，你的任务是清理这个烂摊子，才能考虑构建那个令人惊叹的聊天机器人。

幸运的是，在本文中，我们将踏上征服 Confluence 混乱的激动人心的旅程，利用 Python 和 BERTopic 的力量来识别和消除那些恼人的异常值。所以，系好安全带，准备将你的知识库转变为适合前沿 GPT 聊天机器人的完美训练场。

# 手动方法与启发式诱惑

当你面对清理 Confluence 知识库这一艰巨任务时，你可能会考虑手动处理，一一梳理每个文档。然而，手动方式速度慢、劳动密集且容易出错。毕竟，即便是最细心的员工也可能忽略重要细节或误判文档的相关性。

凭借你对 Python 的知识，你可能会被诱惑创建一个基于启发式的解决方案，使用一组预定义规则来识别和消除异常值。虽然这种方法比手动清理更快，但也有其局限性。启发式方法可能过于死板，难以适应 Confluence 空间复杂且不断发展的特性，往往导致次优结果。

# Python 和 BERTopic — Confluence 清理的强大组合

让我们介绍 Python 和 BERTopic，这一强大的组合可以帮助你更有效地解决清理 Confluence 知识库的挑战。Python 是一种多功能的编程语言，而 BERTopic 是一个先进的话题建模库，可以分析你的文档并根据其潜在话题对它们进行分组。

在接下来的段落中，我们将探讨 Python 和 BERTopic 如何协作，以自动化识别和消除 Confluence 空间中的异常值的过程。通过发挥它们的综合力量，你将节省时间和资源，同时提高清理工作的准确性和效果。

# Python-BERTopic 项目 — 步骤指南

好的，从这一点开始，我将带你了解如何使用 BERTopic 创建 Python 脚本，以识别和消除 Confluence 知识库中的异常值。目标是根据“无关性”得分（我们稍后会定义）生成一个文档排名列表。最终输出将包括文档的标题、文本预览（前 100 个字符）以及无关性得分。最终输出将如下所示：

（标题：“医疗保健中的人工智能”，预览：“人工智能正在改变……”，无关性：0.95）

（标题：“办公室生日派对指南”，预览：“为确保有趣和安全……”，无关性：0.8）

这个过程的基本步骤包括：

+   连接到 Confluence 并下载文档：建立与 Confluence 账户的连接并获取用于处理的文档。本节提供有关设置连接、身份验证和下载必要数据的指南。

+   使用 Beautiful Soup 进行 HTML 处理和文本提取：使用 Beautiful Soup 这一强大的 Python 库来管理 HTML 内容并从 Confluence 文档中提取文本。这个步骤包括清理提取的文本，去除不需要的元素，并为分析准备数据。

+   应用 BERTopic 并创建排名：手头有了清理过的文本后，应用 BERTopic 来分析和分组文档，依据其潜在主题。获取主题表示后，计算每个文档的“无关性”度量，并创建排名，以识别和消除 Confluence 知识库中的异常值。

# Confluence 连接和文档下载

最后是代码。在这里，我们将从 Confluence 空间开始下载文档，然后处理 HTML 内容，并提取文本以进入下一阶段（BERTopic！）。

首先，我们需要通过 API 连接到 Confluence。借助 atlassian-python-api 库，这可以通过几行代码完成。如果你没有 Atlassian 的 API 令牌，请阅读 [这个指南](https://support.atlassian.com/atlassian-account/docs/manage-api-tokens-for-your-atlassian-account/) 来设置。

```py
import os
import re
from atlassian import Confluence
from bs4 import BeautifulSoup

# Set up Confluence API client
confluence = Confluence(
    url='YOUR_CONFLUENCE URL',
    username="YOUR_EMAIL",
    password="YOUR_API_KEY",
    cloud=True)

# Replace SPACE_KEY with the desired Confluence space key
space_key = 'YOUR_SPACE'

def get_all_pages_from_space_with_pagination(space_key):
    limit = 50
    start = 0
    all_pages = []

    while True:
        pages = confluence.get_all_pages_from_space(space_key, start=start, limit=limit)
        if not pages:
            break

        all_pages.extend(pages)
        start += limit

    return all_pages

pages = get_all_pages_from_space_with_pagination(space_key)
```

在获取页面后，我们将创建一个文本文件的目录，提取页面内容并将文本内容保存到单独的文件中：

```py
# Function to sanitize filenames
def sanitize_filename(filename):
    return "".join(c for c in filename if c.isalnum() or c in (' ', '.', '-', '_')).rstrip()

# Create a directory for the text files if it doesn't exist
if not os.path.exists('txt_files'):
   os.makedirs('txt_files')

# Extract pages and save to individual text files
for page in pages:
   page_id = page['id']
   page_title = page['title']

   # Fetch the page content
   page_content = confluence.get_page_by_id(page_id, expand='body.storage')

   # Extract the content in the "storage" format
   storage_value = page_content['body']['storage']['value']

   # Clean the HTML tags to get the text content
   text_content = process_html_document(storage_value)
   file_name = f'txt_files/{sanitize_filename(page_title)}_{page_id}.txt'
   with open(file_name, 'w', encoding='utf-8') as txtfile:
       txtfile.write(text_content)
```

函数 `process_html_document` 执行所有必要的清理任务，以从下载的页面中提取文本，同时保持一致的格式。你希望对这个过程进行多大程度的细化取决于你的具体需求。在这种情况下，我们专注于处理表格和列表，以确保生成的文本文档保留与原始布局类似的格式。

```py
import spacy

nlp = spacy.load("en_core_web_sm")

def html_table_to_text(html_table):
    soup = BeautifulSoup(html_table, "html.parser")

    # Extract table rows
    rows = soup.find_all("tr")

    # Determine if the table has headers or not
    has_headers = any(th for th in soup.find_all("th"))

    # Extract table headers, either from the first row or from the <th> elements
    if has_headers:
        headers = [th.get_text(strip=True) for th in soup.find_all("th")]
        row_start_index = 1  # Skip the first row, as it contains headers
    else:
        first_row = rows[0]
        headers = [cell.get_text(strip=True) for cell in first_row.find_all("td")]
        row_start_index = 1

    # Iterate through rows and cells, and use NLP to generate sentences
    text_rows = []
    for row in rows[row_start_index:]:
        cells = row.find_all("td")
        cell_sentences = []
        for header, cell in zip(headers, cells):
            # Generate a sentence using the header and cell value
            doc = nlp(f"{header}: {cell.get_text(strip=True)}")
            sentence = " ".join([token.text for token in doc if not token.is_stop])
            cell_sentences.append(sentence)

        # Combine cell sentences into a single row text
        row_text = ", ".join(cell_sentences)
        text_rows.append(row_text)

    # Combine row texts into a single text
    text = "\n\n".join(text_rows)
    return text

def html_list_to_text(html_list):
    soup = BeautifulSoup(html_list, "html.parser")
    items = soup.find_all("li")
    text_items = []
    for item in items:
        item_text = item.get_text(strip=True)
        text_items.append(f"- {item_text}")
    text = "\n".join(text_items)
    return text

def process_html_document(html_document):
    soup = BeautifulSoup(html_document, "html.parser")

    # Replace tables with text using html_table_to_text
    for table in soup.find_all("table"):
        table_text = html_table_to_text(str(table))
        table.replace_with(BeautifulSoup(table_text, "html.parser"))

    # Replace lists with text using html_list_to_text
    for ul in soup.find_all("ul"):
        ul_text = html_list_to_text(str(ul))
        ul.replace_with(BeautifulSoup(ul_text, "html.parser"))

    for ol in soup.find_all("ol"):
        ol_text = html_list_to_text(str(ol))
        ol.replace_with(BeautifulSoup(ol_text, "html.parser"))

    # Replace all types of <br> with newlines
    br_tags = re.compile('<br>|<br/>|<br />')
    html_with_newlines = br_tags.sub('\n', str(soup))

    # Strip remaining HTML tags to isolate the text
    soup_with_newlines = BeautifulSoup(html_with_newlines, "html.parser")

    return soup_with_newlines.get_text()
```

# 使用 BERTopic 识别异常值

在这一最后章节中，我们将最终利用 BERTopic，这是一种利用 BERT 嵌入的强大主题建模技术。你可以在他们的 [GitHub 仓库](https://github.com/MaartenGr/BERTopic) 和 [文档](https://maartengr.github.io/BERTopic/) 中了解更多关于 BERTopic 的信息。

我们发现离群点的方法包括用不同的主题数量运行 BERTopic。在每次迭代中，我们将收集所有属于离群点簇（-1）的文档。文档在 -1 簇中出现的频率越高，被认为是离群点的可能性就越大。这种频率形成了我们无关度分数的第一个组成部分。BERTopic 还为 -1 簇中的文档提供了一个概率值。我们将计算所有迭代中每个文档这些概率的平均值。这个平均值代表了我们无关度分数的第二个组成部分。最后，我们通过计算两个分数（频率和概率）的平均值来确定每个文档的总体无关度分数。这个综合分数将帮助我们识别数据集中最无关的文档。

这是初始代码：

```py
import numpy as np
from bertopic import BERTopic
from bertopic.vectorizers import ClassTfidfTransformer
from bertopic.representation import MaximalMarginalRelevance
from sklearn.feature_extraction.text import CountVectorizer

vectorizer_model = CountVectorizer(stop_words="english")
representation_model = MaximalMarginalRelevance(diversity=0.2)
ctfidf_model = ClassTfidfTransformer(reduce_frequent_words=True)

# Collect text and filenames from chunks in the txt_files directory
documents = []
filenames = []

for file in os.listdir('txt_files'):
    if file.endswith('.txt'):
        with open(os.path.join('txt_files', file), 'r', encoding='utf-8') as f:
            documents.append(f.read())
            filenames.append(file)
```

在这个代码块中，我们通过导入所需的库并初始化模型来设置 BERTopic 所需的工具。我们定义了 3 个模型，这些模型将被 BERTopic 使用：

+   `vectorizer_model`：`CountVectorizer` 模型对文档进行分词，并创建一个文档-词项矩阵，其中每个条目表示一个术语在文档中的计数。它还会从文档中移除英文停用词，以提升主题建模性能。

+   `representation_model`：`MaximalMarginalRelevance` (MMR) 模型通过考虑主题的相关性和多样性来多样化提取的主题。`diversity` 参数控制这两个方面之间的权衡，更高的值会导致更具多样性的主题。

+   `ctfidf_model`：`ClassTfidfTransformer` 模型调整文档-词项矩阵的词频-逆文档频率（TF-IDF）分数，以更好地表示主题。它减少了跨主题频繁出现词语的影响，并增强了主题之间的区分度。

然后，我们从‘txt_files’目录中收集文档的文本和文件名，以便在下一步中使用 BERTopic 处理它们。

```py
def extract_topics(docs, n_topics):
    model = BERTopic(nr_topics=n_topics, calculate_probabilities=True, language="english",
                     ctfidf_model=ctfidf_model, representation_model=representation_model, 
                     vectorizer_model=vectorizer_model)
    topics, probabilities = model.fit_transform(docs)
    return model, topics, probabilities

def find_outlier_topic(model):
    topic_sizes = model.get_topic_freq()
    outlier_topic = topic_sizes.iloc[-1]["Topic"]
    return outlier_topic

outlier_counts = np.zeros(len(documents))
outlier_probs = np.zeros(len(documents))

# Define the range of topics you want to try
min_topics = 5
max_topics = 10

for n_topics in range(min_topics, max_topics + 1):
    model, topics, probabilities = extract_topics(documents, n_topics)
    outlier_topic = find_outlier_topic(model)

    for i, (topic, prob) in enumerate(zip(topics, probabilities)):
        if topic == outlier_topic:
            outlier_counts[i] += 1
            outlier_probs[i] += prob[outlier_topic]
```

在上述部分中，我们使用 BERTopic 通过在指定的最小值到最大值范围内迭代主题数量来识别离群文档。对于每个主题数量，BERTopic 提取主题及其相应的概率。然后，它识别离群主题并更新分配给该离群主题的文档的 `outlier_counts` 和 `outlier_probs`。这一过程迭代地累积计数和概率，提供了文档被分类为离群点的频率和“强度”的度量。

最后，我们可以计算我们的无关度分数并打印结果：

```py
def normalize(arr):
    min_val, max_val = np.min(arr), np.max(arr)
    return (arr - min_val) / (max_val - min_val)

# Average the probabilities
avg_outlier_probs = np.divide(outlier_probs, outlier_counts, out=np.zeros_like(outlier_probs), where=outlier_counts != 0)

# Normalize counts 
normalized_counts = normalize(outlier_counts)

# Compute the combined unrelatedness score by averaging the normalized counts and probabilities
unrelatedness_scores = [(i, (count + prob) / 2) for i, (count, prob) in enumerate(zip(normalized_counts, avg_outlier_probs))]
unrelatedness_scores.sort(key=lambda x: x[1], reverse=True)

# Print the filtered results
for index, score in unrelatedness_scores:
    if score > 0:
        title = filenames[index]
        preview = documents[index][:100] + "..." if len(documents[index]) > 100 else documents[index]
        print(f"Title: {title}, Preview: {preview}, Unrelatedness: {score:.2f}")
        print("\n")
```

就是这样！在这里，你将得到按无关程度排名的异常文档列表。通过清理你的 Confluence 空间和移除无关内容，你可以为创建一个更高效、更有价值的聊天机器人铺平道路，充分利用你组织的知识。清理愉快！

你喜欢这篇文章吗？想要保持对未来类似内容的更新吗？别忘了在 Medium 上关注我，以便获得有关我最新文章和人工智能、机器学习等领域的见解的通知。让我们一起继续学习之旅！
