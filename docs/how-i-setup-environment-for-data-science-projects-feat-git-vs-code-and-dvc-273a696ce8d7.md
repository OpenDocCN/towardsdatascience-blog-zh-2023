# 我如何设置数据科学项目（使用 VS Code 和 DVC）

> 原文：[https://towardsdatascience.com/how-i-setup-environment-for-data-science-projects-feat-git-vs-code-and-dvc-273a696ce8d7?source=collection_archive---------8-----------------------#2023-01-26](https://towardsdatascience.com/how-i-setup-environment-for-data-science-projects-feat-git-vs-code-and-dvc-273a696ce8d7?source=collection_archive---------8-----------------------#2023-01-26)

## 来源于软件开发的灵感

[](https://medium.com/@thanakornpanyapiang?source=post_page-----273a696ce8d7--------------------------------)[![Thanakorn Panyapiang](../Images/4e68a74a2039c8404c5873d7d364be43.png)](https://medium.com/@thanakornpanyapiang?source=post_page-----273a696ce8d7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----273a696ce8d7--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----273a696ce8d7--------------------------------) [Thanakorn Panyapiang](https://medium.com/@thanakornpanyapiang?source=post_page-----273a696ce8d7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3946a0d40f8c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-i-setup-environment-for-data-science-projects-feat-git-vs-code-and-dvc-273a696ce8d7&user=Thanakorn+Panyapiang&userId=3946a0d40f8c&source=post_page-3946a0d40f8c----273a696ce8d7---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----273a696ce8d7--------------------------------) ·6分钟阅读·2023年1月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F273a696ce8d7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-i-setup-environment-for-data-science-projects-feat-git-vs-code-and-dvc-273a696ce8d7&user=Thanakorn+Panyapiang&userId=3946a0d40f8c&source=-----273a696ce8d7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F273a696ce8d7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-i-setup-environment-for-data-science-projects-feat-git-vs-code-and-dvc-273a696ce8d7&source=-----273a696ce8d7---------------------bookmark_footer-----------)

# 介绍

设置开发环境通常是开始任何编码项目时的第一步。一个有效的开发环境可以极大提升生产力，帮助我们产生高质量的工作。然而，与软件开发等其他领域相比，数据科学中的这个过程非常模糊，因为其自身的独特性和挑战。

在这篇文章中，我将解释如何为数据科学项目设置工作环境，包括我的动机以及我和我的团队使用的工具。

![](../Images/81d81dcf13120361caf70fffa9b7c47b.png)

图片来源于[Remy_Loz](https://unsplash.com/@remyloz?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 动机

当我还是软件工程师时，加入一个团队时我做的第一件事就是设置开发环境。这通常包括检查代码仓库、安装所需的库/框架以及熟悉第三方工具。一旦完成，你就有了一个可以与团队一起使用的工作环境。但当我转向数据科学时，这样的过程在我的公司并不存在。数据科学家在基于云的笔记本服务上工作，代码没有版本控制，数据被上传到云存储却没有任何文档。融入团队完全是一片混乱，更不用说与他人协作了。

为了减少痛苦，我开始设计一个标准的团队工作环境，以软件开发中的环境为灵感。然而，数据科学不同于软件开发。简单地复制和粘贴是不够的。数据科学有其自身的挑战，因此需要一些修改。以下是我在设计数据科学工作环境时考虑的3个额外需求：

1.  **代码和数据跟踪**

    与软件项目不同，数据科学项目不仅仅是代码的产物，还有数据。一个有效的数据科学工作环境需要对代码和数据进行版本控制。

1.  **无缝的远程开发体验**

    处理超出普通机器容量的数据是数据科学项目中的常见情况，这就是为什么人们更喜欢在远程工作站上运行代码。我们设计的环境应该使远程开发体验尽可能无缝。

1.  **易于使用的实验跟踪**

    数据科学工作是实验性的。数据科学家可以整天调整超参数，以比较其对模型的影响。因此，易于使用的实验跟踪设施是必不可少的。

既然我们已经明确了需求，下一步就是找到实现这些需求的工具。

# 工具

## Git + DVC

从项目跟踪开始，Git是多年来软件开发中的通用标准，因此代码版本控制只有一个选项。然而，数据版本控制更为棘手。Git被设计用于处理小型和基于文本的文件，而我们的大多数机器学习项目包含大量和无结构的数据。我们探索了两个选项：Git LFS和DVC。最终我们选择了DVC，因为它的功能更适合数据科学。

![](../Images/37bb7b4c65c6ad8aec19217768220286.png)

用 Git 和 DVC 跟踪代码和数据（图片由作者提供）。

## Visual Studio Code

如前所述，ML 项目通常需要超出本地机器的计算能力。这就是为什么 ML 从业者使用 Google Colab、SageMaker 和 Vertex AI 等基于云的 notebook 服务。但 Jupyter notebook 不具备现代 IDE 的任何编码辅助功能。那些功能如果使用得当，可能会大幅提高生产力。因此，我们从 Jupyter notebook 迁移到了 Visual Studio Code。

对于计算资源，我们使用 VS Code 的远程开发功能连接到云远程服务器上的代码和数据仓库。这样，我们不必在高计算能力和良好的开发体验之间做出选择。我们可以兼得二者。有关 VS Code 远程开发的更多细节可以在[这里](https://code.visualstudio.com/docs/remote/ssh)找到。

![](../Images/26bdf81904050c59d10be50f08f86a95.png)

在 Jupyter notebook 上编码 VS 在 VS Code 上编码（图片由作者提供）。

## DVC VS Code 扩展

数据科学工作流的另一个关键方面是实验跟踪。数据科学家在获得最终模型之前会反复运行不同参数的 ML 实验。跟踪这些实验是一个极其耗费精力的任务。有许多实验跟踪工具，如 TensorBoard、MLFlow 和 WandB 等。我们对这些工具的担忧是，它们要么是需要托管的独立服务，要么是需要将数据发送到其 API 的第三方 SaaS。

我们最终选择了 [dvc metrics](https://dvc.org/doc/command-reference/metrics)，这是 DVC 的一个功能。DVC metrics 允许你将指标保存到 JSON 或 YAML 文件（我们在使用 DVC 之前已经这样做了）并指定这是一个指标文件。DVC 会开始类似于其他数据那样跟踪它，只是你可以使用 CLI 绘制、可视化和比较该文件的内容。通过 CLI 生成的实验报告在用户友好性方面明显不如 TensorBoard 和 MLFlow 提供的那些。幸运的是，DVC 提供了一个 VS Code 扩展，这有助于将这个问题缓解到我们认为可以接受的程度。

我们发现使用 DVC 管理实验的一个局限性是，当多个工程师同时在同一个模型上工作并希望比较他们的结果时，他们必须克隆其他人的分支，因为指标是在 Git 上跟踪的。由于我们团队较小，这种情况不常发生。因此，这是我们可以接受的局限性。

![](../Images/8a86cfb98746e587dc8c795971edc671.png)

DVC VS Code 扩展（图片来自 [Code Extension Marketplace](https://marketplace.visualstudio.com/items?itemName=Iterative.dvc)）。

# 工作流程

以下是我们在开发数据科学项目时使用的流程。

1.  **创建 git 和 DVC 仓库**

    我们首先创建一个使用 Cookie Cutter Data Science 项目模板的 git 仓库。拥有一个标准项目模板有助于开发人员更顺畅地协作，因为项目已经有了明确的结构。一旦代码设置完成，我们使用 DVC 将数据添加到项目中，并推送到我们的远程存储，以便团队可以访问。

1.  **设置远程开发环境** 接下来是设置一个数据科学家运行实验的开发服务器。我们打开一个 EC2 实例，登录到服务器，从 git 中检出代码，并使用 DVC 从远程存储中拉取数据。

1.  **实施与实验**

    现在开发环境已准备就绪，我们可以开始工作。如前所述，我们使用 VS Code 进行开发，需要安装两个扩展：[*Remote SSH*](https://code.visualstudio.com/docs/remote/ssh) 和 [*DVC*](https://marketplace.visualstudio.com/items?itemName=Iterative.dvc)。第一个用于远程连接到上一阶段创建的环境，以便我们可以在高性能机器上运行代码。DVC 扩展主要用作实验跟踪工具。安装和配置完毕后，数据科学家可以开始编写代码、运行实验和调整参数。

1.  **打开合并请求并进行代码审查**

    一旦数据科学家获得满意的模型，他们将提交所有更改并向 git 仓库打开合并请求，要求进行代码审查。当拉取请求被提出时，我们的 CI/CD 流水线将创建一个报告，比较新分支与目标分支的指标，以便其他人可以在批准/拒绝请求之前查看新更改对模型的影响（如果你对这部分感兴趣，请查看这篇[文章](https://medium.com/towards-data-science/how-i-apply-continuous-integration-to-machine-learning-projects-8273274a565a)）。

![](../Images/842fd1f27796c8a86733c4421ad03010.png)

工作流程概述（图由作者提供）。

# 结论

就是这样。这就是我如何使用 Git、VS Code 和 DVC 为数据科学项目设置开发环境的方法。一旦我的团队开始在我们的团队中使用这个标准，新成员的入职过程变得更加顺利，因为对新成员应该使用的工具和流程没有了模糊之处。此外，团队成员之间的合作变得更加有效，因为他们使用的是相同的*语言*。

*最初发表于* [*https://thanakornp.com*](https://thanakornp.com/how-i-setup-data-science-project-feat-vs-code-and-dvc)*.*
