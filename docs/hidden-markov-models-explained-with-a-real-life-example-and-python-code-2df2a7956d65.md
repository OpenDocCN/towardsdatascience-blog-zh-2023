# 隐藏马尔可夫模型：通过实际例子和 Python 代码进行解释

> 原文：[https://towardsdatascience.com/hidden-markov-models-explained-with-a-real-life-example-and-python-code-2df2a7956d65?source=collection_archive---------0-----------------------#2023-11-05](https://towardsdatascience.com/hidden-markov-models-explained-with-a-real-life-example-and-python-code-2df2a7956d65?source=collection_archive---------0-----------------------#2023-11-05)

[](https://carolinabento.medium.com/?source=post_page-----2df2a7956d65--------------------------------)[![Carolina Bento](../Images/9585232979bf7c2dbad05934f0735d89.png)](https://carolinabento.medium.com/?source=post_page-----2df2a7956d65--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2df2a7956d65--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2df2a7956d65--------------------------------) [Carolina Bento](https://carolinabento.medium.com/?source=post_page-----2df2a7956d65--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe960c0367546&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhidden-markov-models-explained-with-a-real-life-example-and-python-code-2df2a7956d65&user=Carolina+Bento&userId=e960c0367546&source=post_page-e960c0367546----2df2a7956d65---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2df2a7956d65--------------------------------) ·14分钟阅读·2023年11月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2df2a7956d65&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhidden-markov-models-explained-with-a-real-life-example-and-python-code-2df2a7956d65&user=Carolina+Bento&userId=e960c0367546&source=-----2df2a7956d65---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2df2a7956d65&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhidden-markov-models-explained-with-a-real-life-example-and-python-code-2df2a7956d65&source=-----2df2a7956d65---------------------bookmark_footer-----------)![](../Images/418afba8ee3df1346601a1d1e5c1623f.png)

作者提供的图片

*隐藏马尔可夫模型是用于解决现实生活中的问题的概率模型，问题范围从每个人每周至少考虑一次的简单问题——明天的天气会如何？*[1] — *到复杂的分子生物学问题，例如预测肽与人类MHC II类分子的结合位点*[2]。

*隐藏马尔可夫模型是马尔可夫链的近亲，但其隐藏状态使其成为确定随机变量序列概率时的独特工具。*

*在本文中，我们将拆解隐马尔可夫模型的所有不同组件，并一步步展示数学和Python代码，看看哪些情感状态导致了你狗狗在训练考试中的结果。我们将使用维特比算法来确定观察到特定观测序列的概率，并展示如何使用前向算法来确定观察到序列的可能性，当你给出一个隐藏状态序列时。*

现实世界充满了这样的现象：我们可以看到最终结果，但无法实际观察生成这些结果的潜在因素。例如，根据过去的天气观测和不同天气结果的观察概率来预测天气，确定明天是雨天还是晴天。

尽管受我们无法观察的因素驱动，利用**隐马尔可夫模型**可以将这些现象建模为概率系统。

# 隐藏状态的马尔可夫模型

[隐马尔可夫模型](https://en.wikipedia.org/wiki/Hidden_Markov_model)，简称HMM，是一种统计模型，作为一系列标记问题进行工作。这些问题描述了可观察事件的演变，而这些事件本身依赖于无法直接观察的内部因素——它们是**隐藏的**[3]。

隐马尔可夫模型由两个不同的[随机过程](https://en.wikipedia.org/wiki/Stochastic_process)组成，这些过程可以定义为[随机变量](https://en.wikipedia.org/wiki/Random_variable)的序列——这些变量依赖于随机事件。

存在**隐形过程**和**可观察过程**。

**隐形过程**是一个[马尔可夫链](/markov-models-and-markov-chains-explained-in-real-life-probabilistic-workout-routine-65e47b5c9a73)，如同将多个**隐藏状态**串联在一起，随着时间的推移以达到某个结果。这是一个概率过程，因为马尔可夫链的所有参数以及每个序列的得分实际上都是概率[4]。

隐马尔可夫模型描述了可观察事件的演变，这些事件本身依赖于无法直接观察的内部因素——它们是**隐藏的**[3]。

就像其他[马尔可夫链](/markov-models-and-markov-chains-explained-in-real-life-probabilistic-workout-routine-65e47b5c9a73)一样，为了知道你接下来会进入哪个状态，唯一重要的因素是你现在的位置——即你目前处于马尔可夫链的哪个状态。你过去的*历史*状态对于理解你接下来要去的地方并没有意义。

这种*短期*记忆是HMM的关键特征之一，称为**马尔可夫假设**，表示到达下一个状态的概率仅依赖于当前状态的概率。

![](../Images/097a3c7f00c78d7bb4e6a10be3600758.png)

马尔可夫假设。（作者提供的图像）

HMM 的另一个关键特征是，它还假设每次观察仅依赖于产生该观察的状态，因此完全独立于链中的其他状态[5]。

> **马尔可夫假设**表明，达到下一个状态的概率仅依赖于当前状态的概率。

这些都是关于 HMM 的极好背景信息，但它们实际上用于哪些问题类别？

HMM 帮助建模现象的行为。除了建模和允许进行模拟，你还可以提出关于这些现象的不同类型的问题：

+   **似然性**或**评分**，即确定观察到序列的概率

+   **解码**生成特定观察的最佳状态序列

+   **学习** HMM 参数，这些参数用于观察到特定序列，该序列遍历了特定的状态集合。

让我们看看实际操作吧！

# 作为 HMM 的狗训练表现

今天你不太担心天气预报，思考的重点是你的狗是否可能从训练课程中毕业。经过所有的时间、精力和狗狗零食，你只希望它们能成功。

在狗狗训练过程中，你的四脚朋友会做一些动作或把戏，教练可以*观察*并评分它们的表现。通过合并三次试验的分数，他们会判断你的狗是否毕业或是否需要额外的训练。

教练只看到结果，但涉及到的几个因素是无法直接观察的，例如你的狗是否疲倦、快乐、是否不喜欢教练或周围的其他狗。

这些状态都无法直接观察，除非你的狗在特定情绪下做出明显的行动。如果它们能用语言表达自己的感受，那就太好了，也许未来能实现！

在脑海中还新鲜的隐马尔可夫模型，这看起来是预测你狗在考试期间感觉如何的绝佳机会。它们可能因为感到疲倦、饥饿或对教练感到烦恼而获得某个分数。

你的狗已经接受了一段时间的训练，基于训练过程中收集的数据，你拥有了建立隐马尔可夫模型所需的所有基础构件。

为了建立一个建模你狗在训练评估中表现的 HMM，你需要：

+   隐藏状态

+   转移矩阵

+   观察序列

+   观察似然矩阵

+   初始概率分布

**隐藏状态**是那些影响观察序列的不可观察因素。你只会考虑你的狗是疲倦还是快乐。

![](../Images/787e1db3f5cad33f98426995562f4dc4.png)

*HMM 中的不同隐藏状态。（作者提供的图像）*

了解你的狗后，非可观察因素可能影响它们的考试表现，仅仅是疲劳或快乐。

接下来你需要知道从一个状态转移到另一个状态的概率，这些信息被记录在**转移矩阵**中。这个矩阵也必须是[**行随机矩阵**](https://en.wikipedia.org/wiki/Stochastic_matrix)，意味着链中从一个状态到任何其他状态的概率，每一行的总和必须为一。

![](../Images/33188ec7cf3bd8bc24208d2287b6f5fa.png)

*转移矩阵：表示从一个状态转移到另一个状态的概率。*（图像来源：作者）

不论你解决的是什么类型的问题，你总是需要一个**观测序列**。每个观测值表示在马尔可夫链中遍历的结果。每个观测值都是从特定的词汇表中抽取的。

![](../Images/303710a9d5933e8f0a24d35346e02be6.png)

*词汇表（图像来源：作者）*

在你狗的考试中，你会观察到每次试验后的得分，这可能是*失败*、*OK*或*完美*。这些都是观测词汇表中的所有可能*术语*。

你还需要**观测似然矩阵**，即从特定状态生成观测值的概率。

![](../Images/374897edecaebef5b8b473b802176c83.png)

观测似然矩阵。（图像来源：作者）

最后是**初始概率分布**。这是马尔可夫链在每个特定隐藏状态下开始的概率。

在马尔可夫链中，有些状态可能永远不会成为起始状态。在这些情况下，它们的初始概率为零。就像转移矩阵中的概率一样，这些初始概率的总和也必须加起来为一。

![](../Images/50d55e6acc0101b554e7719829e25c82.png)

初始概率（图像来源：作者）

初始概率分布，加上转移矩阵和观测似然矩阵，组成了**隐马尔可夫模型的参数**。这些是你在拥有观测序列和隐藏状态的情况下，需要弄清楚的概率，并尝试*学习*哪个特定的HMM可能生成了这些数据。

将这些部分组合在一起，这就是表示你狗在训练考试中表现的隐马尔可夫模型的样子。

![](../Images/1a03ebaab0ee07dd8844ca64513e59b4.png)

隐藏状态以及它们之间的转移概率。（图像来源：作者）

在考试期间，你的狗将进行三次试验，只有在这三次试验中不*失败*两次，才能毕业。

到头来，如果你的狗需要更多训练，你仍然会照顾它。你脑海中的大问题是*它们在考试期间感觉如何*。

想象一个场景，如果它们以*OK — Fail — Perfect*的顺序毕业，它们将处于什么样的情感状态？它们会大多数时间感到疲惫，还是饥饿，或者两者的混合？

这种问题正好属于HMM可以应用的*解码*问题。在这种情况下，你希望找出生成特定观察序列*OK — Fail — Perfect*的最佳状态序列。

解码生成给定观察序列的状态序列的问题利用了**维特比算法**。然而，值得稍微绕个弯子，了解一下如何使用**前向算法**计算给定观察序列的概率——这是一项似然任务。这将为更好地理解维特比算法的工作原理奠定基础。

# 前向算法

如果你将这个问题建模为常规的[马尔可夫链](https://medium.com/p/65e47b5c9a73)，并希望计算观察序列*OK, Fail, Perfect*的似然，你需要通过每个特定的状态生成期望的结果。在每一步，你会根据前一个观察结果的条件概率来观察当前结果，并将该概率乘以从一个状态到另一个状态的转移概率。

大的区别在于，在常规的马尔可夫链中，所有状态都是已知和可观察的。但在隐马尔可夫模型中却不是这样！在隐马尔可夫模型中，你观察到的是一个结果序列，而不知道为了观察到这个序列需要经过哪个特定的隐藏状态序列。

> 大的区别在于，在常规的马尔可夫链中，所有状态都是已知和可观察的。但在隐马尔可夫模型中却不是这样！

此时你可能会想，*好吧，我可以简单地遍历所有可能的路径，然后最终有一个规则来选择等效路径。* 这种方法的数学定义如下所示。

![](../Images/33c668175694c5cdd6158a95bfffe1f3.png)

*计算观察结果序列的概率，遍历所有可能的隐藏状态序列。（图片来自作者）*

这肯定是一种策略！你需要计算观察到序列*OK, Fail, Perfect*的概率，考虑所有可能生成该序列的隐藏状态组合。

当隐藏状态和观察结果序列的数量足够小时，可以在合理的时间内完成计算。

![](../Images/97983f02f7cfe479a82bfea5e9e494d7.png)

*HMM中可能路径的概述（图片来自作者）*

值得庆幸的是，你刚定义的隐马尔可夫模型相对简单，包含3个观察结果和2个隐藏状态。

对于长度为L的观察序列，在具有M个隐藏状态的HMM中，你有“M的L次方”种可能的状态，在你的例子中，这意味着*2的3次方*，即8条可能的路径来生成序列*OK — Fail — Perfect*，涉及到的计算复杂度为O(M^L L)，在[大O符号](https://en.wikipedia.org/wiki/Big_O_notation)中描述。随着模型复杂度的增加，你需要考虑的路径数量呈指数级增长。

> 随着模型复杂度的增加，你需要考虑的路径数量呈指数级增长。

这就是**前向算法**的亮点所在。

前向算法计算观察序列中新符号的概率，而无需计算形成该序列的所有可能路径的概率[3]。

算法定义了**前向变量**，而不是计算所有形成该序列的可能路径的概率，前向变量的值是通过[递归](https://en.wikipedia.org/wiki/Recursion)计算的。

![](../Images/cd803b5f94fc52d8c67669787bf43c05.png)

前向变量是如何递归计算的。（图片来源：作者）

它使用递归是该算法比计算所有可能路径的概率更快的关键原因。实际上，它可以在仅需“L乘以M平方”的计算中计算观察到序列*x*的概率，而不是“M的L次方乘以L”。

在你的例子中，使用2个隐藏状态和3个观察结果的序列，计算概率的次数为O(MˆL L) = 2³x3 *=* 8x3 *=* 24次，而不是O(L Mˆ2)*=*3*2²=3x4 = 12次。

> 这种减少计算次数的方式是通过[动态规划](https://en.wikipedia.org/wiki/Dynamic_programming)实现的，这是一种编程技术，使用辅助数据结构存储中间信息，从而确保不会多次进行相同的计算。

每次算法要计算新概率时，它会检查是否已经计算过，如果是，它可以轻松地在中间数据结构中访问该值。否则，计算概率并存储该值。

回到你的解码问题，使用维特比算法。

# 维特比算法

以*伪代码*的思维方式，如果你要用暴力破解的方法解码生成特定观察序列的隐藏状态序列，你只需：

+   生成所有可能的路径排列，以达到期望的观察序列

+   使用前向算法计算每个观察序列的可能性，对于每个可能的隐藏状态序列

+   选择概率最高的隐藏状态序列

![](../Images/844da6164710e90f629645ea79e948a8.png)

所有可能的隐藏状态序列生成观察序列*OK — Fail — Perfect*（图片来源：作者）

对于你的特定HMM，有8条可能的路径可以得到*OK — Fail — Perfect*的结果。再添加一个观察值，你将会有双倍数量的隐藏状态序列！与前向算法所描述的类似，你很容易会遇到指数级复杂度的算法，并达到性能瓶颈。

Viterbi算法可以帮助你解决这个问题。

当HMM中的隐藏状态序列被遍历时，在每一步，概率*vt(j)*是HMM在观察后处于隐藏状态*j*的概率，并且是通过最可能的状态到达*j*的。

![](../Images/287242d38d5a258ccf9cba1292bb79aa.png)

Viterbi路径到隐藏状态*j*在时间步*t*。（图像来源：作者）

解码生成特定观察序列的隐藏状态序列的关键是**最可能路径**的概念。也称为**Viterbi路径**，最可能路径是从所有可能的路径中，具有最高概率的路径。

> 解码生成特定观察序列的隐藏状态序列的关键是使用Viterbi路径。最可能的路径通向任何给定的隐藏状态。

你可以将前向算法和Viterbi算法进行对比。前向算法通过将所有概率相加来获得达到某个状态的可能性，考虑所有到达该状态的路径，而Viterbi算法则不想探索所有可能性。它专注于通向任何给定状态的最可能路径。

回到解码隐藏状态序列的任务，以获得考试中OK — Fail — Perfect的分数，*手动*运行**Viterbi算法**将会像这样：

![](../Images/786b06523dc656786c27591176108ea4.png)

Viterbi路径和解码序列。（图像来源：作者）

Viterbi算法的另一个独特特点是它必须有一种方法来跟踪所有到达任何给定隐藏状态的路径，以便比较它们的概率。为此，它使用动态规划算法的典型辅助数据结构来跟踪**回溯指针**到每个隐藏状态。这样，它可以轻松访问过去遍历的任何Viterbi路径的概率。

**回溯指针是确定通向观察序列的最可能路径的关键。**

在你狗的考试示例中，当你计算Viterbi路径*v3(Happy)*和*v3(Tired)*时，你选择概率最高的路径，然后开始向后遍历，即回溯，通过所有到达你所在位置的路径。

# Python中的Viterbi算法

手动完成这些工作既费时又容易出错。错过一个重要的数字，你可能需要从头开始并重新检查所有的概率！

好消息是，你可以利用像 [hmmlearn](https://hmmlearn.readthedocs.io/en/stable/tutorial.html) 这样的软件库，只需几行代码就可以解码隐藏状态的序列，使你的狗在试验中获得*OK — Fail — Perfect*，并且按照这个顺序。

```py
from hmmlearn import hmm
import numpy as np

## Part 1\. Generating a HMM with specific parameters and simulating the exam
print("Setup HMM model with parameters")
# init_params are the parameters used to initialize the model for training
# s -> start probability
# t -> transition probabilities
# e -> emission probabilities
model = hmm.CategoricalHMM(n_components=2, random_state=425, init_params='ste')

# initial probabilities
# probability of starting in the Tired state = 0
# probability of starting in the Happy state = 1
initial_distribution = np.array([0.1, 0.9])
model.startprob_ = initial_distribution

print("Step 1\. Complete - Defined Initial Distribution")

# transition probabilities
#        tired    happy
# tired   0.4      0.6
# happy   0.2      0.8

transition_distribution = np.array([[0.4, 0.6], [0.2, 0.8]])
model.transmat_ = transition_distribution
print("Step 2\. Complete - Defined Transition Matrix")

# observation probabilities
#        Fail    OK      Perfect
# tired   0.3    0.5       0.2
# happy   0.1    0.5       0.4

observation_probability_matrix = np.array([[0.3, 0.5, 0.2], [0.1, 0.5, 0.4]])
model.emissionprob_ = observation_probability_matrix
print("Step 3\. Complete - Defined Observation Probability Matrix")

# simulate performing 100,000 trials, i.e., aptitude tests
trials, simulated_states = model.sample(100000)

# Output a sample of the simulated trials
# 0 -> Fail
# 1 -> OK
# 2 -> Perfect
print("\nSample of Simulated Trials - Based on Model Parameters")
print(trials[:10])

## Part 2 - Decoding the hidden state sequence that leads
## to an observation sequence of OK - Fail - Perfect

# split our data into training and test sets (50/50 split)
X_train = trials[:trials.shape[0] // 2]
X_test = trials[trials.shape[0] // 2:]

model.fit(X_train)

# the exam had 3 trials and your dog had the following score: OK, Fail, Perfect (1, 0 , 2)
exam_observations = [[1, 0, 2]]
predicted_states = model.predict(X=[[1, 0, 2]])
print("Predict the Hidden State Transitions that were being the exam scores OK, Fail, Perfect: \n 0 -> Tired , "
      "1 -> Happy")
print(predicted_states)
```

几秒钟内你就能得到一个与手工计算结果匹配的输出，速度更快，且错误空间更小。

![](../Images/41b354a93fb104c925d85c22edcd5966.png)

运行上述代码的输出结果。（图片由作者提供）

# 结论

隐马尔可夫模型令人着迷之处在于，这一统计工具诞生于20世纪60年代中期[6]，却如此强大，并且在从天气预报到找出句子中的下一个词等不同领域中应用广泛。

在这篇文章中，你有机会了解HMM的不同组件，它们如何应用于不同类型的任务，以及如何发现前向算法和维特比算法之间的相似性。这两个非常相似的算法使用动态规划来处理涉及的指数级计算。

无论是手工计算还是将参数输入到TensorFlow代码中，希望你享受深入探索HMM世界的过程。

*感谢阅读！*

# 参考文献

1.  D. Khiatani 和 U. Ghose，“使用隐马尔可夫模型进行天气预报”，2017年国际智能国家计算与通信技术会议（IC3TSN），印度古尔冈，2017年，第220–225页，doi: 10.1109/IC3TSN.2017.8284480。

1.  Noguchi H, Kato R, Hanai T, Matsubara Y, Honda H, Brusic V, Kobayashi T. 基于隐马尔可夫模型的抗原肽预测，这些抗原肽与MHC II类分子相互作用。《生物科学与生物工程杂志》。2002；94(3)：264–70。doi: 10.1263/jbb.94.264。PMID: 16233301。

1.  Yoon BJ. [隐马尔可夫模型及其在生物序列分析中的应用](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2766791/)。《当前基因组学》。2009年9月；10(6)：402–15。doi: 10.2174/138920209789177575。PMID: 20190955；PMCID: PMC2766791。

1.  Eddy, S. [什么是隐马尔可夫模型？](https://www.nature.com/articles/nbt1004-1315)。*自然生物技术* **22**，1315–1316（2004）。[https://doi.org/10.1038/nbt1004-1315](https://doi.org/10.1038/nbt1004-1315)

1.  Jurafsky, Dan 和 Martin, James H.，《*语音与语言处理：自然语言处理、计算语言学和语音识别导论*》。上萨德尔河，新泽西州：Pearson Prentice Hall，2009。

1.  Baum, Leonard E., 和 Ted Petrie。“[有限状态马尔可夫链的概率函数的统计推断。](https://projecteuclid.org/journals/annals-of-mathematical-statistics/volume-37/issue-6/Statistical-Inference-for-Probabilistic-Functions-of-Finite-State-Markov-Chains/10.1214/aoms/1177699147.full)” *《数学统计年鉴》* 37, no. 6 (1966): 1554–63。
