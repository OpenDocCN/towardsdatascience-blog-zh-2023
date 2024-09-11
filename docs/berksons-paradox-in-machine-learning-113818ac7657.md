# 机器学习中的伯克森悖论

> 原文：[`towardsdatascience.com/berksons-paradox-in-machine-learning-113818ac7657?source=collection_archive---------3-----------------------#2023-12-22`](https://towardsdatascience.com/berksons-paradox-in-machine-learning-113818ac7657?source=collection_archive---------3-----------------------#2023-12-22)

## 理解数据分析中的隐性偏差

[](https://medium.com/@ocaelen?source=post_page-----113818ac7657--------------------------------)![Olivier Caelen](https://medium.com/@ocaelen?source=post_page-----113818ac7657--------------------------------)[](https://towardsdatascience.com/?source=post_page-----113818ac7657--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----113818ac7657--------------------------------) [Olivier Caelen](https://medium.com/@ocaelen?source=post_page-----113818ac7657--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd7268030c8a8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fberksons-paradox-in-machine-learning-113818ac7657&user=Olivier+Caelen&userId=d7268030c8a8&source=post_page-d7268030c8a8----113818ac7657---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----113818ac7657--------------------------------) · 8 分钟阅读 · 2023 年 12 月 22 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F113818ac7657&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fberksons-paradox-in-machine-learning-113818ac7657&user=Olivier+Caelen&userId=d7268030c8a8&source=-----113818ac7657---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F113818ac7657&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fberksons-paradox-in-machine-learning-113818ac7657&source=-----113818ac7657---------------------bookmark_footer-----------)![](img/1331ec1fae3eb1c3317bf49f07732fa7.png)

由 DALL-E 生成

有时，统计数据显示出令人惊讶的现象，让我们质疑日常看到的事物。伯克森悖论就是一个例子。这个悖论与抽样偏差问题密切相关，当我们误以为两件事物有关联时，因为我们没有看到全貌。作为一名机器学习专家，你应该对这个悖论有所了解，因为它可能通过导致对变量关系的错误假设，显著影响你预测模型的准确性。

# 让我们从一些例子开始

+   基于[Berkson](https://en.wikipedia.org/wiki/Joseph_Berkson)的原始例子，我们来设想一个在医院进行的回顾性研究。在这家医院，研究人员正在研究胆囊炎（胆囊疾病）的风险因素，其中一个风险可能是糖尿病。由于样本来源于住院人群而非一般人群，这存在抽样偏差，这可能导致错误地认为糖尿病对胆囊炎有保护作用。

+   另一个著名的例子来自[Jordon Ellenberg](https://en.wikipedia.org/wiki/Jordan_Ellenberg)。在这个例子中，Alex 创建了一个约会池。这个小组并不能很好地代表所有男人；我们有一个抽样偏差，因为她挑选的是非常友好、吸引人，或者两者兼具的人。在 Alex 的约会池中，发生了一些有趣的事情……在她约会的男人中，看起来他们越友好，越不吸引人，反之亦然。这种抽样偏差可能导致 Alex 错误地认为友好与吸引力之间存在负相关。

# 让我们尝试稍微形式化一下问题

假设我们有两个独立事件*X*和*Y*。由于这些事件是独立的：

这些随机事件可以是，例如，得胆囊炎或有糖尿病，如第一个例子，或者是友好或美丽，如第二个例子。当然，当我说这两个事件是独立的时，我指的是整个总体！

在之前的例子中，抽样偏差总是同一种类型：没有事件都未发生的情况。在医院样本中，没有患者既没有胆囊炎也没有糖尿病。在 Alex 的样本中，没有男人既不友好又丑。因此，我们被条件限制在至少发生一个事件：事件*X*发生了，或者事件*Y*发生了，或者两者都发生了。为此，我们可以定义一个新的事件*Z*，它是事件*X*和*Y*的并集。

现在，我们可以写下以下内容，以表明我们在抽样偏差假设下：

这是事件*X*发生的概率，已知事件*X*或*Y*（或两者）已经发生。从直觉上，我们可以感觉到这个概率比*P(X)*要高……但也可以通过正式证明来显示这一点。

为了做到这一点，我们知道：

通过假设这两个事件不可能同时发生（例如，有些人既丑又不友好），之前的陈述可以变成一个严格的不等式；因为集合*(X* ∪ *Y)*不是样本空间Ω：

现在，如果我们将这个严格不等式的两边同时除以*P(X ∪ Y)*，然后乘以*P(X)*，我们得到：

其中

因此，我们确实得到在抽样偏差下的概率*P(X|Z)*高于*P(X)*，在整个总体中：

好的，明白了……但现在让我们回到伯克森悖论。我们有两个独立的随机变量 *X* 和 *Y*，我们想要展示它们在上述采样偏倚 *Z* 下变得相关。

为了做到这一点，让我们从 *P(X | Y ∩ Z)* 开始，这是在知道事件 *Y* 已经发生且我们处于偏倚采样 *Z* 下的情况下观察事件 *X* 的概率。请注意，*P(X | Y ∩ Z)* 也可以写作 *P(X | Y, Z)*。

由于 *(Y ∩ Z) = (Y ∩ (X ∪ Y)) = Y*，并且 *X* 和 *Y* 是独立变量，我们有：

然后……最终，知道 *P(X) < P(X | Z)*，我们得到了我们寻找的结果：

这个方程式显示，在由 Z 定义的采样偏倚下，两个最初独立的事件 *X* 和 *Y* 变得相关（否则，我们会看到等号而不是">"）。

回到亚历克斯的约会池的例子，如果

+   Z 是处于亚历克斯约会池中的事件

+   X 是选择一个友好的人的事件

+   Y 是选择一个有吸引力的人的事件

那么 *(X | Z)* 是亚历克斯遇到一个好人的事件，而 *(X | Y ∩ Z)* 是在给定他是帅哥的情况下亚历克斯遇到好人的事件。由于用于构建亚历克斯约会池的选择过程，以及伯克森悖论，亚历克斯会感觉当她遇到帅哥时，他们不会那么友好，而如果从整个群体中抽取，这可能是两个独立事件……

# 也许一个数值例子会帮助我们更具体地理解

为了说明伯克森悖论，我们使用两个骰子：

+   事件 X：第一个骰子显示 6。

+   事件 Y：第二个骰子显示 1 或 2。

这两个事件显然是独立的，其中 *P(X)=1/6* 和 *P(Y)=1/3*。

现在，让我们引入我们的条件 (*Z*)，表示通过排除所有第一个骰子不是六而第二个骰子既不是 1 也不是 2 的结果来进行偏倚采样。

在我们的偏倚采样条件下，我们需要计算事件 *X* 发生的概率，前提是至少发生了事件 (*X* 或 *Y*)，这用 *P(X|Z)* 表示。

首先，我们需要确定 Z = (X ∪ Y) 的概率……对不起，从现在开始我们需要做一点计算……我会为你做的…… :-)

接下来，我们计算在 Z 给定的情况下 X 的概率：

要查看在假设 *Z* 发生的情况下 *X* 和 *Y* 是否存在依赖关系，我们需要计算 *P(X | Y ∩ Z)*。

因为

我们有

为了演示伯克森悖论，我们将 *P(X|Z)* 与 *P*(*X* ∣ Y ∩ Z) 进行比较，我们得到：

+   *P(X | Z) = 0.375*

+   *P(X |Y ∩ Z) ≈ 0.1666…*

我们确实恢复了在伯克森悖论下，由于采样偏倚 Z，我们有 *P(X | Z) > P(X ∣ Y ∩ Z)* 的性质。

我个人感到很惊讶！我们有两个骰子……两个明显独立的随机事件……通过采样过程，我们可以获得骰子掷出的结果变得相关的印象。

# 为了进一步说服我们，让我们进行一点模拟

在下面的代码中，我将使用 Python 模拟骰子掷出。

以下代码模拟了百万次掷两个骰子的实验，对于每次实验，它检查第一个骰子是否掷出 6（事件 X），以及第二个骰子是否掷出 1 或 2（事件 Y）。然后，它将这些检查的结果（真或假）分别存储在列表 X 和 Y 中。

```py
import random

#Get some observations for random variables X and Y
def sample_X_Y(nb_exp):
    X = []
    Y = []
    for i in range(nb_exp):
        dice1 = random.randint(1,6)
        dice2 = random.randint(1,6)
        X.append(dice1 == 6)
        Y.append(dice2 in [1,2])
    return X, Y

nb_exp=1_000_000
X, Y = sample_X_Y(nb_exp)
```

接下来，我们需要检查这两个事件是否确实独立。为此，以下代码计算了事件 X 的概率以及在事件 Y 给定的情况下事件 X 的条件概率。它通过将成功结果的数量除以每个概率的实验总数来完成这一过程。

```py
# compute P(X=1) and P(X1=1|Y=1) to check if X and Y are independent
p_X = sum(X)/nb_exp
p_X_Y = sum([X[i] for i in range(nb_exp) if Y[i]])/sum(Y)

print("P(X=1) = ", round(p_X,5))
print("P(X=1|Y=1) = ", round(p_X_Y,5))
```

```py
P(X=1) =  0.16693
P(X=1|Y=1) =  0.16681
```

如我们所见，这两个概率接近；因此（如预期 ;-) ）或两个骰子是独立的。

现在，让我们看看引入抽样偏差*Z*时会发生什么。以下代码过滤实验结果，仅保留 X = 1、Y = 1 或两者都为 1 的结果。它将这些过滤后的结果存储在列表 XZ 和 YZ 中。

```py
# keep only the observations where X=1, Y=1 or both (remove when X=0 and Y=0)
XZ = []
YZ = []
for i in range(nb_exp):
    if X[i] or Y[i]:
        XZ.append(X[i])
        YZ.append(Y[i]) 
nb_obs_Z = len(XZ)
```

现在，让我们检查这些新变量是否仍然独立。

```py
# compute P(X=1|Z=1) and P(X1=1|Y=1,Z=1) to check if X|Z and Y|Z are independent
p_X_Z = sum(XZ)/nb_obs_Z
p_X_Y_Z = sum([XZ[i] for i in range(nb_obs_Z) if YZ[i]])/sum(YZ)

print("P(X=1|Z=1) = ", round(p_X_Z,5))
print("P(X=1|Y=1,Z=1) = ", round(p_X_Y_Z,5))
```

```py
P(X=1|Z=1) =  0.37545
P(X=1|Y=1,Z=1) =  0.16681
```

我们有一个不等式（与前一节相同的值），这意味着如果 Z 为真，那么拥有 Y 的信息会改变 X 的概率；因此，它们不再是独立的。

# 这种悖论对机器学习专家的影响是什么？

我认为机器学习专家没有足够关注这种偏差。当我们谈论伯克森悖论时，我们是在深入探讨一个对从事机器学习的人员至关重要的主题。这个概念是关于理解我们如何被使用的数据所误导。伯克森悖论警告我们使用偏倚或片面数据的危险。

**信用评分系统**：在金融领域，基于高收入或高信用分数申请人的数据训练的模型，但这两者很少同时出现，可能会错误地推断出这两个因素之间存在负相关。这有可能导致不公平的贷款实践，偏向某些特定的人群。

**社交媒体算法**：在社交媒体算法中，当模型训练基于极端用户数据时，如具有高人气但低参与度的病毒内容和深度参与但低人气的利基内容，可能会出现伯克森悖论。这种偏倚的抽样通常导致错误结论，认为人气和参与深度之间是负相关的。因此，算法可能低估那些在中等人气和参与度之间平衡的内容，从而扭曲内容推荐系统。

**求职者筛选工具**：基于具有高学历或丰富经验的申请人的筛选模型可能错误地暗示这些属性之间存在反向关系，可能忽视了那些在这两个方面都平衡的候选人。

在每种情况下，忽视伯克森悖论可能导致模型偏倚，影响决策和公平性。机器学习专家必须通过多样化数据来源和持续验证模型以应对这一点。

# 结论

总之，伯克森悖论是对机器学习专业人士的重要提醒，提醒他们仔细审查数据来源并避免误导性的相关性。通过理解和考虑这一悖论，我们可以构建更准确、公平和实际的模型，真正反映现实世界的复杂性。请记住，强健的机器学习的关键在于复杂的算法以及周到、全面的数据收集和分析。

# 感谢阅读！

如果你希望及时了解我的最新发布并提高博客的可见性，请考虑关注我。
