# 创建一个优秀 PyPI 包的最完整指南

> 原文：[`towardsdatascience.com/the-complete-guide-for-creating-a-good-pypi-package-acb5420a03f8`](https://towardsdatascience.com/the-complete-guide-for-creating-a-good-pypi-package-acb5420a03f8)

## 你需要了解的所有内容——无论是新手还是经验丰富的用户

[](https://eliselandman.medium.com/?source=post_page-----acb5420a03f8--------------------------------)![Elise Landman](https://eliselandman.medium.com/?source=post_page-----acb5420a03f8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----acb5420a03f8--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----acb5420a03f8--------------------------------) [Elise Landman](https://eliselandman.medium.com/?source=post_page-----acb5420a03f8--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----acb5420a03f8--------------------------------) ·11 分钟阅读·2023 年 3 月 27 日

--

![](img/726e161623b0a5e82e1c2c669eee378e.png)

图片来源于 [Unsplash](https://unsplash.com/photos/DYHx6h3lMdY)。

**Python 包索引**（或[PyP](https://pypi.org/)I）是第三方**Python 包**的官方**软件库**。它是 Python 程序员获取任何预构建和可重用的软件组件的一站式商店——让我们的编码生活更加轻松和高效。一个简单的`pip install`就可以轻松下载并安装任何 PyPI 包。

在对[我自己的 PyPI 包](https://github.com/elisemercury/Duplicate-Image-Finder)进行两年的持续开发和更新后，我学到了许多希望一开始就知道的东西。在本指南中，我将**分享我所有的经验和技巧**，这些可以**让你的 PyPI 包上传**和**维护**变得**更加成功**。

本指南分为两个部分：

👉 如果你是*PyPI 新手*，并且这是你的第一个包，从以下部分开始：**1 | 为 PyPI 准备你的包**

👉 如果你*已经是 PyPI 用户*并且已经上传了你的包，从以下部分开始：**2 | 上传和维护你的 PyPI 包**

# 1 | 为 PyPI 准备你的包

当你将你的包上传到[PyPI](https://pypi.org/)时，你是让任何人都可以通过`pip`安装器进行安装。任何人都可以运行以下命令：

```py
pip install your_package
```

并且它将被安装到他们的机器上。为了让你的**代码适合 PyPI**，你需要遵循一些步骤：

首先，你需要[**创建一个 PyPI 账户**](https://pypi.org/account/register/)。我强烈建议启用双因素认证以增强账户安全。

其次，你需要**准备包的文件夹结构**并添加一些必需的文件。这些文件包括：

1.  `__init__.py`文件将初始化你的包。

1.  一个包含你的包版本的`version.py`文件。

1.  实际构建 Python 包所需的文件：`setup.py`、`pyproject.toml`、`setup.cfg`和`MANIFEST.in`。

1.  `LICENSE.txt`文件。

1.  以及`README.md`文件。

在添加了所有这些文件后，你的包的**最终文件结构**将如下所示：

```py
.
|- your_package
|  |- __init__.py
|  |- your_package.py
|  |- version.py
|- setup.py
|- pyproject.toml
|- setup.cfg
|- MANIFEST.in
|- LICENSE.txt
|- README.md
```

只需稍加想象，你的包很快就会类似于下图：

![](img/201500802c8dd528773e9e40676f2052.png)

示例 PyPI 包。图片来自[PyPI.org](https://pypi.org/)由作者提供。

一起**逐个查看**所需的文件，了解我们为什么需要它们，以及它们可能的样子。

## 1.1 | __init__.py

这个文件将**标记你的文件夹为 Python 包**，并确保在启动时你的包能被正确导入。`__init__.py`文件的示例如下：

```py
# __init__.py
from .version import __version__
from .your_package import your_package
```

`your_package.py`是你的包的主要脚本，它将通过`__init__.py`文件被导入。当然，你也可以根据需要导入多个主要脚本。

## 1.2 | your_package.py

如*1.1*节中提到的，`your_package.py`是你应该已经创建的包的主要**脚本**。如果需要，你可以在包中包含**多个脚本**。

## 1.3 | version.py

通常，流行的 Python 包允许你通过调用`.__version__`函数来**查找它们的版本**。例如，你可以通过执行以下代码行来查找你机器上安装的[Pandas](https://pandas.pydata.org/)版本：

```py
import pandas as pd
pd.__version__

> '1.5.3'
```

为了确保你的包也能完成相同的操作，你需要在包文件夹中包含一个`version.py`文件。这个文件可以像下面这样简单：

```py
# version.py
__version__ = "1.0.0"
```

## 1.4 | setup.py 和 pyproject.toml

`setup.py`文件和`pyproject.toml`文件是用于**构建你的包**的配置文件。`pyproject.toml`是随着[PEP 517](https://peps.python.org/pep-0517/)和[PEP 518](https://peps.python.org/pep-0518/)标准引入的新型配置文件。

*那么，什么时候用哪个呢？*

当你想创建一个 Python 包时，首先必须构建它。有各种流行的 Python 构建系统，例如`setuptools`就是其中之一。但在某些情况下，你可能会选择使用其他工具，如[Poetry](https://python-poetry.org/)、[Flit](https://flit.pypa.io/en/stable/)或[Bento](https://cournape.github.io/Bento/)。当使用**其他**打包工具时，`pyproject.toml`会替代`setup.py`文件。

当使用`setuptools`作为构建系统时，建议包括`setup.py`文件和`pyproject.toml`文件。如果你使用其他构建系统，通常只需要`pyproject.toml`文件。对于本指南，我们将使用`setuptools`，因此我强烈建议**在你的包中包含这两个文件**。有关更多详细信息，我建议查看[这篇文章](https://stackoverflow.com/questions/62983756/what-is-pyproject-toml-file-for)。

下面是一个`setup.py`的模板，你可以编辑并重复使用：

```py
# setup.py
from setuptools import setup

if __name__ == "__main__":
  setup(packages = ['your_package'])
```

其次，下面是`pyproject.toml`的模板：

```py
# pyproject.toml
[build-system]
requires = ["setuptools >= 40.8.0", "wheel"]
```

## 1.5 | setup.cfg

这个文件是必需的，以便在构建你的包时**包含元数据**。例如：包的名称、作者的名字、主页 URL、支持的 Python 版本等。它还确保 PyPI 使用你的`README.md`作为包描述，并引用`LICENSE.txt`文件。

```py
# setup.cfg
[metadata]
name = your_package
version = attr: your_package.version.__version__
author = FirstName LastName
author_email = youremail@email.com
keywords = tags, that, describe, your, package

# add a description of your package
description = A description of your package.
long_description = file: README.md
long_description_content_type = text/markdown

# add the url to your repository/homepage and version release
url = https://github.com/username/your-package
download_url = https://github.com/username/your-package/archive/refs/tags/v1.0.0.tar.gz

# define your license and license file
license = MIT
license_files = LICENSE.txt

classifiers = 
    # for a full list of classifiers see https://pypi.org/classifiers/
    Development Status :: 5 - Production/Stable
    Intended Audience :: Developers      
    Topic :: Software Development :: Build Tools
    License :: OSI Approved :: MIT License
    Programming Language :: Python
    Programming Language :: Python :: 3.8
    Programming Language :: Python :: 3.9
    Programming Language :: Python :: 3.10
    Programming Language :: Python :: 3.11

[options]
install_requires =
    # list the dependencies required by your package
    numpy >=1.24.2
    matplotlib >= 3.7.0 

python_requires = >=3.8
```

## 1.6 | MANIFEST.in

你可能创建了一个由**多个不同脚本**组成的包，或者包含**主脚本使用的附加文件**。例如，你的包可能看起来像这样：

```py
.
|- your_package
|  |- __init__.py
|  |- your_package.py
|  |- **additional_script.py**
|  |- version.py
|  |- **additional_file.csv**
|- setup.py
|- pyproject.toml
|- setup.cfg
|- MANIFEST.in
|- LICENSE.txt
|- README.md
```

默认情况下，当上传到 PyPI 时，只有在`__init__.py`文件中引用的脚本会被包含在上传的包中。如果我们希望包含其他脚本或文件，需要在此处进行引用：

```py
# MANIFEST.in
include your_package/addtional_script.py
include your_packager/addtional_file.csv
```

## 1.7 | LICENSE.txt

这个文件将包含你**分发包的许可**。选择使用哪个许可取决于你，不过，确保尊重你包所依赖的包的许可。如果你使用了已经有版权的包，你可能不能对你的包进行版权保护。

对于**开源软件**，常用的许可是 MIT。你可以在[这里](https://opensource.org/license/mit/)找到 MIT 许可内容的示例。

## 1.8 | README.md

`README.md`文件可以用作**你包的描述**（即 PyPI 页面内容）。在本指南中，我们使用基于 Markdown 的`README`，但你可以选择任何[支持的标记语言](https://packaging.python.org/en/latest/guides/making-a-pypi-friendly-readme/#creating-a-readme-file)。如果你决定更改标记类型，请确保在`setup.cfg`文件中调整`long_description_content_type`变量。

你可以使用与 GitHub 仓库中相同的`README`，但要注意 PyPI 的 Markdown 与 GitHub 的 Markdown 不完全相同，可能在使用[表情符号](https://github.com/ikatyang/emoji-cheat-sheet/blob/master/README.md)或[徽章](https://github.com/Ileriayo/markdown-badges)时会遇到格式问题。

在添加所有必需的文件后，你的包已经**准备好上传到 PyPI**了！🚀

# **2 | 上传和维护你的 PyPI 包**

如果你的包已经完全更新并准备好发布到 PyPI，那么接下来的部分正适合你。下面你将找到一些避免常见错误和陷阱的提示和建议。应用这些建议将使你的 PyPI 包的上传和更新周期更加顺畅，最终效果更好。

## 2.1 | 首先将你的包上传到 TestPyPI

要提交你的包到 PyPI，你需要先**构建它**，然后使用[Twine](https://pypi.org/project/twine/)包来**上传它**。Twine 可以通过 `pip` 安装程序轻松安装，如下所示：

```py
pip install twine
```

但是，在将任何东西上传到 PyPI 之前，我强烈建议**首先将你的包上传到**[**TestPyPI**](https://test.pypi.org/)。为什么？因为一旦上传到 PyPI：

+   你**不能编辑已上传的内容**。这意味着你不能修复包元数据、代码、包描述（即 README）中的问题等。错误和拼写错误将保留，直到你在下一次上传时修复它们。

+   更糟糕的是：包的**版本不能重新上传**。例如，一旦你上传了版本 1.0.0，即使你从 PyPI 中删除了该版本，你也不能再重新上传版本 1.0.0 —— 永远不能。

TestPyPI 允许你**上传并测试你的包**，在实际上传到 PyPI 之前。这是 PyPI 的一个完全独立的实例，意味着你需要[创建一个 TestPyPI 账户](https://test.pypi.org/account/register/)。

要**将包上传到 TestPyPI**，在你的包文件夹中执行以下命令：

```py
# build the package
python setup.py sdist bdist_wheel
# upload to TestPyPI
twine upload --repository-url https://test.pypi.org/legacy/ dist/*
```

*这将上传你的包的源分发版和构建分发版（更多细节请参见第 2.2 节）。*

在完成测试和检查后，通过执行以下命令**将你的包上传到 PyPI**：

```py
# build the package
python setup.py sdist bdist_wheel
# upload to PyPI
twine upload dist/* 
```

我花了相当长的时间才发现 TestPyPI，自那时起，我再也离不开它 —— 我相信你也会如此。

## 2.2 | 与你的包一起上传 Wheels

我注意到在线指南中“如何上传包到 PyPI”经常显示以下命令：

```py
# create a source distribution 
python setup.py sdist
# upload to PyPI 
twine upload dist/*
```

这将上传**你的包的源分发版**到 PyPI，供通过 `pip` 安装程序下载和安装。

除了上传源分发版，我强烈推荐同时**上传构建分发版**（即 Python wheel）。因为 wheel 通常**体积更小**且安装时无需从源分发版重新构建，所以对用户来说**安装时间更快**。

![](img/695cf961f1b0c2fbb730b6ede8c2065b.png)

PyPI 包文件。图片来源于[PyPI.org](https://pypi.org/)作者。

要创建你包的构建分发版，你首先需要安装[Wheel](https://pypi.org/project/wheel/)包：

```py
pip install wheel
```

然后你可以执行以下命令**创建源分发版和构建分发版**，并将它们上传到 PyPI：

```py
# create a source and a built distribution
python setup.py sdist bdist_wheel
# upload to PyPI
twine upload dist/*
```

可以专门为特定的 Python 版本和操作系统类型构建 Wheels。上述命令将**创建一个 Python 3 操作系统独立的 wheel**。然而，对于你的使用场景，你可能需要添加多个 wheel 类型：例如，如果你的包应该支持 Python 2，你可以包括一个 Python 2 的 wheel。有关 wheel 的更多信息，我建议查看[以下文章](https://realpython.com/python-wheels/)。

## 2.3 | 正确版本化你的包

每次对你的包进行更改时，确保**将其上传到新版本发布**。版本管理你的包非常重要，因为你的用户将能够跟踪你所做的更改，并在必要时更新他们的包安装。不要在现有版本中进行更新，因为这不是最佳实践。

以下是你可以遵循的**实施正确版本管理的几个步骤**：

1.  **定义一个版本控制语法**并保持一致。例如：从 1.0.0 开始。如果你对包进行了一些小的更改，发布 1.0.1。如果你进行了一次重大更改，发布 1.1.0，等等。

1.  在你的`version.py`文件中**更新版本**（如第*1.3*节所述）

1.  在 GitHub（或你为你的包使用的任何其他源代码仓库）上**创建一个新版本发布**并添加发布说明（添加/删除了哪些功能？你修复了哪些 bug？）。

1.  **更新** `download_url` **变量** 在 `setup.cfg` 文件中（如第*1.5*节所述），将其指向 GitHub 上新发布的 `tar.gz` 包的 URL（或你为包使用的任何其他源代码仓库）。

1.  最后：**将你的新版本上传**到 PyPI（但首先：**测试 PyPI**！）

这些步骤**对你来说可能会有所不同**，但如果你一直**遵循了*第一部分*的指南**，那么它们将包含所有实施你包的正确版本管理所需的操作。

## 2.4 | 记录你的包

在为公众使用创建包时，重要的是创建**结构清晰的文档**，并且**如果你对代码做出任何更改，始终更新文档**。

> *一个包的好坏取决于其文档的质量。*

所需的文档量取决于你的包的复杂性：一个简单的包可能只需要几行说明，这些说明可以直接包含在仓库的`README`中。如果你的包更复杂，`README`可能会显得过于有限。

如果我可以重新开始，我会从第一天起**使用** [**readthedocs.org**](https://readthedocs.org/) 。Readthedocs 是一个流行的**软件文档托管平台**。你会发现许多流行的 Python 包都在那儿托管他们的文档（见 [Pandas](https://pandasguide.readthedocs.io/en/latest/) 或 [Pillow](https://pillow.readthedocs.io/en/stable/)）。

托管在 readthedocs 上有各种好处，包括：

+   自动**创建可下载格式**的文档，例如 PDF 或 HTML 格式。

+   支持**文档版本控制**，

+   遵循用户通常已经熟悉的**标准页面结构**。

你可以将 readthedocs 连接到你的 GitHub 仓库，这样你可以直接在本地机器上编辑文档。然后，每当你向 GitHub 仓库提交更改时，它将**立即触发文档的新构建**。

我推荐你参考[这篇官方教程](https://docs.readthedocs.io/en/stable/tutorial/index.html)来了解如何设置 readthedocs。作为参考示例，这是我在[GitHub](https://github.com/elisemercury/Duplicate-Image-Finder/tree/main/docs)上的[readthedocs 文档 difPy 包](https://difpy.readthedocs.io/en/latest/index.html)。

## 2.5 | 维护代码卫生

最后，在将你的包发布到 PyPI 时，保持一定的**代码卫生标准**也非常重要。源代码的质量越高，你的包就会越可信。

一些代码卫生的最佳实践包括：

+   **一致使用变量和函数名称**：要么使用首字母大写的变量名，例如`variableName`，要么使用下划线分隔的变量名，如`variable_name`。不要在一个脚本中混合使用这两种方式。

+   **一致使用引号**：Python 支持单引号`'`和双引号`"`。选择在你的脚本中使用哪一种，并且不要混用（除非绝对必要，例如在格式化字符串时）。

+   **注释你的代码**！这对你来说可能很明显，但作为最佳实践必须包含在内。

当然，除了我在这里列出的最佳实践，还有更多的实践可以遵循，因此我建议你查看[这篇文章](https://www.codingdojo.com/blog/python-best-practices)以获取更多信息。

# 结论

上传和维护 PyPI 包可以非常有趣且充满成就感。通过让其他人使用你的工作来分享你的成果，**帮助你学习**并通过获得来自 Python 社区的**反馈**和**改进建议**来**获得更多编码经验**。

我希望本指南为你提供了一些**宝贵的见解**，帮助你开始使用 PyPI——或者如果你已经在使用 PyPI，能给你一些新的提示来改进你当前的使用方式。✨

如果你有任何反馈，请告诉我，也可以**分享你自己的 PyPI 使用技巧**，留下评论吧！🍀

## 参考文献

[1] PyPA, [打包 Python 项目](https://packaging.python.org/en/latest/tutorials/packaging-projects/) (2023)

[2] Setuptools, [使用 pyproject.toml 文件配置 setuptools](https://setuptools.pypa.io/en/latest/userguide/pyproject_config.html) (2023)

[3] Readthedocs, [Read the Docs：简化文档](https://docs.readthedocs.io/en/stable/index.html) (2023)
