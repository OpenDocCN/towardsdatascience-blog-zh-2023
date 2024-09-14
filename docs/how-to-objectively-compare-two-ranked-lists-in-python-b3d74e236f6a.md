# 如何在 Python 中客观地比较两个排名列表

> 原文：[`towardsdatascience.com/how-to-objectively-compare-two-ranked-lists-in-python-b3d74e236f6a`](https://towardsdatascience.com/how-to-objectively-compare-two-ranked-lists-in-python-b3d74e236f6a)

## 对 Rank Biased Overlap 的简化解释和实现

[](https://krupesh-raikar.medium.com/?source=post_page-----b3d74e236f6a--------------------------------)![Krupesh](https://krupesh-raikar.medium.com/?source=post_page-----b3d74e236f6a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b3d74e236f6a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b3d74e236f6a--------------------------------) [Krupesh](https://krupesh-raikar.medium.com/?source=post_page-----b3d74e236f6a--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b3d74e236f6a--------------------------------) ·阅读时间 10 分钟·2023 年 1 月 5 日

--

![](img/8ce80da25708928b0424823ff8e67be0.png)

**假设你和你的朋友都看过所有 8 部《哈利·波特》电影。**

*但有一个问题* —— 你是每部电影在首映当天就观看的，没有错过任何一次首映。

然而，你的朋友却是先看了第二部电影，然后是第四部和第五部，最后在 Netflix 上追完了其余的电影。

理论上，你和你的朋友处于同等条件 —— 都看过该系列的所有电影。

***它真的平等吗？***

![](img/8368f473939f56ce003463091bdd50b2.png)

图片由作者使用商业可用字体（[Harry P](https://fontswan.com/harry-potter-font/#4-caslon-antique)）和 CC 图片来自 [Wikimedia](https://commons.wikimedia.org/wiki/File:Harry_Potter_Lightning.svg) 制作

作为一个真正的粉丝，你绝对不能将它视为平等。

怎么可能有人不按顺序观看一本书系列的电影，**这完全不合理？！**

***你认为这是亵渎！***

好消息是 —— **数学对你有利。** *你可以将这个问题简化为排名列表比较。*

有几种方法可以比较列表。

在这篇文章中，你将：

+   理解为什么我们实际上需要一个排名列表的比较

+   查看比较度量应该满足的要求

+   了解一种叫做 Rank Biased Overlap 的方法

+   理解其背后的数学方法

+   在 Python 中创建一个简单的实现

*最后，你将彻底解决与你朋友的分歧：*

**你 != 你的朋友**

# 为什么你需要排名列表比较？

除了上述的电影观看顺序分歧，我们周围充满了比较列表的例子！

实际上，我们所有人都在不断进行这样的比较。

+   对同一查询在 Google 和 Bing 上的结果进行比较，哪个更好？

+   《纽约时报》畅销书榜单与《今日美国》的榜单有多相似？

+   Rotten Tomatoes 上的前 10 部电影排名与 IMDB 相比有多不同？

**在自然语言处理和机器学习领域：**

+   两个段落在转换为标记化单词列表后有多相似？

+   如何比较由两个不同的学习排序/机器学习排名（MLR）¹模型生成的排名输出？

**在制造/生产和物流领域：**

+   将传入部分的序列与传出部分的序列进行比较（FIFO）。一个理想的 FIFO 序列如下所示。一个***1***首先进入，一个***1***首先出来。

![](img/519d672799c368ca6a79ff634f26e335.png)

但这在现实世界中可能并不总是发生。也许一个***1***先进去，但一个***3***先出来？

***是的，这可能导致完全的混乱。***

![](img/3c411ebf2cc2c4ba4478210626748f53.png)

那么，你应该如何衡量序列的质量？

***你需要某种度量。***

这种度量必须满足哪些要求？

# 比较度量的要求

理想情况下，比较度量应该以分数的形式出现，指示排名的相似程度。

再次考虑上述示例。

你可以从中得到关于这种度量应该告诉我们什么的线索：

1.  排名列表可能是无限的，且可能长度不同——因此度量***应该能够处理两个不同的列表大小***

1.  应该有能力在选择的交集长度下进行比较——这意味着度量***应该处理列表的交集长度***

1.  顶部元素比底部元素更重要（例如在搜索结果、电影排序等方面）——因此应该有***在需要时赋予权重的可能性***

现在让我们来看看满足这些条件的东西。

# 排名偏差重叠

正如名字所暗示的，这种方法是排名偏向的。

这意味着你可以根据更高的*权重*优先考虑列表中的前几个元素。

这个方法在一篇题为——**W. Weber 等人²的《无限排名的相似性度量》**的论文中提出。

你不必通读整个研究论文中的复杂内容，我在下面简化了大部分部分，以便我们可以尝试在 Python 中进行快速实现。

***不过，系好安全带，戴上数学眼镜，我们将穿越一些复杂的公式！***

设 S 和 T 为我们标题图像中两个排名列表的名称：

![](img/d6af78c3e0ca5e959b4c0b0439082065.png)

现在让我们定义这两个列表在深度（d）下的交集。

![](img/8d5f71a407d3d0ae457d78ef94ff1c99.png)

这实际上意味着什么？

这是 S 和 T 在给定长度（称为深度***d***）下的共同元素列表。从中可以清楚地看到：

![](img/18114de3b6b8447fe6cf7c5ffa49fd05.png)

深度被认为是*最多为较小列表的长度*，否则，交集就没有意义。

这个新的交集列表的长度称为**重叠**，在这种情况下是 7。

![](img/997b92176df6ac85c999912cfccd4b13.png)

接下来，我们定义一个称为**一致性**的概念。它只是按深度计算的重叠。

![](img/1a6485faee812b787c6d399bc0af9519.png)

**平均重叠（AO）**是所有深度列表的平均一致性，范围从 1 到整数 k。

当 k = 1 时，深度范围从 1 到 1，这意味着只比较第一个元素。

![](img/162de8fd4fde359b0b11ac1769ccdbeb.png)

由于元素匹配，交集 X 为 1，Agreement A 也是 1。

让我们看一下 k = 2。

![](img/03984ece2d4e5a426d4136f3a25f0ca4.png)

在这种情况下，***d*** 从 1 到 2，即我们比较列表中的前两个元素。在这种情况下，重叠为 1，因为只有第一个元素是共同的。因此，一致性为 1/2 或 0.5。

同样，我们可以继续进行 k = 3。

![](img/d23813cc91da60f9dc8e5dea22e52149.png)

好吧，现在你明白它是如何工作的，你可以继续这个过程，直到 k = 7。

你将得到**平均重叠（AO）**为：

![](img/489a81fbe260bc14194cf33b87c98cf0.png)

这可以适用于两个列表中的任何元素数量。

我们可以将上述求和概括为：

![](img/fba4278a591d351f3926218ce6f7c743.png)

**这就是相似度度量的基础。**

平均重叠是一个相似度度量，用于比较两个列表在给定深度上的相似度。

取一组这样的相似度度量（SIM）：

![](img/b0f39debd3de5ea3d119fae6af0bf412.png)

这是一个几何级数，其中***d'***项的形式为 1/(1-**p**)，其中 0 < **p** < 1。

将其代入上面的公式并重新排列项，我们可以得到**列表 S 和 T 的排名基础重叠**为：

![](img/477a2f114ddb7881cb13aef4969ca202.png)

变量**p**位于范围（0，1）内，可以用于确定前**d**个元素对最终相似度分数的影响。

为了得到一个单一的 RBO 分数，可以对上述公式进行外推。假设深度**k**的**一致性**在两个列表之间无限延续，可以将其概括为：

![](img/c64fb564aeb99e3b3f0342c65ec1680a.png)

RBO 分数在 0 和 1 之间。

+   0 表示完全不相交，而 1 表示完全相同的排名列表。

**我们将使用这个公式来实现 RBO 函数的 Python 代码！**

论文还展示了我们如何选择**p**来给前**d**个元素赋予权重（Wrbo）（我在这里不深入推导）：

![](img/aa7d049a61b23affc6753d9177076822.png)

**我们将使用这个公式来实现权重计算器功能。**

***呼，*** 这真是信息量很大啊！

但不要担心，这些公式看起来比实际要复杂得多——**你会看到用 Python 实现起来是多么简单。**

所以我希望你还在这里，因为有趣的部分现在开始了！

# Python 中的排名偏倚重叠的简单实现

我使用上述公式在 python 中编写了 Rank Biased Overlap 的简单实现和一个权重计算函数。

现在你了解了 RBO 的推导过程，实施起来很简单。

```py
import math

def rbo(S,T, p= 0.9):
    """ Takes two lists S and T of any lengths and gives out the RBO Score
    Parameters
    ----------
    S, T : Lists (str, integers)
    p : Weight parameter, giving the influence of the first d
        elements on the final score. p<0<1\. Default 0.9 give the top 10 
        elements 86% of the contribution in the final score.

    Returns
    -------
    Float of RBO score
    """

    # Fixed Terms
    k = max(len(S), len(T))
    x_k = len(set(S).intersection(set(T)))

    summation_term = 0

    # Loop for summation
    # k+1 for the loop to reach the last element (at k) in the bigger list    
    for d in range (1, k+1): 
            # Create sets from the lists
            set1 = set(S[:d]) if d < len(S) else set(S)
            set2 = set(T[:d]) if d < len(T) else set(T)

            # Intersection at depth d
            x_d = len(set1.intersection(set2))

            # Agreement at depth d
            a_d = x_d/d   

            # Summation
            summation_term = summation_term + math.pow(p, d) * a_d

    # Rank Biased Overlap - extrapolated
    rbo_ext = (x_k/k) * math.pow(p, k) + ((1-p)/p * summation_term)

    return rbo_ext
```

让我们检查一下我们的示例列表 S 和 T。

```py
S = [1,2,3,4,5,6,7]
T = [1,3,2,4,5,7,6,8]

print (rbo(S,T))

>> 0.8853713875
```

RBO 得分 0.88 意味着列表几乎 90%相似（这在之前的平均重叠计算中也看到过）。

这绝对不是一个稳健的 python 实现。

*然而，它显然足够好，可以开始使用！*

**现在让我们实现权重计算器函数，帮助我们选择 p。**

```py
import numpy as np
import math

def weightage_calculator(p,d):
""" Takes values of p and d
    ----------
    p : Weight parameter, giving the influence of the first d
        elements on the final score. p<0<1.
    d : depth at which the weight has to be calculated

    Returns
    -------
    Float of Weightage Wrbo at depth d
    """

    summation_term = 0

    for i in range (1, d): # taking d here will loop upto the value d-1 
        summation_term = summation_term + math.pow(p,i)/i

    Wrbo_1_d = 1 - math.pow(p, d-1) +
                   (((1-p)/p) * d *(np.log(1/(1-p)) - summation_term))

    return Wrbo_1_d
```

让我们检查一下它是否有效。

根据研究论文，p 为 0.9 时，前 10 个元素在最终相似性评分中的权重为 86%¹。

使用我们上面的函数：

```py
weightage_calculator(0.9,10)

>> 0.8555854467473518
```

太好了，它运行良好——与论文中指出的一致，得到了 86%！

*我们现在准备好了工具。*

**让我们解决你和你朋友之间的分数吧！**

# 解决你的争论！

让我们通过创建哈利·波特电影的列表，以你和你朋友观看的顺序来测试一下。

然后在其上运行你的 RBO 函数！

```py
you = ['''Harry Potter and the Philosopher's Stone''', 
              'Harry Potter and the Chamber of Secrets', 
              'Harry Potter and the Prisoner of Azkaban',
              'Harry Potter and the Goblet of Fire',
              'Harry Potter and the Order of Phoenix',
              'Harry Potter and the Half Blood Prince',
              'Harry Potter and the Deathly Hallows - Part_1'
              'Harry Potter and the Deathly Hallows - Part_2']

your_friend = ['Harry Potter and the Chamber of Secrets',
              'Harry Potter and the Goblet of Fire',
              'Harry Potter and the Order of Phoenix',
              '''Harry Potter and the Philosopher's Stone''', 
              'Harry Potter and the Prisoner of Azkaban',
              'Harry Potter and the Half Blood Prince',
              'Harry Potter and the Deathly Hallows - Part_1'
              'Harry Potter and the Deathly Hallows - Part_2']

rbo (you, your_friend)
>> 0.782775
```

**0.78！**

所以，按顺序观看电影确实很重要，否则分数会是 1！

***但你可以更进一步。***

通常，前几部电影介绍了角色并构建了虚构的世界——***因此，按正确的顺序首先观看这些电影具有更高的质量重要性。***

让我们使用权重计算器，并给前四部电影更多的权重（86%）。

通过对权重计算函数的一些试验，我们得到 p = 0.75 和 d = 4 时的权重为 86%。

```py
weightage_calculator(0.75,4)

>> 0.8555854467473518
```

因此，在 RBO 函数中使用 p 为 0.75 将使前四部电影在排名比较中有更大的影响：

```py
rbo (you, your_friend, 0.75)
>> 0.5361328125
```

得到的比较得分是**0.53！**

这意味着如果你跳过前几部电影，或以错误的顺序观看，它是客观上的不良。

**实际上，你朋友的观看顺序仅比你的顺序好一半（53%）！**

*现在你有数学证明这一点！*

# RBO 的优势

Rank-biased overlap 并不是比较列表的唯一方法——其他方法包括：

+   Kendall Tau³

+   Pearson 相关系数⁴

然而，RBO 在以下几个方面优于其他方法：

+   RBO 适用于不同长度的比较列表（不相交或不相似）

+   它具有权重测量——你可以赋予比较中顶部或底部更多的重要性

由于这些好处，我决定在这篇文章中详细解释 RBO。

但是，欢迎查看我在来源中链接的其他两个——*它们在不同的情况下也会使用！*

# 结论

总结来说，在这篇文章中你学到了比较排名列表的一种度量——**Rank Biased Overlap。**

+   你已经深入了解了它的数学原理

+   在 python 中做了一个简单的实现

+   使用这些函数对电影观看顺序进行了实际比较

+   理解了它的好处！

现在每当出现关于比较电影排名、观看顺序或基本上任何序列的分歧时，***你可以像专家一样解决问题！***

*完结。*

**来源和注释：**

[1] [学习排名](https://en.wikipedia.org/wiki/Learning_to_rank)

[2] [不确定排名的相似性度量 — 威廉·韦伯，阿利斯泰尔·莫法特，贾斯廷·佐贝尔](https://dl.acm.org/doi/10.1145/1852102.1852106)

[3] [肯德尔秩相关系数](https://en.wikipedia.org/wiki/Kendall_rank_correlation_coefficient)

[4] [皮尔逊相关系数](https://en.wikipedia.org/wiki/Pearson_correlation_coefficient)

图片、动图和脚本的版权归作者所有。
