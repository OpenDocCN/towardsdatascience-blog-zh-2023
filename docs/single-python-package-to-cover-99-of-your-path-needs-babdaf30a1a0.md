# 单一 Python 包以满足 99% 的路径需求

> 原文：[`towardsdatascience.com/single-python-package-to-cover-99-of-your-path-needs-babdaf30a1a0`](https://towardsdatascience.com/single-python-package-to-cover-99-of-your-path-needs-babdaf30a1a0)

## Pathlib: 你一直梦寐以求的库

[](https://medium.com/@arli94?source=post_page-----babdaf30a1a0--------------------------------)![Arli](https://medium.com/@arli94?source=post_page-----babdaf30a1a0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----babdaf30a1a0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----babdaf30a1a0--------------------------------) [Arli](https://medium.com/@arli94?source=post_page-----babdaf30a1a0--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----babdaf30a1a0--------------------------------) ·阅读时间 5 分钟·2023 年 1 月 12 日

--

*如果你想亲自体验 Medium，考虑通过* [***注册会员***](https://medium.com/@arli94/membership)*来支持我和成千上万的其他作者。每月只需 5 美元，这对我们作者支持巨大，并且你可以访问 Medium 上所有精彩的故事。*

![](img/3c0a5685f8c3c20cb4214c4c801e2afd.png)

图片由 [Alice Donovan Rouse](https://unsplash.com/@alicekat?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

操作路径是任何生产项目中的基本任务。你可能需要从远程服务器加载符合特定模式的文件，移动或存储管道中的不同处理过的文件和版本，或者只是读取或写入文件。

在所有这些步骤中，你都会遇到路径的操作。通常，你会发现自己反复在互联网上搜索解决常见问题的方法，例如：

+   *如何从一个文件夹中获取所有文件？*

+   *如何检查一个文件夹是否存在？*

+   *如何在 Python 中创建一个文件夹？*

虽然这些问题的答案很容易找到，但你会遇到整个管道中代码不一致的情况。代码的某些部分可能使用 `os.path`，另一部分可能使用 `shutil`，还有一些部分可能使用 `glob`。

为了避免这种不一致性，我建议使用 `pathlib` 模块，它提供了一种在不同操作系统中处理路径的一致方式，并且提供了面向对象的接口，使其比 `os.path` 或 `shutil` 模块更具可读性、更简单且更一致。

# pathlib 和 Path 对象

`pathlib` 是一个简化 Python 文件路径处理的模块。它提供了一个 `Path` 类，代表一个文件或目录路径，并提供了一系列方法来对路径执行各种操作。

`pathlib`的一个关键优点是它对路径操作的面向对象方法。与使用单独的函数来操作字符串路径不同，`pathlib`提供了一个单一的`Path`类，具有一系列可以用来对路径执行各种操作的方法。这使得理解和操作路径变得更加容易，因为所有相关功能都包含在一个对象中。

`pathlib`的另一个优点是它为路径操作提供了统一的接口。它提供了一整套方法，可以用来对不同类型的路径执行各种操作，如本地文件路径、FTP/SFTP 路径等。这使得编写处理不同类型路径的代码变得更容易，因为你不需要为每种路径类型使用不同的函数或库。

为了展示稍后如何操作所有函数，我们需要定义一个指向*“origin/data/for_arli”*的路径，通过定义一个`Path`对象来实现：

```py
from pathlib import Path
path = Path('origin/data/for_arli')
```

你已经准备好使用`Path`对象的方法了，你会发现它非常简单。

# 路径存在性和类型

在许多情况下，我们希望检查文件夹或文件在给定路径中是否可用，如果不存在，我们希望进行某些操作，比如引发错误。

```py
 if path.exists():
    print(f"{path} exists.")
    if path.is_file():
        print(f"{path} is a file.")
    elif path.is_dir():
        print(f"{path} is a directory.")
else:
    raise ValueError(f"{path} does not exists")
```

这段代码将检查路径*‘origin/data/for_arli’*是否存在，如果存在，它会检查它是文件还是目录。如果路径不存在，它将打印并引发一个错误，指示路径不存在。

# 文件和目录操作

假设现在我们有兴趣列出路径中的所有文件/文件夹，我们可以这样做：

```py
for f in path.iterdir():
    print(f)
```

它将遍历路径并打印其中的每个文件或文件夹。你可以将其与之前的`is_dir()`和`is_file()`方法结合使用，以列出文件或目录。

现在假设我们想要删除路径，我们可以这样做：

```py
path.rmdir()
```

如你所见，这个命令仅在路径为空时有效。

因此，你需要一种方法在删除文件夹之前移除文件夹中的所有文件：

```py
for f in path.iterdir():
    f.unlink()
```

在这里，我们使用`unlink()`删除路径中的文件。现在你可以使用`rmdir()`毫无问题地删除文件夹。

哦，如果你想重新创建文件夹：

```py
path.mkdir()
```

注意，`mkdir()`具有非常有用的参数，如：

+   *parents=True* 用于创建路径中任何缺失的父级

+   *exists=True* 如果文件夹已经存在，将忽略任何错误

你也可以使用下面的命令重命名文件或目录：

```py
path.rename('origin/data/for_arli2')
```

# 操作和信息

`Path`的最常用方法之一肯定是`joinpath()`，用于将路径与字符串连接（它也处理两个`Path`对象之间的连接）：

```py
path = Path("/origin/data/for_arli")
# Join another path to the original path
new_path = path.joinpath("la")
print(new_path)  # prints 'origin/data/for_arli/bla'
```

有时你可能需要获取文件或文件夹的一些关键统计信息（创建时间、修改时间）或所有者（用户或组）。为此，你可以使用：

```py
print(path.stat()) # print statistics 
print(path.owner()) # print owner
```

# 输入/输出

也可以使用`pathlib`来读取或写入文件。

`open()`方法用于打开指定路径的文件并返回一个文件对象。此方法的工作方式类似于内置的`open()`：你可以使用文件对象来读取或写入文件。以下是使用`write()`方法从`Path`写入文件的示例。

```py
# Open a file for writing
path = Path('origin/data/for_arli/example.txt')
with path.open(mode='w') as f:
    # Write to the file
    f.write('Hello, World!')
```

请注意，你不需要手动创建*example.txt*。

对于读取操作，原理是相同的，但使用的是`read()`方法：

```py
path = Path('example.txt')
with path.open(mode='r') as f:
    # Read from the file
    contents = f.read()
    print(contents) # Output: Hello World!
```

总的来说，`pathlib`是一个对 Python 开发者非常有用的库，因为它提供了一个面向对象的接口，用于表示文件路径并在不同操作系统上以一致的方式执行各种操作。它使得处理文件路径和目录变得更加方便、直接且易于理解，提供了一组统一的方法。此外，与使用基于字符串的路径操作库如`os.path`或`shutil`相比，`pathlib`的面向对象设计允许编写更具可读性和可维护性的代码。

*没有额外费用，你可以通过我的推荐链接订阅 Medium。*

[](https://medium.com/@arli94/membership?source=post_page-----babdaf30a1a0--------------------------------) [## 通过我的推荐链接加入 Medium — Arli

### 阅读 Arli 和其他成千上万的 Medium 作家的每一个故事。你的会员费用直接支持 Arli 和……

medium.com](https://medium.com/@arli94/membership?source=post_page-----babdaf30a1a0--------------------------------)

*或者你可以将我的所有帖子发送到你的收件箱中。* [***在这里操作***!](https://arli94.medium.com/subscribe)

*如果你想亲自体验 Medium，可以考虑通过* [***注册会员***](https://arli94.medium.com/membership) *来支持我和其他成千上万的作家。这只需每月$5，它极大地支持我们作家，同时你可以访问 Medium 上的所有精彩故事。*
