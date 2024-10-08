# 这就是为什么你从数据中提取价值的努力没有进展

> 原文：[`towardsdatascience.com/heres-why-your-efforts-extract-value-from-data-are-going-nowhere-8e4ffacbdbc0`](https://towardsdatascience.com/heres-why-your-efforts-extract-value-from-data-are-going-nowhere-8e4ffacbdbc0)

## 数据驱动的领导力与职业生涯

## 行业内对数据设计和数据质量的忽视（以及你可以做些什么）

[](https://kozyrkov.medium.com/?source=post_page-----8e4ffacbdbc0--------------------------------)![Cassie Kozyrkov](https://kozyrkov.medium.com/?source=post_page-----8e4ffacbdbc0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8e4ffacbdbc0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8e4ffacbdbc0--------------------------------) [Cassie Kozyrkov](https://kozyrkov.medium.com/?source=post_page-----8e4ffacbdbc0--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----8e4ffacbdbc0--------------------------------) ·8 分钟阅读·2023 年 2 月 25 日

--

我最喜欢的解释[数据科学](http://bit.ly/quaesita_datascim)和[数据工程](http://bit.ly/quaesita_dataeng)之间区别的方法是：

> 如果数据科学是“让数据有用”，那么数据工程就是“让数据可用”。

这些学科非常令人兴奋，以至于我们容易超前，忘记在我们能够使数据可用（更不用说有用）之前，我们首先需要*制作数据*。

> 那么“制作数据”本身呢？

制作优质数据的艺术被严重忽视了。如果你没有数据——没有输入——可以使用，那么你的数据工程师和数据科学家也帮不了你太多。

即使你确实有一些数据，也有可能你忽视了一个重要因素：数据质量。如果你收集的是非常糟糕的数据，忘记从中提取价值吧。与自然界基本定律“垃圾进，垃圾出”作斗争是徒劳的。

![](img/971d090f635d0e38e39ee5b6c846f2fe.png)

作者从文章“[为什么企业在机器学习上失败](http://bit.ly/quaesita_failm)”中引出的人工智能类比

数据在数据科学和人工智能中的角色，就像食材在烹饪中的角色一样。即使你有一个现代化的厨房，也无法拯救你；如果你的食材是垃圾，那你最好放弃。不管你如何切割和处理，你都不可能做出有价值的东西。这就是为什么在你急于进入项目之前，*首先*需要考虑投资于优质数据。

> 如果你关心结果，在追求华丽的算法、模型和一系列数据科学家之前，先投资于优质数据。

![](img/71569c0e6eac60a1b83421c628d50b00.png)

说到垃圾进垃圾出，你的作者进入这个地方后出来时一模一样。¯\_(ツ)_/¯

让我对你做个小小的猜测，亲爱的读者：你对垃圾进垃圾出（GIGO）并不陌生。或者对于那些性格乐观、看到半杯水的人（Q 是指质量），你可能更喜欢称之为 QIQO。你实际上在迫切希望我说一些你没听过的内容，但我仍在以 GIGO 的讨论磨练你的耐性。[再一次](http://bit.ly/quaesita_philadelphia)。是的，我们都已经厌倦了重复 GIGO 原则。我和你一样厌倦。

但请解答我这个难题。如果我们拥有一整支尊重垃圾进垃圾出（GIGO）原则的行业，同时我们也明白设计高质量数据集[并非易事](http://bit.ly/quaesita_srstrees2)，那我们有什么证据表明我们把钱投入到了我们口中的理念中呢？

如果数据质量如此显而易见地重要——毕竟，它是整个数十亿美元[数据/人工智能/机器学习/统计/分析](http://bit.ly/quaesita_universe)大工程的基础——那我们称负责这方面的专业人员为什么呢？这不是一个难题。我只想知道的是：

> 负责高质量数据集设计、收集、策划和文档工作的人的*职位名称*是什么？

不幸的是，这可能也是一个技巧性问题。每当我在会议上与数据专家聊天时，我都会尝试把这个问题偷偷问进去。而每次我问他们谁负责他们组织中的数据质量时，他们从未给出任何接近共识的答案。谁的工作是？数据工程师说是数据工程师，统计学家说是统计学家，研究人员说是研究人员，用户体验设计师说是用户体验设计师，产品经理说是产品经理……的确，GIGO 反复出现。数据质量似乎正是那种“每个人的工作”最终却变成没人做的工作，因为它需要技能（！），但似乎没有人故意在投资这些技能，更不用说分享最佳实践了。

> 数据质量正是那种“每个人的工作”最终却变成没人做的工作。

也许我对数据科学职业的关注稍显过多。如果我只是为了自己的职业发展，我会通过[数据江湖术](http://bit.ly/quaesita_charlatan)快速赚钱，但我希望数据职业总体上能有所意义，值得被重视，发挥作用，使世界变得比我们发现时更好。因此，当我看到两个最重要的前提条件（数据质量和[数据领导力](http://bit.ly/quaesita_dsleaders)）被忽视时，我感到心碎。

如果{*数据质量专业人员 / 数据设计师 / 数据策展人 / 数据收集员 / 数据管家 / 数据集工程师 / 数据卓越专家*}这个职业甚至没有名字（看到了吧？）或社区，那么在简历或大学课程中找不到它也就不足为奇了。你的招聘人员会使用什么关键字来搜索候选人？你会用什么面试问题来筛选核心技能？祝好运找到卓越——你的候选人将需要一整套丰富的技能。

> 你的招聘人员会使用什么关键字来搜索候选人？你会用什么面试问题来筛选核心技能？

首先，我们要认识到我们不是在谈论你小表弟的“数据标注”暑期工作，这种工作涉及无脑的数据录入和/或从一堆烘焙缩略图中选择所有的杯子蛋糕照片和/或逐门逐户进行纸质调查。我提到这一点是因为“这不就是数据标注吗？”这是我被多次以礼貌的关切语气问到的问题。真是低估了整个天才领域。

> “这不就是数据标注吗？”不是的。（真是低估了整个天才领域。）

不，我们说的是那种最初设计[数据收集过程](http://bit.ly/quaesita_srstrees2)的人。它至少需要一点用户体验设计，一点[决策科学](http://bit.ly/quaesita_di)，一点调查设计经验，一些心理学，一点有实地经验的实验社会科学（任何有实际经验的人都会在睡梦中预见[费城问题](http://bit.ly/quaesita_philadelphia)），以及一块[统计学](http://bit.ly/quaesita_statistics)培训（尽管你不需要整一个统计学家），再加上扎实的[分析](http://bit.ly/quaesita_careeranalyst)经验，大量的领域专业知识，一些项目/程序管理技能，一点数据产品管理经验，以及足够的[数据工程](http://bit.ly/quaesita_dataeng)背景来考虑大规模的数据收集。这是一种稀有的融合——我们迫切需要一种新的专业化。

> 要有任何希望建立成熟的数据生态系统，我们必须为新一代专家提供一个良好的环境，让他们的专业技能得到认可。

但在我们争取一个得到良好认可、良好管理和良好回报的数据制作职业之前，我们将陷入困境。那些对这一系列技能有天赋的新兴高手如果贸然投入其中，就像是跳入悬崖。现在，这几乎是个在地下室工作的职位，如果它还能算作工作的话。要有任何希望建立成熟的数据生态系统，我们***必须***为新一代专家提供一个良好的环境，让他们的专业技能得到认可。

那么*你*能做什么？

# 认可你的专家

如果你的组织中已经有那些有这些技能和才华的人，尽管历史上被忽视，他们仍在积极承担数据质量工作，你是否在鼓励他们？你是否在培养他们？你是否在奖励他们？我希望你是在。相反，如果你在创建奖励机制来追逐引人注目的[MLOps](http://bit.ly/mfml_078)或带有博士头衔的[数据科学](http://bit.ly/mfml_036)，你是在自毁（也是在害我们整个行业）。

# 贡献知识分享

Google 的 People + AI Research (PAIR)团队最近发布了[数据卡片手册](https://bit.ly/datacardsplaybook)，以帮助培训社区进行数据设计、数据透明度、数据质量和数据文档最佳实践。我对我们的工作感到非常自豪，并且很高兴这些材料可以免费供大家使用，但还有很多需要学习的东西。如果你也在这条路上，并且热情支持数据卓越，请与世界分享你正在学习的课程。

![](img/7ee296d49fe11a4a53666ca819c6c63e.png)

获取它：[bit.ly/datacardsplaybook](https://bit.ly/datacardsplaybook)（图片由[Mahima Pushkarna](https://www.linkedin.com/in/mahimapushkarna/)，手册共同创作者，已获得许可）

# 告诉别人

如果一篇研究论文在森林里落下而没有人使用，它会发出声音吗？从好点子到建立一个卓越的学科是一个漫长的旅程……一个需要所有鼓励和宣传的旅程。如果你相信这一点，并且你能激励甚至一个人认真对待，你将为构建未来发挥重要作用。谢谢你提前传播这个消息。

# 摘要

我们的社区在庆祝[数据科学家](https://bit.ly/quaesita_roles)方面做得很好。我们在庆祝[MLOps](http://bit.ly/mfml_078)和[数据工程师](https://bit.ly/quaesita_dataeng)方面做得不错。但我们在庆祝所有其他数据职业依赖的那些人方面做得很差：那些设计数据收集并负责数据卓越、文档和策展的人。也许我们可以从命名他们（我很乐意听听你的建议）开始，并至少承认他们的重要性。从这里开始，我们会进步到培训他们、雇用他们并欣赏他们的专业技能吗？我真的希望如此。

# 感谢阅读！怎么样来一个 YouTube 课程？

如果你在这里玩得很开心，并且你正在寻找一个为初学者和专家设计的完整应用 AI 课程，这里有一个我为你的娱乐而制作的课程：

在 YouTube 上享受这个课程[这里](https://bit.ly/funaicourse)。

*P.S. 你是否尝试过多次点击 Medium 上的拍手按钮看看会发生什么？* ❤️

# 寻找动手的 ML/AI 教程？

这里有一些我最喜欢的 10 分钟教程：

+   [Vertex AI](https://bit.ly/kozvertex)

+   [AutoML](https://console.cloud.google.com/?walkthrough_id=automl_quickstart)

+   [AI 笔记本](https://bit.ly/kozvertexnotebooks)

+   [用于表格数据的 ML](https://bit.ly/kozvertextables)

+   [文本分类](https://bit.ly/kozvertextext)

+   [图像分类](https://bit.ly/kozverteximage)

+   [视频分类](https://bit.ly/kozvertexvideo)

# 别忘了访问数据卡片手册！

![](img/7ee296d49fe11a4a53666ca819c6c63e.png)

获取链接：[bit.ly/datacardsplaybook](https://bit.ly/datacardsplaybook)（图片来源：[Mahima Pushkarna](https://www.linkedin.com/in/mahimapushkarna/)，手册共同创作者，经授权使用）

虽然该网站强调数据文档和 AI（要抓住那个时代精神），但[数据卡片手册](https://bit.ly/datacardsplaybook)远不止于此。这是我所知道的最强大的通用数据设计资源集合。预览：

![](img/949db4e58082043130a30ca99dc6ca0e.png)

获取链接：[bit.ly/datacardsplaybook](https://bit.ly/datacardsplaybook)（图片来源：[Mahima Pushkarna](https://www.linkedin.com/in/mahimapushkarna/)，手册共同创作者，经授权使用）

# 感谢阅读！怎么样，一门课程？

如果你在这里玩得开心，并且你在寻找一个有趣的、旨在愉悦 AI 初学者和专家的领导力导向课程，[这是我为你准备的小东西](https://bit.ly/funaicourse)：

![](img/300b5280620ea948fc3dbffb708084d4.png)

课程链接：[`bit.ly/funaicourse`](https://bit.ly/funaicourse)

[](https://kozyrkov.medium.com/membership?source=post_page-----8e4ffacbdbc0--------------------------------) [## 加入 Medium

### 阅读 Cassie Kozyrkov（以及 Medium 上的其他数千位作者）的每一个故事。你的会员费直接支持…

[kozyrkov.medium.com](https://kozyrkov.medium.com/membership?source=post_page-----8e4ffacbdbc0--------------------------------)

*P.S. 你是否尝试过在 Medium 上多次点击“点赞”按钮以查看会发生什么？* ❤️

# 喜欢作者？与 Cassie Kozyrkov 联系

成为朋友吧！你可以在[Twitter](https://twitter.com/quaesita)、[YouTube](https://www.youtube.com/channel/UCbOX--VOebPe-MMRkatFRxw)、[Substack](http://decision.substack.com)和[LinkedIn](https://www.linkedin.com/in/kozyrkov/)找到我。想让我在你的活动中演讲吗？使用[这个表格](http://bit.ly/makecassietalk)联系我。
