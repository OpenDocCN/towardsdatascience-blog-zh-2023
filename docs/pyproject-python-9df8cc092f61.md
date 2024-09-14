# Python 中的 pyproject.toml 是什么

> 原文：[`towardsdatascience.com/pyproject-python-9df8cc092f61`](https://towardsdatascience.com/pyproject-python-9df8cc092f61)

## 管理 Python 项目依赖的 `pyproject.toml` 文件

[](https://gmyrianthous.medium.com/?source=post_page-----9df8cc092f61--------------------------------)![Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----9df8cc092f61--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9df8cc092f61--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9df8cc092f61--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----9df8cc092f61--------------------------------)

·发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9df8cc092f61--------------------------------) ·5 分钟阅读·2023 年 5 月 9 日

--

![](img/f52d5d1a92753c16cb2f548c0ac9aecd.png)

图片由 [Fré Sonneveld](https://unsplash.com/@fresonneveld?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/s/photos/chain?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

Python 的依赖管理既复杂又令人沮丧。新手通常倾向于安装他们认为有用的任何依赖（即包），即使是在一个虚拟环境中。因此，这种方法增加了依赖包冲突的可能性，并最终陷入所谓的 **依赖地狱**。

在我之前的几篇文章中，我们介绍了几种处理 Python 项目依赖的方法，包括 `setup.py`、`setup.cfg` 和 `requirements.txt` 文件。然而，从 Python 3.6 开始，推出了一种新的标准配置文件 `pyproject.toml`，旨在简化用户管理依赖和元数据定义的方式。

在过去几年中，`pyproject.toml` 文件已成为管理 Python 项目依赖的标准（也是最受欢迎的）方式。在接下来的几个部分中，我们将深入探讨如何使用该文件实现依赖管理。此外，我们还将演示如何在可编辑模式下安装具有`pyproject.toml` 规范的项目。

[**订阅数据管道**](https://thedatapipeline.substack.com/welcome)**，这是一个专注于数据工程的新闻通讯**

## pyproject.toml 之前的依赖管理

当 Python 首次发布时，用于构建发行版的事实标准工具是 `distutils`。随着时间的推移，`setuptools` 出现，旨在在 `distutils` 的基础上构建额外功能。这两个工具都使用了一个 `setup.py` 文件，用户可以在其中指定依赖项和用于软件包构建分发的元数据。

然而，这造成了一个问题，因为任何选择使用 `setuptools` 的项目必须在 `setup.py` 文件中导入该包。因此，`setup.py` 在不知道其依赖项的情况下不能执行，但同时，该文件的目的就是确定这些依赖项。这就是我们在 Python 依赖管理中遇到所谓的鸡和蛋问题的原因。

我希望这些信息足以让你理解为何需要一种新的方法。如果你有兴趣了解有关 `setuptools` 和 `pip` 的鸡和蛋问题的更详细解释，确保阅读 [PEP-518](https://peps.python.org/pep-0518/#rationale)。

新提案（PEP-518 的一部分）旨在为 Python 项目指定一种新的方式来提前列出其依赖项，以便像 `pip` 这样的工具可以确保在项目构建之前安装它们。

## pyproject.toml

`pyproject.toml` 文件作为 Python 增强提案 [(PEP) 518](https://peps.python.org/pep-0518/) 的一部分被引入，规定了 Python 项目必须如何指定构建依赖项。

这些构建依赖项将被存储在位于项目根目录的文件中，该文件遵循 TOML（Tom’s Obvious, Minimal Language）语法。

它包含了元数据，例如项目名称、版本、描述、作者、许可证以及各种其他细节。

`pyproject.toml` 文件的一个关键特性是能够定义项目依赖项。这允许开发人员指定运行项目所需的包及其版本。这有助于保持项目的一致性，并确保其他开发人员可以轻松地重现该项目。

`pyproject.toml` 文件还支持 `extras` 概念，允许开发人员为项目定义可选依赖项。这使得用户可以仅安装运行项目所需的必要依赖项。通常，在 `extras` 部分可以指定作为测试一部分的额外需求（例如 `pytest`）。

除了标准的元数据和依赖项外，`pyproject.toml` 文件还支持自定义字段，第三方工具可以使用这些字段。例如，你可以考虑使用 `black` 和 `mypy` 等代码检查工具、格式化工具和校验工具。这允许开发人员扩展文件的功能，并根据需要添加自定义字段。

## 管理 `pyproject.toml` 中的依赖项

`pyproject.toml` 可以与包依赖管理工具一起使用，例如 `setuptools` 和 `poetry`。

这是一个使用 `poetry` 的项目示例文件：

```py
[build-system]
requires = ["poetry-core>=1.0.0"]
build-backend = "poetry.core.masonry.api"

[tool.poetry]
name = "my-project"
version = "1.0.0"
description = "My Python project"
authors = ["John Doe <john@doe.com>"]
license = "MIT"

[tool.poetry.dependencies]
python = "³.6"

[tool.poetry.dev-dependencies]
pytest = "⁴.6"

[tool.poetry.extras]
docs = ["sphinx"]
```

这是一个使用 `setuptools` 的示例：

```py
[build-system]
requires = ["setuptools"]
build-backend = "setuptools.build_meta"

[project]
name = "my_package"
description = "My package description"
readme = "README.rst"
requires-python = ">=3.7"
keywords = ["one", "two"]
license = {text = "BSD 3-Clause License"}
classifiers = [
    "Framework :: Django",
    "Programming Language :: Python :: 3",
]
dependencies = [
    "requests",
    'importlib-metadata; python_version<"3.8"',
]
dynamic = ["version"]

[project.optional-dependencies]
pdf = ["ReportLab>=1.2", "RXP"]
rest = ["docutils>=0.3", "pack ==1.1, ==1.3"]

[project.scripts]
my-script = "my_package.module:function"
```

## 从 `pyproject.toml` 安装项目的可编辑模式

如果你在积极开发一个项目，你可能希望将项目本地安装为可编辑模式。当从特定位置以可编辑模式安装包时，对源代码的任何更改会立即在环境中反映出来（而无需重新安装“新”版本）。

假设你正在使用 `poetry` 来管理你的 Python 依赖，并且为了以可编辑模式安装 Python 项目，你需要在 `pyproject.toml` 文件中包含以下内容

```py
[build-system]
requires = ["poetry-core>=1.0.8"]
build-backend = "poetry.core.masonry.api"
```

从项目的根目录，只需运行

```py
$ pip install -e .
```

另外，`poetry install` 也会导致可编辑安装。你可以在我最新的文章中了解更多关于如何使用 Poetry 管理 Python 项目依赖的内容：

[](/python-poetry-83f184ac9ed1?source=post_page-----9df8cc092f61--------------------------------) ## 使用 Poetry 管理 Python 依赖

### 使用 Poetry 进行依赖管理和打包

towardsdatascience.com

## 最后的思考

在今天的文章中，我们讨论了在管理依赖和在社区中分发项目时 `pyproject.toml` 在 Python 中的使用。

总体而言，`pyproject.toml` 提供了一个标准且易于使用的 Python 项目配置。它简化了定义元数据和依赖的过程，并确保项目可以被其他开发者轻松重现。

[**订阅数据管道**](https://thedatapipeline.substack.com/welcome)**，一个专注于数据工程的新闻通讯**

**相关的文章你可能也喜欢**

[](/setuptools-python-571e7d5500f2?source=post_page-----9df8cc092f61--------------------------------) ## setup.py 与 setup.cfg 在 Python 中的区别

### 使用 setuptools 管理依赖和分发 Python 包

towardsdatascience.com [](/requirements-vs-setuptools-python-ae3ee66e28af?source=post_page-----9df8cc092f61--------------------------------) ## requirements.txt 与 setup.py 在 Python 中的区别

### 理解在 Python 开发和分发中 `requirements.txt`、`setup.py` 和 `setup.cfg` 的目的……

towardsdatascience.com [](/python-poetry-83f184ac9ed1?source=post_page-----9df8cc092f61--------------------------------) ## 使用 Poetry 管理 Python 依赖

### 使用 Poetry 进行依赖管理和打包

towardsdatascience.com
