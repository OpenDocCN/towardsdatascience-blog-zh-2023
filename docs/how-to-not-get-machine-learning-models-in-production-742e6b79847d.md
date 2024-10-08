# 如何 *避免* 将机器学习模型投入生产

> 原文：[`towardsdatascience.com/how-to-not-get-machine-learning-models-in-production-742e6b79847d`](https://towardsdatascience.com/how-to-not-get-machine-learning-models-in-production-742e6b79847d)

## 一份讽刺性的指南，避免将生产过程与您的机器学习模型接触

[](https://medium.com/@ebbeberge?source=post_page-----742e6b79847d--------------------------------)![Eirik Berge, PhD](https://medium.com/@ebbeberge?source=post_page-----742e6b79847d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----742e6b79847d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----742e6b79847d--------------------------------) [Eirik Berge, PhD](https://medium.com/@ebbeberge?source=post_page-----742e6b79847d--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----742e6b79847d--------------------------------) ·8 分钟阅读·2023 年 7 月 8 日

--

![](img/0d63b46131824d9b39a8fc7793989034.png)

图片由 [Unpad Street Feeding Animal Friend](https://unsplash.com/@unpadstreetfeeding?utm_source=medium&utm_medium=referral) 提供，在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 您旅程的概述

1.  引言 — 没有生产，没问题！

1.  笔记本可以用来做所有事情！

1.  为什么要在有时间的时候自动化？

1.  测试？只需从不出错！

1.  我的头脑中的依赖管理！

1.  总结

# 1 — 引言 — 没有生产，没问题！

作为数据科学家、数据工程师和机器学习工程师，我们被有关生产中机器学习模型的信息淹没。数以百计的视频和数以千计的博客尽力帮助我们避免当前的困境。然而徒劳无功。**现在，我痛苦地说，全球各地都有机器学习模型在生产中。** 这些模型正在为数百万无知的人创造价值。每个街角都有。我们容忍它，只因为它很常见。

在参加会议时，我听到紧张的数据科学家在大观众面前谈论生产。从他们的额头汗水和湿漉漉的手中可以看出情况的严重性。这种情况已经持续了很多年，但我们没有相信他们的预言。现在看看我们自己。我们应该听从的。

现在不是有人说“我早就告诉过你们”的时候。我们需要团结起来，夺回我们的领地。我们需要单方面表达对现代做事方式的厌恶。我们需要回到更好的时代，**当时生产中的机器学习模型只是中型公司招聘广告上的一个非约束性要点。**

必须有人带头引导这次救赎之旅。而谁比我更合适呢？不打算吹嘘，我已经做了几个没有投入生产的 ML 模型。其中一些甚至离生产都很远。我可以与你分享一些最佳技巧，这样你就不需要复制你的开发环境——因为将不会有其他环境。

我已经将接下来的每一部分清晰地分为两个部分。第一部分叫做**正道**。它告诉你如何避免将 ML 模型投入生产。第二部分叫做**歪道**。这让你知道应该避免什么，因为这是将 ML 模型快速投入生产的捷径。不要搞混！

# 2 — 一个单一的笔记本可以用来做所有事情！

## 正道

我发现避免将 ML 模型投入生产的最简单方法之一就是**将你的整个代码库放在一个单一的 Jupyter 笔记本文件中**。这包括模型训练、模型预测、数据处理，甚至配置文件。

想想看。当所有敏感的配置文件都在你的主文档中时，几乎不可能在不引入重大安全风险的情况下将任何东西上传到 GitHub 或 Azure DevOps。此外，你是否曾尝试阅读一个修改了 100,000 行的文件的拉取请求？最好的回应就是干脆放弃远程托管和版本控制。

这，我的朋友，把我们带上了避免生产的快车道。没有远程托管意味着孤岛将是不可避免的。你有没有停下来思考一下每次需要预测时运行整个模型训练的云消耗成本——记住，我们只有一个用于训练和测试的 Jupyter 笔记本。我自豪地称之为**单一笔记本架构**的这种方法简直是天才之作。

## 歪道

相比单一笔记本架构，人们随着时间的推移走了歪路。他们开始**将代码、配置和文档拆分到多个文件中**。这些文件不幸的是遵循了使其易于导航的惯例。甚至还有像[Cookiecutter](https://drivendata.github.io/cookiecutter-data-science/)这样的工具，提供了结构化 ML 项目的易用模板。

代码本身通常被划分为诸如函数、类和文件这样的模块化组件。这鼓励了重用并改善了组织结构。编码标准通过[PEP8 标准](https://peps.python.org/pep-0008/)和[Black](https://medium.com/towards-data-science/tired-of-pointless-discussions-on-code-formatting-a-simple-solution-exists-af11ea442bdc)等自动格式化工具来强制执行。甚至还有一些恶心的博客帖子[指导数据科学家更好的软件开发实践](https://medium.com/towards-data-science/why-software-development-skills-are-essential-for-data-science-f53364670bd7)。真恶心！

不仅仅是源代码被结构化。还有像[数据版本控制（DVC）](https://dvc.org/)这样的工具用于**大数据版本控制**，或者像[MLflow](https://mlflow.org/docs/latest/projects.html)这样的工具用于**结构化机器学习模型打包**。这些工具中的任何一个实际上都不难上手。远离这些吧！

# 3 — 为什么在有时间的时候还要自动化？

![](img/4c824725ae1e391767c68e9a1ba1038b.png)

图片由[Aron Visuals](https://unsplash.com/@aronvisuals?utm_source=medium&utm_medium=referral)拍摄，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 正义的方式

过去，情况有所不同。我们对我们的代码有一种归属感。这种归属感来自于定期重新审视代码。**没错，我们都是手动运行所有代码的。** 这显然是上天的旨意，我已经厌倦了假装不是这样。

手动方法让你拥有完全的控制权。不需要检查数据管道是否在凌晨 3 点启动——你就在那儿！每晚都仔细地从记忆中输入执行命令。当你有时间的时候，为什么要依赖[CRON 表达式](https://crontab.cronhub.io/)或[Airflow DAGs](https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/dags.html)来完成工作？到头来，你真的能相信别人吗？

这种手动方法被证明在生产环境中对机器学习模型是个福音。数据漂移发生在机器学习模型上，然后模型需要重新训练。但使用手动方法，必须有人坐镇监视数据漂移是否足够。在这种经济环境下？不太可能！也许还是放弃整个生产环节，回到更好的方法上来吧。

## 堕落的方式

让我们谈谈明显的问题：**持续集成与持续交付（CI/CD）**。我知道，这也让我感到愤怒。现在，人们会在更新代码库之前自动检查代码质量并运行测试。像[GitHub Actions](https://docs.github.com/en/actions)或[Azure DevOps Pipelines](https://learn.microsoft.com/en-us/azure/devops/pipelines/get-started/what-is-azure-pipelines?view=azure-devops)这样的工具也用于自动化生产环境中机器学习模型的再训练。我们还有机会吗？

还有更多！现在人们使用像[Teraform](https://www.terraform.io/)这样的工具来设置支持生产环境中机器学习模型所需的基础设施。通过这种基础设施即代码的方法，环境可以在各种设置中复制。**突然间，生产环境中的机器学习模型从 Azure 变成了 AWS！**

还有像[Airflow](https://airflow.apache.org/)这样的工具帮助进行**数据管道的编排**。Airflow 确保预处理步骤在执行之前等待前一步骤完成。它还提供了一个 GUI，你可以查看之前的管道运行记录，了解情况。虽然这些功能看似简单，但它们能在错误传播到系统并破坏数据质量之前迅速发现错误。不幸的是，高质量的数据对于成功的生产环境中的 ML 模型至关重要。

# 4 — 测试？就是不要犯错！

## 正确的方式

测试的哲学是对无数生产失败和安全漏洞的回应。这个想法是尽可能多地发现问题，然后再推出产品。虽然确实很高尚，但我认为这是误导性的。对于那些关注的人，一个更简单的方法显现出来。正是这样！一旦你看到它，它就像白天一样清晰。**你只需不犯任何错误。**

对于没有错误的代码，不需要测试。更好的是，当你对代码的有效性 100%确信时，其他人则不那么确信。他们会怀疑你的代码的正确性。因此，他们将避免将你的 ML 模型投入生产，担心一切会崩溃。你知道它不会。但他们不必知道这一点。把你完美的编码技巧留给自己。

不编写测试还有一个额外的好处，那就是你会获得更少的文档。**有时测试对帮助他人理解你的代码应该做什么至关重要。** 没有测试，甚至更少的人会打扰你，你可以继续平静地工作。

## 罪恶的方式

编写测试已经变得很普遍，即使是对于 ML 模型和数据工程管道。主要有两类测试：**单元测试**和**集成测试**。正如名称所示，单元测试测试一个单独的单元，如 Python 函数是否按预期工作。另一方面，集成测试测试不同组件是否无缝地协同工作。让我们关注单元测试。

在 Python 中，你可以使用内置的库[unittest](https://docs.python.org/3/library/unittest.html)来编写，正如你所猜到的，单元测试。这是基于 OOP 的，需要一些样板代码才能开始。越来越多的人使用外部库[pytest](https://docs.pytest.org/en/7.4.x/)来编写 Python 中的单元测试。它是功能性而非基于 OOP 的，并且需要较少的样板代码。

编写单元测试还有一个副作用。它迫使你以模块化的方式编写代码。**经过充分测试的模块化代码在生产中发生故障的概率较低**。这对你的 ML 模型来说是坏消息。如果它们在生产中没有出现故障，那么它们将永远留在那里。考虑一下，ML 模型在生产中出现故障就是它们试图逃脱。谁能怪它们呢？

# 5 — 头脑中的依赖管理！

## 正确的方式

管理依赖项是任何机器学习项目的重要部分。这包括知道你使用了哪些 Python 库。**我的建议是简单地记住你安装了哪些库、你使用的操作系统、运行时版本等等。** 这并不难，我相信甚至有应用程序可以帮助你跟踪这些。

我有时会在夜里醒来，想知道自己运行的是 scikit-learn 的 0.2.3 版本还是 0.3.2 版本。这没关系！所有版本都存在，所以应该没有问题……对吧？如果我不定期解决依赖冲突，那么我的依赖冲突技能就会生疏。

仅仅记住你的依赖项的优势在于**其他人很难运行你的代码**。尤其是当你不想告诉他们所有依赖细节时。这样，你可以避免有人突然产生偷偷将你的机器学习模型投入生产的想法。

## 罪恶的方法

记忆力差的人选择了简单的办法。他们寻找可以为你处理依赖的解决方案。我发誓，人们每天都变得更懒惰！管理运行时版本和库依赖的简单方法是使用**虚拟环境**和*requirements.txt*文件。在 Python 中，有像[virtualenv](https://virtualenv.pypa.io/en/latest/)这样的工具，允许你轻松设置虚拟环境。

那些想更进一步的人使用像[Docker](https://www.docker.com/)这样的**基于容器的技术**。这可以从操作系统及以上层面复制一切。这样，只要每个人知道如何运行 Docker 容器，共享机器学习模型就变得相当轻松。在像[Azure Machine Learning](https://azure.microsoft.com/en-us/products/machine-learning/)这样的现代工具中，有简单的方法来使用标准化的 Docker 镜像。这些镜像可以包括像[scikit-learn](https://scikit-learn.org/stable/index.html)和[tensorflow](https://www.tensorflow.org/)这样的常用库。而且你甚至不需要自己记住版本！

# 6 — 总结

![](img/4a06c775ef80f575c5d7cdfe36d7a151.png)

照片由[斯宾塞·伯根](https://unsplash.com/@spencerbergen?utm_source=medium&utm_medium=referral)提供，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我希望不用指出你确实希望机器学习模型进入生产环境。我在这里提出的观点可以反向操作，帮助你成功将那些麻烦的机器学习模型投入生产。

随时在[LinkedIn](https://www.linkedin.com/in/eirik-berge/)上关注我并打个招呼，我在这篇博客文章中看起来并没有我表现得那么烦躁 ✋

**喜欢我的写作吗？** 可以看看我其他的一些帖子，获取更多 Python 内容：

+   用美丽的类型提示现代化你的罪恶 Python 代码

+   在 Python 中可视化缺失值简直太简单了

+   在 Python 中使用 PyOD 介绍异常/离群点检测 🔥

+   5 个绝妙的 NumPy 函数，关键时刻拯救你

+   5 个专家技巧，让你的 Python 字典技能飞速提升 🚀
