# 最困难的部分：定义分类目标

> 原文：[https://towardsdatascience.com/the-hardest-part-defining-a-target-for-classification-50c34d37e0b8?source=collection_archive---------8-----------------------#2023-11-14](https://towardsdatascience.com/the-hardest-part-defining-a-target-for-classification-50c34d37e0b8?source=collection_archive---------8-----------------------#2023-11-14)

## 在你的生产数据库中，它并没有标记为‘Target_Variable’！

[](https://medium.com/@chrisb-maven?source=post_page-----50c34d37e0b8--------------------------------)[![Chris Bruehl](../Images/72156a3ad97687e0ae9b7fd89f8f915e.png)](https://medium.com/@chrisb-maven?source=post_page-----50c34d37e0b8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----50c34d37e0b8--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----50c34d37e0b8--------------------------------) [Chris Bruehl](https://medium.com/@chrisb-maven?source=post_page-----50c34d37e0b8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8ade87570fa3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-hardest-part-defining-a-target-for-classification-50c34d37e0b8&user=Chris+Bruehl&userId=8ade87570fa3&source=post_page-8ade87570fa3----50c34d37e0b8---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----50c34d37e0b8--------------------------------) ·6分钟阅读·2023年11月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F50c34d37e0b8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-hardest-part-defining-a-target-for-classification-50c34d37e0b8&user=Chris+Bruehl&userId=8ade87570fa3&source=-----50c34d37e0b8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F50c34d37e0b8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-hardest-part-defining-a-target-for-classification-50c34d37e0b8&source=-----50c34d37e0b8---------------------bookmark_footer-----------)![](../Images/5125330cb509fd77f1424608c3f02fb1.png)

图片由 [Kenny Eliason](https://unsplash.com/@neonbrand?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 什么是目标变量？

目标变量是你尝试通过监督机器学习模型进行预测的变量或指标。它也通常被称为因变量、响应变量、‘y’变量，甚至只是模型输出。

无论你喜欢哪个术语（或在统计课上被迫使用哪个术语），这是任何监督建模项目中最重要的变量。虽然对于大多数有机器学习经验的人来说，这显而易见——但对于新手来说，值得重申原因。

从技术角度看，目标变量的数据类型决定了你正在进行的建模项目的类型。数值目标变量属于回归模型的范畴，而分类变量意味着你正在进行分类模型的工作。

![](../Images/5710ef981a1c11b75751c3f24f491b6d.png)

带有数值目标变量的回归模型——来自我的回归课程

但比模型类型更重要的是，你的目标变量是你构建模型的全部原因。

![](../Images/0978929564c7e063b567d6b8911fadf9.png)

一个（非常）简单的决策树，预测客户是否会流失（停止成为客户）——来自 Maven 的分类课程

# **定义分类的目标变量**

表面上，定义一个用于分类的目标变量似乎很简单。但如果你被要求为从未建模过的数据构建模型，你的看法可能会改变。

当人们开始学习机器学习时，他们通常会得到相对干净的数据集，这些数据集有明确的 0 和 1 可以用于分类建模。但根据我的经验，在关系型数据库中找到一个完全符合你最终目标变量的列是极其罕见的。

当然，这在学习机器学习算法的过程中是完全有意义的，但当我看到生成第一个“真实生活”建模数据集的 SQL 查询时，我确实经历了一次严酷的觉醒，这是我在第一个数据科学家工作中有机会接触到的。

让我们看看在生成目标变量的过程中，针对订阅产品的客户流失的情况下，我们可能需要对数据进行哪些转换。这可能是保险单、Netflix 订阅等。我创建了一些模拟的数据，模仿了我在实际工作中看到的数据，但如果它与数据工程师构建这些表格的方式不符，还请见谅。

# 示例 1：取消日期

![](../Images/fd3b1fc5fca210f551ce8f0eb7f19c80.png)

没有明确的 1 或 0 指示客户是否流失（订阅在此结束），但在这个例子中，工程化一个这样的指标非常简单。如果 `subscription_end_date` 为 null，我们可能可以假设客户仍然活跃。因此，我们可以应用一些基本逻辑，如果订阅结束日期不为 null，则赋值 1，如果为 null，则赋值 0，这样我们目标变量的正类就是流失客户。

![](../Images/47cc89a573dc2a681129116d42918116.png)

够简单了。现在我们来看一个更复杂的例子。

# 示例 2：续订日期

![](../Images/c3c1e34572257d65d2f584383069eef6.png)

现在假设我们在订阅表中有另一列。如果客户再次成为付费订阅者，我们有一列来记录这一点发生的时间。

我们的第一位客户在订阅结束后从未回来，所以我们可以放心地说他们已经流失了。

那么客户4呢？他们在订阅结束后的一天重新订阅了。他们可能在订阅结束时有一个月度订阅，或者信用卡过期了，一旦他们意识到这一点，就立即重新激活了他们的订阅。一天的会员空档足以说明这个客户已经流失了吗？可能不够。但如果是7天或30天的空档呢？

客户5确实重新订阅了，但他们已经有一年多没有成为客户了。可以公平地说他们已经流失了，但在这里与利益相关者和主题专家沟通至关重要。我们可能会确定一个逻辑，将流失者定义为已经中断超过30天的人，最终得到以下结果：

![](../Images/cc5c6a89a73697611d57a6c3591e939d.png)

这不是一个微不足道的区别。你的目标定义可能会对哪些特征能成功预测你的目标、模型的整体准确性以及最终项目的成功产生下游影响。

例如，如果客户4是我们平台上的一个非常活跃的用户（他们在失去访问权限时立即注意到），而其他两个客户却不是，那么与流失相关的预测特征的预测能力将会被我们糟糕的目标定义所削弱。

我考虑过添加更多示例，但我认为这两个示例已经很好地阐明了更广泛的观点。作为思考的材料，当我们有交易表，其中每个月的付款都有一行记录，不同的订阅类型和长度，因促销获得免费订阅的客户（我们可能希望完全排除他们），等等，目标创建的逻辑更容易出错。

# 一点建议

你的模型的目标不仅仅是0或1，它代表了你分类建模项目的成功基础单元。因此，我有几点通用建议来帮助你在下一个项目中定义目标变量：

+   你的目标变量可能不会在表格中干净利索地定义给你。

+   花时间，至少几天甚至几周，认真思考如何从手头的数据中定义你的目标变量，以及这种定义如何与业务目标相关。这通常不是一个微不足道的任务，事先投入时间和精力会比后来需要改变定义要好得多。

+   在你的组织中，目标变量的定义可能在概念上并没有达成一致。“流失”可以有多种定义方式，你可能永远无法得到一个完美的定义。但作为建模者，通过与相关利益方沟通，达成对你定义的共识是很重要的。至少，这样你可以在定义需要更改的情况下保护自己的利益，而最好的是，你为构建成功的模型奠定了基础。

感谢你抽出时间阅读我的想法——我知道对于经验丰富的数据科学家来说，这些可能并不突破常规，但我希望刚入门这个领域的人能够意识到课堂与实际工作之间的差距，并理解目标变量不应被忽视——尽管有时会想尽快进入建模阶段，但如果你这样做，可能会错过…目标。

*如果你喜欢这篇文章，请关注我！我定期撰写关于Python、Pandas以及从数据分析到数据科学转型的文章。我还在Maven Analytics平台和Udemy上提供了相关课程——希望能在那里见到你！*
