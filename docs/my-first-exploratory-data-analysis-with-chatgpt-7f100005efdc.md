# 我与 ChatGPT 的第一次探索性数据分析

> 原文：[`towardsdatascience.com/my-first-exploratory-data-analysis-with-chatgpt-7f100005efdc`](https://towardsdatascience.com/my-first-exploratory-data-analysis-with-chatgpt-7f100005efdc)

## 发掘 ChatGPT 的力量：深入探讨探索性数据分析和未来机会

[](https://jyesr.medium.com/?source=post_page-----7f100005efdc--------------------------------)![Jye Sawtell-Rickson](https://jyesr.medium.com/?source=post_page-----7f100005efdc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7f100005efdc--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7f100005efdc--------------------------------) [Jye Sawtell-Rickson](https://jyesr.medium.com/?source=post_page-----7f100005efdc--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7f100005efdc--------------------------------) ·15 分钟阅读·2023 年 5 月 10 日

--

![](img/2d8f14196cf7b2834ea040accdadbbe9.png)

“一个 AI 探索广阔的数据世界。数字艺术。生动的色彩。”（作者通过 DALL-E 2 生成）

ChatGPT 是一个非凡的工具，可以提高工作效率，这不仅仅局限于数据分析。在本文中，我们将通过一个 ChatGPT 执行的探索性数据分析（EDA）示例进行介绍。我们将覆盖 EDA 的各个阶段，看到一些令人印象深刻的输出（词云！），并指出 ChatGPT 表现好的地方（和不太好的地方）。最后，我们将讨论 LLM 在分析中的未来以及我们对此的兴奋之情。

用于分析的数据集是来自 [Common Crawl](https://commoncrawl.org/) 的样本，该数据集 [免费供任何人访问和分析](https://commoncrawl.org/terms-of-use/)。Common Crawl 数据集是一个庞大的网络爬虫数据集合，包含来自互联网的数十亿网页。该数据集包括各种网页内容类型，并且定期更新。它作为训练语言模型（如 LLMs）的重要资源，占 [ChatGPT 训练数据的 60%](https://medium.com/@dlaytonj2/chatgpt-show-me-the-data-sources-11e9433d57e8)。你可以在 Kaggle 上找到由作者策划的数据集样本 [这里](https://www.kaggle.com/datasets/jyesawtellrickson/commoncrawl)。

在整个文章中，内容将会被截断，所以可以直接查看 [用于运行此分析的 Google Colab](https://colab.research.google.com/drive/1REJ3xa37Z3milf8-4-1-2fB69AONI0Zz)。

我们将分析分为五个部分：

+   🤔 总体结构

+   🧹 数据清理

+   💯 基本分析

+   🇬🇧 NLP 方法

+   🔑 关键发现

以上内容介绍完毕，让我们开始分析吧！

# 结构化工作

在开始分析工作之前，制定一个计划是很好的。EDA 通常遵循一个标准结构，开始时的步骤是固定的，然后在分析中发现有趣的见解时再进行扩展。一个这样的模板是这个，它详细介绍了核心组件：预览数据、清理数据、绘制数据。我们可以以此为起点，但假设我们一无所知，看看 ChatGPT 会提出什么。

Prompt:

> `让我们一起对 Common Crawl 文本数据集进行 EDA。数据集是从互联网上许多网页抓取的。列包括'text'、'WARC-Type'、'WARC-Target-URI'、'WARC-Date'、'WARC-Record-ID'、'WARC-Refers-To'、'WARC-Block-Digest'、'WARC-Identified-Content-Language'、'Content-Type'和'Content-Length'。数据集保存在一个名为 common_crawl_text 的 csv 文件中。`
> 
> `你应该给我一个整体计划，然后我们可以一步一步进行。在每一步中，你应该写一些代码，我会在终端中运行它，然后将结果反馈给你。`

Response:

> `当然，这里是对 Common Crawl 文本数据集进行 EDA 的高层次计划：`

1.  `将数据集加载到一个 pandas DataFrame 中。`

1.  `检查每列中的缺失值，并适当地处理它们。`

1.  `探索目标变量'WARC-Type'的分布。`

1.  `检查'WARC-Identified-Content-Language'的分布。`

1.  `检查'Content-Type'的分布。`

1.  `探索'text'列，包括文本长度的分布和最常见的单词。`

1.  `检查不同列之间的相关性。`

1.  `总结发现并识别任何有趣的模式或见解。`

> `让我们从第 1 步开始，将数据集加载到一个 pandas DataFrame 中。`

ChatGPT 在这里做得相当好，覆盖了标准步骤。注意，它并不要求我们深入分析所有列，而是对特定子集感兴趣。这很合理，但我觉得它遗漏了一些有价值的数据点。例如，“WARC-Target-URI”值得探索，“WARC-Date”列将提供数据收集时间的见解。

我用这个提示了 ChatGPT，它很高兴地将这些内容纳入了计划。同时，它还添加了创建 Wordcloud 的步骤，这是我一直在考虑的。

> 我喜欢这种互动方式，感觉像是合作，而不仅仅是阅读一本书。

# 数据清理

按照结构中的每个步骤，我们可以要求 ChatGPT 扩展该步骤，并给我们提供精确的代码。首先，它建议：

> `我们可以从加载 Common Crawl 文本数据集并查看其结构开始。以下是将数据集加载到 Pandas DataFrame 中的代码：`
> 
> `import pandas as pd`
> 
> `df = pd.read_csv("common_crawl_text.csv")`
> 
> `一旦加载了数据集，你可以使用以下代码检查 DataFrame 的结构：`
> 
> `df.head()`
> 
> `这将显示数据框的前五行。你也可以使用 df.info() 方法来获取列及其数据类型的概述，以及每列中非空值的数量。`

在提供结果后，它能够分享一些基本信息：

> `根据提供的数据，似乎我们有一个包含不同属性的网页档案记录的数据集，例如记录的类型、记录的 URI（统一资源标识符）、记录的日期、内容的长度和内容本身。内容以不同的语言和格式存在，例如纯文本，内容长度也各不相同。数据集包含 37832 条记录，其中一些记录在内容长度列中有空值。`

我们可以进一步要求它给出每一列的详细描述：

提示：

> `你能提供一个详细的列列表和预期含义吗？`

响应：

> `当然！这是数据框中列的详细列表及其预期含义：`

1.  `WARC-Type: WARC 记录的类型。WARC（Web ARChive）记录是一种用于存储互联网资源（如网页及其相关元数据）的格式。`

1.  `WARC-Target-URI: 被存档网页的目标 URI（统一资源标识符）。`

1.  `WARC-Date: 网页被存档的日期和时间。`

1.  `WARC-Record-ID: WARC 记录的唯一标识符。`

1.  `WARC-Refers-To: 此记录所引用的相关 WARC 记录的标识符。`

1.  `WARC-Block-Digest: 对 WARC 记录内容计算的加密哈希值。`

1.  `WARC-Identified-Content-Language: 在网页内容中识别的语言。`

1.  `Content-Type: 内容的媒体类型，例如 text/plain 或 text/html。`

1.  `Content-Length: 内容的字节长度。`

1.  `text: 网页提取的文本内容。`

> `请注意，其中一些列是特定于 WARC 格式的，可能在其他类型的数据中不存在。`

它在猜测这些列的含义方面做得相当不错。虽然有些可能不正确（例如 Content-Length），但这是一个有用的起点。它可能确实知道这些特定名称，因为它的训练数据，但我已经尝试了任意列名，它通常可以从名称和返回的数据的组合中猜测列将包含什么。

现在，它注意到有缺失值，我们来看看如何处理这些问题：

提示：

> `我们应该对缺失值做些什么吗？`

响应：

> `是的，处理 Content-Length 列中的缺失值是个好主意。一种选择是删除那些缺失值的行，如果它们不是数据的重要部分。另一种选择是使用一些统计方法（例如均值、中位数或众数）来填补缺失值。然而，最佳方法将取决于数据的性质和所执行的分析。`

这些可能是我处理它的方式。在这种情况下，我很乐意直接删除这些条目，因为数量不是很多。

提示：

> `我们就将这些删除，因为数量非常少。我该怎么做呢？`

响应：

> `你可以使用 pandas 中的 dropna() 函数删除包含缺失值的行。以下是一个示例：`
> 
> `import pandas as pd`
> 
> `# 创建一个数据框`
> 
> `df = pd.DataFrame({'A': [1, 2, None, 4], 'B': [5, None, 7, 8]})`
> 
> `# 删除缺失值的行`
> 
> `df.dropna(inplace=True) print(df)`
> 
> `输出：`
> 
> `A B 0 1.0 5.0 3 4.0 8.0`
> 
> `在这个示例中，dropna() 函数会删除第二行，该行包含缺失值。inplace=True 参数会修改原始数据框，因此结果数据框只包含没有缺失值的行。`

注意它如何提供有效的代码，并对每一行代码进行注释。它还在最后提供了发生了什么的解释。这很好，因为我们可以了解代码的运行情况，如果有需要修改的地方，也会变得容易得多。

我们现在可以开始分析的第一部分。

# 基本分析

步骤 3–5 大致相同，绘制一些数据分布图并查看是否有任何有趣的发现。

当提示开始步骤 3 时：

```py
Sure, let's explore the distribution of the target variable 'WARC-Type'. We can do this by creating a histogram or a bar chart of the variable to see the count or percentage of each value in the dataset. If you are using Python, you can use libraries like Matplotlib or Seaborn to create visualizations. Here is an example code snippet to create a bar chart using Matplotlib: python import matplotlib.pyplot as plt
# Count the number of occurrences of each value in the 'WARC-Type' column
value_counts = df['WARC-Type'].value_counts()
# Create a bar chart of the value counts
plt.bar(value_counts.index, value_counts.values)
# Set the title and axis labels
plt.title('Distribution of WARC-Type')
plt.xlabel('WARC-Type')
plt.ylabel('Count')
# Show the plot plt.show() This code will create a bar chart showing the count of each value in the 'WARC-Type' column. You can modify the code to show the percentage of each value instead of the count.
```

这样会为我们输出一个图表。在这种情况下，图表并不特别有趣，因为实际上只有一个“WARC-Type”，但这是个不错的主意。图像的问题在于很难将结果反馈给 ChatGPT。我发现，建议 ChatGPT 返回数据表以解决这个问题会更好，同时在有意义的地方附上额外的图表。

了解列中只有一个值后，ChatGPT 很乐意继续进行下一阶段。当我们探索“WARC-Content-Language”时，我们开始看到一些更复杂的东西。返回的结果是：

> `eng 15903`
> 
> `rus 1551`
> 
> `...`
> 
> `hrv,nno,bel 1`
> 
> `zul,eng 1`

从中学到的是：

> `似乎“WARC-Identified-Content-Language”列有 1187 个独特的值，最常见的语言是英语（'eng'），其次是俄语（'rus'）、德语（'deu'）和中文（'zho'）。由于有如此多的独特值，分析变量的分布可能会很困难。简化分析的一种方法可能是将语言分为更广泛的类别，例如“英语”、“罗曼语系语言”、“斯拉夫语言”等，具体取决于研究问题。`

关于分组的观点非常好，但这里有遗漏——每行可以包含多种语言。当多次询问这个问题时，它提出了一个很好的方法。

```py
Here's an updated code that splits the rows with multiple languages and weighs them accordingly: import pandas as pd

# Split the rows with multiple languages
df['WARC-Identified-Content-Language'] = df['WARC-Identified-Content-Language'].str.split(',')

# Create a new DataFrame to store the split rows
df_split = pd.DataFrame({ 'WARC-Identified-Content-Language': [lang for langs in df['WARC-Identified-Content-Language'].tolist() for lang in langs], 'count': [1/len(langs) for langs in df['WARC-Identified-Content-Language'].tolist() for lang in langs] })

# Aggregate by language
df_agg = df_split.groupby('WARC-Identified-Content-Language')['count'].sum().reset_index()

# Plot the distribution of languages
plt.figure(figsize=(10,6))
sns.barplot(data=df_agg, x='WARC-Identified-Content-Language', y='count', order=df_agg.sort_values('count', ascending=False)['WARC-Identified-Content-Language'])
plt.title('Distribution of WARC-Identified-Content-Language') plt.xticks(rotation=90)
plt.show() 

This code splits the rows with multiple languages into separate rows, and weights them by the number of languages in each row. Then it aggregates the rows by language and plots the distribution of languages.
```

将这与一些关于语言分组的提示结合后，我们可以得到一个不错的图表：

我特别喜欢的一点是它给图表添加了标签。每次都写标签可能有点麻烦，但确实会有区别，所以看到它足够聪明地提供合理的标签是很不错的。

看起来英语仍然是主导语言——主导程度如何？

提示：

> `看起来主要是英语。你能计算一下汇总数据中英语的百分比吗？`

Response:

> `当然，这里有一些代码来计算汇总数据中英语的百分比：english_percentage = (agg_data.loc['eng', 'weight'] / agg_data['weight'].sum()) * 100`
> 
> `print(f"The percentage of the aggregated data that is English is: {english_percentage:.2f}%")`
> 
> `这应该输出汇总数据中英语的百分比，保留到小数点后两位。`

![](img/97e45cc4ac8d41f1e2bc71e205b86932.png)

（作者创建的图像）

# 自然语言处理

现在我们可以进入第 6 步，这涉及对文本列进行一些探索。这将更注重分析，并需要自然语言处理（NLP）技术。让我们看看 ChatGPT 的表现。

Prompt:

> `很好。我们进入第 6 步：`
> 
> `"探索“text”列，包括文本长度的分布和最常见的词汇。"`

Response:

```py
Sure! To explore the 'text' column, we can start by calculating the length of each text and creating a histogram to visualize the distribution of text lengths. Here's the code to do that:

# Calculate the length of each text
df['text_length'] = df['text'].str.len()

# Create a histogram of text lengths
plt.hist(df['text_length'], bins=50)

This will create a histogram that shows the distribution of text lengths in the 'text' column. We can then look at the most common words in the 'text' column by first removing any HTML tags, punctuation, and stop words. Here's the code to do that: import re

import nltk from nltk.corpus
import stopwords

# Remove HTML tags from text
df['text'] = df['text'].apply(lambda x: re.sub('<.*?>', '', x))
# Remove punctuation from text
df['text'] = df['text'].str.replace('[^\\w\\s]','')
# Convert text to lowercase
df['text'] = df['text'].str.lower()
# Remove stop words from text
stop_words = set(stopwords.words('english'))
df['text'] = df['text'].apply(lambda x: ' '.join([word for word in x.split() if word not in stop_words]))
# Tokenize text into individual words
df['text'] = df['text'].apply(nltk.word_tokenize)
# Flatten list of words into a single list
word_list = [word for sublist in df['text'] for word in sublist]

# Calculate frequency distribution of words
freq_dist = nltk.FreqDist(word_list)

# Print the 20 most common words
print(freq_dist.most_common(20)) 

This code will remove HTML tags and punctuation from the 'text' column, convert all text to lowercase, remove stop words, tokenize the text into individual words, and then calculate the frequency distribution of words. Finally, it will print the 20 most common words in the 'text' column.
```

结果如下：

> `[('nan', 36847), ('0', 3467), ('1', 2809), ('keresőoptimalizálás', 2498), ('idősgondozás', 1852), ('2', 1740), ('3', 1559), ('szerződés', 1529), ('eltartási', 1527), ('brands', 1442), ('5', 1362), ('4', 1350), ('usd', 1245), ('us', 1205), ('2023', 1204), ('new', 1178), ('sex', 1165), ('2022', 1031), ('home', 919), ('cookies', 904)]`

我对此不是很满意，因为我们可以看到很多“常见词汇”只是数字，或者是其他语言的停用词（它只使用了英语停用词）。我能够在这方面进行调整，通过过滤掉非字母字符的词汇和包括其他语言的停用词列表来帮助清理数据。现在让我们在词云中可视化这些数据。

Prompt:

> `你能制作一个词云吗？`

Response:

```py
Sure! To create a wordcloud from the top 50 most frequent words, you can use the WordCloud library in Python as follows: from wordcloud import WordCloud

import matplotlib.pyplot as plt

# Convert the top 50 most frequent words to a dictionary
freq_dict = dict(top_words)

# Generate the wordcloud
wordcloud = WordCloud(width=800, height=400, background_color='white').generate_from_frequencies(freq_dict)

# Display the wordcloud
plt.figure(figsize=(12, 8))
plt.imshow(wordcloud, interpolation='bilinear')
plt.axis('off')
plt.show()
This code will generate a wordcloud with the top 50 most frequent words in the text column. You can adjust the parameters of the WordCloud function to customize the appearance of the wordcloud.
```

![](img/f2578e3bf779c3c2340e134247601bd0.png)

警告：包含明显的语言。对于那些好奇的人，“keresőoptimalizálás”是匈牙利语中的“搜索引擎优化”（作者创建的图像）。

我不确定从这个图中能得到什么。似乎有一些词可能关联有用的内容（例如“信息”、“博客”、“品牌”），但也有很多不需要的内容。我将这些常见词汇分享给 ChatGPT 并询问它的看法：

Prompt:

> `我们对此可以说些什么？`

Response:

> `根据“text”列中发现的常见词汇，数据可能包含很多垃圾信息或不相关的信息，因为许多常见词汇与在线约会、评论、贷款和陪玩相关。此外，像“nan”这样的词汇表明“text”列中可能有缺失或不完整的数据。进一步调查“text”列的数据性质和质量对于确定其在后续分析中的有用性是重要的。`

这是一个合理的结论，实际上，与 Common Crawl 数据集合作的公司确实表示，他们投入了大量资源来清理数据集，以使其适合训练 AI 模型。

# Wrapping Up

这就结束了 ChatGPT 概述的计划，所以我们将在这里结束分析，并查看它有什么发现。

提示：

> `很好。让我们继续第 8 步。你能总结一下我们迄今为止的所有分析结果吗？`

回复：

> `当然，基于我们目前进行的分析，以下是一些关键发现：语言分布：数据集中最常见的语言是英语，其次是俄语、德语、中文和西班牙语。内容类型分布：数据集中大多数内容类型为"text/plain"。文本长度：数据集中，文本长度范围从 0 到 8,542,524，平均长度为 20,566。常见词：数据集中最频繁的词汇并不特别有信息量，因为它们包含许多停用词和其他非信息性术语。相关性：文本长度与内容长度之间存在强正相关。检测到的语言数量与内容长度之间也存在弱正相关。`

如果有数据分析师带着这些内容来找我，我会感到非常失望。正如开始时提到的，有一些列数据可以分析得更详细，并且通常更好的是提出一些下一步的建议。然而，我们从未给它设定分析的目标，也没告诉它给出建议，所以我不会太过苛责。

# 讨论

如你所见，整个过程中使用的提示相对简单。考虑到这一点，ChatGPT 能够很好地理解我想要的并给出有用的回答，这一点令人印象深刻。这些提示可以通过在每个提示中提供更多上下文以及对我们希望返回的内容更严格来改进。例如，每个提示可以包含对其专注的具体任务的引用，以及额外的文本以确保其做我们想要的：

> `不要回应多余的文本。假设 pandas、numpy 和 matplotlib 已按标准方式导入。`

这些可以保存在你自己的提示模板中以加快这类工作的进度，或者使用诸如[LangChain](https://python.langchain.com/en/latest/index.html)这样的工具来完成。

我们还可以定义自己的总体模板。我让 ChatGPT 提出一个计划，但它并不完美。我们可以定义一个总体结构供其遵循，以及分析每个变量的标准方法。通过模板，ChatGPT 在这种分析中遗漏洞察的可能性较小。

虽然与 ChatGPT 反复交流以获取数据输出很有趣，但很快变得疲惫。ChatGPT 在能够直接运行代码时更为强大。ChatGPT 可以通过使用[Python API](https://medium.com/geekculture/a-simple-guide-to-chatgpt-api-with-python-c147985ae28)连接到 Python 运行时。在这种情况下，代码可以自动运行，但要将人从循环中剔除，我们还需要一个额外的工具。

在过去一个月里，AutoGPT 作为 ChatGPT 的增强工具非常受欢迎，它有效地为 ChatGPT 代理提供了指南，允许他们持续朝着某个目标执行。AutoGPT 可以在这种情况下取代我，让 ChatGPT 代理设计代码，然后执行它，将结果反馈给 ChatGPT，直到它得到详细的分析。它还将与一个内存数据库接口，从而允许它执行更大规模的分析。

使用像 AutoGPT 这样的工具，我们可以设定一个明确的目标，包括分析的细节和期望的结论风格。在这种情况下，我们可以较少地检查结果，并最终很少需要工作即可得到一个不错的分析。

最后，我们应该指出 ChatGPT 远未达到“完美”，即便在这次模拟分析中，我也需要调整提示，以获得接近我想要的答案。虽然这比我预期的要容易得多，但仍然值得注意。它创建了一些有错误的代码，尽管每次被告知后都能修复这些错误。有时它创建的代码是我不想运行的，我需要建议它采用不同的方法，但再次提示后，它能够提出一个不错的解决方案。

# 结论

在本文中，我们已经看到 ChatGPT 如何用于支持探索性数据分析（EDA）的运行。我们发现，与系统合作时能够获得意想不到的良好结果，几乎不需要外部帮助。我们还注意到，已经存在一些工具可以扩展这一想法，例如 AutoGPT，它可能成为一个更强大的助手。

作为一名数据分析师，我已经在使用 ChatGPT 帮助我的分析，尽管我很少使用它进行本文中详细描述的端到端分析。随着像 AutoGPT 这样的工具的更多集成，以及使用摩擦的减少，我预计会越来越多地使用它，并且对此非常期待（虽然我不会被取代 ;) ）。
