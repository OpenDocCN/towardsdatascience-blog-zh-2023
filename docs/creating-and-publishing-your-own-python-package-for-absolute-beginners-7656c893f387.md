# 为绝对初学者创建和发布自己的 Python 包

> 原文：[`towardsdatascience.com/creating-and-publishing-your-own-python-package-for-absolute-beginners-7656c893f387`](https://towardsdatascience.com/creating-and-publishing-your-own-python-package-for-absolute-beginners-7656c893f387)

## 在 5 分钟内创建、构建和发布一个 Python 包

[](https://mikehuls.medium.com/?source=post_page-----7656c893f387--------------------------------)![Mike Huls](https://mikehuls.medium.com/?source=post_page-----7656c893f387--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7656c893f387--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7656c893f387--------------------------------) [Mike Huls](https://mikehuls.medium.com/?source=post_page-----7656c893f387--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----7656c893f387--------------------------------) ·阅读时间 6 分钟·2023 年 9 月 23 日

--

![](img/376b56645a3b29e6dba99d79a2a5fd01.png)

(图片由[Erda Estremera](https://unsplash.com/@erdaest)提供，来源于[Unsplash](https://unsplash.com/photos/sxNt9g77PE0))

Python 包是可以在多个项目中轻松共享和实现的可重用代码集合。我们可以编写一次代码，然后在许多地方多次使用。包允许我们与同事或甚至全球开发者社区共享我们的代码。作为数据科学家，你可以分享包，而不是分享 Jupyter 笔记本，以确保便于更新、重用和版本控制。

在本文中，我们将详细讲述创建、构建和发布你自己的包到 Python 包索引（PyPI；你从中`pip install`）的现代方法。我们将创建一个名为“**mikes-toolbox2**”的[真实包](https://pypi.org/project/mikes-toolbox2/)，并将其部署到 PyPI，以便我们可以通过`pip install mikes-toolbox2`来安装它。开始编码吧！

## 在我们开始之前…

本文详细讲述了如何将包发布到*公共* Python 包索引。这意味着，一旦发布，你的包就对任何人开放。我正在撰写一篇关于如何设置你自己的私人 PyPI 的文章，如果你感兴趣，请确保[**关注我**](http://mikehuls.medium.com/membership)。

如果你在本文的代码示例中迷路了：[**在这里查看源代码**](https://github.com/mike-huls/mikes-toolbox2)。

## 1. 设置 Python 包项目

在本节中，我们将通过创建一个文件夹并安装我们的虚拟环境和包来准备我们的项目。

我们将开始 **创建一个文件夹** 在 `c:/my_packages/new_package` 并在代码编辑器中打开此文件夹。接下来，我们需要 **设置我们的虚拟环境**。请参阅下面的文章以深入了解如何操作。简而言之：你可以让 PyCharm 处理它，或者使用 `python -m venv venv`。

[虚拟环境完全入门——什么是虚拟环境以及如何创建一个（附示例）](https://towardsdatascience.com/virtual-environments-for-absolute-beginners-what-is-it-and-how-to-create-one-examples-a48da8982d4b?source=post_page-----7656c893f387--------------------------------)

### 深入了解 Python 虚拟环境、pip 以及如何避免依赖冲突

[虚拟环境完全入门——什么是虚拟环境以及如何创建一个（附示例）](https://towardsdatascience.com/virtual-environments-for-absolute-beginners-what-is-it-and-how-to-create-one-examples-a48da8982d4b?source=post_page-----7656c893f387--------------------------------)

最后我们需要 **安装 Poetry**。这个包使得依赖管理和打包变得非常简单。使用 `pip install poetry` 安装。

## 2\. 包要求

Python 包需要一些 **文件和文件夹** 才能成为有效的包，因此让我们创建这些文件。确保你的文件夹如下所示：

```py
c/
└── my_packages/
    └── new_package/                <-- Project dir
        ├── src/                    <-- source dir for our package
        │   ├── mikes_toolbox2/     <-- the package we're building
        │   │   └── __init__.py     <-- empty file
        │   └── __init__.py         <-- empty file
        ├── tests/                  <-- tests go here
        │  └── __init__.py          <-- empty file
        └── README.md               <-- required readme for our project
```

*从技术上讲，我们不需要 `*src*` 文件夹在 `*new_package*` 和 `*mikes_toolbox2*` 之间，但我还是喜欢这样做，因为它可以将包代码与源目录隔离得更远。我认为这种结构更明确、更干净、更易于管理，但这是可选的；你可以省略 `*src*` 文件夹。*

[再也无需编写 SQL：SQLAlchemy 的 ORM 绝对入门](https://towardsdatascience.com/no-need-to-ever-write-sql-again-sqlalchemys-orm-for-absolute-beginners-107be0b3148f?source=post_page-----7656c893f387--------------------------------)

### 使用这个 ORM，你可以创建一个表，插入、读取、删除和更新数据，而无需编写一行 SQL。

[再也无需编写 SQL：SQLAlchemy 的 ORM 绝对入门](https://towardsdatascience.com/no-need-to-ever-write-sql-again-sqlalchemys-orm-for-absolute-beginners-107be0b3148f?source=post_page-----7656c893f387--------------------------------)

在创建我们的文件和文件夹后，我们需要一个 `**pyproject.toml**`。这是一个通用的 Python 项目配置文件，设计用来被所有类型的工具使用，如构建系统、包管理器和代码检查器。Poetry 使用 `pyproject.toml` 来跟踪依赖关系。

创建 `pyproject.toml` 很简单，因为 Poetry 帮助我们通过 `poetry init` 生成一个。只需回答提示即可生成 `pyproject`：

![](img/b642fd9ee4e82dfbabf8279bc3f2e4eb.png)

[poetry init] 会提示我们输入有关包的详细信息（图像由作者提供）

`pyproject.toml` 应位于项目目录的根目录下。所以在我的例子中，它位于 [这里](https://github.com/mike-huls/mikes-toolbox2) `c:/my_packages/new_pacakge/pyproject.toml`。

也许我们的项目或包**需要依赖**；我们如何安装这些呢？Poetry 使用`pyproject.toml`来跟踪依赖（以及其他事项）。我将演示如何安装两个包：`requests`和`black`。我们以不同的方式使用这些包：

+   `request`由我们自己的包使用

+   `black`由我们开发者使用来对包进行代码检查

区别在于`black`是***开发依赖***，我们只在开发依赖时需要它。因此，当有人用 pip 安装我们的包时，`requests`也需要被安装，但`black`则不需要。

这里是如何以不同方式安装包：

```py
poetry add requests             <-- will install regularly
poetry add black --group dev    <-- will install as a dev dependency
```

同样，我会将`pytest`作为开发依赖进行安装。最终我们的`pyproject.toml`将包含以下几组依赖：

![](img/ff4ecec6cc709a2ded84abdb825fb248.png)

在`pyproject.toml`中的包依赖和开发依赖（作者提供的图片）

[](/create-a-fast-auto-documented-maintainable-and-easy-to-use-python-api-in-5-lines-of-code-with-4e574c00f70e?source=post_page-----7656c893f387--------------------------------) ## 创建一个快速的自动文档、可维护且易于使用的 Python API，只需 5 行代码

### 非常适合（经验不足的）开发人员，他们只需要一个完整的、工作良好、快速且安全的 API

towardsdatascience.com

## 3 包内容和测试

让我们**打包我们的函数**吧！我在`c:/my_pacakges/new_pacakge/src/mikes_toolbox2/my_toolbox.py`中添加了`mess_up_casing`函数：

```py
import random

def mess_up_casing(my_string: str) -> str:
    """Messes up the casing of a string completely"""
    return "".join(
        [l.upper() if (round(random.random()) == 1) else l.lower() for l in my_string]
    )
```

接下来我们可以**测试**这段代码。为此，我在`tests`文件夹中编写了一个[小的单元测试](https://github.com/mike-huls/mikes-toolbox2/blob/main/tests/test_toolbox_functions.py)。只需调用`pytest`并检查所有测试是否成功。最后，我们甚至可以用`black src`**格式化**我们所有的代码。

[](/why-is-python-so-slow-and-how-to-speed-it-up-485b5a84154e?source=post_page-----7656c893f387--------------------------------) ## 为什么 Python 这么慢，如何加速

### 深入了解 Python 的瓶颈所在

towardsdatascience.com

## 4\. 构建和发布我们的包

现在，构建我们的包变得非常简单，只需`poetry build`即可。

构建成功后，你会看到一个新的`dist`文件夹，其中包含一个`.tar.gz`和一个`.whl`文件。这些就是我们想要上传到 pypi 的文件。

为此，首先你需要在 [pypi.org](https://pypi.org/account/register/) **注册**。登录后，你可以进入你的账户设置并**创建一个 API 令牌**。你需要这个令牌来进行包发布时的身份验证。确保将令牌保存到某个地方，因为关闭窗口后你无法再次查看它。

接下来，我们需要**配置 Poetry**，因为我们将使用它来发布我们的包；Poetry 必须知道如何验证你的 pypi.org 账户。我们为此使用之前创建的**API 令牌**：

`poetry config pypi-token.pypi THETOKEN`。

现在我们有了正确的项目结构并配置了 Poetry，**发布我们的包**变得简单了。我们只需调用：`poetry publish` 就完成了！在这一步之后，你可以 `pip install mikes-toolbox2`。

[](/thread-your-python-program-with-two-lines-of-code-3b474407dbb8?source=post_page-----7656c893f387--------------------------------) ## 用两行代码为你的 Python 程序添加线程

### 通过同时做多个事情来加快你的程序速度

towardsdatascience.com

# 结论

在这篇文章中，我们已经看到 Poetry 使打包和发布代码到 PyPI.org 变得非常简单。你是否对打包你的代码感兴趣但不想将其发布到公共 PyPI？你也可以托管自己的 Python 包索引，[关注我](http://mikehuls.medium.com/membership) 以便获取未来相关的文章。

我希望这篇文章能达到我期望的清晰度，但如果不是，请告诉我我可以做什么来进一步澄清。同时，查看我关于各种编程相关主题的[其他文章](https://mikehuls.com/articles)：

+   [Git 入门：通过视频游戏理解 Git](https://mikehuls.medium.com/git-for-absolute-beginners-understanding-git-with-the-help-of-a-video-game-88826054459a)

+   [创建和发布你自己的 Python 包](https://mikehuls.medium.com/create-and-publish-your-own-python-package-ea45bee41cdc)

+   [用 FastAPI 在 5 行代码中创建一个快速的自动文档、可维护且易于使用的 Python API](https://mikehuls.medium.com/create-a-fast-auto-documented-maintainable-and-easy-to-use-python-api-in-5-lines-of-code-with-4e574c00f70e)

+   [使用环境变量和文件与 docker 和 compose 的完整指南](https://mikehuls.medium.com/a-complete-guide-to-using-environment-variables-and-files-with-docker-and-compose-4549c21dc6af)

编程愉快！

— Mike

*P.S: 喜欢我做的事情吗？* [*关注我！*](https://mikehuls.medium.com/membership)

[](https://mikehuls.medium.com/membership?source=post_page-----7656c893f387--------------------------------) [## 使用我的推荐链接加入 Medium — Mike Huls

### 阅读 Mike Huls 的每一个故事（以及 Medium 上其他成千上万的作者的故事）。你的会员费用直接支持 Mike……

[mikehuls.medium.com](https://mikehuls.medium.com/membership?source=post_page-----7656c893f387--------------------------------)
