# 分析车辆尺寸与行人安全

> 原文：[`towardsdatascience.com/suvs-are-killing-people-de6ce08bac3d`](https://towardsdatascience.com/suvs-are-killing-people-de6ce08bac3d)

## 公开的交通事故数据表明，SUV 导致行人死亡和受伤的比例高于小型汽车

[](https://medium.com/@djcunningham0?source=post_page-----de6ce08bac3d--------------------------------)![Danny Cunningham](https://medium.com/@djcunningham0?source=post_page-----de6ce08bac3d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----de6ce08bac3d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----de6ce08bac3d--------------------------------) [Danny Cunningham](https://medium.com/@djcunningham0?source=post_page-----de6ce08bac3d--------------------------------)

·发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----de6ce08bac3d--------------------------------) ·8 分钟阅读·2023 年 1 月 10 日

--

![](img/2ef0182719b9631493fb39ee5b606e8a.png)

图片由[Alexandru Acea](https://unsplash.com/@alexacea?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

《纽约时报》[最近强调](https://www.nytimes.com/2022/11/27/upshot/road-deaths-pedestrians-cyclists.html)了“极具美国特色”的道路死亡上升问题。除了美国之外，几乎所有发达国家的道路变得越来越安全。即使在 COVID-19 疫情高峰期，当时道路上的汽车大幅减少，[交通死亡人数仍在增加](https://www.nytimes.com/2021/01/01/nyregion/nyc-traffic-deaths.html)。

美国的道路对行人特别危险。保险公路安全研究所（IIHS）[2022 年 3 月的一项研究](https://www.iihs.org/news/detail/suvs-other-large-vehicles-often-hit-pedestrians-while-turning)发现，自 2009 年以来，行人死亡人数增加了 59%，而 2020 年所有机动车死亡中有 20%是行人。导致这些严峻统计数据的因素有很多，但一个主要因素（言外之意）是道路上车辆的尺寸。

大型 SUV 和皮卡车由于其较大的重量和更高的前端，明显比小型汽车更容易伤害或杀死行人。而且，如果你生活在美国，你肯定会注意到，美国人喜欢大型车辆。一些报告显示，现在[超过 80%的新车销售在美国是 SUV 或皮卡](https://jalopnik.com/trucks-and-suvs-are-now-over-80-percent-of-new-car-sale-1848427797)。这对行人来说是个坏消息。并且我们还将面临[荒谬沉重的电动汽车](https://www.google.com/search?q=hummer+ev+weight&client=safari&rls=en&sxsrf=AJOqlzXQvjl37e6JayYntx6U_ZmVu3H1iw%3A1673159351587&ei=t2K6Y_jCI4GcptQPiaCbKA&ved=0ahUKEwi4-ramrLf8AhUBjokEHQnQBgUQ4dUDCA8&uact=5&oq=hummer+ev+weight&gs_lcp=Cgxnd3Mtd2l6LXNlcnAQAzIICAAQgAQQsQMyBQgAEIAEMgUIABCABDIFCAAQgAQyBggAEBYQHjIGCAAQFhAeMgkIABAWEB4Q8QQyBggAEBYQHjIGCAAQFhAeMgYIABAWEB46CggAEEcQ1gQQsAM6DQgAEEcQ1gQQsAMQiwM6CggAELADEEMQiwM6DQgAEOQCENYEELADGAE6FQguEMcBENEDENQCEMgDELADEEMYAjoSCC4QxwEQ0QMQyAMQsAMQQxgCOgwILhDIAxCwAxBDGAI6EAgAEIAEEIcCELEDEIMBEBQ6CwgAEIAEELEDEIMBOgoIABCABBCHAhAUOgQIABBDOgUIABCRAjoICAAQsQMQkQI6CAgAEBYQHhAKOgsIABAWEB4Q8QQQCkoECEEYAEoECEYYAVCcAli8CmCXC2gBcAF4AIABZ4gByAOSAQM2LjGYAQCgAQHIARO4AQLAAQHaAQYIARABGAnaAQYIAhABGAg&sclient=gws-wiz-serp)。

但我们真的可以将这些行人安全统计数据视为绝对真实吗？我决定自己分析数据，以查看大型车辆是否确实导致了行人伤害和死亡的显著增加（剧透：确实如此）。

*以下分析的所有代码可以在 GitHub 上的* [*Jupyter 笔记本*](https://gist.github.com/djcunningham0/84c473db968f82bdf58b575ff3c0fb47)*中找到*。该笔记本包含了本文中未包含的额外细节。

# 数据

我分析了我所在城市芝加哥的数据。芝加哥发布了[交通碰撞数据](https://data.cityofchicago.org/Transportation/Traffic-Crashes-Crashes/85ca-t3if)（以及大量其他数据集），并通过 Socrata API 公开提供。开始使用 Socrata 数据集很简单：只需按照[文档](https://dev.socrata.com/consumers/getting-started.html)获取应用程序令牌，找到你感兴趣的 API 端点，然后开始查询。我使用了[sodapy](https://github.com/xmunoz/sodapy) Python 包来与 API 交互。

具体来说，我使用了与芝加哥交通碰撞相关的三个数据集：

+   [碰撞记录](https://dev.socrata.com/foundry/data.cityofchicago.org/85ca-t3if)：有关碰撞的基本细节，例如发生的时间和地点。每次碰撞一个记录。

+   [人员记录](https://dev.socrata.com/foundry/data.cityofchicago.org/u6pd-qa9d)：有关涉及碰撞的人员的详细信息。识别一个人是否受伤，以及他们是司机还是行人。每次碰撞至少一个记录。

+   [车辆](https://dev.socrata.com/foundry/data.cityofchicago.org/68nd-jvt3)：涉及碰撞的车辆的详细信息。包括品牌、型号和车辆类型。每次碰撞至少包含一条记录。

这些数据集中的数据是由每次碰撞事件中的报告警官收集的。

![](img/0d91a0d0984c4c6d4adb5128604c53cc.png)

照片由 [Sawyer Bengtson](https://unsplash.com/@sawyerbengtson?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 方法论

分析的目标是确定大型车辆（SUV 和皮卡）是否比小型汽车更可能致使行人遇难或严重受伤。我们将通过拟合逻辑回归模型来实现这一点，该模型将显示哪些因素在决定伤害是否发生中是相关的。

## 定义目标变量和特征

在拟合模型之前，我们需要将原始数据转换为模型可以使用的格式。这意味着我们需要定义我们的目标变量（我们试图预测的内容）和一些特征（我们将用来进行预测的内容）。

在我们的案例中，目标变量是一个二进制值，指示在碰撞中是否有行人遇难或严重受伤（1 = 受伤，0 = 无受伤）。这可以从 People 数据集中轻松计算得出。首先，我们通过“person_type”字段确定碰撞中的人员是否为行人。然后，我们使用“injury_classification”字段确定是否有任何行人受到了致残性或致命伤害。我们实际上会在这里创建两个目标变量——一个用于致残伤害，一个用于致命伤害——并在后续单独建模。*（注：数据集将致残性伤害定义为“阻止受伤者行走、驾驶或正常继续其能够进行的活动”的任何伤害。）*

我们需要包括的主要特征是车辆类型。这通常比较简单，因为车辆数据集中包含了区分汽车、SUV、皮卡等的分类。然而，数据集中对车辆的标注并不一致。例如，一位警官可能将一辆丰田 RAV4 标记为 SUV，而另一位警官可能将其标记为汽车。为了考虑这些差异，我们将检查每个品牌/型号*最常*被标记的类别，并将其用于该品牌/型号的所有实例。在完成这小部分特征工程后，我们可以很容易地为每次碰撞计算两个二进制特征：一个表示是否涉及 SUV，另一个表示是否涉及皮卡。

我们还应该控制其他可能影响碰撞结果的因素，如天气条件和限速。我们将通过在回归模型中包括额外的特征来做到这一点，这将使我们能够估计每个单独特征的独立效应。

最后，我们将从数据集中移除所有不涉及行人的事故，因为这些事故与当前的问题无关。

## 逻辑回归模型

我们试图回答的问题可以很容易地框定为一个二元分类问题。根据关于事故的信息，预测行人是否会被杀害（或受伤）。在我们的用例中，理解*为什么*做出这种预测也是至关重要的。

逻辑回归是一个不错的选择，因为它产生了一个简单且易于解释的模型。我们将能够分析模型以了解哪些特征对目标变量的概率有正面或负面影响。具体来说，我们将能够看到车辆类型是否对行人死亡和受伤有显著影响。

逻辑回归的完整数学推导超出了本文的范围，但本质上模型会产生一个线性预测器：

![](img/8241fd21b75da4843deeec0b3228470c.png)

其中*β*系数表示每个特征*X*对目标变量的影响。我们可以将系数转换为易于解释的赔率比（例如，当车辆是 SUV 时，行人受伤的可能性增加*Z%*）。

我们将拟合两个独立的逻辑回归模型：一个用于预测行人死亡，另一个用于预测行人致残伤害（包括死亡）。除了目标变量外，模型结构是相同的。

## 假设

我们在模型中做了几个假设，无论是明确还是隐含的：

1.  SUV 和较小的汽车在涉及行人的事故中发生率相同。我们只是在预测事故的结果，而不是预测事故是否会因不同的汽车而发生。*(注意：* [*有一些证据*](https://www.iihs.org/news/detail/suvs-other-large-vehicles-often-hit-pedestrians-while-turning) *表明 SUV 撞击行人的频率高于小型汽车，这可能会质疑这一假设。如果这确实是事实，那么 SUV 对行人死亡的影响可能比下面的结果所显示的更糟。)*

1.  所有 SUV 都是相同的（所有皮卡车也是如此）。显然，在现实世界中情况并非如此，因为 SUV 的形状和尺寸差异很大。

![](img/8a8af27f7f0364e6a0dfc50a8c9a6d02.png)

显然，这两辆 SUV 并不相同，但在模型中被视为相同。图像已获得[求助于 carsized.com](https://www.carsized.com/en/cars/compare/toyota-rav4-2019-suv-swb-vs-cadillac-escalade-2020-suv-lwb/)的许可。

# 结果

那么 SUV 和皮卡车对行人更危险吗？是的。

逻辑回归模型显示，与较小的汽车相比，SUV 更可能导致 16%的致残伤害和 36%的行人死亡。皮卡车导致致残伤害的可能性增加 33%，导致行人死亡的可能性增加 108%（是两倍多！）。

模型预测，在过去五年中，由于 SUV 和皮卡车，芝加哥大约发生了 **20 起额外死亡**（即，如果这些车辆被更小的汽车替代，这些死亡将不会发生）。

![](img/c1e4d984aa0bf8f17a65d2e43ec581d7.png)

2017 到 2022 年间，205 名行人被个人车辆撞击致死。如果 SUV 和皮卡车被更小的汽车替代，大约可以拯救 20 条生命。（图像由作者提供）

模型还识别了一些其他有趣（但可能并不完全令人惊讶）的因素，这些因素导致了行人死亡。下表显示了模型发现的所有统计显著特征的影响。以下是关键发现的总结：

+   **大型车辆对行人更危险。** SUV 和皮卡车在统计上更可能造成致残伤害。这是一个相当明显的结果，也是本文的主要话题。

+   **高速行驶对行人危险。** 更重要的是，*允许高速的条件*对行人危险。这最明显地表现为与限速的正相关关系，但也通过其他特征显示出来（有更多车道或由中央隔离带分隔的道路通常允许更高速度；停车场则不允许）。令人惊讶的是，雪天条件*降低*了行人受伤的可能性。这可能是因为雪天道路迫使汽车减速行驶。

+   **低能见度条件对行人危险。** 晚上的伤害概率增加。巷道中的碰撞也可能导致伤害，这可能是因为司机在狭窄的巷道中能见度降低（且巷道中可能有行人与车辆共同通过）。

+   **在 COVID 期间，严重的行人伤害变得更加常见。** 即使在控制了数据集中的其他特征时，COVID-19 大流行期间行为也发生了统计学上显著的变化。模型中添加了两个二元日期特征（“高峰 COVID”和“高峰后 COVID”）以解释这种差异。这可能归因于[疫情高峰期街道更空旷导致速度更快](https://www.nytimes.com/2021/01/01/nyregion/nyc-traffic-deaths.html)。 （注：致残伤害的总数不一定增加；模型检测到的是 COVID 期间致残伤害的*发生率*增加。）

下表显示了支持上述总结信息的数据点，更多细节（如置信区间）可以在[GitHub](https://gist.github.com/djcunningham0/84c473db968f82bdf58b575ff3c0fb47)上找到。

![](img/0d67cd36f2c2ec8452ce003da752a6a5.png)

一张表格总结了各种碰撞特征对行人致残伤害可能性的影响。特征只有在模型识别为统计显著（p *< 0.05*）时才被包含。“相对几率”+X% 表示如果该特征为真（或对于数值特征增加一个单位），而其他碰撞特征保持不变，那么致残伤害的可能性*提高了 X%*。

# 结论…我们可以对此做些什么？

我们得出的结论是——请鼓掌！——比起小型、轻型汽车，大型、重型汽车更容易致使行人死亡。好吧…这并不算突破性发现。然而，许多人没有意识到这是我们城市中的一个问题——直到被指出来我才想到这个问题。我希望一些有力的证据能提高人们的意识。

那我们能做些什么呢？

首先，如果你开的是 SUV 或皮卡车，这并不意味着你是一个坏人。我甚至不会试图劝你不要买这种车。人们购买这些车辆有很多原因（其中一些理由可能还是真正有效的）。

值得注意的是，SUV 确实在碰撞中能使驾驶员更安全。这就形成了一个每个驾驶员都被激励购买大型车辆的情况，但如果每个人都做出这个决定，整个系统对所有人（特别是车外的人）来说会变得更危险。在这种[“安全军备竞赛”](https://www.cbc.ca/radio/thecurrent/the-current-for-nov-1-2019-1.5344114/driver-safety-arms-race-fuelling-boom-in-gas-guzzling-suvs-says-journalist-1.5344145)场景中，唯一无可争议受益的群体是那些宁愿向你出售昂贵 SUV 而不是便宜轿车的汽车公司。

冒着有点政治风险的说，解决这个问题的一种方法是通过法规。例如，[欧洲车辆安全法规](https://usa.streetsblog.org/2017/12/07/while-other-countries-mandate-safer-car-designs-for-pedestrians-america-does-nothing/)包括了行人安全的条款；而美国的安全法规则没有。下次去投票的时候，尤其是在地方选举中，值得记住这一点，因为当选官员实际上能影响你所在城市的街道。

成为 Medium 会员以获取成千上万作家的故事！
