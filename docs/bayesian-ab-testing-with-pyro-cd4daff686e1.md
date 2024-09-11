# 使用 Pyro 的贝叶斯 AB 测试

> 原文：[https://towardsdatascience.com/bayesian-ab-testing-with-pyro-cd4daff686e1?source=collection_archive---------8-----------------------#2023-11-15](https://towardsdatascience.com/bayesian-ab-testing-with-pyro-cd4daff686e1?source=collection_archive---------8-----------------------#2023-11-15)

## 使用 Pyro 的贝叶斯思维和 AB 测试入门

[](https://medium.com/@fraserdbrown99?source=post_page-----cd4daff686e1--------------------------------)[![弗雷泽·布朗](../Images/d5f99897c571a61ed8832e5542751973.png)](https://medium.com/@fraserdbrown99?source=post_page-----cd4daff686e1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cd4daff686e1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----cd4daff686e1--------------------------------) [弗雷泽·布朗](https://medium.com/@fraserdbrown99?source=post_page-----cd4daff686e1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1843b94382f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbayesian-ab-testing-with-pyro-cd4daff686e1&user=Fraser+Brown&userId=1843b94382f&source=post_page-1843b94382f----cd4daff686e1---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cd4daff686e1--------------------------------) ·13 分钟阅读·2023年11月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcd4daff686e1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbayesian-ab-testing-with-pyro-cd4daff686e1&user=Fraser+Brown&userId=1843b94382f&source=-----cd4daff686e1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcd4daff686e1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbayesian-ab-testing-with-pyro-cd4daff686e1&source=-----cd4daff686e1---------------------bookmark_footer-----------)![](../Images/b9404c0cdb80e4403e9849c9bfe08141.png)

版权归 [Free-Photos](https://pixabay.com/fr/users/free-photos-242387/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1030852) 所有，来源于 [Pixabay](https://pixabay.com/)

本文介绍了使用 Python 概率编程语言（PPL）Pyro 进行 AB 测试的基本知识，Pyro 是 PyMC 的一个替代品。撰写这篇文章的动机是为了进一步理解使用 Pyro 框架的贝叶斯统计推断，并在此过程中帮助他人。因此，我们欢迎并鼓励反馈。

**介绍**

我之前在 Python 中使用 PyMC 进行贝叶斯建模的经验，但我发现使用一些最新版本有些麻烦。我研究其他概率编程语言时发现了 Pyro，这是由 Uber 构建的通用 PPL，并由 PyTorch 在后端支持。下面链接了 Pyro 和 PyTorch 的文档。

[](https://pyro.ai/?source=post_page-----cd4daff686e1--------------------------------) [## Pyro

### 深度通用概率编程

[pyro.ai](https://pyro.ai/?source=post_page-----cd4daff686e1--------------------------------)  [## PyTorch 文档 - PyTorch 2.1 文档

### PyTorch 是一个优化的张量库，用于使用 GPU 和 CPU 进行深度学习。本文档描述的功能…

[pytorch.org](https://pytorch.org/docs/stable/index.html?source=post_page-----cd4daff686e1--------------------------------)

在探索 Pyro 的过程中，我发现很难找到使用该软件包进行端到端教程的资源。本文旨在填补这一空白。

本文共有五个部分。在第一部分中，我将介绍贝叶斯思维的基础，这是一种哲学背景，方法论由此而言。我将简要介绍 Pyro 和用于进行统计推断的贝叶斯方法的技术背景。接下来，我将使用 Pyro 进行 AB 测试并讨论结果。然后，我将解释在业务环境中进行贝叶斯 AB 测试的案例。最后一部分将进行总结。

**贝叶斯思维入门**

贝叶斯过程在高层次上相对简单。首先，您有一个感兴趣的变量。我们陈述我们对该变量的当前理解，我们将其表达为一个概率分布（在贝叶斯的世界中，一切都是概率分布）。这称为*先验*。贝叶斯推断中的概率是认识论的，这意味着它通过我们的知识程度而来。因此，我们陈述的概率分布作为我们的先验，既是对*不确定性*的陈述，也是对理解的理解。然后，我们观察来自现实世界的事件。然后使用这些观察结果*更新*我们对我们感兴趣的变量的理解。我们理解的新状态称为*后验*。我们还用概率分布来表征后验，即给定我们观察到的数据，我们感兴趣的变量的概率。此过程如下图所示的流程图所示，其中 theta 是我们感兴趣的变量，data 是我们观察到的事件的结果。

![](../Images/3225ead89cc91c8851582f819f38898f.png)

贝叶斯过程

贝叶斯过程实际上是循环的，因为我们会重复这个过程，从头开始，但在观察世界后使用我们新获得的知识。我们的旧后验变成了我们的新先验，我们观察新的事件，并再次更新我们的理解，正如内特·西尔弗在他的书*信号与噪声*中所说，我们变得“越来越少错误”。我们不断减少对变量的不确定性，趋向于一种确定状态（但我们永远无法确定）。在数学上，这种思维方式由贝叶斯定理封装。

![](../Images/be8935f2bf3df8177a7ffb19dfe05953.png)

贝叶斯定理

在实际操作中，贝叶斯定理很快会因为大模型的高维度变得计算不可行。解决这一问题的方法有很多，但我们今天使用的方法称为马尔可夫链蒙特卡洛（Markov Chain Monte Carlo，简称MCMC）。MCMC是一类算法（我们将使用一种叫做哈密顿蒙特卡洛的方法），它智能地从概率分布中抽取样本。深入探讨这一点超出了本文的范围，但我会在文末提供一些有用的参考资料以供进一步阅读。目前，知道贝叶斯定理过于复杂而无法计算，因此我们利用先进算法如MCMC的强大功能来帮助解决问题就足够了。本文将重点介绍使用Pyro的MCMC。然而，Pyro确实强调使用基于近似的方法的随机变分推断（Stochastic Variational Inference，简称SVI），但这里不会涉及。

**使用Pyro进行AB测试**

设想一个公司设计了一个新的网站着陆页，并希望了解这对转化率的影响，即访客在访问该页面后是否会继续在网站上停留。在测试组A中，网站访客将看到当前的着陆页。在测试组B中，网站访客将看到新的着陆页。在接下来的文章中，我将把测试组A称为对照组，将组B称为处理组。该公司对变化持怀疑态度，并选择了80/20的会话流量分配。每个测试组的访客总数和页面转化总数总结如下。

![](../Images/a67d81d5f4a39335be11b2a99633af50.png)

测试观察

AB测试的原假设是两个测试组的页面转化率没有变化。在频率派框架下，对于双侧检验，这将表达为以下形式，其中r_c和r_t分别是对照组和处理组的页面转化率。

![](../Images/c854b401ebce5ba39d5ad28d56cd6c80.png)

原假设和替代假设

然后，显著性检验将尝试拒绝或未能拒绝原假设。在贝叶斯框架下，我们通过对每个测试组施加相同的*先验*来略微不同地表达原假设。

让我们暂停一下，准确概述一下我们测试期间发生的情况。我们感兴趣的变量是页面转化率。这可以通过计算独立转化访客的数量与总访客数量的比率来简单得出。产生这个比率的事件是访客是否点击了页面。对于每个访客，这里只有两种可能的结果，访客要么点击页面并转化，要么不点击。你们中的一些人可能会认识到，对于每个独立的访客，这是伯努利试验的一个例子；有一次试验和两种可能的结果。现在，当我们收集一组这些伯努利试验时，我们就有了一个二项分布。当随机变量X具有二项分布时，我们使用以下符号：

![](../Images/1365c7af26417ec922f6c552cda0f2fe.png)

二项分布符号

其中n是访客数量（或伯努利试验的数量），p是每次试验中事件发生的概率。p是我们在这里感兴趣的，我们想了解每个测试组中访客在页面上转化的概率。我们已经观察到了一些数据，但如前一部分所述，我们首先需要定义我们的先验。正如贝叶斯统计学中所示，我们需要将这个先验定义为一个概率分布。正如之前提到的，这个概率分布是我们不确定性的一个表征。Beta分布通常用于建模概率，因为它定义在[0,1]的区间之间。此外，使用beta分布作为二项似然函数的先验给了我们一个有用的性质——共轭性，这意味着我们的后验将从与先验相同的分布中生成。我们称beta分布为*共轭*先验。Beta分布由两个参数定义，alpha和混淆的beta。

![](../Images/2ff329a5c166acf1124b8dcf35c3a483.png)

Beta分布符号

有了历史数据，我们可以断言一个有根据的先验。我们不一定需要历史数据，我们可以使用我们的直觉来指导我们的理解，但现在假设我们两者都没有（在本教程稍后我们将使用有根据的先验，但为了演示影响，我将从无先验开始）。假设我们对公司网站的转化率没有理解，因此将我们的先验定义为Beta(1,1)。这被称为平坦先验。这个函数的概率分布如下图所示，与定义在区间[0,1]之间的均匀分布相同。通过断言Beta(1,1)先验，我们说所有可能的页面转化率值是等概率的。

![](../Images/7b897406a4832fde19684cc17ed692af.png)

版权：作者

现在我们拥有了所需的所有信息，包括先验和数据。让我们跳入代码中。此处提供的代码将为使用 Pyro 进行 AB 测试提供一个框架；因此，它忽略了该软件包的一些功能。为了进一步优化您的代码并充分利用 Pyro 的能力，我建议参考官方文档。

首先，我们需要导入我们的包。最后一行是良好的实践，特别是在处理笔记本时，用于清除我们已建立的参数存储。

```py
import pyro
import pyro.distributions as dist
from pyro.infer import NUTS, MCMC
import torch
from torch import tensor
import matplotlib.pyplot as plt
import seaborn as sns
from functools import partial
import pandas as pd

pyro.clear_param_store()
```

Pyro 中的模型被定义为常规的 Python 函数。这一点很有帮助，因为它使得跟随过程变得直观。

```py
def model(beta_alpha, beta_beta):
    def _model_(traffic: tensor, number_of_conversions: tensor):
        # Define Stochastic Primatives
        prior_c = pyro.sample('prior_c', dist.Beta(beta_alpha, beta_beta))
        prior_t = pyro.sample('prior_t', dist.Beta(beta_alpha, beta_beta))
        priors = torch.stack([prior_c, prior_t])
        # Define the Observed Stochastic Primatives
        with pyro.plate('data'):
            observations = pyro.sample('obs', dist.Binomial(traffic, priors),\
                             obs = number_of_conversions)
    return partial(_model_)
```

这里有一些需要分解和解释的内容。首先，我们有一个包裹在外层函数中的函数，外层函数返回内层函数的部分函数。这使我们可以更改先验，而无需更改代码。我将内层函数中定义的变量称为原始变量，可以将原始变量视为模型中的变量。模型中有两种类型的原始变量，即随机和观察随机。在 Pyro 中，我们不需要显式定义差异，只需在 sample 方法中添加 obs 参数，当它是观察原始变量时，Pyro 会相应地解释它。观察原始变量包含在上下文管理器 pyro.plate() 中，这是最佳实践，使我们的代码看起来更清晰。我们的随机原始变量是两个由 Beta 分布表征的先验，由我们从外层函数传入的 alpha 和 beta 参数控制。如前所述，我们通过将它们定义为相等来断言零假设。然后，我们使用 tensor.stack() 将这两个原始变量堆叠在一起，该操作类似于连接一个 Numpy 数组。这将返回一个张量，这是 Pyro 中推断所需的数据结构。我们已经定义了模型，现在让我们进入推断阶段。

如前所述，本教程将使用 MCMC。下面的函数将使用我们上面定义的模型和我们希望用于生成后验分布的样本数量作为参数。我们还将数据传递给函数，正如我们为模型所做的那样。

```py
def run_infernce(model, number_of_samples, traffic, number_of_conversions):
    kernel = NUTS(model)

    mcmc = MCMC(kernel, num_samples = number_of_samples, warmup_steps = 200)

    mcmc.run(traffic, number_of_conversions)

    return mcmc
```

此函数中的第一行定义了我们的内核。我们使用NUTS类定义我们的内核，NUTS代表No-U-Turn Sampler，是Hamiltonian Monte Carlo的自调版本。这告诉Pyro如何从后验概率空间进行采样。同样，深入探讨这个主题超出了本文的范围，但目前知道NUTS允许我们智能地从概率空间中进行采样就足够了。然后在第二行中使用该内核初始化MCMC类，指定使用NUTS。我们将number_of_samples参数传递给MCMC类，该参数是用于生成后验分布的样本数量。我们将初始化的MCMC类分配给mcmc变量，并调用run()方法，将我们的数据作为参数传递。该函数返回mcmc变量。

这就是我们需要的一切；以下代码定义了我们的数据并调用了我们刚刚使用Beta(1,1)先验创建的函数。

```py
traffic = torch.tensor([5523., 1379.])
conversions =torch.tensor([2926., 759.])
inference = run_infernce(model(1,1), number_of_samples = 1000, \
               traffic = traffic, number_of_conversions = conversions)
```

交通和转换张量的第一个元素是对照组的计数，而每个张量中的第二个元素是处理组的计数。我们将模型函数、控制我们先验分布的参数以及我们定义的张量一起传入。运行此代码将生成我们的后验样本。我们运行以下代码以提取后验样本并将它们传递到一个Pandas数据框中。

```py
posterior_samples = inference.get_samples()
posterior_samples_df = pd.DataFrame(posterior_samples)
```

请注意该数据框的列名是我们在定义模型函数时传递的字符串。数据框中的每一行包含从后验分布中抽取的样本，每个样本代表页面转换率的估计值，即控制我们的二项分布的概率值p。现在我们已经返回了样本，我们可以绘制我们的后验分布。

**结果**

一种有洞察力的方式来可视化两个测试组的AB测试结果是通过联合核密度图。它允许我们可视化两个分布中概率空间的样本密度。下面的图可以从我们刚刚构建的数据框中生成。

![](../Images/b2df9ebb9cab17eea7484b968bc80bf8.png)

信誉：作者

上图中的概率空间可以沿对角线进行划分，任何在该线以上的区域表明处理组的转化率估计高于对照组，反之亦然。如图所示，从后验中抽取的样本在该区域密集分布，这表明处理组的转化率更高。值得强调的是，处理组的后验分布比对照组更宽，这反映了更高的的不确定性。这是由于在处理组中观察的数据较少。然而，该图强烈表明处理组优于对照组。通过从后验中收集一系列样本并计算元素间差异，我们可以说处理组优于对照组的概率为90.4%。这一数据表明，90.4%的从后验中抽取的样本将在上述联合密度图中的对角线以上分布。

这些结果是通过使用平坦（无信息）先验获得的。使用有信息的先验可能有助于改进模型，尤其是在观察数据有限的情况下。一个有用的练习是探讨使用不同先验的效果。下图显示了Beta(2,2)概率密度函数以及当我们重新运行模型时生成的联合图。我们可以看到，使用Beta(2,2)先验为两个测试组产生了非常相似的后验分布。

![](../Images/734d781ef99d44e895478e86c720d88c.png)

版权归：作者

从后验中抽取的样本表明，处理组比对照组表现更好的概率为91.5%。因此，我们可以以更高的确定性认为处理组优于对照组，而不是使用平坦先验。然而，在这个例子中，差异微不足道。

我还想强调一件事。当我们进行推断时，我们告诉Pyro从后验中生成1000个样本。这是一个任意的数字，选择不同数量的样本可能会改变结果。为了突出增加样本数量的效果，我运行了一个AB测试，其中控制组和处理组的观察数据相同，每组的总体转化率为50%。使用Beta(2,2)先验会生成以下后验分布，随着样本数量的逐步增加。

![](../Images/09acb2be339d0b6ce5713d6a385e3eb9.png)

版权归：作者

当我们仅使用 10 个样本进行推断时，控制组和治疗组的后验分布相对较宽且采用不同的形状。随着我们绘制的样本数增加，这些分布最终会收敛，生成几乎相同的分布。此外，我们观察到统计分布的两个性质，即中心极限定理和大数定律。中心极限定理表明，随着样本数量的增加，样本均值的分布会收敛于正态分布，我们可以在上述图中看到这一点。此外，大数定律表明，随着样本大小的增加，样本均值会收敛于总体均值。我们可以看到底部右侧瓷砖中的分布均值约为 0.5，这是在每个测试样本中观察到的转化率。

**贝叶斯 AB 测试的商业案例**

贝叶斯 AB 测试可以帮助提升企业的测试与学习文化。贝叶斯统计推断允许快速检测测试中的小幅提升，因为它不依赖于长期概率来得出结论。测试结论可以更快地得出，从而提高学习速度。贝叶斯 AB 测试还允许在测试过程中早期停止测试，如果通过“窥探”获得的结果表明测试组表现显著不如对照组，则可以停止测试。因此，测试的机会成本可以显著降低。这是贝叶斯 AB 测试的一个主要优势；结果可以不断监测，并且我们的后验概率不断更新。相反，对测试对象的提升的早期检测可以帮助企业更快地实施变更，减少实施提高收入变更的延迟。面向客户的企业必须能够快速实施和分析测试结果，这正是贝叶斯 AB 测试框架的优势。

**总结**

本文简要介绍了贝叶斯思维的背景，并使用 Pyro 探索了贝叶斯 AB 测试的结果。希望您会觉得这篇文章有见地。正如介绍中所述，欢迎并鼓励反馈。正如承诺的那样，我已在下面链接了一些进一步阅读材料。

**推荐材料**

下列书籍深入探讨贝叶斯推断：

+   The Signal and the Noise: The Art and Science of Prediction — Nate Silver

+   *The Book of Why: The New Science of Cause and Effect* — Judea Pearl 和 Dana Mackenzie。尽管这本书主要关注因果关系，但第三章关于贝叶斯的阅读也是值得一读的。

+   *Bayesian Methods for Hackers: Probabilistic Programming and Bayesian Inference —* Cameron Davidson-Pilon。这本书也可以在 Git 上找到，我已经链接在下面。

[](https://github.com/CamDavidsonPilon/Probabilistic-Programming-and-Bayesian-Methods-for-Hackers?source=post_page-----cd4daff686e1--------------------------------) [## GitHub — CamDavidsonPilon/Probabilistic-Programming-and-Bayesian-Methods-for-Hackers: 又名“黑客的贝叶斯方法”]

### 又名“黑客的贝叶斯方法”：介绍贝叶斯方法 + 概率编程的...

github.com](https://github.com/CamDavidsonPilon/Probabilistic-Programming-and-Bayesian-Methods-for-Hackers?source=post_page-----cd4daff686e1--------------------------------)

这些Medium文章提供了MCMC的详细解释。

[](/monte-carlo-markov-chain-mcmc-explained-94e3a6c8de11?source=post_page-----cd4daff686e1--------------------------------) [## Monte Carlo Markov Chain (MCMC) 解释

### MCMC方法是一类使用马尔可夫链来执行蒙特卡罗估计的算法。

towardsdatascience.com](/monte-carlo-markov-chain-mcmc-explained-94e3a6c8de11?source=post_page-----cd4daff686e1--------------------------------) [](/bayesian-inference-problem-mcmc-and-variational-inference-25a8aa9bce29?source=post_page-----cd4daff686e1--------------------------------) [## 贝叶斯推断问题、MCMC和变分推断

### 贝叶斯推断问题在统计学中的概述。

towardsdatascience.com](/bayesian-inference-problem-mcmc-and-variational-inference-25a8aa9bce29?source=post_page-----cd4daff686e1--------------------------------)
