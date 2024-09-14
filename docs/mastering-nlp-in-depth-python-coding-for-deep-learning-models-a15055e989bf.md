# 掌握 NLP：深度学习模型的深入 Python 编码

> 原文：[`towardsdatascience.com/mastering-nlp-in-depth-python-coding-for-deep-learning-models-a15055e989bf`](https://towardsdatascience.com/mastering-nlp-in-depth-python-coding-for-deep-learning-models-a15055e989bf)

## 一份逐步指南，包含全面的代码解释，介绍了如何使用 Python 进行深度学习的文本分类。

[](https://eligijus-bujokas.medium.com/?source=post_page-----a15055e989bf--------------------------------)![Eligijus Bujokas](https://eligijus-bujokas.medium.com/?source=post_page-----a15055e989bf--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a15055e989bf--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a15055e989bf--------------------------------) [Eligijus Bujokas](https://eligijus-bujokas.medium.com/?source=post_page-----a15055e989bf--------------------------------)

·发布在[数据科学前沿](https://towardsdatascience.com/?source=post_page-----a15055e989bf--------------------------------) ·21 分钟阅读·2023 年 10 月 13 日

--

![](img/240511bbc44eecab740578093612ef8b.png)

图片来源：[Waypixels](https://unsplash.com/@waypixels?utm_source=medium&utm_medium=referral) 在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

这篇文章是在阅读了大量文档资源和观看了 YouTube 上的文本数据、分类、递归神经网络及其他热门话题的视频之后完成的。许多信息并不友好，有些部分比较晦涩，因此，我希望节省读者大量时间，并阐明在任何机器学习项目中使用文本数据的最重要概念。

这里展示的示例支持代码可以在以下网址找到：[`github.com/Eligijus112/NLP-python`](https://github.com/Eligijus112/NLP-python)

本文覆盖的主题包括：

+   ***将文本转换为序列***

+   ***将序列索引转换为嵌入向量***

+   ***深入的 RNN 解释***

+   ***分类的损失函数***

+   ***使用 Pytorch 的完整 NLP 管道***

**NLP** 代表 **N**atural **L**anguage **P**rocessing¹。这是一个关于如何在任务中使用硬件和软件的广泛话题，例如：

+   将一种语言翻译成另一种语言

+   文本分类

+   文本摘要

+   下一步的标记预测

+   命名实体识别

还有更多。在这篇文章中，我想覆盖最受欢迎的技术，并通过简单的示例和代码使读者熟悉这些概念。

在 NLP 中，许多任务从***对文本进行分词²开始***。

文本分词是将原始文本拆分成较小部分——**标记**的过程。这些标记可以是字符、子词、单词或这三者的混合。

考虑一下字符串：

**“Python 中的 NLP 很有趣，文档也做得很好。让我们开始吧！”**

我将使用词级标记，因为相同的逻辑也适用于更低级别的标记化。

首先，让我们定义并应用一个将标点符号与单词分开的函数：

```py
# Regex 
import re

def preprocess_text(x: str) -> str:
    """
    Function that preprocess the text before tokenization

    Args:
        x (str): text to preprocess

    Returns:
        str: preprocessed text
    """ 
    # Create whitespaces around punctuation
    x = re.sub(r'([.,!?;:])', r' \1 ', x)

    # Returns the text 
    return x

# Original text 
text = "NLP in Python is fun and very well documented. Let's get started!"

# Applying the function 
text_preprocessed = preprocess_text(text)

# Printing the results 
print(text)
```

将上述函数应用于文本，我们得到：

```py
“NLP in Python is fun and very well documented . Let’s get started !”
```

现在，让我们定义一个函数，该函数创建一个将**单词映射到索引**和**索引映射到单词**的字典。文本中出现的第一个单词将具有较小的索引号。索引的排序完全是任意的——唯一的规则是**每个单词应有一个唯一的索引**。

```py
# Typehinting
from typing import Tuple

# Defining the function 
def create_word_index(x: str) -> Tuple[dict, dict]: 
    """
    Function that scans a given text and creates two dictionaries:
    - word2idx: dictionary mapping words to integers
    - idx2word: dictionary mapping integers to words

    Args:
        x (str): text to scan

    Returns:
        Tuple[dict, dict]: word2idx and idx2word dictionaries
    """
    # Spliting the text into words
    words = x.split()

    # Creating the word2idx dictionary 
    word2idx = {}
    for word in words: 
        if word not in word2idx: 
            # The len(word2idx) will always ensure that the 
            # new index is 1 + the length of the dictionary so far
            word2idx[word] = len(word2idx)

    # Adding the <UNK> token to the dictionary; This token will be used 
    # on new texts that were not seen during training.
    # It will have the last index. 
    word2idx['<UNK>'] = len(word2idx)

    # Reversing the above dictionary and creating the idx2word dictionary
    idx2word = {idx: word for word, idx in word2idx.items()}

    # Returns the dictionaries
    return word2idx, idx2word

# Applying the function 
word2idx, idx2word = create_word_index(text_preprocessed)
```

**word2idx** 字典如下：

```py
{
  'NLP': 0,
  'in': 1,
  'Python': 2,
  'is': 3,
  'fun': 4,
  'and': 5,
  'very': 6,
  'well': 7,
  'documented': 8,
  '.': 9,
  "Let's": 10,
  'get': 11,
  'started': 12,
  '!': 13,
  '<UNK>': 14
}
```

**idx2word** 字典如下：

```py
{
  0: 'NLP',
  1: 'in',
  2: 'Python',
  3: 'is',
  4: 'fun',
  5: 'and',
  6: 'very',
  7: 'well',
  8: 'documented',
  9: '.',
  10: "Let's",
  11: 'get',
  12: 'started',
  13: '!',
  14: '<UNK>'
}
```

从上述字典中，我们可以看到我们的数据总共有**15 个标记**。在本文中，我将把单词的索引称为标记。

现在让我们定义两个函数——一个创建索引序列，另一个将索引序列翻译为单词。

```py
# Defining a function that splits a text into tokens 
def text2tokens(x: str, word2idx: dict) -> list: 
    """
    Function that tokenizes a text

    Args:
        x (str): text to tokenize
        word2idx (dict): word2idx dictionary

    Returns:
        list: list of tokens
    """
    # Spliting the text into words
    words = x.split()

    # Creating the list of tokens
    tokens = []
    for word in words: 
        # The bellow line searches for the word in the word2idx dictionary
        # and returns the index of the word. If the word is not found,
        # it returns the index of the <UNK> token
        tokens.append(word2idx.get(word, word2idx['<UNK>']))

    # Returns the list of tokens
    return tokens

# Defining a function that converts the tokens to text 
def tokens2text(x: list, idx2word: dict) -> str:
    """
    Function that converts tokens to text

    Args:
        x (list): list of tokens
        idx2word (dict): idx2word dictionary

    Returns:
        str: text
    """
    # Creating the list of words
    words = []
    for idx in x: 
        # The bellow line searches for the index in the idx2word dictionary
        # and returns the word. If the index is not found,
        # it returns the <UNK> token
        words.append(idx2word.get(idx, '<UNK>'))

    # Returns the text
    return ' '.join(words)

# Applying the text2tokens function to the text
tokens_seq = text2tokens(text_preprocessed, word2idx)

# Transforming the tokens back to text
text_seq = tokens2text(tokens_seq, idx2word)
```

标记序列：

```py
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13]
```

从标记索引解码得到的词序列：

```py
"NLP in Python is fun and very well documented . Let's get started !"
```

让我们创建一个新的文本，这是索引器未见过的：

**“正如我所说，Python 是一个非常好的 NLP 工具”**

从预处理到标记序列的整个流程：

```py
# Putting everything together with a new text
text = "As I said, Python is a very good tool for NLP"

# Preprocessing the text
text_preprocessed = preprocess_text(text)

# Applying the text2tokens function to the text
tokens_seq = text2tokens(text_preprocessed, word2idx)

# Transforming the tokens back to text
text_seq = tokens2text(tokens_seq, idx2word)

print(f"Original text:\n {text_preprocessed}")
print(f"Tokens:\n {tokens_seq}")
print(f"Text:\n {text_seq}")
```

结果：

```py
Original text:
 As I said ,  Python is a very good tool for NLP
Tokens:
 [14, 14, 14, 14, 2, 3, 14, 6, 14, 14, 14, 0]
Text:
 <UNK> <UNK> <UNK> <UNK> Python is <UNK> very <UNK> <UNK> <UNK> NLP
```

现在标记序列是这样，我们可能在实际应用中遇到它——不是所有单词都被索引器见过，且标记索引没有排序。

这结束了本文的第一部分。回顾一下，我们已经创建了一些关键函数来从文本中创建标记，我们将在接下来的步骤中使用它们。

既然我们已经为每个单词创建了一个索引，我们可以开始为机器学习模型创建**特征**。目前，我们只有索引器从文本中看到单词的次数。这既不能捕捉单词的大小，也不能捕捉顺序信息，我们无法以任何方式比较单词——索引仅用作查找值。

从索引到特征，我们将创建一个词向量，或者使用更流行的术语，词嵌入。

**词嵌入是一个单词的数值向量表示³**。它将文本链接到一个向量空间。例如，

```py
# Bellow are examples of valid embeddings
'dog' = [-0.25, 0.35, 0.87]
'cat' = [0.98, 0.21, -0.78, 0.77]
'person' = [1.25, 3.75]
```

嵌入维度是固定的，由用户定义。在构建 NLP 系统时，每个标记必须与嵌入具有相同的维度。

创建词嵌入的最常见技术是定义向量的坐标数，并从均值为 0、方差为 1 的正态分布中随机模拟每个坐标。

```py
# Defining a function that creates the embedding vector 
# Defining a function that creates the embedding vector 
def create_embedding_vector(
        n_dim: int = 16, 
        mean: float = 0.0, 
        variance: float = 1.0,
        precision: int = None
        ) -> np.array: 
    """
    Function that creates a random embedding vector

    Args:
        n_dim (int, optional): embedding dimension. Defaults to 16.
        mean (float, optional): mean of the normal distribution. Defaults to 0.0.
        variance (float, optional): variance of the normal distribution. Defaults to 1.0.
        precision (int, optional): precision of each of the gotten coordinate. Defaults to None.

    Returns:
        np.array: embedding vector
    """
    # Creating a random normal distribution 
    X = np.random.normal(mean, variance, (n_dim, ))

    # Rounding the coordinates to the given precision
    if precision: 
        X = np.round(X, precision)

    # Returns the embedding vector 
    return X
```

默认情况下，上述函数将创建一个长度为 16 的向量。让我们将函数应用于字典中的每个单词。为了阅读方便，我们将嵌入维度限制为 6：

```py
# To recap, the idx2word dictionary is: 
{
  0: 'NLP',
  1: 'in',
  2: 'Python',
  3: 'is',
  4: 'fun',
  5: 'and',
  6: 'very',
  7: 'well',
  8: 'documented',
  9: '.',
  10: "Let's",
  11: 'get',
  12: 'started',
  13: '!',
  14: '<UNK>'
}

# Initiating the embedding dictionary 
idx2embeddings = {}

# Iterating over the idx2word dictionary
for idx in range(len(idx2word)): 
    # Creating the embedding vector 
    X = create_embedding_vector(n_dim=6, precision=3)

    # Adding the embedding vector to the embedding dictionary
    idx2embeddings[idx] = X

# Creating the word2embeddings dictionary
word2embeddings = {word: idx2embeddings[idx] for word, idx in word2idx.items()}
```

**idx2embeddings** 和 **word2embeddings** 字典如下所示：

```py
# idx2embeddings
{
  0: array([ 0.308, -1.003, -0.36 ,  0.57 , -1.106, -0.997]),
  1: array([-1.283,  0.709,  0.812,  0.201,  0.339,  1.264]),
  2: array([ 1.095, -0.666,  1.32 , -0.668, -0.705,  0.311]),
  3: array([ 0.417,  1.088,  1.242,  0.905, -0.061,  1.316]),
  4: array([ 1.432, -0.072,  0.622, -0.077,  0.597,  0.722]),
  5: array([-0.724, -0.496, -1.652,  1.118, -2.108, -0.996]),
  6: array([ 0.733,  0.021,  0.972,  0.363,  0.074, -0.661]),
  7: array([-0.284,  1.453,  2.522,  1.027, -1.484,  0.301]),
  8: array([ 0.921,  0.19 ,  0.068,  0.517, -0.767,  0.225]),
  9: array([-0.317,  0.691, -1.281,  0.624,  2.004,  1.377]),
  10: array([ 0.171,  0.607, -1.064, -0.064, -0.091,  0.748]),
  11: array([-0.151,  1.137,  0.783, -0.689, -1.473, -0.753]),
  12: array([1.699, 0.021, 1.422, 0.316, 0.317, 0.064]),
  13: array([-0.804,  0.156, -0.298,  0.15 , -0.686,  0.752]),
  14: array([ 0.538,  0.405,  0.501, -0.245, -1.946,  0.282])
}

# word2embeddings
{
  'NLP': array([ 0.308, -1.003, -0.36 ,  0.57 , -1.106, -0.997]),
  'in': array([-1.283,  0.709,  0.812,  0.201,  0.339,  1.264]),
  'Python': array([ 1.095, -0.666,  1.32 , -0.668, -0.705,  0.311]),
  'is': array([ 0.417,  1.088,  1.242,  0.905, -0.061,  1.316]),
  'fun': array([ 1.432, -0.072,  0.622, -0.077,  0.597,  0.722]),
  'and': array([-0.724, -0.496, -1.652,  1.118, -2.108, -0.996]),
  'very': array([ 0.733,  0.021,  0.972,  0.363,  0.074, -0.661]),
  'well': array([-0.284,  1.453,  2.522,  1.027, -1.484,  0.301]),
  'documented': array([ 0.921,  0.19 ,  0.068,  0.517, -0.767,  0.225]),
  '.': array([-0.317,  0.691, -1.281,  0.624,  2.004,  1.377]),
  "Let's": array([ 0.171,  0.607, -1.064, -0.064, -0.091,  0.748]),
  'get': array([-0.151,  1.137,  0.783, -0.689, -1.473, -0.753]),
  'started': array([1.699, 0.021, 1.422, 0.316, 0.317, 0.064]),
  '!': array([-0.804,  0.156, -0.298,  0.15 , -0.686,  0.752]),
  '<UNK>': array([ 0.538,  0.405,  0.501, -0.245, -1.946,  0.282])
}
```

通常在文献和实践中，嵌入矩阵对象用于解决自然语言处理任务。嵌入矩阵不过是将 idx2embeddings 字典转换为数组。矩阵的第一行是 idx2embeddings 中的第一个条目，第二行是第二个条目，以此类推。让我们创建我们自己的嵌入矩阵：

```py
# Creating the embedding matrix
embedding_matrix = np.array([idx2embeddings[idx] for idx in idx2embeddings])

# Printing the matrix
print(embedding_matrix)

# The resulting embedding matrix of shape 15 x 6
[[ 0.308 -1.003 -0.36   0.57  -1.106 -0.997]
 [-1.283  0.709  0.812  0.201  0.339  1.264]
 [ 1.095 -0.666  1.32  -0.668 -0.705  0.311]
 [ 0.417  1.088  1.242  0.905 -0.061  1.316]
 [ 1.432 -0.072  0.622 -0.077  0.597  0.722]
 [-0.724 -0.496 -1.652  1.118 -2.108 -0.996]
 [ 0.733  0.021  0.972  0.363  0.074 -0.661]
 [-0.284  1.453  2.522  1.027 -1.484  0.301]
 [ 0.921  0.19   0.068  0.517 -0.767  0.225]
 [-0.317  0.691 -1.281  0.624  2.004  1.377]
 [ 0.171  0.607 -1.064 -0.064 -0.091  0.748]
 [-0.151  1.137  0.783 -0.689 -1.473 -0.753]
 [ 1.699  0.021  1.422  0.316  0.317  0.064]
 [-0.804  0.156 -0.298  0.15  -0.686  0.752]
 [ 0.538  0.405  0.501 -0.245 -1.946  0.282]]
```

一般规则是，**嵌入矩阵的行数等于文本中唯一标记的总数，列数等于嵌入维度**。

现在我们为每个单词都创建了一个嵌入，我们实际上为每个单词创建了 6 个特征。就现在而言，它们的意义并不比词到索引的链接更大。

但我们应该把这些嵌入视为**机器学习模型权重初始化的开始**。在训练过程中，每个坐标都是可训练的，最终的嵌入矩阵会与我们初始化时的矩阵大相径庭。

现在我们为每个单词都有了一个嵌入向量，让我们停下来重新审视我们的分词***序列***：

```py
# Text
"NLP in Python is fun and very well documented . Let's get started !"

# The token sequence 
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13]
```

在人类语言中，我们在句子中想要表达的单词顺序非常重要。交换两个单词可能会改变整个句子的含义。因此，为机器学习模型创建能够捕捉序列本质的数据是非常重要的。

为此，我们将创建一个***张量***，格式如下⁴：

**（序列长度，批量大小，嵌入维度大小）**

通常很难想象这个 3 维张量是什么。要可视化它，可以参考这个立方体：

![](img/53b0478b80b5c50f2e37037015f08bec.png)

典型的序列张量（4 x 5 x 4）；作者图表

上述张量是由以下句子创建的：

```py
# 5 random texts
texts = [
  "I love my dog", 
  "The sun is rising", 
  "Bacon lettuce tomatoe yum", 
  "I love my mother", 
  "The frogs are gray"
]
```

批量大小等于我们数据中文档或数据点或任何其他文本单位的数量。在上述示例中，我们有 5 篇文本，因此，**批量大小等于 5**。

为了简化起见，我创建了包含 4 个单词的句子。因此，最大序列长度等于 4：

```py
"I" -> "love" -> "my" -> "dog"
"The" -> "sun" -> "is" -> "rising"
"Bacon" -> "lettuce" -> "tomatoe" -> "yum" 
"I" -> "love" -> "my" -> "mother" 
"The" -> "frogs" -> "are" -> "gray"
```

嵌入向量维度也等于 4（这只是巧合，嵌入大小可以远大于序列长度）。因此，句子中的每个标记都有 4 个与之关联的数字（一个嵌入向量）。在张量图中，每个标记的嵌入向量用不同的颜色标记。因此，如果嵌入向量看起来像这样：

```py
{
  "The": [0.01, 0.14, 0.71, 0.69],
  "frogs": [-1.25, 0.69, -0.54, 0.19],
  "are": [2.10, -1.13, -0.15, -0.13], 
  "gray": [-0.22, 0.55, 1.12, 0.25],
}
```

然后序列是：

![](img/3e4705e4e3e893e2f22a46d1678fe15d.png)

“青蛙是灰色的”序列；作者图表

请注意，有些作者以不同的轴方向可视化序列建模的张量。我的观点是，当考虑嵌入矩阵的序列建模时，应该始终在脑海中有一个随手可得的立方体。

现在我们脑海中有了序列的图像，我们可以使用什么样的机器学习模型来输出序列末尾的结果？一种特别适合序列的模型是递归神经网络，简称**RNN**。

根据定义，递归神经网络是一种包含循环的神经网络，允许在网络内存储信息。

让我们绘制一个简单的前馈神经网络，并使用我们拥有的输入。在序列中的每个时间步，令牌被“拆分”成 4 个特征，从嵌入矩阵中提取并通过网络传递：

![](img/787d01ca98a5324aec2b447d601765a4.png)

前馈神经网络；图表由作者提供

每个权重 w1、w2、w3、w4 和 w5 在网络中随机初始化，并用于计算通过网络的前馈传递的最终输出。中间神经元的激活函数选择**RELU**。输出神经元的激活函数是**linear**。

例如，对于单词**“The”**，权重初始化为**0.01、1.14、-1.16、1.75**和**2.1**，网络将如下所示：

![](img/e10ccceef65f49d3f372892a3bd2075f.png)

“The”令牌的预测；图表由作者提供

为了使我们的网络成为递归网络，我们需要在每个新令牌通过网络前馈后保存一些信息。我们通过在最终输出信息之前保存输出信息来实现这一点。为此，我们向网络中添加了一个权重并扩展了网络：

![](img/8c1c463ba7d8eeceb9f6a5571a2af6a7.png)

展开的 RNN 图；图表由作者提供

上述图表中有 3 组权重：**Wx**、**Wa**和**Wo**。

**Wx**和**Wo**是与简单前馈神经网络相同的权重。

**Wa**权重用于乘以在每个令牌的输出层前馈的信号。然后，这个信号在下一个令牌级别的***sum and activation***神经元处被使用。

让我们初始化相同的**Wx**和**Wo**权重，并额外将**Wa**权重初始化为 1.12。然后，对于我们拥有的完整序列的计算如下：

在“frog”令牌级别（或序列中的第二步）的激活函数输入计算如下：

**1.25 * 0.01 + 0.69 * 1.14 -0.54 * -1.16 + 0.19 * 1.75 + 0.544 * 1.12 = 2.34228**

![](img/9cff425dbcf6116c83949e15f1410999.png)

从第一步到第二步；图表由作者提供

最后两个步骤在下面的图表中呈现：

![](img/4535f6608e2b4f98199e087487677dc3.png)

序列的末尾；图表由作者提供

RNN 的每一步产生了以下数值序列：

**[1.14, 4.91, 2.73, 2.56]**

默认情况下，在大多数软件包（如 Pytorch 或 Tensorflow）中，只返回最后一步的输出：**2.56**。如果参数**return_sequences = True**，则返回完整序列。

请记住，在网络的展开过程中，**Wx、Wa** 和 **Wo** 是固定的，并保持不变。这些权重在反向传播过程中会被更新。

上述管道的最后部分是将最终输出层连接到我们 RNN 网络的输出。根据我们要解决的任务，我们可以：

+   如果我们想要解决回归问题（例如，给定职位广告的描述，预测薪资），请添加一个线性层。

+   如果想要解决分类问题（例如，电影评论的情感是正面还是负面），请添加一个 sigmoid 层。

+   用于自定义任务的其他层。

我们将在网络的末尾添加一个 sigmoid 层，模型预测一个介于 0 和 1 之间的值。

Sigmoid 是一个函数，它输入一个数字并输出一个范围在 (0, 1) 之间的数字。公式为：

![](img/15f9bf96242b9fafb991d114d9af69fb.png)

Sigmoid 公式

如果我们将 RNN 输出上的权重 Ws 定义为 0.85，那么序列在我们网络中的完整流动将如下所示：

![](img/1506875c584a48c20abf45d16493428a.png)

完整的 RNN 管道；图表由作者提供

记住，RNN 的输出是一个等于 2.56 的数字（因此 sigmoid 层的输入是 2.56 * 0.85 = 2.18）。

在训练过程中，将会调整的权重包括：

+   Wx (4)

+   Wa (1)

+   Wo (1)

+   Ws (1)

+   嵌入矩阵中的所有数字（唯一标记 * 4）

在训练过程中，所有权重将会被调整，以根据用户定义的损失函数产生最佳的概率输出。

请注意，我在权重计算中省略了偏置变量。逻辑并没有改变，但额外的 1 个参数会被添加到 Wx、Wa、Wo 和 Ws。

在深入实际实现上述图表之前，我们需要定义最后一个要优化的东西——损失函数。我们将使用一种非常流行的二分类损失函数⁶，称为 **二元交叉熵损失函数**：

![](img/776ca9456ed447dc0a20a0a2d85433ad.png)

BCE 损失；公式由作者提供

上述方程看起来很吓人，但让我们逐一解析每个符号。

**BCE(w)** 部分意味着我们只能在网络中改变权重，这也是函数的唯一参数。

**i 从 1 到 N 的总和** 意味着当我们计算 BCE 时，我们使用了所有的数据。

**y_i 和 x_i** 是来自数据集的第 i 个数据点。这些数据是固定的，我们不能以任何方式调整它们。

**f(w, x_i)** 代表在给定一组权重和第 i 个 x 观测值的情况下，我们的函数（一个带有嵌入层和 sigmoid 输出的 RNN 网络）的输出。

为了更好地理解上述损失，假设我们有两个观测值 (y, x) 的形式：[1, 0.47]，[0, 65.23]。假设我们有一组权重 **w** 和 f(**w**, 0.47) = 0.88，f(**w**, 65.23) = 0.12。

然后，为了计算 BCE 损失，我们只需代入这些值：

```py
BCE(w) = -0.5 [(1 * log(0.88) + (1 - 1) log(1 - 0.88)) + (0 * log(0.12) + (1 - 0) log(1 - 0.12))]
BCE(w) = 0.056
```

现在假设我们有另一组权重 w，产生以下概率：

f(**w**, 0.47) = 0.95 和 f(**w**, 65.23) = 0.08

那么 BCE(w) 是：

```py
BCE(w) = -0.5 [(1 * log(0.95) + (1 - 1) log(1 - 0.95)) + (0 * log(0.08) + (1 - 0) log(1 - 0.08))]
BCE(w) = 0.029
```

新的权重集更好，因为损失函数较小。

这里的直觉是，二进制交叉熵越小，模型在真实 Y 值为 1 时给出高概率的确定性越高，而在真实 Y 值为 0 时给出低概率的确定性越高。

现在让我们使用 Pytorch 将一切结合起来。我们将尝试分类推文的情感是正面还是负面。数据集来源于此⁷：[`www.kaggle.com/datasets/jp797498e/twitter-entity-sentiment-analysis`](https://www.kaggle.com/datasets/jp797498e/twitter-entity-sentiment-analysis)

数据是关于计算机游戏的推文是正面还是负面。

我们将对 Y 变量进行编码，其中 1 表示负面情绪，0 表示正面情绪。

我们将数据集拆分为训练集和测试集（分别为 34410 和 8603 行），并应用之前定义的所有函数：

```py
import pandas as pd 

# Reading the data 
d = pd.read_csv('input/Tweets.csv', header=None)

# Adding the columns 
d.columns = ['INDEX', 'GAME', "SENTIMENT", 'TEXT']

# Leaving only the positive and the negative sentiments 
d = d[d['SENTIMENT'].isin(['Positive', 'Negative'])]

# Encoding the sentiments that the negative will be 1 and the positive 0
d['SENTIMENT'] = d['SENTIMENT'].apply(lambda x: 0 if x == 'Positive' else 1)

# Dropping missing values
d = d.dropna()

# Spliting to train and test sets 
train, test = train_test_split(d, test_size=0.2, random_state=42)

# Reseting the indexes 
train.reset_index(drop=True, inplace=True)
test.reset_index(drop=True, inplace=True)
```

对 **create_word_index()** 函数进行了小的调整，我们将每个令牌索引移位 1，因为我们稍后将引入 **padding** 概念，这是 Pytorch 所必需的。

```py
def create_word_index(x: str, shift_for_padding: bool = False) -> Tuple[dict, dict]: 
    """
    Function that scans a given text and creates two dictionaries:
    - word2idx: dictionary mapping words to integers
    - idx2word: dictionary mapping integers to words

    Args:
        x (str): text to scan
        shift_for_padding (bool, optional): If True, the function will add 1 to all the indexes.

    Returns:
        Tuple[dict, dict]: word2idx and idx2word dictionaries
    """
    # Spliting the text into words
    words = x.split()

    # Creating the word2idx dictionary 
    word2idx = {}
    for word in words: 
        if word not in word2idx: 
            # The len(word2idx) will always ensure that the 
            # new index is 1 + the length of the dictionary so far
            word2idx[word] = len(word2idx)

    # Adding the <UNK> token to the dictionary; This token will be used 
    # on new texts that were not seen during training.
    # It will have the last index. 
    word2idx['<UNK>'] = len(word2idx)

    if shift_for_padding:
        # Adding 1 to all the indexes; 
        # The 0 index will be reserved for padding
        word2idx = {k: v + 1 for k, v in word2idx.items()}

    # Reversing the above dictionary and creating the idx2word dictionary
    idx2word = {idx: word for word, idx in word2idx.items()}

    # Returns the dictionaries
    return word2idx, idx2word
```

应用函数后，我们得到约 29k 个唯一令牌：

```py
# Joining all the texts into one string
text = ' '.join(train['TEXT'].values)

# Creating the word2idx and idx2word dictionaries
word2idx, idx2word = create_word_index(text, shift_for_padding=True)

# Printing the size of the vocabulary
print(f'The size of the vocabulary is: {len(word2idx)}')
The size of the vocabulary is: 29568
```

让我们从数据集中创建令牌序列：

```py
# For each row in the train and test set, we will create a list of integers
# that will represent the words in the text
train['text_int'] = train['TEXT'].apply(lambda x: [word2idx.get(word, word2idx['<UNK>']) for word in x.split()])
test['text_int'] = test['TEXT'].apply(lambda x: [word2idx.get(word, word2idx['<UNK>']) for word in x.split()])

# Calculating the length of sequences in the train set 
train['seq_len'] = train['text_int'].apply(lambda x: len(x))

# Describing the length of the sequences
train['seq_len'].describe()
count    34410.000000
mean        21.825574
std         17.707891
min          0.000000
25%          9.000000
50%         17.000000
75%         31.000000
max        312.000000
```

关于航空公司推文的平均词数等于**21.8**个词。目前，我们的数据集如下：

![](img/40fb31f78b6e8eb72594311dc2bfdc3f.png)

数据片段；图片由作者提供

**text_int** 列的长度不相等。大多数机器学习框架期望 X 的维度是相同的。因此，我们将引入填充函数，该函数将序列右侧填充 0 直到所需的序列长度：

```py
def pad_sequences(x: list, pad_length: int) -> list:
    """
    Function that pads a given list of integers to a given length

    Args:
        x (list): list of integers to pad
        pad_length (int): length to pad

    Returns:
        list: padded list of integers
    """
    # Getting the length of the list
    len_x = len(x)

    # Checking if the length of the list is less than the pad_length
    if len_x < pad_length: 
        # Padding the list with 0s
        x = x + [0] * (pad_length - len_x)
    else: 
        # Truncating the list to the desired length
        x = x[:pad_length]

    # Returning the padded list
    return x
```

让我们应用这个函数，看看会发生什么：

```py
# Padding the train and test sequences 
train['text_int'] = train['text_int'].apply(lambda x: pad_sequences(x, 30))
test['text_int'] = test['text_int'].apply(lambda x: pad_sequences(x, 30))
```

![](img/0c451a88a739374b5bbfce5d7947d850.png)

填充后的 text_int 列；图片由作者提供

上述片段中的最后一行展示了填充的效果。

为了更清晰的了解，填充的小例子如下：

假设我们有两个序列：[1, 2, 5, 9, 3] 和 [1, 6, 12]。如果我们将 **pad_length 设置为 4**，那么这些序列将变成：

**[1, 2, 5, 9, 3] -> [1, 2, 5, 9]**

**[1, 6, 12] -> [1, 6, 12, 0]**

现在我们可以在 Pytorch 中定义我们的模型类和数据加载器类：

```py
# Importing the needed packages
import torch 
from torch import nn

# Defining the torch model for sentiment classification 
class SentimentClassifier(torch.nn.Module):
    """
    Class that defines the sentiment classifier model
    """
    def __init__(self, vocab_size, embedding_dim):
        super(SentimentClassifier, self).__init__()

        self.embedding = nn.Embedding(vocab_size + 1, embedding_dim)
        self.rnn = nn.RNN(input_size=embedding_dim, hidden_size=1, batch_first=True)
        self.fc = nn.Linear(1, 1)  # Output with a single neuron for binary classification
        self.sigmoid = nn.Sigmoid()  # Sigmoid activation

    def forward(self, x):
        x = self.embedding(x)  # Embedding layer
        output, _ = self.rnn(x)  # RNN layer

        # Leaving only the last step of the RNN sequence
        x = output[:, -1, :]

        # Fully connected layer with a single neuron
        x = self.fc(x) 

        # Converting to probabilities
        x = self.sigmoid(x)

        # Flattening the output
        x = x.squeeze()

        return x

# Initiating the model 
model = SentimentClassifier(vocab_size=len(word2idx), embedding_dim=16)

# Initiating the criterion and the optimizer
criterion = nn.BCELoss() # Binary cross entropy loss
optimizer = torch.optim.Adam(model.parameters(), lr=0.001)
```

```py
# Defining the data loader 
from torch.utils.data import Dataset, DataLoader

class TextClassificationDataset(Dataset):
    def __init__(self, data):
        self.data = data

    def __len__(self):
        return len(self.data)

    def __getitem__(self, idx):
        # The x is named as text_int and the y as airline_sentiment
        x = self.data.iloc[idx]['text_int']
        y = self.data.iloc[idx]['SENTIMENT']

        # Converting the x and y to torch tensors
        x = torch.tensor(x)
        y = torch.tensor(y)

        # Converting the y variable to float 
        y = y.float()

        # Returning the x and y
        return x, y

# Creating the train and test loaders
train_loader = DataLoader(TextClassificationDataset(train), batch_size=32, shuffle=True)
test_loader = DataLoader(TextClassificationDataset(test), batch_size=32, shuffle=True)
```

模型的训练和测试：

```py
# Defining the number of epochs
epochs = 100

# Setting the model to train mode
model.train()

# Saving of the loss values
losses = []

# Iterating through epochs
for epoch in range(epochs):
    # Initiating the total loss 
    total_loss = 0

    for batch_idx, (inputs, labels) in enumerate(train_loader):
        # Zero the gradients
        optimizer.zero_grad()  # Zero the gradients
        outputs = model(inputs)  # Forward pass

        loss = criterion(outputs, labels)  # Compute the loss
        loss.backward()  # Backpropagation
        optimizer.step()  # Update the model's parameters

        # Adding the loss to the total loss
        total_loss += loss.item()

    # Calculating the average loss
    avg_loss = total_loss / len(train_loader)

    # Appending the loss to the list containing the losses
    losses.append(avg_loss)

    # Printing the loss every n epochs
    if epoch % 20 == 0:
        print(f'Epoch: {epoch}, Loss: {avg_loss}')
```

损失与时代图：

```py
# Ploting the loss 
import matplotlib.pyplot as plt

plt.plot(losses)
plt.xlabel('Epoch')
plt.ylabel('Loss')
plt.title('Loss vs. Epoch')
plt.show()
```

![](img/d1d56352f3d3cce6cf6f0fe483064b50.png)

损失与时代；图片由作者提供

验证集中的准确率：

```py
# Setting the model to eval model
model.eval()

# List to track the test acc 
total_correct = 0
total_obs = 0

# Iterating over the test set
for batch_idx, (inputs, labels) in enumerate(test_loader):
    outputs = model(inputs)  # Forward pass

    # Getting the number of correct predictions 
    correct = ((outputs > 0.5).float() == labels).float().sum()

    # Getting the total number of predictions
    total = labels.size(0)

    # Updating the total correct and total observations
    total_correct += correct
    total_obs += total

print(f'The test accuracy is: {total_correct / total_obs}')
The test accuracy is: 0.7420667409896851
```

结果测试准确率为 **~75%**，对于如此少的代码和特征工程来说真的不错。

总结一下，在本文中，我涵盖了以下概念：

+   文本到索引（分词）。

+   词嵌入。

+   对 RNN 层的深入解释。

+   二分类的损失函数。

+   如何在 Pytorch 中封装一切并创建一个模型。

祝学习愉快，编码愉快！

[1]

**名称：** 自然语言处理

**网址:** [`en.wikipedia.org/wiki/Neuro-linguistic_programming`](https://en.wikipedia.org/wiki/Natural_language_processing)

[2]

**名称:** NLP | 如何进行文本、句子、词语的分词

**网址:** [`www.geeksforgeeks.org/nlp-how-tokenizing-text-sentence-words-works/`](https://www.geeksforgeeks.org/nlp-how-tokenizing-text-sentence-words-works/)

[3]

**名称:** 词嵌入

**网址:** [`en.wikipedia.org/wiki/Word_embedding`](https://en.wikipedia.org/wiki/Word_embedding)

[4]

**名称:** 时间序列预测

**网址:** [`www.tensorflow.org/tutorials/structured_data/time_series`](https://www.tensorflow.org/tutorials/structured_data/time_series)

[5]

**名称:** 循环神经网络

**网址:** [`en.wikipedia.org/wiki/Recurrent_neural_network`](https://en.wikipedia.org/wiki/Recurrent_neural_network)

[6]

**名称:** 分类的损失函数

**网址:** [`en.wikipedia.org/wiki/Loss_functions_for_classification`](https://en.wikipedia.org/wiki/Loss_functions_for_classification)

[7]

**名称:** Twitter 情感分析

**网址:** [`www.kaggle.com/datasets/jp797498e/twitter-entity-sentiment-analysis`](https://www.kaggle.com/datasets/jp797498e/twitter-entity-sentiment-analysis)

**数据集许可证:** [`creativecommons.org/publicdomain/zero/1.0/`](https://creativecommons.org/publicdomain/zero/1.0/)
