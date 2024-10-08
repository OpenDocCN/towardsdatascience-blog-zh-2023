# ChatGPT 生成的食品行业评论：现实性评估

> 原文：[`towardsdatascience.com/chatgpt-generated-food-industry-reviews-realism-assessment-2ee28155970f`](https://towardsdatascience.com/chatgpt-generated-food-industry-reviews-realism-assessment-2ee28155970f)

## 探讨如何通过 ChatGPT 生成的数据来支持食品行业公司收集评论和调查。

[](https://ben-mccloskey20.medium.com/?source=post_page-----2ee28155970f--------------------------------)![Benjamin McCloskey](https://ben-mccloskey20.medium.com/?source=post_page-----2ee28155970f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2ee28155970f--------------------------------)![数据科学前沿](https://towardsdatascience.com/?source=post_page-----2ee28155970f--------------------------------) [Benjamin McCloskey](https://ben-mccloskey20.medium.com/?source=post_page-----2ee28155970f--------------------------------)

·发表于 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----2ee28155970f--------------------------------) ·阅读时间 13 分钟·2023 年 6 月 9 日

--

![](img/45ea0b6597515a537982a20603e6b762.png)

图片由 [Annie Spratt](https://unsplash.com/@anniespratt?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 起点

我过去的大部分研究使用了*生成对抗网络（GAN）*来创建数据集的深度伪造图像。我这样做是为了增加数据集中的信息多样性，我预测这会导致更好的目标检测模型（[了解更多关于这项研究的信息！](https://journals.sagepub.com/doi/abs/10.1177/15485129231170225?journalCode=dmsa)）。虽然这是与深度伪造图像创建完全不同的任务，但我想知道：***是否有办法增加我用于不同食品公司评论的数据集的规模？***

我能训练一个 GAN 吗？可以，但 GAN 在生成表格数据方面表现不佳，而对我而言，文本中的词汇更符合电子表格中的数据。然后出现了 ChatGPT。瞧！我是否可以通过简单地*请求* ChatGPT 用不同的提示生成新的评论来为我的数据集创建新的评论？

# 为什么这很重要

我们想要增加数据集规模有几个原因。

> 缺乏足够的数据来训练模型。
> 
> 数据集存在偏见（因此我们需要用少数类数据来矫正它）。
> 
> 数据集缺乏多样性

我创建的数据集（获得了今天示例中公司批准）缺乏负面评论、评论多样性和规模，因此需要实施数据集增强。

**数据不足以训练模型**。如果你试图在缺乏数据的情况下构建模型，可能会出现各种问题。一个问题可能是模型对数据过拟合，并在实际应用中表现不佳。

**偏见数据集**。如果数据集被某一类主导，则缺乏其他类的代表，这将导致模型和分析不适用于该类。我们希望拥有一个平衡的数据集，以确保我们的模型在我们感兴趣的所有数据类的操作中表现良好。

**数据集缺乏多样性**。现实世界是混乱的。如果我们的数据集缺乏多样性，一旦投入生产，我们的模型将无法*泛化*到样本细节的变化上。**泛化能力**是模型能够分类或识别样本属于某一类，即使该样本包含与其所属类不同或在该类中未充分代表的特征。

# 分析

今天的分析使用了以下 API。

```py
from gensim.models import Word2Vec,KeyedVectors
import gensim.downloader as api
import pandas as pd 
import matplotlib.pyplot as plt
import numpy as np
import seaborn as sns
import spacy
import spacy.cli
import spacy
import numpy as np
from random import sample
from sklearn.metrics.pairwise import cosine_similarity
from sklearn.feature_extraction.text import TfidfVectorizer

#To download 
spacy.cli.download("en_core_web_lg")Dataset
```

# 数据集

原始数据集是与公司（Altomontes Inc）授权的情况下汇编的，用于使用评论（请参阅这两篇文章，我展示了如何对评论使用自然语言处理）。

[## 机器学习不仅仅是为了大科技公司](https://towardsdatascience.com/machine-learning-is-not-just-for-big-tech-using-natural-language-processing-to-support-small-8f571249c073?source=post_page-----2ee28155970f--------------------------------) 

### 使用自然语言处理来支持小型企业。

towardsdatascience.com [## 针对小型企业的主题建模分析](https://towardsdatascience.com/topic-modeling-analysis-for-small-businesses-73ba23474261?source=post_page-----2ee28155970f--------------------------------)

### 介绍

towardsdatascience.com

**原始评论示例：**

> *“强烈推荐！！！我之前从未听说过 Altomontes，直到最近一个朋友把餐点送到我家。我的丈夫和我决定去看看，在那里我们遇到了老板，她是最可爱的人！她基本上给我们做了一个参观。我们买了鸡肉玛莎拉作为晚餐，非常棒！我们还买了布鲁克林比萨作为午餐，非常美味！我们品尝了他们的咖啡，他们让我们尝试了卡诺利、饼干和小饼干。非常好！我们买了贝壳面、意大利面酱、一瓶酒、一些奶酪等等。一切看起来都很不错，味道也很好，我们可以在那儿呆几个小时！我们会回来的！！也许今天？”*

我使用 ChatGPT 创建了接下来的两个数据集。一个包含有关意大利市场及其销售商品的积极评论，另一个包含有关意大利市场及其销售商品的负面评论。

**ChatGPT 生成的正面评论示例：**

> *“我买的佩科里诺托斯卡纳奶酪味道浓郁而美味。其坚硬且易碎的质地，带有一丝草香，使其非常适合磨碎、刮削或单独享用。”*

**ChatGPT 生成的负面评论示例：**

> *“我尝试的火腿和芝麻菜比萨有枯萎的芝麻菜，火腿也很硬。没有胃口。”*

一旦所有评论被创建并放入 CSV 文件（在 [这里](https://medium.com/towards-data-science/topic-modeling-analysis-for-small-businesses-73ba23474261) 找到），我将它们格式化为一个字典，每个键是源（源为 ***原始、生成的正面*** 或 ***生成的负面***）而项目是评论及其来源的元组列表。

```py
#Dictionary: Keys=Source, Values=Reviews
#Lists: List of reviews for each dataset

reviews = []
reviews_dict = {}

reviews_dict['original review'] = []
reviews_dict['fake positive review'] = []
reviews_dict['fake negative review'] = []

#Original Revews
orig_reviews = pd.read_csv('/content/drive/MyDrive/reviews/Altomontes_reviews.csv')
for rev in orig_reviews.Review:
  reviews_dict['original review'].append(rev)
  reviews.append((rev,'original review'))

#positive reviews
pos_reviews = pd.read_csv('/content/drive/MyDrive/reviews/generated_positive_reviews - Sheet1.csv')
for rev in pos_reviews.Review:
  reviews.append((rev,'fake positive review'))
  reviews_dict['fake positive review'].append(rev)

#negstive reviews
neg_reviews = pd.read_csv('/content/drive/MyDrive/reviews/generated_negative_reviews - Sheet1.csv')
for rev in neg_reviews.Review:
  reviews.append((rev,'fake negative review'))
  reviews_dict['fake negative review'].append(rev)An example of an original review:
```

# 句子评估

## 现实性评估

首先，我只是想评估这些评论是否看起来“现实”。这类似于计算给定文本的 *连贯性* 及其联系。我的初步想法是它们都会被认为是现实的，但我觉得也有必要可视化原始评论、人工正面评论和人工负面评论的得分。

要开始这个评估，我们首先要创建一个 *assess_sentence_realism* 函数。这个函数旨在检查一个句子的连贯性，以及输入句子是否“现实”到符合人类的解读方式。

```py
def assess_sentence_realism(sentence, model):
    """
  A function that accepts a sentence and embeddings model as inputs, and outputs a 
  'realism' score based on the cohesion and similarity between words of the 
  sentence

  Inputs:
  sentence (str): A string of words.
  model (.model): An embedding model (user's choice)

  Returns:
  avg_similarity: An average similarity score between the words of the sentence.

  """
    tokens = sentence.split()

    # Calculate the average similarity between adjacent word pairs
    similarities = []
    for i in range(len(tokens) - 1):
        word1 = tokens[i]
        word2 = tokens[i + 1]
        if word1 in model.key_to_index and word2 in model.key_to_index:
            word1_index = model.key_to_index[word1]
            word2_index = model.key_to_index[word2]
            similarity = model.cosine_similarities(
                model.get_vector(word1),
                [model.get_vector(word2)]
            )[0]
            similarities.append(similarity)

    # Calculate the average similarity score
    if similarities:
        avg_similarity = sum(similarities) / len(similarities)
    else:
        avg_similarity = 0.0

    return avg_similarity
```

你需要下载一个嵌入模型来创建你的词嵌入。我选择使用的模型是 Google News 300 模型（查看 [这里](https://huggingface.co/fse/word2vec-google-news-300)）。

```py
# Download the pre-trained Word2Vec model
#model_name = 'word2vec-google-news-300'  # Example model name
#model = api.load(model_name)
#model.save('/content/drive/MyDrive/models/word2vec-google-news-300.model') 

pretrained_model_path = '/content/drive/MyDrive/models/word2vec-google-news-300.model'
model = KeyedVectors.load(pretrained_model_path)

scores = {}
sources = []

# Evaluate the realism score for each sentence and store in scores dictionary
for sentence, source in reviews:
    realism_score = assess_sentence_realism(sentence, model)
    if source in scores:
        scores[source].append(realism_score)
    else:
        scores[source] = [realism_score]
    sources.append(source)
    #print(f"Realism Score for {source}: {realism_score}")

# Calculate the mean score for each source
mean_scores = {source: np.mean(score_list) for source, score_list in scores.items()}

# Plot the mean scores in a scatter plot
colors = {'original review': 'green','fake positive review':'blue','fake negative review':'red'}
sns.set_style("darkgrid", {"axes.facecolor": ".9"})
plt.figure(figsize=(8, 6))
plt.bar(mean_scores.keys(), mean_scores.values(),color=['green','blue','red'])
plt.xlabel("Source")
plt.ylabel("Mean Realism Score")
plt.title("Mean Realism Scores by Source")
plt.show()
```

→ **原始数据集得分：** 0.17

→ **ChatGPT 生成的正面数据集得分：** 0.15

→ **ChatGPT 生成的负面数据集得分：** 0.16

![](img/29b9e7c8486e85ac4e1987444ec88289.png)

平均现实性得分（图像来自作者）

正如预期（也许），原始评论被评为最现实。为什么会这样呢？首先，**它们是真实的评论**。我还怀疑 ChatGPT 得分较低的一个重要原因是，随着时间的推移，模型遵循了一定的评论模式。一些评论几乎相同，ChatGPT 只是简单地改变了几个词。

> (即 *The* ***比萨*** *不好且很干 → The* ***意大利面*** *不好且很干)*

与真实评论相比，ChatGPT 评论的 **多样性** 也有所欠缺，这在预期之中（考虑一下，真实评论是由多人写的，而假评论则是由一个模型生成的）。尽管如此，与原始评论相比，ChatGPT 生成的评论得分仍然相对较高，我认为值得尝试未来用这些评论来训练我们的模型。

**在此之前，我们还要进行一次评论的相似性评估。**

## **相似性评估**

接下来，我想查看每一批生成的评论与原始评论之间的相似性。为此，我们可以使用*余弦相似性*来计算每个来源的不同句子向量之间的相似性。首先，我们可以创建一个余弦相似性矩阵，该矩阵将首先使用 TfidVectorizer()将我们的句子转换为向量，然后计算这两个新的句子向量之间的余弦相似性。

```py
def cosine_similarity(sentence1, sentence2):
  """
  A function that accepts two sentences as input and outputs their cosine
  similarity

  Inputs:
  sentence1 (str): A string of word
  sentence2 (str): A string of words 

  Returns:
  cosine_sim: Cosine similarity score for the two input sentences
  """
  # Initialize the TfidfVectorizer
  vectorizer = TfidfVectorizer()

  # Create the TF-IDF matrix
  tfidf_matrix = vectorizer.fit_transform([sentence1, sentence2])

  # Calculate the cosine similarity
  cosine_sim = cosine_similarity(tfidf_matrix[0], tfidf_matrix[1])

  return cosine_sim[0][0]
```

我遇到的一个问题是数据集现在变得非常庞大，以至于计算需要很长时间（有时我在 Google Colab 上没有足够的 RAM 继续）。为了解决这个问题，我从每个数据集中随机抽取了 200 条评论来计算相似性。

```py
#Random Sample 200 Reviews
o_review = sample(reviews_dict['original review'],200)
p_review = sample(reviews_dict['fake positive review'],200)
n_review = sample(reviews_dict['fake negative review'],200)

r_dict = {'original review': o_review,
          'fake positive review': p_review,
          'fake negative review':n_review}
```

现在我们有了随机选择的样本，我们可以查看不同数据集组合之间的余弦相似性。

```py
#Cosine Similarity Calcualtion
source = ['original review','fake negative review','fake positive review']
source_to_compare = ['original review','fake negative review','fake positive review']
avg_cos_sim_per_word = {}
for s in source:
  count = []
  for s2 in source_to_compare:
    if s != s2:
      for sent in r_dict[s]:
          for sent2 in r_dict[s2]:
            similarity = calculate_cosine_similarity(sent, sent2)
            count.append(similarity)
      avg_cos_sim_per_word['{0} to {1}'.format(s,s2)] = np.mean(count)

results = pd.DataFrame(avg_cos_sim_per_word,index=[0]).T 
```

![](img/d594f9d323c86bc698842286861e4799.png)

余弦相似性结果（图片来源：作者）

对于原始数据集，负面评论的相似性更高。我的假设是因为我用更多的提示来创建负面评论，而不是正面评论。毫不奇怪，ChatGPT 生成的评论显示出它们之间的相似性最高。

很好，我们已经得到了余弦相似性，但我们还能采取其他步骤来评估评论的相似性吗？可以的！让我们将句子可视化为向量。为此，我们必须将句子嵌入（将它们转化为数字向量），然后可以在二维空间中可视化它们。我使用了 Spacy 来嵌入我的向量并可视化它们。

```py
# Load pre-trained GloVe model
nlp = spacy.load('en_core_web_lg')

source_embeddings = {}

for source, source_sentences in reviews_dict.items():
    source_embeddings[source] = []
    for sentence in source_sentences:
        # Tokenize the sentence using spaCy
        doc = nlp(sentence)

        # Retrieve word embeddings
        word_embeddings = np.array([token.vector for token in doc])

        # Save word embeddings for the source
        source_embeddings[source].append(word_embeddings)
def legend_without_duplicate_labels(figure):
    handles, labels = plt.gca().get_legend_handles_labels()
    by_label = dict(zip(labels, handles))
    figure.legend(by_label.values(), by_label.keys(), loc='lower right')

# Plot embeddings with colors based on source

fig, ax = plt.subplots()
colors = ['g', 'b', 'r']  # Colors for each source
i=0
for source, embeddings in source_embeddings.items():
    for embedding in embeddings:
        ax.scatter(embedding[:, 0], embedding[:, 1], c=colors[i], label=source)
    i+=1
legend_without_duplicate_labels(plt)
plt.show()
```

![](img/b9f69002cab2db51681c3d5d439a6204.png)

句子向量（图片来源：作者）

好消息是，我们可以清楚地看到句子向量的嵌入和分布紧密对齐。目测显示，*原始评论*的分布变异性更大，这支持了**它们更*多样化*的断言**。由于 ChatGPT 生成了正面和负面评论，我们会怀疑它们的分布应该是一样的。然而，请注意，假负面评论实际上具有比正面评论更广泛的分布和更多的变异性。*这可能是为什么呢？* 可能部分原因是因为我不得不欺骗 ChatGPT 以生成假负面评论（ChatGPT 被设计成说正面评价），而且我实际上需要给 ChatGPT 提供更多的提示来获取足够的负面评论与正面评论。这对数据集有帮助，因为通过数据集的额外多样性，我们可以训练出更高性能的机器学习模型。

![](img/9242966364ffcbedb0fdbfdb78a7c957.png)

数据集中的句子向量（图片来源：作者）

接下来，我们可以检查三种不同分布的评论之间的差异，看看是否存在任何区分性的模式。

*我们看到了什么*？从视觉上看，我们可以看到数据集中大部分评论都集中在原点附近，并且范围从-10 到 10。这是一个积极的迹象，支持使用虚假评论来训练预测模型。方差基本相同，但原始评论的分布在横向和纵向上都有更宽的方差，这表明这些评论中的词汇多样性更大。ChatGPT 的评论分布也非常相似，但正面评论有更多的异常值。如前所述，这些区别可能是由于我提示系统生成评论的方式造成的。

# **陷阱和不足**

尽管增加数据集的大小和多样性有许多额外的好处，但这种方法也存在弱点和陷阱。新生成的数据可能无法代表或接近真实数据的格式。虽然我们可以进行一些数学计算和可视化以支持相似性，但我们永远不能确定评论在机器语言中会被如何解读。我们可以用这些数据为食品行业的公司开发读取调查问卷和评论的模型，但当面对来自现实世界的非结构化“脏”数据时，模型可能会崩溃，因为我们训练的模型更多是基于遵循潜在模式的虚假数据。

另一个问题是，一旦加入虚假数据，我们就失去了使用各种分析技术提取信息的能力。例如，如果我用这个新数据集进行主题建模分析，主题将不仅定义原始数据。它们现在也将定义虚假数据，这对我的客户没有任何意义。当我创建了虚假评论声称某个事实时，为什么我的客户会关心“意大利面是否干燥”是一个主题？这是我的问题，而不是他们的问题。坦白说，这个过程阻碍了我们进行探索性数据分析（EDA）的能力。我认为这是最大的权衡：使用这个数据集，我们可以创建分类和预测模型，这些模型可能适用于解释新的评论（可能由于数据集规模的增加而更好，但你需要构建测试过程来验证这一点），代价是无法从公司已经拥有的数据中提取更多信息（如果我们使用这个数据集的话）。

我对任何使用生成数据的人最大的警告是，不要忘记你收集的原始数据。不要忘记你试图解决的原始问题和问题。忘记这一点可能会导致你陷入试图解决一个存在于虚假数据中的问题的死胡同！

# 结论

数据科学中一个常见的问题是数据的缺乏以及数据的多样性不足。生成新数据的方法有很多，今天展示了如何利用 ChatGPT 来为你的数据集创建更多的数据。今天的发现对那些在*食品行业*工作的人员尤其有帮助。这样做可以缓解数据集中的不平衡和多样性缺乏问题，从而使模型在训练后的真实世界数据上表现更佳。

***今天展示了什么？***

ChatGPT 数据可能对你的下一个自然语言处理项目（NLP）有帮助，特别是当你为食品行业的业务实施数据科学技术时。**我建议首先尽量收集真实数据。** 如果你发现你的数据集需要更多数据，可以考虑探索生成对抗网络（GAN）或大型语言模型（LLM），例如 ChatGPT。最后，我想特别强调，尤其是在使用生成性人工智能时，重要的是以伦理和积极的方式使用这些工具，以便对所有相关方产生良好的影响。

**你可能会想，这些工具的伦理使用意味着什么？** 你应该使用生成性人工智能来支持人们，并对他们的生活产生积极影响。有些人使用深度伪造技术来损害他人形象，这是绝对不可接受的。此外，生成性人工智能不应该被用来欺骗他人或用虚假的、不真实的数据改变他们的想法。今天的例子是一个完美的使用案例，展示了我们如何创建数据以训练模型，使公司理解客户评价的情感。这将帮助公司调整其产品和流程，以满足客户需求，从而对双方都产生积极影响！

**如果你喜欢今天的阅读，请关注我，并告诉我是否有其他话题你希望我探讨！如果你没有 Medium 账户，可以通过我的链接** [**这里**](https://ben-mccloskey20.medium.com/membership) **注册（这样我会获得少量佣金）！另外，可以在** [**LinkedIn**](https://www.linkedin.com/in/benjamin-mccloskey-169975a8/) **上加我，或者随时联系我！感谢阅读！**

# 来源

1.  数据使用经 Altomontes Inc.批准

1.  [完整代码请见这里](https://github.com/benmccloskey/food_industry)
