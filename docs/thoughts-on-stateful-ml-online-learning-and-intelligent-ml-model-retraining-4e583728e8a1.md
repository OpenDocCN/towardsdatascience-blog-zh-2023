# 关于状态保持机器学习、在线学习和智能机器学习模型再训练的思考

> 原文：[https://towardsdatascience.com/thoughts-on-stateful-ml-online-learning-and-intelligent-ml-model-retraining-4e583728e8a1?source=collection_archive---------14-----------------------#2023-04-05](https://towardsdatascience.com/thoughts-on-stateful-ml-online-learning-and-intelligent-ml-model-retraining-4e583728e8a1?source=collection_archive---------14-----------------------#2023-04-05)

## 设计可扩展的在线和离线持续学习系统架构

[](https://medium.com/@kylegallatin?source=post_page-----4e583728e8a1--------------------------------)[![Kyle Gallatin](../Images/ee2796ba575412e9caf6034a65d741e5.png)](https://medium.com/@kylegallatin?source=post_page-----4e583728e8a1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4e583728e8a1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4e583728e8a1--------------------------------) [Kyle Gallatin](https://medium.com/@kylegallatin?source=post_page-----4e583728e8a1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F51ff4b76ebf4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthoughts-on-stateful-ml-online-learning-and-intelligent-ml-model-retraining-4e583728e8a1&user=Kyle+Gallatin&userId=51ff4b76ebf4&source=post_page-51ff4b76ebf4----4e583728e8a1---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4e583728e8a1--------------------------------) ·6 min read·2023年4月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4e583728e8a1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthoughts-on-stateful-ml-online-learning-and-intelligent-ml-model-retraining-4e583728e8a1&user=Kyle+Gallatin&userId=51ff4b76ebf4&source=-----4e583728e8a1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4e583728e8a1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthoughts-on-stateful-ml-online-learning-and-intelligent-ml-model-retraining-4e583728e8a1&source=-----4e583728e8a1---------------------bookmark_footer-----------)

自从我读了Chip Huyen的[*实时机器学习：挑战与解决方案*](https://huyenchip.com/2022/01/02/real-time-machine-learning-challenges-and-solutions.html)后，我一直在思考生产环境中机器学习的未来。短反馈循环、实时特性以及能够在线学习的状态保持机器学习模型部署需要一种与我今天所用的无状态机器学习模型部署截然不同的系统架构。

![](../Images/cb765555b528f3298223d7073c4cf1b4.png)

我在墨西哥科苏梅尔思考有状态的机器学习——图片来自作者

过去几个月，我一直在进行非正式用户研究、白板讨论和临时开发，以探讨真正有状态的机器学习系统可能是什么样的。大部分内容都概述了我的思维过程，并且我继续深入这个领域，发现有趣且独特的架构挑战。

# 定义

**有状态（或连续）学习** 涉及到*更新*模型参数，而不是从头开始再训练，以便于：

+   缩短训练时间

+   节省成本

+   更频繁地更新模型

![](../Images/32dadd54d68b8ab84e7e751df71e43a8.png)

无状态与有状态的再训练——来自[Chip Huyen](https://huyenchip.com/2022/01/02/real-time-machine-learning-challenges-and-solutions.html)的许可

**在线学习** 涉及实时从真实样本中学习，以便于：

+   提升模型性能和反应性

+   缓解因漂移/过时导致的性能问题

目前，大多数行业中的学习都是*离线*进行的。

**智能模型再训练** 通常指的是使用某些性能指标自动再训练模型，而不是按照固定的时间表进行，以便于：

+   降低成本而不牺牲性能

目前，大多数行业中的模型都是使用DAG按照时间表再训练的。

![](../Images/5da4771588b8116ac934298acf5fe48e.png)

来自[自动模型再训练指南](https://arize.com/resource/a-guide-to-optimizing-automated-model-retraining/?utm_campaign=Newsletter+-+DRIFT&utm_medium=email&_hsmi=252645972&_hsenc=p2ANqtz-_--qVbD_z1d-GRI1rSslPJDb1J6jevmfkOcQTOkQ3IwCJME58-WrfDPvra_rpfXHrDv_BIXyJmcTrtSiIRQvVn4DKr7Q&utm_content=252645972&utm_source=hs_email)的智能再训练架构——由Arize AI授权

# 为在线学习设计MVP

在[上一篇文章](/building-a-lil-stateful-ml-application-for-online-learning-66624d62afae?sk=cd01b0115ea189cf7abdc295c35f4d43)中，我尝试运用基础工程原理来创建一个极其简单的在线学习架构。我的第一个想法是——将有状态的在线学习架构建模为有状态的网页应用程序。通过将“模型”视作数据库（其中预测为读取，增量训练会话为写入），我认为可以简化设计过程。

![](../Images/8b4f9d6bf80e4313dad2071275aa7c3b.png)

图片来自作者

在某种程度上，我确实做到了！通过使用在线学习库[River](https://riverml.xyz/0.15.0/)，我[构建了一个小型有状态的在线学习应用](https://github.com/kylegallatin/stateful-ml-app)，这让我能够*实时*更新模型并提供预测。

![](../Images/8aa2c88281674d66b569ccfba1569df2.png)

Flask应用程序在多个工作进程之间共享内存中的模型——图片来自作者

这种方法在编码时很酷很有趣——但在规模上存在一些根本性的问题：

1.  **无法横向扩展：** 我们可以轻松地在单个应用程序的内存中共享一个模型——但这种方法在像Kubernetes这样的编排引擎中无法扩展到多个pod。

1.  **混合应用职责：** 我不知道（也不想成为第一个知道）关于尝试支持一个混合训练和服务的部署的注意事项。

1.  **预先引入复杂性：** 在线学习是最积极主动的机器学习类型，但我们甚至还没有验证我们是否需要它。必须有一个更好的起点……

# 设计一种可扩展的架构

从现有标准开始——分布式模型训练。使用类似参数服务器的东西作为集中存储，同时多个工作者计算部分/分布式梯度……或其他什么……并在之后调整参数是相当常见的做法。

所以——我想尝试在实时模型服务部署的背景下思考这个问题，结果想出了最傻的架构。

![](../Images/d812ed7402ef09badbf527beb4e9bba6.png)

一种没有意义的架构——作者提供的图片

分布式模型训练旨在加快训练过程。然而，在这种情况下，没有必要以分布式方式进行训练*和*服务——保持训练去中心化会引入复杂性，并且在在线训练系统中没有实际用途。完全分开训练更有意义。

![](../Images/5970ff10b7470e23e06e1428d352d154.png)

一种稍微有意义的架构——作者提供的图片

很好！有点。此时我不得不退后一步，因为我做了很多假设，可能有些过于前瞻性：

1.  我们可能无法在接近实时的情况下获得真实情况。

1.  持续*在线*训练可能不会比持续*离线*训练提供净收益，而且是一种过早的优化。

1.  离线/在线学习也可能不是二元的——有些场景中我们可能需要/想要两者都具备！

# 一些合理的智能再训练、持续学习和在线学习架构

让我们从一个更简单的离线场景开始——我想使用某种机器学习可观察性系统，根据性能指标的退化自动再训练模型。在进行持续训练（且模型权重更新不会花费太长时间）的场景下，这在不显著影响业务的情况下是可行的。

![](../Images/69c2c535406ca8e5372514fe2decfad2.png)

智能再训练和持续*在线*学习——作者提供的图片

真棒——这是我今天画的第一个合理的东西！这个系统可能比*无状态*训练架构有更低的成本开销，并且对模型/数据的变化反应迅速。通过仅在需要时进行再训练，我们节省了大量的$，总体上也非常简单！

然而，这种架构有一个大问题……它远没有那么有趣！什么样的系统能兼具在线学习的所有反应性、连续学习的成本节约和在线学习的弹性呢？希望，像这样……

![](../Images/559693798798de7e56dc5406c2beef80.png)

连续的在线学习 — 作者提供的图像

虽然还有很多细节我还没完全弄清楚，但这种架构有很多好处。它允许混合在线和离线学习（正如特征存储允许访问流式特征和离线计算的特征一样），对数据分布的变化或个别用户偏好（个性化系统（recsys））非常稳健，并且仍然允许我们集成 ML 可观测性（O11y）工具来不断测量数据分布和性能。

然而，尽管这可能是我迄今为止创建的最合理的图表，它仍然留有很多未解的问题：

+   在在线系统中，我们如何/何时评估模型，以及使用什么数据？如果数据分布发生大幅变化，我们需要创建新的数据驱动方法和最佳实践，设计一个包含旧数据和最新数据的保留评估集。

+   我们如何调和将训练过程分为批处理/离线和在线的 ML 模型？我们需要尝试新的技术和系统架构，以允许在像这样的系统中涉及大型 ML 模型的复杂计算操作。

+   我们如何拉取/推送模型权重？按照一定的节奏？在某些事件中或根据某些指标的变化？这些架构决策中的每一个都可能对我们系统的性能产生重大影响——而且没有在线 A/B 测试或其他研究，将很难验证这些选择。

# 下一步

当然，我的下一步是开始构建这些东西，看看会发生什么。然而，我希望能从业内的任何人那里获得见解、想法和参与，以考虑一些前进的路径！

请通过 [twitter](https://twitter.com/kylegallatin)、[LinkedIn](https://www.linkedin.com/in/kylegallatin/) 联系我，或报名参加我课程的下一期：[Designing Production ML Systems this May](https://nycdatascience.com/courses/designing-and-implementing-production-machine-learning-systems-mlops/)！
