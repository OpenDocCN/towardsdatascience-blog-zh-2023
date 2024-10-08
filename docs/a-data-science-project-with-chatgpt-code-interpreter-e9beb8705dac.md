# 一个使用 ChatGPT 代码解释器的数据科学项目

> 原文：[`towardsdatascience.com/a-data-science-project-with-chatgpt-code-interpreter-e9beb8705dac`](https://towardsdatascience.com/a-data-science-project-with-chatgpt-code-interpreter-e9beb8705dac)

## 使用 ChatGPT 最新插件构建一个端到端的数据科学项目。

[](https://natassha6789.medium.com/?source=post_page-----e9beb8705dac--------------------------------)![Natassha Selvaraj](https://natassha6789.medium.com/?source=post_page-----e9beb8705dac--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e9beb8705dac--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e9beb8705dac--------------------------------) [Natassha Selvaraj](https://natassha6789.medium.com/?source=post_page-----e9beb8705dac--------------------------------)

·发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----e9beb8705dac--------------------------------) ·12 分钟阅读·2023 年 7 月 20 日

--

![](img/2e8aaf8196668f54a02ff892d70ad943.png)

图片由[Firmbee.com](https://unsplash.com/@firmbee?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供，[Unsplash](https://unsplash.com/s/photos/data-analysis?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

作为一个目前同时管理全职数据科学工作和多个自由职业项目的人，我通常是第一个尝试可能减少我周转时间的工具的人。

当 ChatGPT 在过去一周开始向订阅者推出代码解释器插件时，我迫不及待想将其融入我的数据科学项目中。

如果你还没有听说过这个工具，[代码解释器](https://openai.com/blog/chatgpt-plugins)是一个允许你上传文档并在 ChatGPT 界面**内**运行 Python 程序的插件。

过去我们需要手动将数据复制粘贴到 ChatGPT 中并等待回应的时代已经过去。

借助代码解释器，你可以简单地上传数据集，让工具在几分钟内分析数据、构建机器学习模型并生成可视化。

在这篇文章中，我将向你展示如何使用代码解释器来执行一个端到端的数据科学项目。

# 任务——客户细分

在我以前的公司，我担任市场数据科学家。

这意味着我可以利用客户数据来提高销售——通过识别我们最盈利的用户、预测流失率，并建立应该在未来营销活动中针对的目标人群画像。

我甚至写了一篇[教程](https://365datascience.com/tutorials/python-tutorials/build-customer-segmentation-models/)关于如何用 Python 构建客户细分模型，其中我使用了一个公开的数据集来识别每个客户对电子商务公司的价值。

在本文中，我们将对相同的数据集进行客户细分。不过这一次，我们将使用 ChatGPT Code Interpreter 来帮助我们建立模型。

# 前提条件

我们将使用 Kaggle 的 [电子商务数据集](https://www.kaggle.com/datasets/carrie1/ecommerce-data?select=data.csv) 进行这次分析。该数据集来源于 [UCI 机器学习库](https://archive.ics.uci.edu/dataset/352/online+retail)，包括了一个基于英国的电子商务公司真实的零售交易信息。

该数据集在 [Creative Commons Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/legalcode) (CC BY 4.0) 许可下发布。

要开始这个分析，确保你安装了 Python IDE，并且安装了 Pandas、Matplotlib 和 Seaborn 库。

最后，如果你能访问 Code Interpreter，将更容易跟随本教程。

不过，这个插件目前仅对 ChatGPT Plus 订阅用户开放，因此如果你无法访问它，你仍然可以在我的 [GitHub 仓库](https://github.com/Natassha/KMeans-Code-Interpreter) 找到本次分析中使用的所有代码。

如果你有 ChatGPT Plus 订阅，可以观看 [这个](https://youtu.be/-HLxrQIFRKw) YouTube 视频来了解如何访问 Code Interpreter。

# 第一步：理解数据集

在我们将数据上传到 Code Interpreter 之前，让我们先加载到 Python 中，自行查看：

```py
import pandas as pd

df = pd.read_csv('ecommerce_dataset.csv', encoding='latin-1')
df.head()
```

![](img/c02dd877e94ff3d540fd5a791ccbee11.png)

图片来自作者

这个数据集包含了电子商务公司收集的交易数据。包括了所购商品、价格、购买来源和数量的信息。

注意，我们没有很多定性的信息可以使用。

如果数据集中包含了关于客户人口统计的信息、他们互动过的广告活动或他们访问过的网站，我们可以进行处方分析，并提出目标对象的推荐。

然而，这个数据集仅包含交易数据。我们可以进行的分析类型受到限制。

如前所述，我们将对该数据集进行客户细分。

不过，让我们向 Code Interpreter 请求一些关于如何分析这个数据集的想法，看看它会提出什么。

![](img/940c1d38e2be39c6a25140875d1f24b3.png)

图片来自作者

看那边！

在查看数据集后，模型的第一个建议是进行客户细分。

其他建议包括分析销售趋势随时间的变化以及识别表现最佳的产品。

在继续之前，我希望你再次查看数据框的前几行：

![](img/c02dd877e94ff3d540fd5a791ccbee11.png)

图片来自作者

**客户细分** 是根据共享特征创建不同受众群体的过程。

例如，如果我在卖瑜伽垫，我可以创建两个客户细分——健身爱好者和关注健康的成年人。

然而，我们在这个数据框中没有那种人口统计或心理数据。由于我们只有历史交易数据可以使用，因此可用的细分技术不多。

在我之前对这个电子商务数据集的分析中，我使用了一个叫做 [RFM 分析](https://www.natasshaselvaraj.com/rfm-analysis-in-python/)的技术来进行客户细分。

这帮助我使用有限的数据点识别了公司最有利润的客户。

> 让我们看看 Code Interpreter 能否提出如何进行客户细分的建议：

![](img/36316d6a0448c9a32ac7e94241abf417.png)

作者提供的图像

看这！

Code Interpreter 建议我们使用 RFM 分析来识别高利润用户的细分。

让我们进一步分解这个问题。

# 第 2 步：Code Interpreter 解释——什么是 RFM 分析？

![](img/cce30d146b97afe8150f6ce3e0bec9cf.png)

作者提供的图像

RFM 代表最近一次购买、频率和货币价值。

## 最近一次购买：

使用数据集中交易信息，我们可以识别每个客户最近一次购买的时间。

最近一次购买公司的客户**一个月前**的利润更高，而不是**一年前**见过的客户。

Code Interpreter 告诉我们，我们可以通过从数据集中**最近一次发票**中减去每个客户的**最近一次购买**的日期来计算客户的最近一次购买。

## 频率：

频率是一个相当直接的指标——它告诉我们客户与组织做生意的频率。

Code Interpreter 告诉我们，我们可以通过简单地计算每个客户的唯一发票数量来计算它。

## 货币价值：

最后，客户的货币价值告诉我们他们在公司交易中花费的总金额。

# 第 3 步：执行 RFM 分析

在解释了 RFM 的含义后，Code Interpreter 运行了一些 Python 代码以对电子商务数据集进行 RFM 分析。以下是生成的结果：

![](img/b3a346dc51653e1ca5574dabc0fa28f6.png)

作者提供的图像

*提示：你可以点击“显示工作”以查看 Code Interpreter 生成的代码。*

注意，每个客户 ID 现在都映射到一个 RFM 分数。

Code Interpreter 还解释了这些结果的含义：它表示一个 326 的最近一次购买分数表明客户在数据集中最近的日期之前的 326 天进行了最后一次购买。

如果你不理解解释的任何概念，你可以简单地提示 Code Interpreter，让它提供更多示例或甚至一个描述该主题的视觉图。

# 第 4 步：客户细分

![](img/af7c33af121dddad74d8da8bd2e14d40.png)

作者提供的图像

生成每个客户的 RFM 分数后，Code Interpreter 说我们现在可以使用这些分数进行客户细分。

这给我们提供了两个选项——我们可以通过根据分位数划分客户群体来手动进行分段，或者使用像 K-Means 聚类这样的无监督算法。

在决定使用哪个方法之前，让我们了解每种方法的优缺点：

```py
What is the best way to perform this segmentation? 
What are some advantages/disadvantages of ranking over clustering 
with K-Means?
```

![](img/3d8d3ed97e8b4b1934855ff01870e1e6.png)

图片由作者提供

![](img/56688a37d4077a91798fe0d3e05c03be.png)

图片由作者提供

代码解释器表示，虽然基于分位数的方法更易于实现，但没有固定的方法论，这意味着我们必须根据对业务的理解来决定每个分段的切割点。

## 代码解释器一针见血！

它识别出我认为的分位数基础分段方法的最大缺点——RFM 分数将作为整体来生成段。

例如，如果 Alison 的最近购买评分为 5，购买频次为 4，货币值为 2，她的总评分为**11**。分位数基础的方法将只考虑这个总评分（**11**）：

![](img/71ed266a98fe7045e0ccff97f0f2aed6.png)

图片由作者提供

如果我们使用这种技术，我们将失去一个关键的信息。

Alison 的最近购买频率和购买频次评分很高，这意味着她购物频繁且最近有过购买。但她每次购物的花费不多。

为了最大限度地利用 Alison 作为客户，商店需要针对她提供偶尔的折扣和特别促销。Alison 的价值在于她的品牌忠诚度和频繁的店铺访问。

她无法作为高价产品的目标，因为她可能没有足够的购买力。

由于分位数基础的分段方法的这个缺点，我们继续使用**K-Means 聚类**。

# 第 5 步：代码解释器执行 K-Means 聚类

K-Means 聚类是一种无监督的机器学习技术，用于将数据分成不重叠的子群体。

如果你想了解更多关于 K-Means 聚类如何工作的知识，你可以阅读[这个](https://365datascience.com/tutorials/python-tutorials/build-customer-segmentation-models/)教程。

代码解释器提到，K-Means 聚类技术的一个缺点是确定我们想要创建的子群体数量。

![](img/ebe5f8d507065fcf4364ad9acde834ed.png)

K-Means 聚类的 3 个簇示例（图片由作者提供）

这是因为 K-Means 聚类依赖于用户输入来决定我们想要构建的簇或受众群体的数量。

让我们看看代码解释器是否能帮助我们做出这个决定。

![](img/1025f8ff6fadb38b851e8c075183ba82.png)

图片由作者提供

模型建议了诸如肘部法、轮廓分析和间隙统计等技术。

我们继续使用**肘部法**。

这是一种简单的技术，通过绘制模型在一系列簇（例如 1 到 10）上的误差值图，并选择曲线的肘部作为簇的数量。

这是我给代码解释器的提示：

```py
Can we build a K-Means clustering model using the RFM scores that were 
calculated? Use the elbow method to determine the appropriate number of 
clusters.
```

模型生成了这张图：

![](img/bd8464961dd3116cdaed2a3b1c4c06ab.png)

图片由作者提供

![](img/a3cf17f2caec475783282edb5eea1ce0.png)

图片由作者提供

模型显示“肘部”点，即其拐点，大约在 3 或 4 附近——具体位置还不明确。

Code Interpreter 正在要求我们根据对业务的了解，决定是否继续使用 3 个或 4 个聚类。

由于更多的聚类可以让我们分析更多的用户细分市场，接下来我们用 4 个聚类来建立模型。

![](img/890804583c7a93a7c4424b888b50a5a2.png)

图片由作者提供

我请 Code Interpreter 给出输出数据框的一个示例：

![](img/e3e8633dbf815fe8c10bd95ea1e9795b.png)

图片由作者提供

很好！

Code Interpreter 已成功将数据集中所有用户分组为**4 个客户细分市场**。每个客户 ID 现在都映射到一个聚类。

# **第 6 步：Code Interpreter 进行聚类分析**

到目前为止，我们采取的所有步骤都很顺利。

Code Interpreter 已成功描述数据集，建议了分析技术，进行了 RFM 分析，甚至建立了一个机器学习模型。

然而，数据科学家的真正价值在于他们能够生成具有商业价值的洞察。

让我们看看 Code Interpreter 是否能够解释所创建的客户细分市场，并提出可能带来收入的建议。

我打算让模型可视化每个聚类的平均 RFM 值：

```py
Can we visualize the clusters that were created? 
Let's create a separate chart for each cluster. 
Each chart should showcase the cluster's mean R, F, and M scores.
```

Code Interpreter 生成了这张图表以可视化每个聚类的 RFM 分数：

![](img/4df1bd9bb5dd618b619a566d98cfc2d8.png)

图片由作者提供

注意图表上近期性和频率值几乎不可见。

这是因为这些变量的尺度不同：

![](img/360cdda160c0bf715c58f8f2b503662c.png)

图片由作者提供

注意“货币价值”列中的数据点明显高于“近期性”和“频率”分数。

## 我很惊讶 Code Interpreter 在我们要求其创建图表时没有指出这一点。

让我们询问模型一些改善清晰度的建议：

![](img/02671af375054cad313a6ce0b31455b6.png)

图片由作者提供

好的，Code Interpreter 建议我们要么标准化数据，要么为每个指标使用单独的图表，或者创建雷达图。

让我们选择第二个选项，为每个指标创建单独的条形图：

```py
To overcome the issue of them being on different scales, 
can we create separate bar charts for recency, frequency, and monetary value?
Each chart would display 4 clusters and 1 metric.
```

这是 Code Interpreter 生成的图表：

![](img/43ab3876e43bd088fb8efd8637845fbd.png)

图片由作者提供

请记住，**较低的近期性分数**是**更好的**，因为这意味着客户最近有过购买行为。

**频率**和**货币价值**则正好相反。

我认为第 1 类是最有利可图的细分市场——它具有较低的近期性，以及较高的频率和货币价值。

让我们看看 Code Interpreter 说了什么：

![](img/0d13a22cb176af775d451c52f309fdd0.png)

图片由作者提供

好吧，它只是读取图表并告诉我们哪些细分市场具有最佳的最近性、频率和货币价值。

让我们尝试从模型中获得更多见解：

![](img/a0885362d291a901594d4f4b631b0272.png)

图片由作者提供

这表明 Cluster 2 似乎是最有利可图的细分市场。

## 噢哦……看起来代码解释器犯了个错误！

Cluster 1 的 RFM 评分似乎比 Cluster 2 更好。不过，当我指出这一点时，代码解释器纠正了这一遗漏：

![](img/d5164566a214540c80ae9a739afe3294.png)

图片由作者提供

让我们更进一步，要求模型提出每个细分市场应该如何被针对的建议：

```py
Based on their RFM scores alone, can you come up with some 
personalized targeting recommendations on the best way to 
reach each customer segment?
```

我要求模型将这些信息以表格形式呈现，以便更清晰：

![](img/a86b5fa5e9570c459c3812f3e30519cb.png)

图片由作者提供

这些建议很不错。

在 RFM 方面，Cluster 0 远远是表现最差的细分市场，因此如果公司尚未失去这些客户，重新接触至关重要。

Cluster 1 是表现最好的细分市场，包含最有可能在新高端产品上消费的用户。

Cluster 2 包含购买次数**和**购物频率都很高的个人。他们在公司的产品上花费适中，如果我们想要获得更多的收益，就需要进行追加销售。

最后，Cluster 3 的用户在最近性、频率和货币价值方面表现中等。他们比 Cluster 0 中的个人更有价值，但比 Cluster 1 和 Cluster 2 的群体收益少。

为他们提供特别优惠并获取反馈以了解如何将他们转化为高价值客户是有意义的。

这些建议很有道理，考虑到模型没有额外的数据点作为参考，也没有关于公司的背景信息，我感到很印象深刻。

# 使用代码解释器进行数据科学——下一步

ChatGPT 代码解释器将 GPT-4 的推理能力与分析你的文件和运行代码的能力结合起来。

这使得它成为一个强大的工具，可以在创纪录的时间内构建机器学习模型和执行数据科学工作流。

它也是学习数据科学的一个很好的工具。

如果你在进行数据科学项目但不知道如何继续或对应该构建的模型类型有疑问，代码解释器是你最好的朋友。

你可以将其用作个人数据科学导师——只需上传你正在处理的数据集，并询问应该使用什么技术进行分析。

但请记住，由于它在后台使用了 ChatGPT，代码解释器会产生幻觉和犯错，就像在本教程的“集群分析”部分一样。

因此，核实代码解释器告诉你的所有信息是很重要的，特别是如果你正在构建一个模型或进行分析并将其展示给第三方时。

本文到此为止，感谢阅读！
