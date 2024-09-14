# GPT 与 BERT：哪一个更好？

> 原文：[`towardsdatascience.com/gpt-vs-bert-which-is-better-2f1cf92af21a`](https://towardsdatascience.com/gpt-vs-bert-which-is-better-2f1cf92af21a)

## 比较两个大型语言模型：方法与示例

[](https://pranay-dave9.medium.com/?source=post_page-----2f1cf92af21a--------------------------------)![Pranay Dave](https://pranay-dave9.medium.com/?source=post_page-----2f1cf92af21a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2f1cf92af21a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2f1cf92af21a--------------------------------) [Pranay Dave](https://pranay-dave9.medium.com/?source=post_page-----2f1cf92af21a--------------------------------)

·发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----2f1cf92af21a--------------------------------) ·阅读时长 6 分钟·2023 年 6 月 23 日

--

![](img/948d3d929fc0378773816d8614d45deb.png)

图片由 DALLE 创建，PPT 由作者制作（[`labs.openai.com/s/qQgpAQbLi0srYlZHOHQjGKWh`](https://labs.openai.com/s/qQgpAQbLi0srYlZHOHQjGKWh)）

生成式 AI 的流行也导致了大型语言模型数量的增加。在这个故事中，我将对两个模型进行比较：GPT 和 BERT。GPT（Generative Pre-trained Transformer）由 OpenAI 开发，基于仅解码器架构。而 BERT（Bidirectional Encoder Representations from Transformers）由 Google 开发，是一个仅编码器的预训练模型。

虽然技术上有所不同，但它们有一个相似的目标——执行自然语言处理任务。许多文章从技术角度比较这两者。然而，在这个故事中，我将基于它们的目标质量，即自然语言处理，进行比较。

# 比较方法

如何比较两种完全不同的技术架构？GPT 是仅解码器架构，而 BERT 是仅编码器架构。因此，将仅解码器与仅编码器架构进行技术比较，就像比较法拉利与兰博基尼——两者都很棒，但底盘下的技术完全不同。

然而，我们可以基于它们能够执行的共同自然语言任务的质量进行比较——即生成嵌入。嵌入是文本的向量表示。嵌入是任何自然语言处理任务的基础。因此，如果我们能够比较嵌入的质量，这将有助于我们判断自然语言任务的质量，因为嵌入是变换器架构进行自然语言处理的基础。

下面展示的是我将采取的比较方法。

![](img/24359bd94d2eed67508bada548a63ec7.png)

比较方法（图片由作者制作）

# 让我们从 GPT 开始

我掷了一枚硬币，GPT 赢了！所以我们先从 GPT 开始。我将从亚马逊的优质食品评论数据集中提取文本。评论是测试两个模型的好方法，因为评论用自然语言表达，非常自发。它们包含了客户的感受，可以包含所有类型的语言——好的、坏的、丑的！此外，它们可能有许多拼写错误的词、表情符号以及常用的俚语。

这里是评论文本的一个示例。

![](img/4ea4d5c9a51dd915cf658041d04f337e.png)

客户评论示例（图像由作者提供）

为了使用 GPT 获取文本的嵌入，我们需要向 OpenAI 发起 API 调用。结果是每个文本的嵌入或向量大小为 1540。以下是包括这些嵌入的示例数据。

![](img/a73855bdf98f492a735fdfbc55496629.png)

从模型获得的嵌入（图像由作者提供）

下一步是聚类和可视化。可以使用 KMeans 对嵌入向量进行聚类，并使用 TSNE 将 1540 维降至 2 维。下图展示了聚类和降维后的结果。

![](img/543dc8fc36262e8c118c163b3f02e17c.png)

GPT 嵌入聚类（图像由作者提供）

可以观察到集群形成得很好。悬停在某些集群上可以帮助理解集群的含义。例如，红色集群与狗粮相关。进一步分析还表明，GPT 嵌入正确识别了“Dog”和“Dawg”是相似的，并将它们放在同一集群中。

总体而言，GPT 嵌入提供了良好的结果，聚类质量也很高。

# 现在轮到 BERT 了

BERT 能否表现得更好？让我们来看看。BERT 模型有多个版本，如 bert-base-case、bert-base-uncased 等。它们的嵌入向量大小不同。以下是基于 Bert base 的结果，其嵌入大小为 768。

![](img/fef028ff4f6f8e13ba43cafd60909911.png)

BERT 嵌入（768）聚类（图像由作者提供）

绿色集群对应于狗粮。然而，可以观察到这些集群分布较广，相较于 GPT 并不够紧凑。主要原因是 768 的嵌入向量长度相对于 GPT 的 1540 嵌入向量长度较低。

幸运的是，BERT 也提供了更高的嵌入大小 1024。以下是结果。

![](img/a38adeaea80af457eb5c32b792053121.png)

BERT 嵌入（1024）聚类（图像由作者提供）

这里橙色集群对应于狗粮。该集群相对紧凑，相比于 768 的嵌入效果更好。然而，仍有一些点离中心较远。这些点被错误分类。例如，有一条咖啡评论，但由于包含了词“Dog”，被错误地分类为狗粮。

# 结论

显然，GPT 在嵌入质量上比 BERT 表现更好。然而，我不想将所有功劳归于 GPT，因为还有其他方面需要比较。以下是总结表格

![](img/275c1b72b0c27fc30ca42c4354f1fac3.png)

GPT 因提供更高嵌入质量而优于 BERT。然而，GPT 需要付费 API，而 BERT 是免费的。此外，BERT 模型是开源的，非黑箱，你可以进一步分析以更好地理解它。OpenAI 的 GPT 模型是黑箱的。

总之，我建议使用 BERT 处理中等复杂度的文本，如有策划的网页或书籍。GPT 可用于处理非常复杂的文本，如完全自然语言且未经过策划的客户评论。

# 技术实现

这里是一个 Python 代码片段，实施了故事中描述的过程。为了说明，我给出了 GPT 的例子。BERT 的实现类似。

```py
 ##Import packages
import openai
import pandas as pd
import re
import contextlib
import io
import tiktoken
from openai.embeddings_utils import get_embedding
from sklearn.cluster import KMeans
from sklearn.manifold import TSNE

##Read data
file_name = 'path_to_file'
df = pd.read_csv(file_name)

##Set parameters
embedding_model = "text-embedding-ada-002"
embedding_encoding = "cl100k_base"  # this the encoding for text-embedding-ada-002
max_tokens = 8000  # the maximum for text-embedding-ada-002 is 8191
top_n = 1000
encoding = tiktoken.get_encoding(embedding_encoding)
col_embedding = 'embedding'
n_tsne=2
n_iter = 1000

##Gets the embedding from OpenAI
def get_embedding(text, model):
  openai.api_key = "YOUR_OPENAPI_KEY"
  text = text.replace("\n", " ")
  return openai.Embedding.create(input = [text], model=model)['data'][0]['embedding']

col_txt = 'Review'
df["n_tokens"] = df[col_txt].apply(lambda x: len(encoding.encode(x)))
df = df[df.n_tokens <= max_tokens].tail(top_n)
df = df[df.n_tokens > 0].reset_index(drop=True) ##Remove if there no tokens, for example blank lines
df[col_embedding] = df[col_txt].apply(lambda x: get_embedding(x, model='text-embedding-ada-002'))
matrix = np.array(df[col_embedding].to_list())

##Make clustering
kmeans_model = KMeans(n_clusters=n_clusters,random_state=0)
kmeans = kmeans_model.fit(matrix)
kmeans_clusters = kmeans.predict(matrix)

#TSNE
tsne_model = TSNE(n_components=n_tsne, verbose=0, random_state=42, n_iter=n_iter,init='random')
tsne_out = tsne_model.fit_transform(matrix)
```

# 数据集引用

数据集在这里提供，许可证为 CC0 公共领域。 [**商业和非商业使用均被允许**](https://www.ibm.com/community/terms-of-use/download/)**.**

[](https://www.kaggle.com/datasets/snap/amazon-fine-food-reviews?source=post_page-----2f1cf92af21a--------------------------------) [## 亚马逊优质食品评论

### 分析来自亚马逊的约 500,000 条食品评论

[www.kaggle.com](https://www.kaggle.com/datasets/snap/amazon-fine-food-reviews?source=post_page-----2f1cf92af21a--------------------------------)

请**订阅**以便在我发布新故事时保持信息更新。

[](https://pranay-dave9.medium.com/subscribe?source=post_page-----2f1cf92af21a--------------------------------) [## 每当 Pranay Dave 发布新内容时获取电子邮件通知。

### 每当 Pranay Dave 发布新内容时获取电子邮件通知。注册后，如果你还没有 Medium 账户，将会创建一个...

[pranay-dave9.medium.com](https://pranay-dave9.medium.com/subscribe?source=post_page-----2f1cf92af21a--------------------------------)

你也可以通过我的推荐链接**加入 Medium**

[](https://pranay-dave9.medium.com/membership?source=post_page-----2f1cf92af21a--------------------------------) [## 使用我的推荐链接加入 Medium - Pranay Dave

### 作为 Medium 会员，你的一部分会员费用将转给你阅读的作者，并且你可以完全访问每一个故事...

[pranay-dave9.medium.com](https://pranay-dave9.medium.com/membership?source=post_page-----2f1cf92af21a--------------------------------)

# 附加资源

# 网站

你可以访问我的网站进行零编码分析。[**https://experiencedatascience.com**](https://experiencedatascience.com/)

# YouTube 频道

请访问我的 YouTube 频道，通过演示学习数据科学和 AI 的应用案例

[](https://www.youtube.com/c/DataScienceDemonstrated?source=post_page-----2f1cf92af21a--------------------------------) [## 数据科学展示

### 通过演示学习数据科学。无论你从事什么职业，坐下来，放松心情，享受这些视频。我的名字是……

[www.youtube.com](https://www.youtube.com/c/DataScienceDemonstrated?source=post_page-----2f1cf92af21a--------------------------------)
