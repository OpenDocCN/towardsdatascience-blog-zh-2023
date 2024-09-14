# 无需多言：自动化开发环境和构建

> 原文：[`towardsdatascience.com/without-further-ado-automate-dev-environments-and-build-f2f9bcaaae1e`](https://towardsdatascience.com/without-further-ado-automate-dev-environments-and-build-f2f9bcaaae1e)

## 通过环境和构建自动化使你的软件易于使用，给你的开发同事带来快乐。包括 Python 和 Hatch 中的代码示例。

[](https://medium.com/@mattiadigangi?source=post_page-----f2f9bcaaae1e--------------------------------)![Mattia Di Gangi](https://medium.com/@mattiadigangi?source=post_page-----f2f9bcaaae1e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f2f9bcaaae1e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f2f9bcaaae1e--------------------------------) [Mattia Di Gangi](https://medium.com/@mattiadigangi?source=post_page-----f2f9bcaaae1e--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f2f9bcaaae1e--------------------------------) ·11 分钟阅读·2023 年 2 月 24 日

--

![](img/9b740db055fe462f411ece0c9cc9fe21.png)

图片来源：[Carol Jeng](https://unsplash.com/@carolran?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

大多数开发人员讨厌遗留软件，为什么？在我们的行业中，“遗留”意味着一个已经服务了多年的代码库，通常原始开发者不再在公司，而没有人能够真正维护它。

遗留软件配方中的一些重要成分是：缺乏文档、难以理解的代码、**尝试修改**的困难，以及**构建软件以进行发布**的严重困难。

然而，遗留软件之所以被称为遗留，是因为公司依赖它，这也是为什么尽管大多数开发人员不知道如何处理它，它仍然在使用。我们从过去的同事那里继承了它，因此我们有责任让它在未来继续像过去一样工作。

然而，有一些软件满足上述遗留软件的要求，但却没有服务多年的优点。部分代码也相当近期。这意味着，人们编写的代码已经根据其负面定义成为“遗留”代码。

# 不愉快的一天

今天是星期一，你被分配了一个新项目。在早上 9 点的会议上，你了解到你现在在**Petty**工作，这是一个用于搜索宠物图片的新公司产品。你的第一个任务是开发**Petognizer**，一个识别图片中宠物的组件。它目前在生产中，但只能识别猫和狗。现在管理层希望它也能识别仓鼠和兔子。

你得到了公司 GitLab 账户中的仓库链接，并准备使用它。

结果是你不能。

这个仓库没有 README，也没有叫做`docs`或类似的文件夹。看来你需要自己进行一些探索。

现在是 10:30，你开始挖掘仓库。经过 30 分钟你明白了在哪里可以找到训练代码、数据预处理脚本和推理代码。这看起来过于复杂，但现在不是进入细节的时候，你想对它有一个整体的了解。

你还发现了一个可能用于生产的 Dockerfile，但这个并没有明确写在任何地方，还有一个`requirements.txt`，这当然是一个 Python 项目，因为它涉及到机器学习。

现在是 11:30，你感觉准备好了要尝试一下。你知道新的数据集在哪里，所以现在你想开始数据准备流程。于是，你想安装包，却发现缺少`setup.py`和`pyproject.toml`。你问你的同事是否这个包在公司私有的`pypi`服务器上，但得到的回答是否定的。毕竟，如果没有构建说明，它怎么可能被打包？

显然，你不得不走“脏活”的路。你下载了仓库，找到了数据准备的入口点，这不在`__main__.py`文件中，将项目的根文件夹添加到你的`PYTHONPATH`环境变量中。然后，你创建一个新的 python 环境，使用`venv`，运行`pip install -r requirements.txt`来安装所有必要的依赖，运行脚本，然后……瞧！

`ImportError: module xxx not found`

你以为`requirements.txt`会提供开发依赖，但显然不是。但现在你看了看手表，发现已经一点钟了，于是决定和同事一起去吃午饭。你会在之后搞清楚。

当你从午休回来时，你再次充满活力，准备再次攻克这个问题。

现在你的工作是一个一个地安装缺失的依赖，每次安装一个库后你重新运行它，等待一会儿，接着下一个 ImportError 就会出现。你在 3 点钟前安装完所有东西，仍然在想`requirements.txt`到底是用来干什么的。

你已经在这个过程中几个小时了，你仍然不确定是否真的可以运行这个软件来满足你的需求。而你到目前为止的工作感觉完全浪费了。

![](img/858ac3fa9a12e2b7106605e07c4b9b9a.png)

图片来源：[Anthony Intraversato](https://unsplash.com/@anthonyintraversato?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在另一个平行世界中，你简单地运行了

```py
pip install petognizer
```

或者，也许，如果训练依赖作为额外的提供

```py
pip install petognizer[train]
```

然后在 README 中找到了数据准备的命令，你在大约 10 分钟内准备好，一切都在午休前完成。

如果有一个`pyproject.toml`文件，将会大大简化包的安装（如果它在 pypi 服务器上会更容易）以及准备构建环境的过程。此外，一些适当的文档将使得了解如何实际使用代码进行不同任务变得更加简单。

每天都有如此多才华横溢的开发人员浪费时间，只因为某些代码维护者没有花时间设置文档和自动化，以提供良好开发体验的基础。

现在你负责这个代码库，你可以选择保持原样，把同样的挫败感留给下一个将要使用它的开发人员。或者，你可以决定为组织带来一些欢乐，使得代码库更容易使用。

虽然编写文档是一个需要长期投入的过程，需要对代码有深入理解（但一些正确的文档总比没有文档要好），自动化构建和开发环境的创建可以解放开发人员免于很多麻烦，使他们能更快地在代码库中提高生产力。

[](/tips-for-reading-and-writing-an-ml-research-paper-a505863055cf?source=post_page-----f2f9bcaaae1e--------------------------------) ## 阅读和编写 ML 研究论文的技巧

### 通过多次的同行评审获得的经验教训

towardsdatascience.com

# Hatch

[Hatch](https://hatch.pypa.io/latest/)是一个由 Python 打包机构（PyPA）支持的自动化工具，它能够以高度的简便性创建环境和构建软件。

它使用一个名为`pyproject.toml`的文件，这是“新”的统一 Python 项目设置文件。之所以称之为统一，是因为它被 Hatch 和其他类似工具（如[Poetry](https://python-poetry.org/)）使用，并且它可以指定其他工具的配置，例如[black](https://github.com/psf/black)或[flake8](https://github.com/PyCQA/flake8)等。

我们可以使用 Hatch 创建一个新项目，命令为

```py
hatch new "My Project"
```

它将创建一个标准的项目结构，如

```py
my-project
├── src
│   └── my_project
│       ├── __about__.py
│       └── __init__.py
├── tests
│   └── __init__.py
├── LICENSE.txt
├── README.md
└── pyproject.toml
```

它包含两个独立的包：一个用于源代码，另一个用于测试，还有一个包含一些默认文本的 README.md，一个 LICENSE.txt，一个名为`__about__.txt`的文件，里面包含软件版本，以及`pyproject.toml`。

Hatch 也可以通过简单运行来用于现有项目

```py
hatch new --init
```

在其根文件夹中。这将生成一个默认的`pyproject.toml`文件，需要进行修改。

可以通过`pipx install hatch`来安装 Hatch，使其作为用户命令可用，而不会“污染”现有的默认 Python 环境。

[](/python-polymorphism-with-class-discovery-28908ac6456f?source=post_page-----f2f9bcaaae1e--------------------------------) ## 带有注册表的 Python 多态性 | Python 模式

### 学习一种模式来隔离包，同时扩展 Python 代码的功能。

towardsdatascience.com

## pyproject.toml 的快速浏览

Hatch 文档非常详细，尽管有时清晰度可以提高，因此在这里我们将专注于与本文主题相关的部分和一些必要的背景信息。

`pyproject.toml` 带有一个初始部分，描述性很强。在这里我们可以找到项目的名称、描述、作者、支持的 Python 版本和版本。

以我为说明制作的这个玩具库为例 [`github.com/mattiadg/demo_it-analyze/blob/main/pyproject.toml`](https://github.com/mattiadg/demo_it-analyze/blob/main/pyproject.toml) （感谢我的朋友 [Mario](https://github.com/Marius0092) 帮助制作这个小应用的前端）

```py
[project]
name = "demo-it-analyze"
description = ''
readme = "README.md"
requires-python = ">=3.7"
license = "MIT"
keywords = []
authors = [
  { name = "Mattia Di Gangi", email = "mattiadg@users.noreply.github.com" },
]
classifiers = [
  "Development Status :: 4 - Beta",
  "Programming Language :: Python",
  "Programming Language :: Python :: 3.7",
  "Programming Language :: Python :: 3.8",
  "Programming Language :: Python :: 3.9",
  "Programming Language :: Python :: 3.10",
  "Programming Language :: Python :: 3.11",
  "Programming Language :: Python :: Implementation :: CPython",
  "Programming Language :: Python :: Implementation :: PyPy",
]
dependencies = [
  "flask",
  "pydantic",
  "spacy",
]
dynamic = ["version"]

[project.urls]
Documentation = "https://github.com/unknown/demo-it-analyze#readme"
Issues = "https://github.com/unknown/demo-it-analyze/issues"
Source = "https://github.com/unknown/demo-it-analyze"
```

应该很清楚，除了两个字段。`dynamic = ["version"]` 表示软件版本需要动态计算，稍后文件中提到它必须在 `__about.py__` 文件中找到。另一方面，字段 `dependencies` 列出了**包**的依赖关系。这些是与软件一起安装的包。当打包时，我这里只列出了名称，但也可以应用一些版本约束，如 `==`、`<=` 等… 它们与**pip**使用的相同。

请注意，这些依赖项是在 `[project]` 标签下指定的，但还有其他仅为某些环境指定的依赖项。然而，这些是包和所有环境共同的。代码库没有它们就无法工作。

## 工具配置

如前所述，`pyproject.toml` 可用于指定项目中使用的工具的配置，而 Hatch 是可以在这里配置的工具之一。

```py
[tool.hatch.version]
path = "src/__about__.py"

[tool.hatch.envs.default.env-vars]
  FLASK_APP = "demo_it_analyze/app.py"

[tool.hatch.envs.default]
extra-dependencies = [
  "pytest",
  "pytest-cov",
  "mypy",
]

[tool.hatch.envs.default.scripts]
cov = "pytest --cov-report=term-missing --cov-config=pyproject.toml --cov=demo_it_analyze --cov=tests {args}"
no-cov = "cov --no-cov {args}"
download_ita = "python -m spacy download it_core_news_sm"
serve = "flask --app src.app run"

[tool.hatch.envs.train]
template = "default"
extra-dependencies = [
  "spacy[cuda-autodetect,transformers,lookups]"
]

[tool.hatch.envs.test]
template = "default"

[[tool.hatch.envs.test.matrix]]
python = ["37", "38", "39", "310", "311"]
```

请注意，这里的字段以 `tool.hatch` 开头，而不是 `project`。

我们首先告诉 Hatch 到哪里去寻找软件版本。然后，我们为**默认** Python 环境指定 `FLASK_APP`。Hatch 允许我们创建多个环境，默认环境是始终定义的，并且所有其他环境都从它派生。我们可以为环境定义依赖项，这些是**开发**依赖项，因为环境在包代码中不存在。

使用 Hatch，我们可以通过执行来创建默认环境

```py
hatch env create 
```

它将创建具有所有依赖项的默认环境。我们还可以通过运行指定在 `pyproject.toml` 中的另一个环境（例如示例中也定义了 `test`）来创建另一个环境

```py
hatch env create test
```

然而，Hatch 的一个好处是我们不需要显式创建环境。命令 `hatch run` 允许我们在环境中运行任何命令或脚本。在上面复制的文件中，我们有这个脚本定义：

```py
[tool.hatch.envs.default.scripts]
cov = "pytest --cov-report=term-missing --cov-config=pyproject.toml --cov=demo_it_analyze --cov=tests {args}"
no-cov = "cov --no-cov {args}"
download_ita = "python -m spacy download it_core_news_sm"
serve = "flask --app src.app run"
```

因此，我们可以运行，例如

```py
hatch run serve
```

`hatch` 会创建默认环境（如果不存在的话），**同步其依赖**与 `pyproject.toml` 中指定的依赖，并在环境中运行 `flask --app src.app run`，启动我们的 flask 服务器。我们还可以指定在特定环境中运行命令，例如 `test`。

```py
hatch run test:serve
```

注意，我们不需要安装任何东西。`Hatch` 会读取 `pyproject.toml` 并识别可以运行的脚本。

你看到我们在这里做了什么吗？项目维护者在一个文件中指定了一些配置选项，然后对任何使用该仓库的人来说，创建完全相同的环境变得非常简单。不再为重现开发环境而苦恼！

[](/data-processing-automation-with-inotifywait-663aba0c560a?source=post_page-----f2f9bcaaae1e--------------------------------) ## 使用 inotifywait 进行数据处理自动化

### 如何在拥有一个生产就绪的 MLOps 平台之前进行自动化

[towardsdatascience.com

## 构建

`Hatch` 也是一个不错的构建工具。它有自己的构建后端，称为**hatchling**，而 `pyproject.toml` 附带了一些构建配置。根据示例：

```py
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.build.targets.sdist]
exclude = [
  "/.github",
  "/docs",
]

[tool.hatch.build.targets.wheel]
packages = ["src"]
```

我们必须指定构建后端，因为 `Hatch` 可以作为所有功能的良好前端，然后用其他工具进行构建。然后我们看到可以为 `sdist` 和 `wheel` 两种构建格式指定一些选项。然而，对于像这样的玩具项目，里面的内容不多。

然后，我们可以简单地运行 `hatch build`，它会构建两个目标。或者我们可以用 `hatch build -t wheel` 来指定其中一个目标，例如。

当你拥有 pypi 服务器的凭据时，无论是官方的、测试服务器的还是私有服务器的，你都可以通过简单地使用 `hatch publish` 从构建中发布你的包。

当然，发布包是一项需要谨慎处理的工作，在确保软件正常工作后进行。但随后，`hatch` 处理所有细节，我们只需要运行两个简单的命令。

如果你启用了类似**Github Actions**的 CI/CD 流水线，你也可以在那里指定这些命令，以实现更多的自动化！

# 我们没有覆盖的内容

使代码库易于使用可能是一个具有挑战性的问题，虽然构建自动化是朝着正确方向迈出的有用一步，但这远未满足所有需求。

我们在介绍故事中已经提到过文档，我们将不再详细讨论它。

另一个重要方面是彻底的测试套件，它有三个主要目的。

首先，它有助于验证代码是否正确。任何人都可以阅读测试并检查它们是否证明了正确的内容。如果测试通过，那么我们对代码的正确性有了一些保障。

其次，它们作为防止回归的安全网。当我们修改一个代码库时，特别是一个我们不太熟悉的代码库，全面的测试套件可以捕捉到修改是否破坏了预期的行为。

第三，如果文档不够详尽，测试可以展示我们难以理解的函数或类的使用方法，因为测试本质上就是代码示例。

另一个重要的遗漏部分是持续集成/持续交付管道（CI/CD），它在每次代码添加到远程仓库时运行一些检查，还可以构建和部署软件。CI/CD 管道展示了代码库中预期的代码质量，因为它可以运行诸如代码检查工具或静态类型检查器（针对像 Python 这样的动态语言）以及测试。管道中的构建和部署步骤还确保这些操作容易执行，并具有足够的自动化水平。

# **结论**

Hatch 是一个出色的自动化工具，虽然需要一些维护和配置选项的指定，但它通过显著简化环境和构建创建，带来了极大的回报。

当开发者可以轻松地使用软件，而无需花费数小时去弄清楚如何设置时，他们更容易提高生产力，同时也能找出文档中遗漏的部分，因为他们可以运行代码。

感谢您阅读到这里，这篇文章较长，但我希望我能说服您，使用像 Hatch 这样的工具可以大大提升您和其他开发者的开发体验。

# 参考文献

一些书籍对这篇文章产生了深远的影响。最突出的书是[独角兽项目](https://itrevolution.com/product/the-unicorn-project/)，由著名的 DevOps 作者[Gene Kim](https://itrevolution.com/author/gene-kim/)编写，并由 IT Revolution 编辑。一个高级开发者被流放到一个功能失调的部门，在那里开发者无法本地运行代码。在小说中，她和一群开明的同事尽力解救她的同事，摆脱成千上万的阻碍和官僚主义，去做他们真正被雇佣来做的事情。

[深入理解持续交付](https://www.manning.com/books/grokking-continuous-delivery)由 Christie Wilson 编写，并由 Manning 编辑，是一本从零开始建立 CI/CD 管道并在过程中改进代码库的指南。

[重构遗留系统](https://www.manning.com/books/re-engineering-legacy-software)由 Chris Birchall 编写，并由 Manning 编辑，全面探讨了遗留软件，包括如何处理我们被分配的遗留系统，如何改进它，以及如何确保我们不会编写遗留软件。

# Medium 会员

您喜欢我的写作并考虑订阅 Medium 会员以无限制访问文章吗？

如果你通过此链接订阅，你将通过你的订阅支持我，而无需支付额外费用 [`medium.com/@mattiadigangi/membership`](https://medium.com/@mattiadigangi/membership)
