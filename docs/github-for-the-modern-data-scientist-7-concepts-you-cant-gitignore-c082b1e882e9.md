# GitHub 对现代数据科学家的意义：你不能 .gitignore 的 7 个概念

> 原文：[`towardsdatascience.com/github-for-the-modern-data-scientist-7-concepts-you-cant-gitignore-c082b1e882e9`](https://towardsdatascience.com/github-for-the-modern-data-scientist-7-concepts-you-cant-gitignore-c082b1e882e9)

## 用一点趣味、机智和视觉效果进行解释

![Bex T.](https://ibexorigin.medium.com/?source=post_page-----c082b1e882e9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c082b1e882e9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c082b1e882e9--------------------------------) [Bex T.](https://ibexorigin.medium.com/?source=post_page-----c082b1e882e9--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c082b1e882e9--------------------------------) ·阅读时间 11 分钟·2023 年 5 月 17 日

--

![](img/d9add96480d3a682d10b6fcc7ad06fe8.png)

我的菠萝伙伴们。使用 Midjourney 制作。

## 介绍

如果你认为数据科学家和 ML 工程师只对 Git 感到不适，你应该看看他们的 GitHub 个人资料。它们比太平洋深处的一个无名岛还要荒凉。

当我刚开始时，我的个人资料是一个典型的案例。我认为这主要是因为我参加的所有课程（而且有很多）都想先教我一些炫酷的东西，比如机器学习，以保持我的兴趣，而把枯燥的部分留给我自己，比如学习 GitHub。

所以，我以一种非常困难的方式学到了这七个 GitHub 概念。（夸张一下：我只是阅读了文档并进行了交流 :)）

现在，你可以以比我更有趣的方式来学习这些概念。（希望如此。）

让我们开始吧。

顺便说一下，我假设你已经了解 Git 才能理解这篇文章。如果不了解，请查看我之前的帖子：

## Git 对现代数据科学家的意义：你不能忽略的 9 个 Git 概念

### 编辑描述

towardsdatascience.com

## 0. 远程仓库

Git 仓库有两种类型：本地和远程。

本地 Git 仓库仅存储在你的计算机上。另一方面，其远程副本存储在服务器上（通常是 GitHub），而不是某个生锈的笔记本电脑上，以便其他人可以与你协作而无需实际在同一房间里。

![](img/0651ae5161ebba7b345d3a87ee6d9675.png)

图片由我提供

然而，远程仓库不仅仅是为了协作。由于它在服务器上，你可以从任何有互联网连接的机器上访问它。这比把沉重的笔记本电脑从办公室带到家，再到本地咖啡馆，再到办公室，最后再到家里要方便得多。

## 1\. REAMDE

README 文件是任何项目的重要组成部分，但它们常常被忽视。我们都知道什么是 README 文件。我们不是孩子。那么我们为什么不花时间把它添加到我们的代码库呢？

![](img/b0fb842319c497ac210246d630774740.png)

由我提供的图片

也许是因为我们完全了解我们的代码是什么以及它是如何工作的，所以我们觉得不需要向别人解释。但情况并非总是如此。

比如，想象一下从一个项目中休息几个月，然后再重新开始。你会记得每个文件的作用吗？

个人来说，当我回到一个暂停已久的项目时，总感觉我在阅读别人的代码和想法。在这种情况下，README 文件可以通过告诉你你停在了哪里以及你在做什么来拯救项目。你不会因为感觉需要从头开始而放弃整个项目。

README 文件有两种类型：个人 README 和面向公众的 README。对于个人 README，其目的是帮助你，开发者，就像上面提到的情况一样。

另一方面，面向公众的 README 文件是为了那些可能对你的项目感兴趣的人。99% 的时候，当一个人遇到一个 GitHub 仓库时，他们不会根据代码的质量（他们几乎从不查看代码）来评判，而是根据它对他人的展示情况来判断。

把它看作是你项目的用户手册。它是人们访问仓库时首先看到的内容，它应该给他们一个清晰的了解你项目是什么以及如何使用它的印象。

此外，面向公众的 README 文件是你唯一一次有机会清晰而简洁地解释你在模型选择、验证方法和模型测试结果背后的思路。

如果你在经历了这一切之后仍然讨厌写 README 文件，你应该查看“Awesome README Templates” 仓库，它是为像我们这样的懒人准备的。

[](https://github.com/matiassingers/awesome-readme?source=post_page-----c082b1e882e9--------------------------------) [## GitHub - matiassingers/awesome-readme: 精选的精彩 README 列表

### 精选的精彩 README 列表中包含的元素有：图片、截图等。

github.com](https://github.com/matiassingers/awesome-readme?source=post_page-----c082b1e882e9--------------------------------)

## 2\. 克隆它还是叉取它？

当有人查看远程仓库时，可能会发生四种情况。最可能的情况（情况 0）是他们忽略它，或者如果他们心情好就给它一个星标。

在第一种情况下，如果 README 文件足够有说服力，他们可能会克隆它。

![](img/d1c32c4dec021e8b6ed2f82b3d5caa89.png)

由我提供的图片

使用类似`git clone https://github.com/username/awesome_repo`的命令克隆远程仓库会在你的本地机器上创建一个`awesome_repo`的精确副本，让你可以访问项目的整个 Git 历史记录以及对所有文件的写权限。然而，如果你对这个`awesome_repo`的本地副本进行更改，其远程副本不会有所感知。

在第二种情况下，如果 README 的说服力更强，可能会有人进行分叉。

![](img/f303bcbe2ed13775df0d00b9b34ef289.png)

图片由我提供

当你在 GitHub 上分叉`awesome_person`的`awesome_repo`时，你将会在你的账户下拥有`awesome_repo`的精确副本。

你的 GitHub 页面将拥有一个新的`your_username/awesome_repo`仓库，其内容和历史记录与`awesome_person/awesome_repo`相同。如果你想对这个副本进行更改，你可以克隆`your_username/awesome_repo`，使其也在你的本地机器上。

有很多原因可能促使某人分叉他人的仓库。第一原因是通过提交拉取请求（见下文）向`someone/awesome_repo`做出贡献。另一个原因是基于原始代码创建新项目，而不影响原始代码。

一个值得注意的例子是[Manim GitHub 社区](https://github.com/ManimCommunity/manimhttps://github.com/ManimCommunity/manim)，这是一个维护和文档更完善的 [传奇 Manim 仓库](https://github.com/3b1b/manim)的分支，后者由 Grant Sanderson（3Blue1Brown 及其所有视频的创作者）创建。

> 为了区分原始仓库和分支，GitHub 在仓库页面上添加了“forked from `original_repo`”标签。

第三种情况是当你从另一台机器访问你自己的远程仓库时。例如，你将笔记本电脑留在了干洗店，你想在办公室继续进行项目工作。

在这种情况下，你所要做的就是克隆仓库以将其内容下载到办公室的 Mac 上。但是，如果你想同步你的更改，Mac 上的 Git 安装必须使用你的 GitHub 用户名。

## 3. 推送与拉取

在使用 Git 时，“推送”和“拉取”的货币单位是更改。让我们来看看这些命令的最常见用法。

在情况 0 中，你有一个包含许多提交和分支的本地仓库，并且你想将它们全部发送到 GitHub 上的新空远程仓库。首先，你需要使用命令`git remote add remote_name https://github.com/username/repo_name.git`在 Git 中指定远程仓库的网址。通常，`remote_name` 的替代名称是`origin`。

![](img/9d525e566b95d1a699584b3f066f2593.png)

图片由我提供

然后，你通过调用`git push`进行第一次“推送”，这将把当前分支中的所有提交发送到 GitHub。恭喜！

在情况 1 中，你在仓库中进行了*新的*本地提交，并希望将它们发送到远程仓库，因此你调用了`git push again`。这个命令始终用于保持远程仓库与本地仓库的同步。

在案例 2 中，你可能在一个有许多贡献者的大项目上工作，并且你已经在你的机器上有一个两天前的项目副本。为了下载这段时间内其他人做出的新更改，你需要执行 `git pull`。

![](img/f82eaec3dcd7f0e4c73a4550d9ccd627.png)

图片由我提供

拉取操作确保你的本地仓库与远程仓库保持同步，以防多人同时对其进行更改。

## 4\. 拉取请求

拉取请求就像是敲邻居的门说：“嘿，我对你的草坪做了一些更改。看看你是否喜欢！”但我们谈论的不是草坪，而是仓库。

PR 是开源项目协作性质的重要组成部分。它们允许贡献者（普通程序员）向现有项目提出更改和改进建议，并让项目维护者在这些更改合并到主代码库之前进行审查和批准。

假设你想对 Scikit-learn 仓库提交拉取请求（我的甜蜜梦想 :)）。这是你必须遵循的工作流程（适用于对任何仓库提交拉取请求）：

第 0 步：分叉 Scikit-learn，以便你在你的账户下拥有一个副本。

第 1 步：克隆你的 Scikit-learn 副本，以便你在你的机器上拥有一个副本。

第 2 步：创建一个新分支，以便在不影响主分支的情况下对你的副本进行修改。

第 3 步：进行更改——编写新代码、修复文档中的错误或错别字，或进行其他改进。

第 4 步：测试你的更改——确保你的更改没有错误，并按预期工作。

第 5 步：推送包含更改的分支（`git push` 将它们发送到你远程的 Scikit-learn 副本）。

第 6 步：创建请求——从你分叉的仓库中，点击“新建拉取请求”按钮。这将使你的更改暴露在 Scikit-learn 维护者的眼下。

第 7 步：等待维护者审查你的请求，可能需要很长时间，甚至到你头发变灰。

第 8 步：对他们接受或拒绝你的请求感到失望或高兴。

条件步骤 9：维护者合并你的请求，以便它将在下一个版本中包含。

## 5\. GitHub 问题

将 GitHub 问题看作是 StackOverflow 线程的更文明和民主的版本。它们使人们能够跟踪和报告特定仓库的问题、错误或功能请求。

仓库的维护者可以将问题分配给个人或团队，并根据其严重性和影响为其打标签。

简而言之，GitHub 问题是用于软件项目管理的工具，有助于简化沟通和协作。例如，最受欢迎的仓库有问题模板，确保提出的问题是有用的、可审查的和可回答的。

这些模板旨在帮助你提出有针对性的问题，而不是漫无目的或无焦点的问题，这有助于加快问题解决的过程。

## 6\. GitHub Actions

GitHub Actions 的兔子洞非常深。但我们只会在表面浅尝辄止，学习一些基础知识。

作为数据科学家，你有无数事情要做，比如分析数据、构建模型、编写代码、喝咖啡……所有这些可能会变得相当重复和压倒性。

但如果你了解 GitHub Actions，它就像是一个处理所有仓库相关任务的个人助理。通过它们，你可以设置在特定事件发生时自动运行的工作流程。

例如，你可以创建一个动作，在训练新模型时自动验证模型在测试数据上的性能。或者，你可以设置一个动作，当你推送新的仓库发布时部署模型。

正如你所想象的，还有许多其他工作流程可以这样自动化。以下是一些示例：

1.  运行测试 — 每当你推送新代码时，自动运行单元测试或集成测试。

1.  训练模型 — 定期（例如，每日、每周等）重新训练现有的机器学习模型。

1.  执行预处理 — 运行常见的处理任务，如数据清理、归一化、缺失值插补、特征提取等。

GitHub Actions 很容易设置。它们被定义在 YAML 文件中。下面是一个运行单元测试的动作示例 YAML 文件：

```py
name: Run Tests

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  tests:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2
    - name: Set up Python 3.9
      uses: actions/setup-python@v2
      with:
        python-version: '3.9'
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt
    - name: Run unit tests
      run: |
        python -m pytest tests/
```

要理解上述内容，可以阅读官方 GitHub 文档 [这里](https://docs.github.com/en/actions)。

## 7\. CI/CD 管道

CI/CD（持续集成和开发）是从软件工程中的 DevOps 概念中借用的。

CI/CD 管道是一系列自动化步骤，你的代码会经过这些步骤，以确保在发布到生产环境之前达到最佳状态。我们可以应用相同的过程，确保我们的机器学习模型及其训练/部署代码的最高质量。

![](img/3ec8dca1272999e69495ac00f1223203.png)

图片由 [Made With ML](https://github.com/GokuMohandas/Made-With-ML/blob/main/images/mlops/cicd/workflows.png) 提供。MIT 许可。

换句话说，CI/CD 管道就像运转良好的机器，每当你做出新的提交时，它们就会开始运转。在提交合并之前，它会经历一系列的步骤，在多个方面测试代码和模型。

你猜对了，管道中的每一步都被定义为一个 GitHub 动作。让我们从直升机的视角来看看一个示例机器学习生命周期 CI/CD 管道：

1.  数据收集和预处理 — 从各种来源收集数据并在训练前进行转换。

1.  模型训练 — 机器学习模型在预处理的数据上进行训练。

1.  模型评估 — 在一组单独的数据上评估训练好的模型，以评估其性能。

1.  模型部署 — 最佳性能的模型被部署到生产环境。

1.  持续监控 — 部署的模型被持续监控，以确保其表现符合预期。

每一步都值得独立成文或甚至开设完整的课程。但简而言之，CI/CD 管道在大多数（活跃的）机器学习项目中是默默无闻的英雄。

## 结论

在我的 Git 文章中，我提到 Git 是开发机器学习系统中最关键的工具之一，因为它是协作的首选软件。

同样的论点适用于 GitHub，因为没有它，Git 的协作特性无法真正展现。GitHub 提供了一个存储 Git 跟踪的仓库的地方，以便其他人可以访问它。

尽管 Git 和 GitHub 最初并未专为数据科学和机器学习工作设计，但现在有了像[DVC](https://medium.com/towards-data-science/how-to-version-gigabyte-sized-datasets-just-like-code-with-dvc-in-python-5197662e85bd)和[DagsHub](https://medium.com/towards-data-science/the-easiest-way-to-deploy-your-ml-dl-models-in-2022-streamlit-bentoml-dagshub-ccf29c901dac)这样的辅助工具，使得它们在实际操作中非常相关。

感谢阅读！

喜欢这篇文章和它那奇特的写作风格？想象一下，拥有更多这样的文章，全部由一位才华横溢、迷人、有趣的作者（顺便说一句，就是我:)）编写。

仅需 4.99 美元的会员费用，你不仅能访问我的故事，还能获取来自 Medium 上最优秀智慧的知识宝库。如果你使用[我的推荐链接](https://ibexorigin.medium.com/membership)，你将获得我**超级 nova 的感激**和一个虚拟的击掌，以支持我的工作。

[](https://ibexorigin.medium.com/membership?source=post_page-----c082b1e882e9--------------------------------) [## 使用我的推荐链接加入 Medium — Bex T.

### 获取对我所有⚡优质⚡内容的独家访问权限，并在 Medium 上无限制地浏览。通过购买我一杯…来支持我的工作。

[ibexorigin.medium.com](https://ibexorigin.medium.com/membership?source=post_page-----c082b1e882e9--------------------------------) ![](img/a01b5e4fb641db5f35b8172a4388e821.png)

图片由我提供。通过 Midjourney 制作。
