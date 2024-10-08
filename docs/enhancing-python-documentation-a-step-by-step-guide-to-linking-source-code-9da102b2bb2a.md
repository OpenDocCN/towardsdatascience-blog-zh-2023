# 增强 Python 文档：逐步指南以链接源代码

> 原文：[`towardsdatascience.com/enhancing-python-documentation-a-step-by-step-guide-to-linking-source-code-9da102b2bb2a`](https://towardsdatascience.com/enhancing-python-documentation-a-step-by-step-guide-to-linking-source-code-9da102b2bb2a)

![Pablo Piskunow](https://piskunow.medium.com/?source=post_page-----9da102b2bb2a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9da102b2bb2a--------------------------------) [Pablo Piskunow](https://piskunow.medium.com/?source=post_page-----9da102b2bb2a--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9da102b2bb2a--------------------------------) ·4 分钟阅读·2023 年 12 月 7 日

--

> 你阅读了这个类方法的描述，但仍然不了解发生了什么。如果你能快速阅读源代码就好了…

# 弥合文档与代码之间的差距：简化 Python 学习

Python 的强大不仅体现在它的简洁和高效，还在于其庞大的社区和丰富的文档。但是，如果你能够使这些文档变得更加互动和信息丰富呢？今天，我将带你通过将你的 Sphinx 生成的 Python 文档直接链接到 GitHub 上的相应源代码来增强文档。

![](img/f1ac9810e77866bfc357b8a03fa99355.png)

作者使用 Dalle-3 创建的图像。提示“抽象图像，浅奶油色画布上的黑色水彩，展示与机器内部工作相关的手写片段，带有细箭头”。

# 第一步：使用 Sphinx 进行文档编写

当我们在 Python 代码中编写适当的文档字符串时，我们为生成全面的 API 文档奠定了基础。像 Sphinx 的 `autodoc` 和 `automodule` 这样的工具非常适合从我们的模块、类和函数中提取这些文档字符串。然而，它们通常未能提供直接链接到源代码的功能。

如果你需要开始使用 Sphinx，请查看这些教程：

[## Sphinx](https://www.sphinx-doc.org/en/master/tutorial/automatic-doc-generation.html?source=post_page-----9da102b2bb2a--------------------------------) 

### 在教程的上一部分中，你手动在 Sphinx 中记录了一个 Python 函数。然而，描述…

[www.sphinx-doc.org](https://www.sphinx-doc.org/en/master/tutorial/automatic-doc-generation.html?source=post_page-----9da102b2bb2a--------------------------------)  [## 使用 Sphinx 构建文档项目 - Sphinx 和文档的简介]

### 为了本教程的目的，我们将使用 reStructuredText 编写文档。如果你希望使用…

[sphinx-intro-tutorial.readthedocs.io](https://sphinx-intro-tutorial.readthedocs.io/en/latest/sphinx_first_steps.html?source=post_page-----9da102b2bb2a--------------------------------)

# 第 2 步：设置 Sphinx `linkcode`

要添加此功能，首先我们需要修改 Sphinx 配置。这包括在文档源的 `conf.py` 文件中将 `sphinx.ext.linkcode` 添加到扩展列表中：

```py
# docs/conf.py
extensions = [
    ...,
    "sphinx.ext.linkcode",
]
...
```

# 第 3 步：基本的 Linkcode 实现

我们的下一步是定义 `linkcode_resolve` 函数。这个函数负责确定文档应该指向的 URL：

```py
# docs/conf.py
...
def linkcode_resolve(domain, info):
    if domain != "py":
        return None
    if not info["module"]:
        return None

    filename = "src/" + info["module"].replace(".", "/")
    github_repo = "https://github.com/username/my-package"

    return f"{github_repo}/blob/main/{filename}.py"
```

在这里，我们只是指向 GitHub 仓库中的文件，而不是特定的代码行。

# 第 4 步：查找行号

## 获取模块对象

首先，你需要获取定义目标类、方法、属性或函数（以下简称“对象”）的模块对象。在 Python 中，每个加载的模块都存储在一个名为 `sys.modules` 的字典中。你需要访问这个字典，以根据其名称检索模块对象：

```py
module = sys.modules.get(info[“module”])
```

## 遍历完全限定名称

接下来，你需要遍历对象的完全限定名称。完全限定名称包括访问对象所需的所有层级，例如 `module.class.method`。这个遍历过程帮助你深入模块结构，找到你需要的确切对象：

```py
obj = module
for part in info[“fullname”].split(“.”):
    obj = getattr(obj, part, None)
```

## 使用 Inspect 模块查找行号

最后，你使用 `inspect` 模块找到源代码中定义该对象的行号：

```py
line = inspect.getsourcelines(obj)[1]
```

## **创建一个函数来检索行号**

为了使函数适用于所有情况，我们需要添加一些额外的检查：

```py
def get_object_line_number(info):
    """Return object line number from module."""
    try:
        module = sys.modules.get(info["module"])
        if module is None:
            return None

        # walk through the nested module structure
        obj = module
        for part in info["fullname"].split("."):
            obj = getattr(obj, part, None)
            if obj is None:
                return None

        return inspect.getsourcelines(obj)[1]
    except (TypeError, OSError):
        return None
```

总之，这一步骤涉及定位目标对象所在的模块，遍历模块结构以找到确切的对象（无论是类、方法、属性还是函数），然后使用 `inspect` 模块找到该对象在源代码中定义的行号。

# 第 6 步：完成 Linkcode 解析

我们将行号检索集成到 `linkcode_resolve` 函数中：

```py
def linkcode_resolve(domain, info):
    ...
    line = get_object_line_number(info)
    if line is None:
        return None

    return f"{github_repo}/blob/{github_branch}/{filename}.py#L{line}"
```

这种方法使你的文档能够提供指向源代码中特定行的直接链接，从而提高文档的清晰度和实用性。

# 额外：ReadTheDocs 的分支适配

对于使用 ReadTheDocs 的用户，你可以调整这个函数以参考不同的分支（如 `main` 或 `develop`）。那么如何构建两个不同的文档来指向这两个分支呢？

当 ReadTheDocs 构建文档时，它使用自己的环境，其中包含一个名为 `READTHEDOCS_VERSION` 的变量，通常为 ‘stable’ 或 ‘latest’。我添加了一个名为 ‘develop’ 的构建，它指向我的 Git 仓库中的同名分支。

我们可以在 `linkcode_resolve` 函数中添加以下内容：

```py
def linkcode_resolve(domain, info):
    ...
    rtd_version = os.getenv("READTHEDOCS_VERSION", "latest")
    github_branch = "develop" if rtd_version == "develop" else "main"

    return f"{github_repo}/blob/{github_branch}/{filename}.py#L{line}"
```

请注意，如果在本地构建，或者更一般地在未定义`READTHEDOCS_VERSION`的环境中，默认链接的分支将是`main`。

随时查看这个[GitHub 仓库](https://github.com/piskunow/kpm-tools/blob/b2223ad4fb24c2f995f22f5afab0ebc1cc554abf/docs/conf.py#L68)和[文档](https://kpm-tools.readthedocs.io/en/latest/api.html)，了解相关实现。

# 结论

有了这个设置，你的 Sphinx 文档现在将直接链接到源代码，大大提升了其实用性和用户体验。这一小小的增加可以显著提高文档的可用性和有效性，使其成为新手和经验丰富的开发者的宝贵工具。

记住，编码的旅程不仅仅是解决问题，也关乎分享知识和解决方案。所以，如果你觉得这个教程对你有帮助，我鼓励你传播这个消息，帮助其他人进行他们的编码冒险。

祝编码愉快，下次见！

*在我的* [*Gumroad 个人资料*](https://dataguy.gumroad.com/) *或* [*个人网站*](https://piskunow.com/)*上发现更多我的作品和独家内容。*

*免责声明：我与 Sphinx 没有任何关系，我只是觉得它是一个很棒的开源工具。*
