# 基于词级 BERT 嵌入趋势生成句子级别嵌入

> 原文：[`towardsdatascience.com/generating-sentence-level-embedding-based-on-the-trends-in-token-level-bert-embeddings-eb08fa2ad3c8?source=collection_archive---------16-----------------------#2023-01-05`](https://towardsdatascience.com/generating-sentence-level-embedding-based-on-the-trends-in-token-level-bert-embeddings-eb08fa2ad3c8?source=collection_archive---------16-----------------------#2023-01-05)

![](img/9275b61be55a869b9a7d56ef53ce7455.png)

图片由 Igor Shabalin 提供

## 如何从词嵌入中推导句子级别的嵌入

[](https://jxireal.medium.com/?source=post_page-----eb08fa2ad3c8--------------------------------)![Yuli Vasiliev](https://jxireal.medium.com/?source=post_page-----eb08fa2ad3c8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----eb08fa2ad3c8--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----eb08fa2ad3c8--------------------------------) [Yuli Vasiliev](https://jxireal.medium.com/?source=post_page-----eb08fa2ad3c8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F83cfb869ab36&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenerating-sentence-level-embedding-based-on-the-trends-in-token-level-bert-embeddings-eb08fa2ad3c8&user=Yuli+Vasiliev&userId=83cfb869ab36&source=post_page-83cfb869ab36----eb08fa2ad3c8---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----eb08fa2ad3c8--------------------------------) ·5 min read·2023 年 1 月 5 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Feb08fa2ad3c8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenerating-sentence-level-embedding-based-on-the-trends-in-token-level-bert-embeddings-eb08fa2ad3c8&user=Yuli+Vasiliev&userId=83cfb869ab36&source=-----eb08fa2ad3c8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Feb08fa2ad3c8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenerating-sentence-level-embedding-based-on-the-trends-in-token-level-bert-embeddings-eb08fa2ad3c8&source=-----eb08fa2ad3c8---------------------bookmark_footer-----------)

句子（短语或段落）级别的嵌入常用于许多自然语言处理分类问题中，例如，垃圾邮件检测和问答（QA）系统。在我之前的文章中，[发现 BERT 嵌入不同层次的趋势以确定语义上下文](https://medium.com/p/268733fdb17e)，我讨论了如何生成一个向量表示，该向量包含有关相对于同一标记静态嵌入值的上下文嵌入值变化的信息，然后可以将其用作生成句子级别嵌入的组件。本文扩展了这一主题，探索了从句子中的哪些标记中提取这样的趋势向量，以便能够为整个句子生成有效的嵌入。

# 直觉

与此相关的第一个问题是：从句子中的多少个标记中提取嵌入以生成整个句子的有效嵌入？如果你回顾之前的讨论，我们得到的是一个向量——为句子中最重要的单词提取的——其中包含有关整个句子上下文的信息。然而，为了更好地了解句子上下文，拥有一个与最重要单词在句法上最相关的单词的向量也很有帮助。为什么我们需要这个？

生活中的一个简单类比可以帮助回答这个问题：如果你坐在塔楼内的餐厅里欣赏周围的美景——你所观察的视角将不包括塔楼本身。要拍摄塔楼的视角，你首先需要离开塔楼。

好的，我们如何确定句子中与最重要单词在句法上最相关的单词呢？（即，你需要根据之前的类比确定拍摄塔楼照片的最佳位置）答案是：借助注意力权重，你也可以从 BERT 模型中获得这些权重。

# 实现

在你可以跟随本文其余部分讨论的代码之前，你需要参考[之前的文章](https://medium.com/p/268733fdb17e)中提供的示例（我们将使用该示例中定义的模型和生成的向量表示）。你需要做的唯一修正是：在创建模型时，确保使模型返回不仅是隐藏状态，还有注意力权重：

```py
model = BertModel.from_pretrained(‘bert-base-uncased’,
 output_hidden_states = True, # so that the model returns all hidden-states.
 output_attentions = True
 ) 
```

其他所有内容，包括示例句子，都可以直接使用。实际上，我们将只使用第一个示例句子：“我想要一个苹果。”

下面我们正在确定与最重要的单词（在这个例子中是“想要”）在句法上最接近的单词。为此，我们检查所有 12 层的注意力权重。首先，我们创建一个空数组（不计算特殊符号，排除第一个和最后一个符号）：

```py
a = np.empty([0, len(np.sum(outputs[0].attentions[0][0][11].numpy(), axis=0)[1:-1])])
```

接下来，我们填充注意力权重矩阵：

```py
for i in range(12):
 a = np.vstack([a,np.sum(outputs[0].attentions[0][0][i].numpy(), axis=0)[1:-1]])
```

我们对标点符号不感兴趣。所以，我们将删除矩阵中的最后一列：

```py
a = np.delete(a, -1, axis=1)
```

所以我们的矩阵现在看起来如下（12x4，即 12 层和 4 个单词）

```py
print(a)
```

```py
[[0.99275106 1.00205731 0.76726311 0.72082734]
 [0.7479955 1.16846883 0.63782167 1.39036024]
 [1.23037624 0.40373796 0.57493907 0.25739866]
 [1.319888 1.21090519 1.37013197 0.7479018 ]
 [0.48407069 1.15729702 0.54152751 0.57587731]
 [0.47308242 0.61861634 0.46330488 0.47692096]
 [1.23776317 1.2546916 0.92190945 1.2607218 ]
 [1.19664812 0.51989007 0.48901123 0.65525496]
 [0.5389185 0.98384732 0.8789593 0.98946768]
 [0.75819892 0.80689037 0.5612824 1.10385513]
 [0.14660755 1.10911655 0.84521955 1.00496972]
 [0.77081972 0.79827666 0.45695013 0.36948431]]
```

现在让我们确定“Want”（第二列，索引为 1）在哪些层中吸引了最多的注意力：

```py
print(np.argmax(a,axis=1))
b = a[np.argmax(a,axis=1) == 1]
```

```py
array([1, 3, 0, 2, 1, 1, 3, 0, 3, 3, 1, 1])
```

接下来，我们可以确定在“Want”领先的层中，哪个标记在“Want”之后吸引了更多的注意力。为此，我们首先删除“Want”列，然后探讨其余的三列：

```py
c = np.delete(b, 1, axis=1)
d = np.argmax(c, axis =1)
print(d)
counts = np.bincount(d)
print(np.argmax(counts))
```

```py
[0 2 2 2 0]
2
```

上述内容表明，我们有单词 Apple（在这里删除了“Want”后，Apple 的索引为 2）作为与单词“Want”在句法上最相关的单词。这是相当意料之中的，因为这些词分别代表了直接宾语和及物动词。

```py
_l12_1 = hidden_states[0][12][0][4][:10].numpy()
_l0_1 = hidden_states[0][0][0][4][:10].numpy()
_l0_12_1 = np.log(_l12_1/_l0_1)
_l0_12_1 = np.where(np.isnan(_l0_12_1), 0, _l0_12_1)
```

现在让我们比较从单词 Apple 和 Want 的嵌入中得到的向量。

```py
print(_l0_12_1)
```

```py
array([ 3.753544 , 1.4458075 , -0.56288993, -0.44559467, 0.9137548 ,
 0.33285233, 0\. , 0\. , 0\. , 0\. ],
 dtype=float32)

print(l0_12_1) # this vector has been defined in the previous post
```

```py
array([ 0\. , 0\. , 0\. , 0\. , -0.79848075,
 0.6715901 , 0.30298436, -1.6455574 , 0.1162319 , 0\. ],
 dtype=float32)
```

如你所见，上述两个向量中匹配元素对的一项值在大多数情况下为零，而另一项值非零——即这些向量看起来是互补的（记住塔楼视角的类比：塔楼可以看到邻近的景象，但要看到塔楼本身——也许是主要的吸引物——你需要离开它）。因此，你可以安全地逐元素相加这些向量，将可用的信息合并为一个单一的向量。

```py
s = _l0_12_1 + l0_12_1
print(s)
```

```py
array([ 3.753544 , 1.4458075 , -0.56288993, -0.44559467, 0.11527407,
 1.0044425 , 0.30298436, -1.6455574 , 0.1162319 , 0\. ],
 dtype=float32)
```

上述向量可以作为句子级别分类的输入。

# 结论

本文提供了关于如何基于从静态嵌入到上下文嵌入的转换趋势，在词级 BERT 嵌入中生成句子级别嵌入的直观理解和代码。这种句子级别的嵌入可以作为 BERT 生成的 CLS 标记嵌入的替代方案用于句子分类，这意味着你可以尝试这两种方法，以查看哪一种最适合你的特定问题。
