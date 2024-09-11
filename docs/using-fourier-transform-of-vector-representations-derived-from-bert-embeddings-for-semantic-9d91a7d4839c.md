# 使用从 BERT 嵌入中衍生的向量表示的傅里叶变换进行语义相似度评估

> 原文：[`towardsdatascience.com/using-fourier-transform-of-vector-representations-derived-from-bert-embeddings-for-semantic-9d91a7d4839c?source=collection_archive---------1-----------------------#2023-01-14`](https://towardsdatascience.com/using-fourier-transform-of-vector-representations-derived-from-bert-embeddings-for-semantic-9d91a7d4839c?source=collection_archive---------1-----------------------#2023-01-14)

![](img/7727543fc629cc19f66d6aa3a5236e98.png)

照片由 Igor Shabalin 提供，已获许可

## 探索通过评估 BERT 嵌入的不同表示来了解句子中词语的相互影响

[](https://jxireal.medium.com/?source=post_page-----9d91a7d4839c--------------------------------)![Yuli Vasiliev](https://jxireal.medium.com/?source=post_page-----9d91a7d4839c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9d91a7d4839c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9d91a7d4839c--------------------------------) [Yuli Vasiliev](https://jxireal.medium.com/?source=post_page-----9d91a7d4839c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F83cfb869ab36&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-fourier-transform-of-vector-representations-derived-from-bert-embeddings-for-semantic-9d91a7d4839c&user=Yuli+Vasiliev&userId=83cfb869ab36&source=post_page-83cfb869ab36----9d91a7d4839c---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9d91a7d4839c--------------------------------) ·5 min read·2023 年 1 月 14 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9d91a7d4839c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-fourier-transform-of-vector-representations-derived-from-bert-embeddings-for-semantic-9d91a7d4839c&user=Yuli+Vasiliev&userId=83cfb869ab36&source=-----9d91a7d4839c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9d91a7d4839c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-fourier-transform-of-vector-representations-derived-from-bert-embeddings-for-semantic-9d91a7d4839c&source=-----9d91a7d4839c---------------------bookmark_footer-----------)

BERT 嵌入在程序化提取文本意义方面提供了极大的机会。似乎我们（以及机器）理解文本所需的一切都隐藏在这些数字中。关键在于如何正确地操作这些数字。我在我最近的帖子中讨论了这一概念 [发现 BERT 嵌入在不同层次中的趋势，用于语义上下文确定任务](https://medium.com/towards-data-science/discovering-trends-in-bert-embeddings-of-different-levels-for-the-task-of-semantic-context-268733fdb17e)。本文继续讨论这一主题，探讨通过对 BERT 嵌入应用傅里叶变换得到的向量表示是否在 NLP 任务中的意义提取方面也有用。

# 假设

正如你可能从物理学中知道的那样，傅里叶变换使我们能够理解信号中的频率。表示单词嵌入的向量是否看起来像信号？也就是说，从傅里叶变换得出的频域知识在处理 BERT 嵌入以发现语义接近性时是否有用？简单来说，傅里叶变换在分析 BERT 嵌入时是否有意义？让我们来检查一下。

# 实验

本文剩余部分讨论的例子假设你已经按照 [之前的帖子](https://medium.com/towards-data-science/discovering-trends-in-bert-embeddings-of-different-levels-for-the-task-of-semantic-context-268733fdb17e) 中的示例定义了模型。我们还需要从该 [之前的帖子](https://medium.com/towards-data-science/discovering-trends-in-bert-embeddings-of-different-levels-for-the-task-of-semantic-context-268733fdb17e) 中讨论的样本句子中获得的表示。请注意，本文使用了一组与上述帖子中使用的样本稍有不同的样本。因此，在创建表示之前，你需要定义以下样本：

```py
sents = []
sents.append(‘I had a very good apple.’)
sents.append(‘I had a very good orange.’)
sents.append(‘I had a very good adventure.’)
```

正如你所看到的，所有句子都有相同的直接宾语修饰词集合，而直接宾语在每个句子中都是不同的。我们实验的目的是检查直接宾语的语义接近性如何影响修饰词的接近程度。

之前的帖子中已经介绍了对样本句子进行分词和生成隐藏状态的过程。因此我们在这里不会重复这个过程。

让我们专注于每个句子中直接宾语（“apple/orange/adventure”）的修饰词（“a/very/good”）。为了在获取其嵌入时使用正确的索引，我们首先检查一下句子中的词元：

```py
for i in range(len(sents)):
 print(tokenizer.convert_ids_to_tokens(tokenized_text[i]))
```

```py
[‘[CLS]’, ‘i’, ‘had’, ‘a’, ‘very’, ‘good’, ‘apple’, ‘.’, ‘[SEP]’]
[‘[CLS]’, ‘i’, ‘had’, ‘a’, ‘very’, ‘good’, ‘orange’, ‘.’, ‘[SEP]’]
[‘[CLS]’, ‘i’, ‘had’, ‘a’, ‘very’, ‘good’, ‘adventure’, ‘.’, ‘[SEP]’]
```

由于我们对直接宾语的修饰词（在这个具体的例子中，我们有三个修饰词：*a, very, good*）感兴趣，我们需要索引为 *3,4,5* 的词元。因此，我们需要将索引偏移 3。以下，我们将为每个修饰词在每个句子中获取上下文嵌入。我们将把修饰词嵌入保存在为每个句子定义的列表中：

```py
l12_1 = []
l12_2 = []
l12_3 = []
for i in range(3):
 l12_1.append(hidden_states[0][12][0][i+3][:10].numpy())
 l12_2.append(hidden_states[1][12][0][i+3][:10].numpy())
 l12_3.append(hidden_states[2][12][0][i+3][:10].numpy())
```

现在让我们来探讨不同句子中直接宾语的语义相似性如何影响各自修饰语的相似性。

```py
from scipy import spatial
```

```py
for i in range(3):
 print(1 — spatial.distance.cosine(l12_1[i], l12_2[i]))
```

```py
0.9003266096115112
0.9178041219711304
0.8865049481391907
```

在上述输出中，我们可以看到“Apple”和“Orange”修饰语的上下文嵌入显示了高度的相似性。这是可以理解的，因为直接宾语“Apple”和“Orange”本身非常接近。

虽然从以下结果来看，“Apple”和“Adventure”修饰语的表示并不太接近：

```py
for i in range(3):
 print(1 — spatial.distance.cosine(l12_1[i], l12_3[i]))
```

```py
0.49141737818717957
0.7987119555473328
0.6404531598091125
```

“Orange”和“Adventure”修饰语也不应该太接近：

```py
for i in range(3):
 print(1 — spatial.distance.cosine(l12_2[i], l12_3[i]))
```

```py
0.7402883768081665
0.8417230844497681
0.7215733528137207
```

现在让我们从 BERT 提供的嵌入中推导出更复杂的表示。首先，让我们获取每个句子中修饰语的初始嵌入：

```py
l0_1 = []
l0_2 = []
l0_3 = []
for i in range(3):
 l0_1.append(hidden_states[0][0][0][i+3][:10].numpy())
 l0_2.append(hidden_states[1][0][0][i+3][:10].numpy())
 l0_3.append(hidden_states[2][0][0][i+3][:10].numpy())
```

现在，我们可以，例如，通过将上下文嵌入（在第 12 层编码器中生成）除以相应的初始嵌入（如前面帖子中讨论的），得到一些新的嵌入表示，用于进一步分析。

```py
import numpy as np
l0_12_1 = []
l0_12_2 = []
l0_12_3 = []
for i in range(3):
 l0_12_1.append(np.log(l12_1[i]/l0_1[i]))
 l0_12_2.append(np.log(l12_2[i]/l0_2[i]))
 l0_12_3.append(np.log(l12_3[i]/l0_3[i]))
for i in range(3):
 l0_12_1[i] = np.where(np.isnan(l0_12_1[i]), 0, l0_12_1[i])
 l0_12_2[i] = np.where(np.isnan(l0_12_2[i]), 0, l0_12_2[i])
 l0_12_3[i] = np.where(np.isnan(l0_12_3[i]), 0, l0_12_3[i])
```

为了分析目的，你可能还想创建另一组表示，计算上下文嵌入与非上下文（初始）嵌入之间的逐元素差异。

```py
_l0_12_1 = []
_l0_12_2 = []
_l0_12_3 = []
for i in range(3):
 _l0_12_1.append(l12_1[i]-l0_1[i])
 _l0_12_2.append(l12_2[i]-l0_2[i])
 _l0_12_3.append(l12_3[i]-l0_3[i])
```

在继续评估我们刚创建的表示之前，让我们使用傅里叶变换创建另一组表示，以便随后可以比较不同方法获得的所有表示。

```py
fourierTransform_1=[]
fourierTransform_2=[]
fourierTransform_3=[]
for i in range(3):
 fourierTransform_1.append(np.fft.fft(l0_12_1[i])/len(l0_12_1[i]))
 fourierTransform_2.append(np.fft.fft(l0_12_2[i])/len(l0_12_2[i]))
 fourierTransform_3.append(np.fft.fft(l0_12_3[i])/len(l0_12_3[i]))
```

现在我们可以比较每个修饰语的每对句子的获得表示。

```py
print (sents[0])
print (sents[1])
print()
for i in range(3):
 print(tokenizer.convert_ids_to_tokens(tokenized_text[0][i+3]))
 print(‘diff’, 1 — spatial.distance.cosine(_l0_12_1[i], _l0_12_2[i]))
 print(‘log_quotient’, 1 — spatial.distance.cosine(l0_12_1[i], l0_12_2[i]))
 print(‘fourier’, 1 — spatial.distance.cosine(abs(fourierTransform_1[i]), abs(fourierTransform_2[i])))
 print()
```

生成的输出应如下所示：

```py
I had a very good apple.
I had a very good orange.
```

```py
a
diff 0.8866338729858398
log_quotient 0.43184104561805725
fourier 0.9438706822278501
```

```py
very
diff 0.9572229385375977
log_quotient 0.9539480209350586
fourier 0.9754009221726183
```

```py
good
diff 0.8211167454719543
log_quotient 0.5680340528488159
fourier 0.7838190546462953
```

在上述实验中，我们期望看到前两个句子中的相同修饰语之间具有高度的相似性。实际上，我们可以看到差异和傅里叶变换方法在任务中表现良好。

以下实验的目的是确定那些被修饰的名词不太接近的修饰语的相似性。

```py
print (sents[0])
print (sents[2])
print()
for i in range(3):
 print(tokenizer.convert_ids_to_tokens(tokenized_text[0][i+3]))
 print(‘diff’, 1 — spatial.distance.cosine(_l0_12_1[i], _l0_12_3[i]))
 print(‘log_quotient’, 1 — spatial.distance.cosine(l0_12_1[i], l0_12_3[i]))
 print(‘fourier’, 1 — spatial.distance.cosine(abs(fourierTransform_1[i]), abs(fourierTransform_3[i])))
 print()
```

这是输出结果：

```py
I had a very good apple.
I had a very good adventure.
```

```py
a
diff 0.5641788840293884
log_quotient 0.5351020097732544
fourier 0.8501702469740261
```

```py
very
diff 0.8958494067192078
log_quotient 0.5876994729042053
fourier 0.8582797441535993
```

```py
good
diff 0.6836684346199036
log_quotient 0.18607155978679657
fourier 0.8857107252606878
```

上述输出显示，当评估那些相关名词不是非常接近的修饰语的相似性时，差异和对数商的效果最好。

```py
print (sents[1])
print (sents[2])
print()
for i in range(3):
 print(tokenizer.convert_ids_to_tokens(tokenized_text[0][i+3]))
 print(‘diff’, 1 — spatial.distance.cosine(_l0_12_2[i], _l0_12_3[i]))
 print(‘log_quotient’, 1 — spatial.distance.cosine(l0_12_2[i], l0_12_3[i]))
 print(‘fourier’, 1 — spatial.distance.cosine(abs(fourierTransform_2[i]), abs(fourierTransform_3[i])))
 print()
```

输出结果如下：

```py
I had a very good orange.
I had a very good adventure.
```

```py
a
diff 0.8232558369636536
log_quotient 0.7186723351478577
fourier 0.8378725099204362
```

```py
very
diff 0.9369465708732605
log_quotient 0.6996179223060608
fourier 0.9164374584436726
```

```py
good
diff 0.8077239990234375
log_quotient 0.5284199714660645
fourier 0.9069805698881434
```

我们再次看到，当评估那些相关名词不是非常接近的修饰语的相似性时，差异和对数商的效果最好。

# 结论

在分析 BERT 嵌入时，傅里叶变换是否有意义？根据本文所做的实验，我们可以得出结论，这种方法可以有效地与其他方法结合使用。
