# 如何构建因果推断机器学习模型，探讨全球变暖是否由人类活动引起

> 原文：[`towardsdatascience.com/how-to-build-a-causal-inference-model-to-explore-whether-global-warming-is-caused-by-human-activity-2bcf1830e071`](https://towardsdatascience.com/how-to-build-a-causal-inference-model-to-explore-whether-global-warming-is-caused-by-human-activity-2bcf1830e071)

## 如何使用 Python 和 DoWhy 库构建因果推断模型，探讨全球变暖的原因

[](https://grahamharrison-86487.medium.com/?source=post_page-----2bcf1830e071--------------------------------)![Graham Harrison](https://grahamharrison-86487.medium.com/?source=post_page-----2bcf1830e071--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2bcf1830e071--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2bcf1830e071--------------------------------) [Graham Harrison](https://grahamharrison-86487.medium.com/?source=post_page-----2bcf1830e071--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2bcf1830e071--------------------------------) ·阅读时长 13 分钟·2023 年 1 月 2 日

--

![](img/a61ed753b6249ceb58c57f3115df7f48.png)

图片由 [Patrick Hendry](https://unsplash.com/@worldsbetweenlines?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来自 [Unsplash](https://unsplash.com/s/photos/climate-change?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

我最近发布了一篇文章，提供了如何使用 `Pgmpy` 库进行因果推断的简单教程 -

[](/a-simple-explanation-of-causal-inference-in-python-357509506f31?source=post_page-----2bcf1830e071--------------------------------) ## 用 Python 进行因果推断的简单解释

### 对如何在 Python 中构建一个完整的因果推断模型的直白解释

towardsdatascience.com

文章的读者之一指出，数据的简单性质（例如疫苗接种 = 1 或 0，疾病 = 1 或 0）意味着因果推断模型只能用来解决相对简单的问题，因此它们对实际问题的适用性有限。

这促使我寻找一个大的问题，尝试使用因果推断模型来解决。我选择了“全球变暖”，特别是气候否认，原因如下 -

+   如果我能开发出一个有效的模型，它将驳斥因果推断仅限于解决简单问题的提议。

+   有可用的数据集（只要你足够努力寻找！）。

+   这与最近 2022 年 11 月在埃及沙姆沙伊赫举行的联合国气候变化大会 COP27 息息相关。

# 获取数据

模型中使用的关键数据来源是-

+   世界银行数据

+   全球监测实验室

+   NASA

所有这些数据来源均可供公众使用。有关许可证和使用条款的完整细节可以在文章末尾的“参考文献”部分找到。

这里是综合气候数据…

![](img/ad042670ec5e8bce39751b84d2527850.png)

图片作者提供

… 这里是特征的解释…

![](img/4b8faf983f35a5b780450dcea93495bf.png)

图片作者提供

# 探索性数据分析

接下来的部分将探索数据，以识别和可视化相关性。

我省略了代码以保持文章的因果推断方面的专注，但如果你想查看和运行代码，可以在这里找到 — [`github.com/grahamharrison68/Public-Github/blob/master/Causal%20Inference/Climate%20Change%20Model-v3.ipynb`](https://github.com/grahamharrison68/Public-Github/blob/master/Causal%20Inference/Climate%20Change%20Model-v3.ipynb)。

## 相关性

让我们首先快速查看主要特征之间的相关性…

![](img/62935c420fc8698bf44b87459712055b.png)

图片作者提供

看起来存在一些高度相关的情况，所以让我们更详细地研究这些关系…

## 人口 vs. 能源

我们的初步分析是探讨全球人口与能源消费之间的关系…

![](img/045629f41639e92717b362d988335a2f.png)

图片作者提供

这一比较表明，世界人口增长与能源使用之间存在线性增加的关系，相关系数为 r=0.931。

## 总能源 vs. 化石燃料消费

鉴于人口与能源之间的相关性，下一张图表将探讨能源消费与化石燃料特定消费之间的相关性…

![](img/35f2562ff86c7a658b0daf034477e0bd.png)

图片作者提供

总体能源使用与化石燃料使用之间几乎完美的相关性并不令人惊讶，但仍然有助于可视化。

## 化石燃料能源消费与二氧化碳浓度

按照前向链条，如果能源消费与化石燃料使用之间存在相关性，那么化石燃料使用与二氧化碳浓度水平之间的关联是什么呢？…

![](img/6750459d59d5d1ab190d0664243cd773.png)

图片作者提供

存在强相关性，但暂时不要对因果关系做出任何结论，因为这将是文章下一部分的主题。

## 二氧化碳浓度与温度变化

拼图的最后一块是探索二氧化碳浓度与全球温度之间的潜在相关性…

![](img/b764e71b8fbd32a38fdbfe53a42a8eb7.png)

图片作者提供

这两个特征之间存在相关性，这可能是合理的预期，但要从相关性过渡到因果关系需要建立因果模型…

# 从相关性到因果关系的转变

我们知道存在以下相关关系 -

+   人口增加与能源消耗增加相关。

+   能源消耗的增加与化石燃料使用的增加相关。

+   化石燃料的使用与 CO2 水平的增加相关。

+   CO2 的增加与温度的升高相关。

> 不过，我们也知道卡尔·皮尔逊的名言：“相关性并不意味着因果关系”

([`thedecisionlab.com/reference-guide/philosophy/correlation-vs-causation`](https://thedecisionlab.com/reference-guide/philosophy/correlation-vs-causation))

那么如果我们要探讨人类活动（能源消耗）与全球变暖之间从相关性到合理的因果关系的过渡，我们能说些什么呢？

阅读了 Judea Pearl 的《因果关系之书》并研究了许多其他资料后，可以合理得出以下结论：如果发现相关性，必须有以下其中之一是正确的 -

1.  相关性确实暗示因果关系，或者……

1.  相关性是由“混杂”因素造成的（稍后会详细说明）。

.. 基于额外的阅读和研究，我还想补充第三种可能性 -

1.  相关性完全是虚假的，仅仅是巧合。

让我们逆向探索这三种可能性……

## 虚假相关性

考虑以下这些，它们是我最喜欢的虚假相关性，归因于 Tyler Vigen（见参考文献部分以获取归因）。

![](img/d1b06f3e2fd1f5af2c7f622a25147def.png)

虚假相关性, [`www.tylervigen.com/spurious-correlations`](https://www.tylervigen.com/spurious-correlations)

证明是难以获得的，但显然常识会告诉我们，“想要维持婚姻的夫妻应该少吃人造黄油”这种说法不可能成立。因此，图表中的相关性被认为完全是虚假的、巧合的和偶然的。

高度相关且完全虚假的相关性在现实世界中难以反驳，但可以通过以下方式合理质疑……

+   咨询领域专家。

+   留意数据中 y 轴的显著差异。

+   使用置信区间。

+   继续监测数据，以查看相关性是否持续或未来是否出现偏离。

人造黄油和离婚的置信区间会很高，因此在这种情况下，我们需要找寻并咨询乳制品和婚姻指导方面的专家，以证明或反驳“食用人造黄油导致离婚”的假设，或者更可能只是继续监测趋势，并等待数据中不可避免的未来偏离。

## 混杂因素

为了可视化混杂问题，让我们回到我们的模型，看看非化石燃料消耗与 CO2 浓度之间的相关性……

![](img/bccc8ee7e630bf0de5b329b67c1054c7.png)

图片来源：作者

表面上看，这令人担忧和困惑。非化石燃料被假定为核电和可再生能源，这两者都不应该导致二氧化碳浓度的增加，而这可能进一步导致温度的升高（如果可以建立因果链）。

那么，为什么我们使用的非化石燃料越多，二氧化碳水平却越高呢？

发生的情况是，人口增加导致更多的能源被生产和消费。随着对能源需求的增加，这会导致化石燃料和非化石燃料能源消耗的增加。

化石燃料的能源消耗正在导致二氧化碳水平的增加（如果我们能证明我们的理论），但随着能源的增加，无论是化石燃料还是非化石燃料的消耗都在增加。

能源具有“混杂效应”，导致非化石燃料和二氧化碳排放之间的相关性。

混杂效应可能是一个难以理解的概念，但我认为这个例子很好的说明了这一点。

> 总结来说，非化石燃料能源消费与二氧化碳排放之间存在一种既非因果也非虚假的相关性。它完全是由于能源对非化石燃料和化石燃料能源消费的混杂效应所致。

# 提议的因果模型

本文的目的是展示因果推断机器学习解决方案如何用于构建模型，以解决复杂、大规模和有意义的现实世界问题，因此我将提出一个因果链的提案，这些链条将由气候和能源专家在实际模型中进行测试和改进。

下一步将使用我的`DirectedAcyclicGraph`类。我在文章中省略了源代码，以使内容更简洁，但这里是完整源码的链接，以防你想自己运行代码 - [`gist.github.com/grahamharrison68/9733b0cd4db8e3e049d5be7fc17b7602`](https://gist.github.com/grahamharrison68/9733b0cd4db8e3e049d5be7fc17b7602)。

如果你决定使用它，并且喜欢的话，何不考虑给我买杯咖啡呢？ [`ko-fi.com/grahamharrison`](https://ko-fi.com/grahamharrison)

这是我提议的模型……

![](img/e37b41c9cb23c898d6ea320a80a1eeab.png)

作者提供的图片

……这些链接的含义是……

+   人口（POP）的增加导致了能源消耗的增加。

+   能源使用（ENGY）正在导致化石燃料和非化石燃料的增加。

+   化石燃料使用（FFEC）正在导致二氧化碳浓度（CO2C）水平的增加。

+   二氧化碳浓度（CO2C）的增加正在导致全球温度（TMPI）的升高。

……但如果事情不仅仅如此呢？

# 大问题：是否有自然因素导致全球变暖？

基于现有的科学证据，有强有力的理由认为人类活动对全球变暖有因果影响，但仍有许多人持不同观点……

> ***气候变化否认，或全球变暖否认，是对气候变化的科学共识的否认、驳斥或怀疑，包括对气候变化的范围、是否由人类造成、其对自然和人类社会的影响，或人类行动是否能够适应全球变暖的潜力的否认。***

([`en.wikipedia.org/wiki/Climate_change_denial#:~:text=Climate%20change%20denial%2C%20or%20global,global%20warming%20by%20human%20actions`](https://en.wikipedia.org/wiki/Climate_change_denial#:~:text=Climate%20change%20denial%2C%20or%20global,global%20warming%20by%20human%20actions))

如果我们能够扩展目前开发的模型，以解决这个问题，并提供一些基于因果推断的证据来反驳这一观点，并为接受的科学意见提供进一步的实证证据，证明全球变暖是由人类活动造成的，将会非常有用。

为了探索全球变暖的替代原因，我们需要回到混杂的概念。

我们从早期的例子中知道，能源消费对化石燃料和非化石燃料使用都有因果关系，因此能源是“混杂”非化石燃料使用的因素，导致 CO2 浓度的相关性既不是因果的也不是虚假的。

> 如果存在一个我们不知道的混杂因素导致 CO2 水平上升和温度升高，这将会怎样？这听起来不太可能，但如果是真的，它将会解构化石燃料能源消费导致 CO2 浓度上升的观点，并加强自然因素负责的看法。

如果存在一个未知因素导致 CO2 和温度上升，那么它在当前提出的有向无环图中没有被表示。它不仅是一个混杂因素，而且是一个“未观测到的混杂因素”。

因此，解决方案需要将有向无环图扩展如下，添加“U”表示潜在的“未观测混杂因素” -

![](img/082b1c86946bf5604445b1acd41d8dcb.png)

图片由作者提供

不论你信不信，因果推断可以用来提供一个可靠的衡量指标，来评估即使存在未观测到的混杂因素且数据中没有捕捉到的情况下，增加化石燃料能源使用对温度升高的影响！

数学非常复杂（超出了本文的范围），但幸运的是，一些新兴的 Python 因果推断库可以解决未观测混杂问题。

我最喜欢的轻量级 Python 因果推断库是`Pgmpy`，但它目前不实现未观测混杂因素。我已经记录了这个问题并提出了一个工单，目前`Pgmpy`团队正在处理。你可以在这里查看工单状态 - [`github.com/pgmpy/pgmpy/issues/1574`](https://github.com/pgmpy/pgmpy/issues/1574) - 计划在 2022 年 12 月 31 日的 0.1.21 版本中发布。

同时，本文提供了一个使用`DoWhy`库的模型，该库已经解决了未观察到的混杂因素问题……

```py
Estimand type: EstimandType.NONPARAMETRIC_ATE

### Estimand : 1
Estimand name: backdoor
Estimand expression:
   d            
───────(E[TMPI])
d[FFEC]         
Estimand assumption 1, Unconfoundedness: If U→{FFEC} and U→TMPI then P(TMPI|FFEC,,U) = P(TMPI|FFEC,)

### Estimand : 2
Estimand name: iv
Estimand expression:
 ⎡                               -1⎤
 ⎢   d          ⎛   d           ⎞  ⎥
E⎢───────(TMPI)⋅⎜───────([FFEC])⎟  ⎥
 ⎣d[ENGY]       ⎝d[ENGY]        ⎠  ⎦
Estimand assumption 1, As-if-random: If U→→TMPI then ¬(U →→{ENGY})
Estimand assumption 2, Exclusion: If we remove {ENGY}→{FFEC}, then ¬({ENGY}→TMPI)

### Estimand : 3
Estimand name: frontdoor
No such variable(s) found!
```

# 代码解释

代码的主要部分是单行调用，它创建了`CausalModel`实例。参数如下 -

+   `data=df_climate_change[climate_features]` 指示模型使用哪个`DataFrame`和哪些特征来构建模型。

+   `graph=climate_dag.gml_graph` 指示模型使用在`DirectedAcyclicGraph`的`climate_dag`实例中提出和表示的因果关系。

+   “处理”是我们感兴趣的变化和理解的事物，在本例中是化石燃料的使用。

+   “结果”是我们感兴趣的影响，即当“处理”（化石燃料的使用）变化时，对“结果”（全球温度升高）的影响是什么？

`DoWhy`的工作方式一开始有点不直观。许多其他库此时会直接提供答案，但`DoWhy`会产生一个称为“估算量”的中间步骤。

要计算因果效应，必须在有向无环图中从处理到结果找到三种路径或路线之一 -

1.  一个“后门”路径。

1.  一个“前门”路径。

1.  一个“工具变量”。

注意：对这三种类别的详细解释将是未来文章的主题。

从`print(climate_estimand)`输出的内容看起来很吓人，这让我很长一段时间不愿使用`DoWhy`，但实际上大多数输出内容可以忽略。

值得注意的是，在我们的模型中，`DoWhy`找到了一个后门路径和一个工具变量（iv），但没有找到前门路径。我们可以使用已找到的两者中的任何一个，但我选择使用“工具变量”。

再次强调，`DoWhy`与大多数其他库不同，所选的方法必须在代码中明确声明，无法让`DoWhy`自动选择合适的方法。

幸运的是，选择“工具变量”的代码非常简单 -

需要注意的是，所有三种可能的方法名称如下 -

1.  后门：`method_name='backdoor.linear_regression'`

1.  前门：`method_name='frontdoor.two_stage_regression'`

1.  工具变量：`method_name='iv.instrumental_variable'`

这就只剩下一个简单的辅助函数，用于以更可读的格式打印结果，模型就完成了！

```py
Average Treatment Effect (ATE):
For every unit change (1 unit = 1 x Oil Equivalent per Capita (10K KG)) in "Fossil Fuel Energy Consumption" ...
"Global Temperature Increase" will change by +0.02322620230983087 (1 unit = 1 x Degrees C (Lowess Smoothing))
```

就这样！

> 每增加 10,000 公斤化石燃料能源消费，全球温度将增加 0.023 摄氏度，即使存在一个未观察到的混杂因素作用于 CO2 和温度，这一因果增加仍然成立！

实际上，这种简单的因果推断实现，使用微软的`DoWhy`库，可以用来反驳气候变化否认论，并提供一个“平均处理效应”，即非化石燃料能源消费增加时温度会升高多少！

# 结论

我的一个读者回应了之前关于因果推断的文章，认为当前的库可能仅适用于简单问题。

这促使我寻找一个大问题来解决，而没有比气候变化更大的问题了！

本文展示了因果推断如何解决大问题，并提供解决传统机器学习方法无法解决的问题的解决方案。

具体来说，该模型提供了证据，表明人类活动（例如化石燃料能源消费的增加）对全球温度升高有因果影响。

> 同时，因果推断技术超越了“预测分析”，即模型告诉我们如果未来大致类似于过去会发生什么，转向“处方分析”，即模型可以告诉我们如果我们介入并改变事物会发生什么。

如果您喜欢这篇文章，请考虑……

[通过我的推荐链接加入 Medium](https://grahamharrison-86487.medium.com/membership)（如果您使用此链接注册，我将获得一部分费用）。

[](https://grahamharrison-86487.medium.com/membership?source=post_page-----2bcf1830e071--------------------------------) [## 通过我的推荐链接加入 Medium - Graham Harrison

### 阅读 Graham Harrison 的每一个故事（以及 Medium 上其他数千位作家的故事）。提升您的数据知识……

grahamharrison-86487.medium.com](https://grahamharrison-86487.medium.com/membership?source=post_page-----2bcf1830e071--------------------------------)

[订阅我的免费电子邮件，每当我发布新故事时](https://grahamharrison-86487.medium.com/subscribe)。

[快速查看我之前的文章](https://grahamharrison-86487.medium.com/)。

[下载我的免费战略数据驱动决策框架](https://relentless-originator-3199.ck.page/5f4857fd12)。

访问我的数据科学网站 — [数据博客](https://www.the-data-blog.co.uk/)。

# 参考资料

+   全球变暖博客 by Henry Auer，[`warmgloblog.blogspot.com/2013/06/co2-and-temperature-changes-are.html?m=1`](https://warmgloblog.blogspot.com/2013/06/co2-and-temperature-changes-are.html?m=1)

+   人口数据：世界银行数据，[`data.worldbank.org/`](https://data.worldbank.org/)，数据集 SP.POP.TOTL，CC BY 4.0 许可

+   城市人口数据：世界银行数据，[`data.worldbank.org/`](https://data.worldbank.org/)，数据集 SP.URB.TOTL，CC BY 4.0 许可

+   人均 GDP：世界银行数据，[`data.worldbank.org/`](https://data.worldbank.org/)，数据集 NY.GDP.PCAP.CD，CC BY 4.0 许可

+   总能源使用数据：世界银行数据，[`data.worldbank.org/`](https://data.worldbank.org/)，数据集 EG.USE.PCAP.KG.OE，CC BY 4.0 许可

+   化石燃料能源使用数据：世界银行数据，[`data.worldbank.org/`](https://data.worldbank.org/)，数据集 EG.USE.COMM.FO.ZS，CC BY 4.0 许可

+   非化石燃料能源使用数据：世界银行数据，[`data.worldbank.org/`](https://data.worldbank.org/)，数据集 EG.USE.COMM.FO.ZS，CC BY 4.0 许可证

+   CO2 浓度数据：全球监测实验室（[`gml.noaa.gov/`](https://gml.noaa.gov/)），ftp.cmdl.noaa.gov/ccg/co2/trends/co2_annmean_mlo.txt，“数据免费向公众开放”

+   温度数据：NASA 全球气候变化，[`climate.nasa.gov/vital-signs/global-temperature/`](https://climate.nasa.gov/vital-signs/global-temperature/)，许可：注明出处（[`climate.nasa.gov/faq/32/may-i-use-content-and-imagery-from-your-website-if-so-to-whom-do-i-credit-them/`](https://climate.nasa.gov/faq/32/may-i-use-content-and-imagery-from-your-website-if-so-to-whom-do-i-credit-them/））

+   温度数据：NASA 全球气候变化。（2021 年）。检索于 13/11/2022，来源于[https://climate.nasa.gov/vital-signs/global-temperature/](https://climate.nasa.gov/vital-signs/global-temperature/)

+   CC BY 4.0 许可证 — [`data.worldbank.org/summary-terms-of-use`](https://data.worldbank.org/summary-terms-of-use)

+   虚假相关，[`www.tylervigen.com/spurious-correlations`](https://www.tylervigen.com/spurious-correlations)，创意共享署名许可证（[`www.tylervigen.com/about`](https://www.tylervigen.com/about)）
