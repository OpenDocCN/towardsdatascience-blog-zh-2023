# 从数据湖到数据网：最新企业数据架构指南

> 原文：[`towardsdatascience.com/from-data-lakes-to-data-mesh-a-guide-to-the-latest-enterprise-data-architecture-d7a266a3bc33`](https://towardsdatascience.com/from-data-lakes-to-data-mesh-a-guide-to-the-latest-enterprise-data-architecture-d7a266a3bc33)

## 了解为什么大型公司正在接受数据网

[](https://col-jung.medium.com/?source=post_page-----d7a266a3bc33--------------------------------)![Col Jung](https://col-jung.medium.com/?source=post_page-----d7a266a3bc33--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d7a266a3bc33--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d7a266a3bc33--------------------------------) [Col Jung](https://col-jung.medium.com/?source=post_page-----d7a266a3bc33--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----d7a266a3bc33--------------------------------) ·阅读时间 17 分钟·2023 年 5 月 30 日

--

![](img/d3b6f86acb42601cc9493acc18a92e28.png)

图片来源：[takahiro takuchi](https://unsplash.com/photos/McmRfLKw8yg) (Unsplash)

全球大型组织正在经历重大‘数据地震’。

这是公司数据湖向**数据网**的***去中心化***。

在我曾经在澳大利亚‘四大’银行之一从事分析工作的半个十年中，我们正处于一次巨大的转型旅程中，同时构建多个重要基础设施项目：

+   向[云原生](https://generativeai.pub/cloud-computing-unleashed-how-to-harness-the-power-of-cloud-for-your-business-f72e8e23be9)数据平台迁移至 Azure PaaS；

+   一系列[战略数据产品](https://generativeai.pub/data-products-why-your-organisation-needs-them-4ac7bf2e5953)的构建；

+   我们的数据湖融合成**去中心化的数据网**。

不过我得承认……这对我们可怜的老数据分析师和数据科学家来说有点破坏性。

想象一下在房间装修的时候尝试做书法。

**更新**：我现在在[YouTube](https://www.youtube.com/@col_builds)发布分析内容。

数据湖——数据科学家喜欢从中“饮水”的热门目的地——目前正像面团一样被拆解，并作为网的一部分被分配到各个业务领域。

一些数据科学家带着好奇心观看；其他人则因沮丧而叹息；还有些人则**激动得难以置信**。

为什么？

因为数据网格承诺成为一个**真正可扩展的数据平台**，其中**数据被视为一等公民**。数据科学家将可以访问可发现、可靠且可重用的数据资产，这些资产可以在公司内不同的业务领域之间无缝共享。

正如人们所说的，短期痛苦换取[长期收益](https://generativeai.pub/modern-enterprise-data-strategy-a-guide-for-analysts-data-scientists-engineers-2d4b45a31427)。

在这篇文章中，我将深入探讨如何……

+   数据湖成为了**瓶颈**；

+   为什么组织现在**去中心化**他们的数据湖，转向**数据网格**；

+   如何在公司中**构建**一个网状基础设施。

# 1\. 数据湖的简史

企业数据环境在过去十年里发展得*非常*快。

在 2010 年代中期，**数据湖**在全球范围内开始流行。这个概念实际上已经存在了几十年，但在这一时期，构建这些集中式数据存储庞物所需的技术才变得可行。

时机也相当好。

智能手机、*物联网*（IoT）、数字和社交媒体以及电子商务的爆炸式增长促成了**大数据**的兴起，随着这一趋势，组织迫切需要存储大量非结构化数据，并利用数据分析和[机器学习](https://medium.com/geekculture/ai-revolution-your-fast-paced-introduction-to-machine-learning-914ce9b6ddf)从中提取见解。

数据湖提供了一种可扩展和灵活的解决方案，无需预定义模式，不同于数据仓库。

![](img/cb36ea79b2c8937ac4541bc6971c46df.png)

数据科学、机器学习和大数据的交集。作者图片

那么运行在这些之上的软件呢？

进入**Apache Hadoop**，一个开源框架，提供了分布式存储（*HDFS*）和分布式计算（*MapReduce*）功能，用于处理湖中的大数据。

Hadoop 的起源可以追溯到 2000 年代初期，Yahoo! 研究人员发表的一对[开创性](https://research.google/pubs/pub51/) [论文](https://research.google/pubs/pub62/)。到 2010 年，Facebook 自豪地拥有全球最大的 Hadoop 集群，储存量达 21 [PB](http://hadoopblog.blogspot.com/2010/05/facebook-has-worlds-largest-hadoop.html)。

几年后，半数《财富》50 强公司已经采用了这一框架。

Hadoop 通过使用廉价的消费级机器（商品硬件）和大数据处理能力，实现了成本效益高的存储，这使其成为全球寻求构建数据湖的组织的有吸引力的选择。

![](img/dcfc77fe219530962c0738a3c387b421.png)

自 2010 年代中期以来，基于大数据的预测模型迅猛增长。图像来源于作者

接下来是**云计算**，它在 2015 到 2020 年间 [迅速崛起](https://generativeai.pub/cloud-computing-unleashed-how-to-harness-the-power-of-cloud-for-your-business-f72e8e23be9)。

像*Amazon Web Services* (AWS)、*Microsoft Azure* 和 *Google Cloud Platform* (GCP) 这样的巨头提供了可扩展的存储解决方案，如 Amazon S3、Azure Data Lake Storage 和 Google Cloud Storage。

结果是，许多组织将其本地的数据湖迁移到云上，使它们能够灵活适应变化的工作负载，随心所欲地扩展和收缩，只为实际使用的部分付费。

微软现在甚至提供 SaaS 云分析平台，将数据仓库、大数据、数据工程和数据管理 [汇聚在一个平台下](https://generativeai.pub/azure-synapse-analytics-in-action-7-real-world-use-cases-explored-c73ef231b408)。

这不是美好时光吗？

嗯……

# 2\. 数据湖怪兽

架构师和数据网格发明者 Zhamek Dehghani [浓缩](https://martinfowler.com/articles/data-monolith-to-mesh.html)了企业数据平台的历史为三个阶段：

> **第一代**：*专有企业数据仓库和商业智能*平台；这些解决方案价格昂贵，使得公司背负了大量技术债务，涉及成千上万无法维护的 ETL 任务，以及只有少数专业人员理解的表格和报告，导致对业务的积极影响未能完全实现。
> 
> **第二代**：*以数据湖为银弹的大数据生态系统*；由中心团队的超专业数据工程师操作的复杂大数据生态系统和长期运行的批处理作业创造了**数据湖怪兽**，这些怪兽在最好的情况下仅能支持少量的研发分析；承诺过高而实现不足。
> 
> **第三代**：或多或少与上一代相似，但现代化地向实时数据可用性进行流式处理，统一批处理和流处理以进行数据转换，并完全拥抱基于云的托管服务来处理存储、数据管道执行引擎和机器学习平台。

她的言辞确实很直白。

数十年的数据仓库使组织 [沉溺于](https://from-data-warehouses-and-lakes-to-data-mesh-a-guide-to-enterprise-data-architecture-e2d93b2466b1)一个由混乱数据管道连接的数据系统的海洋中。魔法解决方案原本是将数据集中到一个中央存储库中。不幸的是，数据湖梦想在许多组织中变成了数据湖的*沼泽*。

充满了大量未被利用的数据和未解决的数据质量问题，水变得*陈旧*。

过度集中任何事物可能会打开一个[麻烦缸](https://en.wikipedia.org/wiki/2007%E2%80%932008_financial_crisis)，通常在一段动荡时期后，会导致剧烈的回归去中心化。我们在各个方面的人类社会中都可以看到这一点：

+   商业 → 公司趋向于更水平的层级结构；

+   财务 → [去中心化资产](https://medium.com/geekculture/understanding-why-crypto-exists-677a32b4e3bb)的兴起，如比特币，[以太坊和索拉纳](https://medium.datadriveninvestor.com/solana-polygon-6-killer-use-cases-9687b70487e0)；

+   经济学 → 计划经济采用资本主义；

+   政府 → 决定性的政变或人民的革命。

企业数据经历了其**数据湖实验**形式的过度集中化。

这是 Dehghani [描述](https://martinfowler.com/articles/data-monolith-to-mesh.html)的后果，称之为‘失败模式’。哎呦。

## 问题 1 — 集中化的难题！

我们得到一个**集中化的** **领域无关的多面手**数据平台，由一个过度劳累的集中化数据团队管理，他们并不是他们处理的数据的专家。

这个高超的数据工程团队被期望从***所有***企业角落和***所有***业务领域中摄取操作和交易数据。

他们还需要清洗、丰富和转换数据，以满足**多样化的消费者**的需求，如 仪表板 和 [报告](https://medium.com/swlh/power-of-storytelling-in-business-data-analytics-your-data-is-only-half-the-story-f50fadf9712b)的数据分析师，以及 [建模](https://medium.com/geekculture/ai-revolution-your-fast-paced-introduction-to-machine-learning-914ce9b6ddf)的数据科学家。

哎呀！这要求太高了。

![](img/367ba27cca61fb548aa1fe531b76eaf0.png)

数据湖的 ETL 管道。这些管道由一个集中化的数据团队构建。来源：Z. Dehghani 在 [MartinFowler.com](https://martinfowler.com/articles/data-mesh-principles.html)（已获许可）

随着全球组织[竞相成为数据驱动的组织](https://generativeai.pub/modern-enterprise-data-strategy-a-guide-for-analysts-data-scientists-engineers-2d4b45a31427)以保持竞争力，处理来自多个业务领域决策者的所有分析问题的需求落在了这个集中化团队的灵活性上。

这成为了一个重大问题。

这些倒霉的数据工程师没有时间。

对操作数据库进行调整后，留下了一条通往集中化数据湖的破损 ETL 管道。

团队面临不断的管道待修补，几乎没有时间专注于理解他们从整个组织中提取或推送给数据科学家的特定领域数据。

![](img/3eaed1c1cd3344cebb8fb90c84c5b565.png)

中央数据团队成为了瓶颈。来源: [数据网格架构](https://www.datamesh-architecture.com/)（经许可使用）

结果——在一些初步的快速成功之后，全球的中央数据团队遇到了巨大的可扩展性问题，成为组织敏捷性的瓶颈。

总结来说——就像一场狂野的派对，数据仓库像五彩纸屑一样传播技术债务，让*每个人*及其宠物仓鼠创建数据管道。

然后数据湖抢占了风头，通过将数据管道的创建压缩到公司内的一个单点，策划了一场盛大的集中化表演。

现在的公司发现自己不得不通过中央数据团队的小管道来满足整个分析师、数据科学家和经理队伍的无限数据需求。

## *要点：*

也许**去中心化**数据湖到**领域特定团队**可能是值得追求的理想位置？

## 问题 2 — 迟钝的操作模型

Dehghani [描述](https://martinfowler.com/articles/data-monolith-to-mesh.html)了第二个问题，即**耦合管道分解**，其中数据湖……

> “……在管道的各个阶段之间有较高的耦合，以交付独立的功能或价值。它是正交分解到变化的轴线的。”

这不容易理解，除非你是一个建筑领域的专家，所以让我用一个个人的例子来说明。

几年前，我工作的银行将整个组织结构从**职能**操作模型转变为**业务线**模型。

这是什么意思？我将通过关注抵押贷款销售表现不佳来说明——这是任何传统银行的核心业务。

在职能模型下，有人负责抵押贷款产品开发，有人负责分销和销售，还有人负责法律、风险和合规等事务。

![](img/2285c411e402eca44cf024563e6462b2.png)

图片由作者提供

这导致了对不佳表现缺乏问责，因为这些职能高度耦合，需要这些跨职能团队彼此协作，以交付端到端的抵押贷款产品。

换句话说，变化的轴线是**产品**的方向：抵押贷款、信用卡、商业贷款等，但工作管道却是**正交**分解的，将每个产品切分为一堆高度耦合的职能。

很糟糕，很糟糕，很糟糕。

在新的业务部门模型下，一位**高级主管**负责抵押贷款业务。一位高级主管负责信用卡。一位高级主管负责商业贷款。

![](img/87d7ab7cd01e5be476d0c0cf7225f4f5.png)

图片由作者提供

现在，管道与变化的轴线**平行**。方向舵和船只现在已经对齐。

这驱动了：

+   **问责制** — 如果抵押贷款表现不佳，C-Suite 先生没有奖金。

+   **灵活性** — 抵押贷款业务可以迅速围绕新项目进行重组和创新。变化更容易、更快。

+   **性能** — 灵活性和创新带来更快交付的更好产品和服务。

回到数据湖。

结果是，他们也本质上使用了一种功能性的方法来组织工作，其中数据管道被分解为**处理阶段**，如 *源头*、*摄取*、*处理* 和 *服务* 数据。

![](img/4eb50bc195b82ad3b5cb3f9ebb5a182b.png)

数据管道中的处理阶段。来源：Z. Dehghani 于 [MartinFowler.com](https://martinfowler.com/articles/data-monolith-to-mesh.html)（已授权）

就像我的银行例子一样，这些处理阶段是高度耦合的。

那么，正在构建信用评分模型的数据科学团队是否突然需要不同的数据？这意味着需要处理不同的数据，这可能意味着需要获取和摄取新的数据。

这意味着我们集中化的数据团队需要管理大量不断演变的依赖关系，从而导致数据交付变慢且缺乏灵活性。

实际上，这些依赖关系意味着整个数据湖中的管道是***最小的*变更单元**，必须进行修改以适应新功能。

因此，我们说数据湖是**单体**数据平台。

这实际上是一个难以更改和升级的大块内容，正如 Dehghani [主张](https://martinfowler.com/articles/data-monolith-to-mesh.html)：

> “…限制了我们在响应新消费者或数据源时实现更高速度和规模的能力。”

## 要点：

我们是否可以通过将数据湖去中心化为更**模块化**的架构来解决这个问题，并让**业务领域对其数据承担端到端责任**？

## 问题 3 — 围栏投掷

Dehghani 称第三种也是最后一种失败模式为**隔离和超专业化的所有权**，我认为这导致了无效的**围栏投掷**。

我们在数据湖工作的超专业大数据工程师与数据的来源和消费地点组织上是隔离的。

![](img/0cf8f86a7173e0da8564bafdb9ba3ed9.png)

隔离的超专业化数据平台团队。来源：Z. Dehghani 于 [MartinFowler.com](https://martinfowler.com/articles/data-monolith-to-mesh.html)（已授权）

这创造了一个不良的激励结构，无法促进良好的交付成果。Dehghani 表达为……

> “我个人不羡慕数据平台工程师的生活。他们需要从没有提供有意义、真实和正确数据的团队那里获取数据。这些团队对生成数据的源领域了解甚少，缺乏领域专业知识。他们需要为多样化的需求（无论是操作性的还是分析性的）提供数据，但对数据的应用和访问消费领域专家了解不多。
> 
> 我们发现的是断开的源团队，沮丧的消费者争夺数据平台团队待办事项上的位置，以及过度扩展的数据平台团队。”

数据生产者将‘打包’一些数据，并将其抛给数据工程师。

现在是你们的问题了！祝大家好运！

超负荷的数据显示，数据工程师可能没有公正地处理摄取的数据，因为他们不是数据领域专家，他们会将一些处理过的数据从数据湖中抛出，以服务下游消费者。

祝好运，分析师和数据科学家！是时候小憩一下，然后我将去修复我待办事项上的五十个破损的 ETL 管道。

从问题 2 和 3 可以看出，数据湖实验中出现的挑战既是**组织性的**也是技术性的。

## 要点：

通过将数据管理下放到各个业务领域，也许我们可以促进数据所有权和协作的文化，并**赋予数据生产者、工程师和消费者共同合作的能力**。

对了，我们能否让这些领域真正参与进来？

通过激励他们**以数据作为热销产品对待**，赋予他们**为建立战略数据资产感到自豪**的动力。

喜欢这个故事吗？当我发布类似文章时，请获取一个[邮件](https://col-jung.medium.com/subscribe)。

# 3\. 介绍……数据网格！

在 2019 年，Dehghani 提出了数据网格作为下一代数据架构，倡导去中心化的数据管理方法。

她的初始文章——[这里](https://martinfowler.com/articles/data-monolith-to-mesh.html)和[这里](https://martinfowler.com/articles/data-mesh-principles.html)——在企业数据社区中引起了广泛关注，这促使许多全球组织开始了自己的数据网格之旅，包括我的。

数据网格不是将数据泵送到集中式数据湖，而是将数据所有权和处理下放到**特定领域的团队**，这些团队控制和交付**数据产品**，促进数据在整个组织中的易访问性和互联互通，加快决策速度并促进创新。

![](img/b9c71a8fbf0eadbe1f7c8a0dbf715c3c.png)

数据网格概述。来源：[数据网格架构](https://www.datamesh-architecture.com/)（经许可）

数据网格的梦想是[创建一个基础](https://martinfowler.com/articles/data-mesh-principles.html)，以**规模化**提取分析数据的价值，规模化的应用包括：

+   不断变化的业务、数据和技术环境。

+   数据生产者和消费者的增长。

+   多样化的数据处理需求。各种使用案例要求多样化的工具进行转换和处理。例如，实时异常检测可能利用*Apache Kafka*；客户支持的 NLP 系统常常导致在像*NLTK*这样的 Python 包上进行数据科学原型设计，图像识别利用像*TensorFlow*和*PyTorch*这样的深度学习框架；我银行的欺诈检测团队希望使用*Apache Spark*处理我们的大数据。

所有这些需求都为数据仓库带来了技术债务（表现为大量无法维护的 ETL 作业），并且对数据湖造成了瓶颈（由于大量的多样化工作被挤压到一个小型集中数据团队中）。

组织最终会面临技术债务超过提供的价值的复杂性门槛。

这是一种糟糕的情况。

为了解决这些问题，Dehghani 提出了**四项原则**，任何数据网格的实施都必须体现这些原则，以实现规模、质量和可用性的承诺。

![](img/620c089e140902e5633bbfa21f527e44.png)

数据网格的四项原则。来源：[数据网格架构](https://www.datamesh-architecture.com/)（已获许可）

1.  **数据的领域所有权：** 通过将数据所有权交给领域特定的团队，你**赋能于离数据最近的人来负责**。这种方法提高了应对业务需求变化的敏捷性，并增强了利用数据驱动洞察的有效性，最终导致更好、更具创新性的产品和服务，且速度更快。

1.  **数据即产品：** 每个业务单元或领域都被赋予*产品思维*，以设计、拥有和改进高质量且可重用的**数据产品**——由数据的*生产者*视为产品的自包含且可访问的数据集。目标是发布和共享数据产品给其他领域的*消费者*——被视为网格上的**节点**——以便所有人都能利用这些战略数据资产。

1.  **自助数据平台：** 赋能用户自助能力为加速数据访问和探索铺平道路。通过提供一个用户友好的平台，配备必要的工具、资源和服务，你使团队能够在数据需求方面变得自给自足。数据的民主化促进了更快的决策制定和数据驱动的卓越文化。

1.  **联合治理：** 集中控制扼杀了创新并阻碍了敏捷性。联合的方法确保决策权分布在各个团队中，使他们在关键时刻能够自主决策。通过在控制与自主之间取得正确的平衡，你可以促进问责、协作和创新。

四个原则中，数据产品是最关键的。因此，我们常常看到公司同时执行他们的[数据产品战略](https://generativeai.pub/modern-enterprise-data-strategy-a-guide-for-analysts-data-scientists-engineers-2d4b45a31427)，并在各个业务领域分散他们的数据湖。

阅读我的*解释 101*，了解有关[数据产品](https://generativeai.pub/data-products-why-your-organisation-needs-them-4ac7bf2e5953)的所有详细信息。

关于执行数据网战略的话题…

# 4. 如何构建数据网

对于大多数公司来说，这段旅程不会是干净整洁的。

构建数据网不会是一个被 relegated 给一个隔离的工程团队在地下室里辛苦工作的任务，直到准备好部署。

你可能需要巧妙地逐步联合你现有的数据湖，直到你达到一个‘足够的数据网’的平台。

想象在飞行中将两个飞机引擎换成四个较小的引擎，而不是在一个阴凉的机库里建造一架新飞机。

或者尝试在保持部分车道通行的同时升级道路，而不是在附近铺设一条新的道路并在一切就绪时剪彩。

构建数据网是一个重大的任务，你需要**在所有阶段让业务共同参与**。因为最终将由业务领域来管理自己的端到端数据事务！

完全的数据网成熟可能需要很长时间，因为数据网主要是一种*组织*构造。

它同样涉及*运营模型*——换句话说，**人员和流程——**，以及技术本身，这意味着文化提升和带领人们一起前行是至关重要的。

你需要*教会*组织数据网的*价值*以及如何*使用*它。

正确处理你的策略，随着时间的推移，你的***集中式领域无关的单体数据湖***将转变为**去中心化领域导向的模块化数据网**。

设计阶段的一些考虑因素。查看 [datamesh-architecture.com](https://www.datamesh-architecture.com/) 以深入了解。

+   **领域。** 数据网格架构由一组业务领域组成，每个领域都有一个**领域数据团队**，能够自行进行跨领域的数据分析。一个**支持团队**——通常是组织的*转型*办公室的一部分——在整个组织中推广网格的理念，并作为倡导者。他们以咨询的方式帮助各个领域在成为数据网格的“完整成员”的过程中。支持团队将由数据架构、数据分析、数据工程和数据治理方面的专家组成。

+   **数据产品。** 各领域将摄取自己操作的数据——这些数据离他们很近，他们对此非常了解——并**构建作为数据产品的数据分析模型**，这些模型可以**发布到网络上**。数据产品由领域所有，领域[负责](https://generativeai.pub/data-products-why-your-organisation-needs-them-4ac7bf2e5953)其运营、质量和在整个生命周期中的提升。有效的问责制确保数据的有效性。

![](img/6c0cac78e24891725e23e99915675f4b.png)

数据产品在网格上的共享。来源：[数据网格架构](https://www.datamesh-architecture.com/)（经许可使用）

+   **自助服务。** 还记得学校里的‘多文化食物日’吗？那天每个人都带来自己美味的菜肴，并在自助餐桌上共享？老师的简约角色是监督操作，确保一切顺利进行。同样，网格中新精简的中央数据团队致力于提供和维护一个领域无关的多样数据产品的‘自助餐桌’，供大家自取。业务团队可以进行自己的分析，开销较小，并将自己的数据产品提供给同行。这是一场美味的数据盛宴，每个人也都可以是厨师。

+   **联邦治理**。每个领域将自行管理自己的数据，并有权按照自己的节奏行动——就像*欧盟*成员国一样。在某些方面，如果有必要团结和标准化，他们将与其他领域在**联邦治理小组**中达成协议，制定全球政策，如文档标准、互操作性和安全性——就像*欧洲议会*一样——以便各个领域能够轻松发现、理解、使用和整合在网格上可用的数据产品。

这里有激动人心的一点——我们的网格什么时候会达到**成熟**？

**当团队开始使用其他领域的数据产品时，网格就会显现出来。**

这作为一个有用的基准，旨在证明你的数据网格旅程已经达到了一个成熟的门槛水平。

这是庆祝的好时机！

# 5\. 最后的话

数据网格是一个相对较新的概念，约在 2018 年由架构师**扎梅克·德赫戈尼**发明。

随着越来越多的组织应对集中式数据湖的扩展性问题，这一理念在数据架构和分析社区中获得了显著的关注。

通过摆脱 **由单一团队控制数据的组织结构**，转向由使用数据最多的团队拥有和管理数据的去中心化模型，组织的不同部分可以独立工作——拥有更大的自主权和敏捷性——同时仍然确保数据的一致性、可靠性和良好治理。

数据网格提倡一种责任、所有权和协作的文化，其中 **数据被产品化并被视为一等公民**，以无缝且受控的方式在公司内部自豪地共享。

目标是实现一个真正可扩展且灵活的数据架构，以符合现代组织的数据中心驱动业务价值和创新的需求。

![](img/78ce16a43cdf3dea6c0224df26f55127.png)

总结数据网格的四个原则。来源：Z. Dehghani 于 [MartinFowler.com](https://martinfowler.com/articles/data-mesh-principles.html)（已获许可）

我公司自身的历程 预计需要几年时间完成主要迁移，完全成熟则需要更长时间。

我们正在同时处理三个主要部分：

+   **云。** 从我们在 *Microsoft Azure* 上的 *Cloudera* 堆栈的 **IaaS** 升级到 *Azure* **PaaS** 的原生云服务。更多信息请参见[这里](https://generativeai.pub/cloud-computing-unleashed-how-to-harness-the-power-of-cloud-for-your-business-f72e8e23be9)。

+   **数据产品。** 初始的一系列基础数据产品正在推出，可以像 Lego 积木一样在[不同组合](https://generativeai.pub/data-products-why-your-organisation-needs-them-4ac7bf2e5953)中使用和重新组合，形成更大、更有价值的数据产品。

+   **网格。** 我们将数据湖去中心化，目标是至少五个节点。

这真是一段奇妙的旅程。当我五年前开始时，我们才刚刚开始利用 Apache Hadoop 在本地基础设施上构建数据湖。

无数的挑战和宝贵的经验塑造了我们的旅程。

像任何决心坚定的团队一样，我们迅速失败并从失败中前进。五年短暂的时间里，我们已经完全转变了企业数据环境。

谁知道五年后的情况会是什么样子？我期待着。

在 [Twitter](https://twitter.com/col_jung) 和 YouTube [这里](https://youtube.com/@col_builds)、[这里](https://youtube.com/@col_invests) 以及 [这里](https://youtube.com/@col_shoots) 找到我。

# 我的人气 AI、ML 和数据科学文章

+   AI 和机器学习：快速入门 — [这里](https://col-jung.medium.com/ai-revolution-your-fast-paced-introduction-to-machine-learning-914ce9b6ddf)

+   机器学习与机械建模 — [这里](https://medium.com/swlh/differential-equations-versus-machine-learning-78c3c0615055)

+   数据科学：现代数据科学家的新技能 — 这里

+   生成式 AI：大公司如何争相采纳 — [这里](https://generativeai.pub/how-big-companies-are-scrambling-to-adopt-generative-ai-d52456fb4c69)

+   ChatGPT 与 GPT-4：OpenAI 如何赢得 NLU 战争 — [这里](https://col-jung.medium.com/the-road-to-chatgpt-gpt-4-how-deep-learning-revolutionised-natural-language-processing-835d89560577)

+   GenAI 艺术：DALL-E, Midjourney 与 Stable Diffusion 解析 — [这里](https://col-jung.medium.com/generative-ai-art-the-road-to-dall-e-midjourney-stable-diffusion-3b3219d97f02)

+   超越 ChatGPT：寻找真正智能的机器 — [这里](https://col-jung.medium.com/from-chatgpt-to-singularity-the-search-for-a-truly-intelligent-machine-856c8f4c5e63)

+   现代企业数据战略解析 — [这里](https://generativeai.pub/modern-enterprise-data-strategy-a-guide-for-analysts-data-scientists-engineers-2d4b45a31427)

+   从数据仓库和数据湖到数据网格 — 这里

+   从数据湖到数据网格：最新架构指南 — 这里

+   Azure Synapse Analytics 实战：7 个用例解析 — [这里](https://generativeai.pub/azure-synapse-analytics-in-action-7-real-world-use-cases-explored-c73ef231b408)

+   云计算 101：为您的业务利用云计算 — [这里](https://generativeai.pub/cloud-computing-unleashed-how-to-harness-the-power-of-cloud-for-your-business-f72e8e23be9)

+   数据仓库与数据建模 — 快速速成课程 — 这里

+   数据产品：为分析建立坚实的基础 — [这里](https://generativeai.pub/data-products-why-your-organisation-needs-them-4ac7bf2e5953)

+   数据民主化：5 种“数据普及”策略 — 这里

+   数据治理：分析师的 5 个常见痛点 — 这里

+   数据讲故事的力量 — 销售故事，而非数据 — [这里](https://medium.com/swlh/power-of-storytelling-in-business-data-analytics-your-data-is-only-half-the-story-f50fadf9712b)

+   数据分析入门：谷歌方法 — 这里

+   Power BI — 从数据建模到惊艳报告 — 这里

+   回归分析：使用 Python 预测房价 — 这里

+   分类：使用 Python 预测员工流失 — 这里

+   Python Jupyter Notebook 与 Dataiku DSS 的对比 — 点击这里

+   浅析流行的机器学习性能指标 — 点击这里

+   在 AWS 上构建 GenAI — 我的首次体验 — [点击这里](https://generativeai.pub/how-big-companies-are-scrambling-to-adopt-generative-ai-d52456fb4c69)

+   COVID-19 的数学建模与机器学习 — [点击这里](https://medium.com/swlh/math-modelling-and-machine-learning-for-covid-19-646efcbe024e)

+   工作的未来：在 AI 时代你的职业安全吗 — [点击这里](https://col-jung.medium.com/future-of-work-is-your-career-safe-in-the-age-of-chatgpt-gpt-4-122d5996bd57)
