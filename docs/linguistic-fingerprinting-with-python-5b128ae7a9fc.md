# 使用 Python 进行语言指纹分析

> 原文：[`towardsdatascience.com/linguistic-fingerprinting-with-python-5b128ae7a9fc`](https://towardsdatascience.com/linguistic-fingerprinting-with-python-5b128ae7a9fc)

## 使用标点热图进行作者归属分析

[](https://medium.com/@lee_vaughan?source=post_page-----5b128ae7a9fc--------------------------------)![Lee Vaughan](https://medium.com/@lee_vaughan?source=post_page-----5b128ae7a9fc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5b128ae7a9fc--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5b128ae7a9fc--------------------------------) [Lee Vaughan](https://medium.com/@lee_vaughan?source=post_page-----5b128ae7a9fc--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5b128ae7a9fc--------------------------------) ·9 分钟阅读·2023 年 8 月 1 日

--

![](img/84b60ea1de5e65b1d5632ec1303169e2.png)

单一法医学指纹，黄色调与蓝色分号（图片来源：DALL-E2 和作者）

*文体计量学*是通过计算文本分析对文学风格进行定量研究的学科。它基于这样的观点：我们每个人在写作中都有一种独特、一致且易于识别的风格。这包括我们的词汇、标点符号的使用、单词和句子的平均长度等等。

文体计量学的一个典型应用是*作者归属分析*。这是一种识别文件作者的过程，比如在调查抄袭或解决历史文件来源争议时。

在这个*快速成功数据科学*项目中，我们将使用 Python、seaborn 和自然语言工具包（NLTK）来检验亚瑟·柯南·道尔是否在他的小说《失落的世界》中留下了语言指纹。更具体地说，我们将利用*分号*来确定亚瑟·柯南·道尔还是他的同时代人 H.G. 威尔斯更可能是该书的作者。

# 《猎犬》、《战争》与《失落的世界》

亚瑟·柯南·道尔（1859–1930）最著名的是福尔摩斯系列故事。H.G. 威尔斯（1866–1946）因几部开创性的科幻小说而闻名，如*隐形人*。

1912 年，《Strand Magazine》出版了*失落的世界*，这是一个科幻小说的连载版本。虽然它的作者是已知的，但让我们假设它存在争议，我们的任务是解开这个谜团。专家们将范围缩小到两位作者：道尔和威尔斯。威尔斯略微被倾向，因为*失落的世界*是一部科幻小说，并且包含了类似于他 1895 年书中*莫洛克人*的穴居人。

为了解决这个问题，我们需要每位作者的代表作品。对于道尔，我们将使用*巴斯克维尔的猎犬*，它出版于 1901 年。对于威尔斯，我们将使用*世界大战*，它出版于 1898 年。

对我们来说幸运的是，所有三部小说都在公共领域，并可以通过[*古腾堡计划*](https://www.gutenberg.org/)获得。为了方便，我已将它们下载到这个[Gist](https://gist.github.com/rlvaugh/b5e6033024620e261b9829261b27b6e1)并删除了版权信息。

# 过程

作者归属分析需要应用*自然语言处理（NLP）*。NLP 是语言学和人工智能的一个分支，旨在赋予计算机从书面和口头语言中提取意义的能力。

最常见的 NLP 作者分析测试会分析文本的以下特征：

+   **词语/句子长度：** 文档中词语或句子长度的频率分布图。

+   **停用词：** 停用词的频率分布图（短的、无语境的功能词，如*the*、*but* 和 *if*）。

+   **词性：** 根据词汇的句法功能（如名词和动词）绘制的频率分布图。

+   **最常见的词汇：** 比较文本中最常用的词汇。

+   **Jaccard 相似性：** 用于衡量样本集的相似性和多样性的统计量。

+   **标点符号：** 比较逗号、冒号、分号等的使用情况。

对于这个玩具项目，我们将专注于*标点*测试。具体来说，我们将关注*分号*的使用。作为“可选”的语法元素，它们的使用具有潜在的独特性。它们也不受书籍类型的影响，不像问号在侦探小说中可能比在经典科幻小说中更为丰富。

高级过程将是：

1.  将书籍加载为文本字符串，

1.  使用 NLTK 对每个单词和标点进行标记（拆分），

1.  提取标点符号，

1.  将分号的值设为 1，所有其他标点的值设为 0，

1.  创建数值的 2D NumPy 数组，

1.  使用 seaborn 将结果绘制为[热图](https://en.wikipedia.org/wiki/Heat_map)，

我们将使用每个热图作为数字“指纹”。比较已知书籍的指纹与*未知*书籍（*失落的世界*）的指纹，希望能够暗示出作者之间的不同。

![](img/65382f1d0ad75a3cf108a4aee27703f2.png)

问号使用的热图（蓝色），在侦探小说（左）与科幻小说（右）中（作者提供的图像）

# 自然语言工具包

多个第三方库可以帮助你用 Python 进行 NLP。这些库包括 NLTK、spaCy、Gensim、Pattern 和 TextBlob。

我们将使用[NLTK](https://www.nltk.org/)，它是最古老、最强大且最受欢迎的工具之一。它是开源的，适用于 Windows、macOS 和 Linux。它还配备了详细的[文档](https://www.nltk.org/book/)。

# 安装库

使用 Anaconda 安装 NLTK：

`conda install -c anaconda nltk`

使用 pip 安装：

`pip install nltk`（或参见 [`www.nltk.org/install.html`](https://www.nltk.org/install.html)）

我们还需要 seaborn 来进行绘图。基于 matplotlib，这个开源可视化库提供了一个更易于使用的界面来绘制吸引人且信息丰富的统计图表，如条形图、散点图、热图等。以下是安装命令：

`conda install seaborn`

或

`pip install seaborn`

# 代码

以下代码在 JupyterLab 中编写，并且*由单元格*描述。

## 导入库

在导入模块中，我们需要`string`模块中的标点符号列表。我们将把这个列表转化为`set`数据类型，以便更快地进行搜索。

```py
import math
from string import punctuation
import urllib.request

import numpy as np
import matplotlib.pyplot as plt
from matplotlib.colors import ListedColormap
import seaborn as sns

import nltk

PUNCT_SET = set(punctuation)
```

## 定义一个加载文本文件的函数

由于我们将处理多个文件，我们将首先定义一些可重用的函数。第一个函数使用 Python 的`urllib`库打开存储在 URL 处的文件。第一行打开文件，第二行将其读取为字节数据，第三行将字节转换为字符串（`str`）格式。

```py
def text_to_string(url):
    """Read a text file from a URL and return a string."""
    with urllib.request.urlopen(url) as response:
        data = response.read()  # in bytes
        txt_str = data.decode('utf-8')  # converts bytes to string
        return txt_str
```

## 定义一个分词函数

下一个函数接受一个字典，其中键是作者的名字，值是他们以字符串格式呈现的小说。然后，它使用 NLTK 的`word_tokenize()`方法将字符串重新组织为*令牌*。

> Python 将字符串视为*字符*的集合，如字母、空格和标点符号。分词过程将这些字符分组为其他元素，如单词（或句子）和标点符号。每个元素称为*令牌*，这些令牌允许使用复杂的 NLP 分析。

该函数通过返回一个新的字典来结束，其中作者的名字作为键，标点符号令牌作为值。

```py
def make_punct_dict(author_book_dict):
    """Accept author/text dict and return dict of punctuation by author."""
    punct_by_author = {}
    for author, text in author_book_dict.items():
        tokens = nltk.word_tokenize(text)
        punct_by_author[author] = [token for token in tokens 
                                   if token in PUNCT_SET]
        print(f"Number punctuation marks in {author} = {len(punct_by_author[author])}")
    return punct_by_author
```

我应该在这里指出，可以使用基本的 Python 来搜索文本文件中的分号并完成这个项目。然而，分词带来了两个好处。

首先，NLTK 的默认分词器（`word_tokenize()`）不计算用在缩写或所有格中的撇号，而是将其视为*两个单词*，撇号附在第二个单词上（如*I* + *‘ve*）。这符合[语法用法](https://www.niu.edu/writingtutorial/punctuation/apostrophe.shtml)。

分词器还将特殊标记（如双破折号（— —）或省略号（…））视为*单一*标记，如作者所意图，而不是作为单独的标记。使用基本 Python 时，每个重复的标记都会被计数。

这可能会产生显著影响。例如，使用 NLTK 计算*失落的世界*的总标点符号数量为 10,035，而使用基本 Python 则为 14,352！

其次，所有其他 NLP 归因测试都需要使用分词，因此在这里使用 NLTK 让你有能力在以后扩展分析。

## 定义一个将标点符号转换为数字的函数

Seaborn 的`heatmap()`方法需要*数值*数据作为输入。以下函数接受按作者分类的标点符号令牌字典，将分号赋值为`1`，其他所有赋值为`0`，并返回`list`数据类型。

```py
def convert_punct_to_number(punct_by_author, author):
    """Return list of punctuation marks converted to numerical values."""
    heat_vals = [1 if char == ';' else 0 for char in punct_by_author[author]]
    return heat_vals
```

## 定义一个函数来查找下一个最低的平方值

热值列表是*一维*的，但我们希望将其绘制为*二维*。为此，我们将列表转换为正方形 NumPy 数组。

这必须是一个*真正的*正方形，并且每个列表中的样本数量不太可能有一个整数作为其平方根。因此，我们需要计算每个列表长度的平方根，将其转换为整数，并进行平方。得到的值将是可以作为真正的正方形显示的*最大长度*。

```py
def find_next_lowest_square(number):
    """Return the largest perfect square less than or equal to the given number."""
    return int(math.sqrt(number)) ** 2
```

## 加载和准备数据以进行绘图

定义好函数后，我们准备应用它们。首先，我们将每本书的文本数据加载到一个名为`strings_by_author`的字典中。对于*失落的世界*，我们将作者的名字设置为“未知”，因为这代表一个来源不明的文档。

然后，我们将其转换为一个名为`punct_by_author`的标点字典。从这个字典中，我们找到每个列表的下一个最低平方值，并选择*最小*值继续前进。这确保了*所有*数据集都可以转换为正方形数组，并且还规范化了每个数据集的样本数量（例如，*失落的世界*有 10,035 个标点符号标记，而*巴斯克维尔的猎犬*只有 6,704 个）。

```py
war_url =  'https://bit.ly/3QnuTPX'
hound_url = 'https://bit.ly/44Gdc2a'
lost_url = 'https://bit.ly/3QhTfKJ'

# Load text files into dictionary by author:
strings_by_author = {'wells': text_to_string(war_url),
                     'doyle': text_to_string(hound_url),
                     'unknown': text_to_string(lost_url)}

# Tokenize text strings preserving only punctuation marks:
punct_by_author = make_punct_dict(strings_by_author)

# Find the largest square that fits all datasets:
squarable_punct_sizes = [find_next_lowest_square(len(punct_by_author[author])) 
                         for author in punct_by_author]
perfect_square = min(squarable_punct_sizes)
print(f"Array size for perfect square: {perfect_square}\n")
```

![](img/1c513622c91d423e3d0f8a2b76e9813d.png)

未来使用一个包含 6,561 个标记的数组意味着每个标点符号列表将在绘图前被截断至长度 6,561。

## 绘制热图

我们将使用一个`for`循环为每位作者的标点符号绘制热图。第一步是将标点符号标记转换为数字。记住，分号将用`1`表示，其他所有标点用`0`表示。

接下来，我们使用`perfect_square`值和 NumPy 的`array()`及`reshape()`方法来截断`heat`列表并将其转换为正方形 NumPy 数组。此时，我们只需设置一个 matplotlib 图形并调用 seaborn 的`heatmap()`方法。循环将为每个作者绘制一个单独的图形。

分号将被标记为*蓝色*。你可以通过改变`cmap`参数中的颜色顺序来反转这一点。如果你不喜欢黄色和蓝色，也可以输入新的颜色。

```py
# Convert punctuation marks to numerical values and plot heatmaps:
for author in punct_by_author:
    heat = convert_punct_to_number(punct_by_author, author)
    arr = np.array(heat[:perfect_square]).reshape(int(math.sqrt(perfect_square)), 
                                                  int(math.sqrt(perfect_square)))
    fig, ax = plt.subplots(figsize=(5, 5))
    sns.heatmap(arr,
                cmap=ListedColormap(['yellow', 'blue']),
                cbar=False,
                xticklabels=False,
                yticklabels=False)
    ax.set_title(f'Heatmap Semicolons: {author.title()}')
    plt.show();
```

![](img/a40c96d35c1f9fa54ce0645137d1485f.png)![](img/e01f8c9596cafbbf962bac2f7e975ba2.png)![](img/b1148106f8f5f9545e5c1e5c87e6acbf.png)

## 结果

从之前的图表中应该可以清楚地看出——严格依据分号的使用——Doyle 是*失落的世界*最可能的作者。

这些结果的可视化展示非常重要。例如，每本书的总分号数如下：

`Wells: 243`

`Doyle: 45`

`Unknown: 103`

作为总标点符号的一部分，它们的比例如下：

`Wells: 0.032`

`Doyle: 0.007`

`Unknown: 0.010`

从这些文本数据来看，你是否立即能判断出“未知”作者很可能是**道尔**？试想一下如果把这些呈现给陪审团，陪审员会更被情节还是文本打动？没有什么能比可视化更有效地传达信息了！

当然，要对作者身份进行*稳健*的判断，你需要运行之前列出的*所有*NLP 归属测试，并使用*所有*的道尔和威尔斯的小说。

# 谢谢

感谢阅读，未来请关注我的更多*快速成功数据科学*项目。如果你想看到对该数据集进行更全面的 NLP 测试，请参阅我书中的第二章，[*真实世界的 Python：黑客解决代码问题的指南*](https://a.co/d/gzXvvoB)。
