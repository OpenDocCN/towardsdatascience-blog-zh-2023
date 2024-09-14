# 告别 os.path：15 个 Pathlib 技巧迅速掌握 Python 文件系统

> 原文：[`towardsdatascience.com/goodbye-os-path-15-pathlib-tricks-to-quickly-master-the-file-system-in-python-881213ca7c21`](https://towardsdatascience.com/goodbye-os-path-15-pathlib-tricks-to-quickly-master-the-file-system-in-python-881213ca7c21)

## 不再有`os.path`带来的头疼和难以阅读的代码

[](https://ibexorigin.medium.com/?source=post_page-----881213ca7c21--------------------------------)![Bex T.](https://ibexorigin.medium.com/?source=post_page-----881213ca7c21--------------------------------)[](https://towardsdatascience.com/?source=post_page-----881213ca7c21--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----881213ca7c21--------------------------------) [Bex T.](https://ibexorigin.medium.com/?source=post_page-----881213ca7c21--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----881213ca7c21--------------------------------) ·阅读时间 7 分钟·2023 年 4 月 7 日

--

![](img/d29d81ae086f20c107547e340856efe7.png)

机器人伙伴。 — 来自 Midjourney

Pathlib 可能是我最喜欢的库（显然在 Sklearn 之后）。考虑到有超过 13 万个库，这已经很说明问题了。Pathlib 帮助我将像这样的代码从 `os.path` 转换过来：

```py
import os

dir_path = "/home/user/documents"

# Find all text files inside a directory
files = [os.path.join(dir_path, f) for f in os.listdir(dir_path) \
        if os.path.isfile(os.path.join(dir_path, f)) and f.endswith(".txt")]
```

转换为如下内容：

```py
from pathlib import Path

# Find all text files inside a directory
files = list(dir_path.glob("*.txt"))
```

Pathlib 于 Python 3.4 中推出，作为替代 `os.path` 的噩梦。它还标志着 Python 语言的一个重要里程碑：他们终于将每一件事都变成了对象（即使是 [nothing](https://docs.python.org/3/c-api/none.html)）。

`os.path` 的最大缺陷是将系统路径视为字符串，这导致了难以阅读、混乱的代码和陡峭的学习曲线。

通过将路径表示为完全成熟的 **对象**，Pathlib 解决了所有这些问题，并为路径处理带来了优雅、一致性和一丝清新。

这篇我一直拖延的文章将概述一些 `pathlib` 的最佳函数/特性和技巧，以执行那些在 `os.path` 中会是极其糟糕的任务。

学习这些 Pathlib 的功能将使你作为数据专业人士处理路径和文件变得更容易，特别是在需要处理成千上万的图像、CSV 文件或音频文件的数据处理工作流中。

让我们开始吧！

## 路径操作

**1\. 创建路径**

`pathlib`的几乎所有功能都可以通过其 `Path` 类访问，你可以用它来创建文件和目录的路径。

有几种方法可以使用 `Path` 创建路径。首先，有像 `cwd` 和 `home` 这样的类方法，用于当前工作目录和用户主目录：

```py
from pathlib import Path

Path.cwd()
```

```py
PosixPath('/home/bexgboost/articles/2023/4_april/1_pathlib')
```

```py
Path.home()
```

```py
PosixPath('/home/bexgboost')
```

你也可以从字符串路径创建路径：

```py
p = Path("documents")

p
```

```py
PosixPath('documents')
```

使用正斜杠操作符在 Pathlib 中连接路径是轻而易举的：

```py
data_dir = Path(".") / "data"
csv_file = data_dir / "file.csv"

print(data_dir)
print(csv_file)
```

```py
data
data/file.csv
```

请不要让任何人看到你在使用 `os.path.join`。

要检查路径是否存在，你可以使用布尔函数 `exists`：

```py
data_dir.exists()
```

```py
True
```

```py
csv_file.exists()
```

```py
True
```

有时，整个 Path 对象可能不可见，你需要检查它是目录还是文件。因此，你可以使用`is_dir`或`is_file`函数来判断：

```py
data_dir.is_dir()
```

```py
True
```

```py
csv_file.is_file()
```

```py
True
```

你处理的大多数路径相对于当前目录。但有时你需要提供文件或目录的确切位置，以便从任何 Python 脚本中访问它。这时你使用`absolute`路径：

```py
csv_file.absolute()
```

```py
PosixPath('/home/bexgboost/articles/2023/4_april/1_pathlib/data/file.csv')
```

最后，如果你不幸要处理仍然需要字符串路径的库，你可以调用 `str(path)`：

```py
str(Path.home())
```

```py
'/home/bexgboost'
```

> 数据栈中的大多数库很早就支持了 `Path` 对象，包括 `sklearn`、`pandas`、`matplotlib`、`seaborn` 等。

**2\. 路径属性**

`Path` 对象有许多有用的属性。让我们看看一些使用指向图像文件的路径对象的示例。

```py
image_file = Path("images/midjourney.png").absolute()

image_file
```

```py
PosixPath('/home/bexgboost/articles/2023/4_april/1_pathlib/images/midjourney.png')
```

从 `parent` 开始。它返回一个路径对象，该对象在当前工作目录上一级。

```py
image_file.parent
```

```py
PosixPath('/home/bexgboost/articles/2023/4_april/1_pathlib/images')
```

有时，你可能只想要文件的 `name` 而不是整个路径。这里有一个属性可以做到这一点：

```py
image_file.name
```

```py
'midjourney.png'
```

只返回带有扩展名的文件名。

还有 `stem` 用于没有后缀的文件名：

```py
image_file.stem
```

```py
'midjourney'
```

或带有点的 `suffix` 本身作为文件扩展名：

```py
image_file.suffix
```

```py
'.png'
```

如果你想将路径划分为其组件，你可以使用 `parts` 代替 `str.split('/')`：

```py
image_file.parts
```

```py
('/',
 'home',
 'bexgboost',
 'articles',
 '2023',
 '4_april',
 '1_pathlib',
 'images',
 'midjourney.png')
```

如果你想要那些组件本身成为 `Path` 对象，你可以使用 `parents` 属性，它创建一个生成器：

```py
for i in image_file.parents:
    print(i)
```

```py
/home/bexgboost/articles/2023/4_april/1_pathlib/images
/home/bexgboost/articles/2023/4_april/1_pathlib
/home/bexgboost/articles/2023/4_april
/home/bexgboost/articles/2023
/home/bexgboost/articles
/home/bexgboost
/home
/
```

## 处理文件

![](img/87265966c391ee7d32b99ff9f942387d.png)

分类文件。— Midjourney

要创建文件并向其中写入内容，你不再需要使用 `open` 函数。只需创建一个 `Path` 对象，然后 `write_text` 或 `write_bytes` 即可：

```py
markdown = data_dir / "file.md"

# Create (override) and write text
markdown.write_text("# This is a test markdown")
```

或者，如果你已经有一个文件，你可以 `read_text` 或 `read_bytes`：

```py
markdown.read_text()
```

```py
'# This is a test markdown'
```

```py
len(image_file.read_bytes())
```

```py
1962148
```

但请注意，`write_text`或`write_bytes`会覆盖文件中现有的内容。

```py
# Write new text to existing file
markdown.write_text("## This is a new line")
```

```py
# The file is overridden
markdown.read_text()
```

```py
'## This is a new line'
```

要向现有文件添加新信息，你应该使用 `Path` 对象的 `open` 方法，模式为 `a`（附加）：

```py
# Append text
with markdown.open(mode="a") as file:
    file.write("\n### This is the second line")

markdown.read_text()
```

```py
'## This is a new line\n### This is the second line'
```

重命名文件也很常见。`rename` 方法接受重命名文件的目标路径。

要在当前目录中创建目标路径，即重命名文件，你可以在现有路径上使用 `with_stem`，它替换原始文件的 `stem`：

```py
renamed_md = markdown.with_stem("new_markdown")

markdown.rename(renamed_md)
```

```py
PosixPath('data/new_markdown.md')
```

上述，`file.md` 被转为 `new_markdown.md`。

通过 `stat().st_size` 查看文件大小：

```py
# Display file size
renamed_md.stat().st_size
```

```py
49 # in bytes
```

或文件上次修改的时间，这是几秒钟前的事：

```py
from datetime import datetime

modified_timestamp = renamed_md.stat().st_mtime

datetime.fromtimestamp(modified_timestamp)
```

```py
datetime.datetime(2023, 4, 3, 13, 32, 45, 542693)
```

`st_mtime` 返回一个时间戳，这是自 1970 年 1 月 1 日以来的秒数。为了使其可读，你可以使用 `datetime` 的 `fromtimestamp` 函数。

要删除不需要的文件，你可以 `unlink` 它们：

```py
renamed_md.unlink(missing_ok=True)
```

将 `missing_ok` 设置为 `True`，如果文件不存在不会引发任何警报。

## 处理目录

![](img/97200e350b1dc2ecdae2da62bdf37d35.png)

办公室中的文件夹。— Midjourney

在 Pathlib 中处理目录有几个很棒的技巧。首先，让我们看看如何递归创建目录。

```py
new_dir = (
    Path.cwd()
    / "new_dir"
    / "child_dir"
    / "grandchild_dir"
)

new_dir.exists()
```

```py
False
```

`new_dir`还不存在，所以让我们创建它及其所有子目录：

```py
new_dir.mkdir(parents=True, exist_ok=True)
```

默认情况下，`mkdir`只会创建给定路径的最后一个子目录。如果中间的父目录不存在，你必须将`parents`设置为`True`。

要删除空目录，你可以使用`rmdir`。如果给定的路径对象是嵌套的，则只会删除最后一个子目录：

```py
# Removes the last child directory
new_dir.rmdir()
```

要列出目录的内容，就像终端中的`ls`一样，你可以使用`iterdir`。结果将是一个生成器对象，一次产生一个路径对象的目录内容：

```py
for p in Path.home().iterdir():
    print(p)
```

```py
/home/bexgboost/.python_history
/home/bexgboost/word_counter.py
/home/bexgboost/.azure
/home/bexgboost/.npm
/home/bexgboost/.nv
/home/bexgboost/.julia
...
```

要捕捉所有具有特定扩展名或名称模式的文件，你可以使用带有正则表达式的`glob`函数。

例如，下面我们将使用`glob("*.txt")`在我的主目录中找到所有文本文件：

```py
home = Path.home()
text_files = list(home.glob("*.txt"))

len(text_files)
```

```py
3 # Only three
```

要递归搜索文本文件，即包括所有子目录内的文件，你可以使用*递归 glob*与`rglob`：

```py
all_text_files = [p for p in home.rglob("*.txt")]

len(all_text_files)
```

```py
5116 # Now much more
```

> 了解正则表达式 [这里](https://realpython.com/regex-python/)。

你也可以使用`rglob('*')`递归列出目录内容。这就像`iterdir()`的超级增强版。

其中一个用例是统计目录中出现的文件格式数量。

为此，我们从`collections`中导入`Counter`类，并在`home`的文章文件夹中提供所有文件后缀：

```py
from collections import Counter

file_counts = Counter(
    path.suffix for path in (home / "articles").rglob("*")
)

file_counts
```

```py
Counter({'.py': 12,
         '': 1293,
         '.md': 1,
         '.txt': 7,
         '.ipynb': 222,
         '.png': 90,
         '.mp4': 39})
```

## 操作系统差异

对不起，但我们必须讨论这个噩梦般的问题。

到目前为止，我们一直在处理`PosixPath`对象，它们是 UNIX-like 系统的默认对象：

```py
type(Path.home())
```

```py
pathlib.PosixPath
```

如果你在 Windows 上，你会得到一个`WindowsPath`对象：

```py
from pathlib import WindowsPath

# User raw strings that start with r to write windows paths
path = WindowsPath(r"C:\users")
path
```

```py
NotImplementedError: cannot instantiate 'WindowsPath' on your system
```

实例化另一个系统的路径会引发类似上述的错误。

但如果你被迫处理来自其他系统的路径，比如同事们用 Windows 写的代码呢？

作为解决方案，`pathlib`提供了像`PureWindowsPath`或`PurePosixPath`这样的纯路径对象：

```py
from pathlib import PurePosixPath, PureWindowsPath

path = PureWindowsPath(r"C:\users")
path
```

```py
PureWindowsPath('C:/users')
```

这些是原始路径对象。你可以访问一些路径方法和属性，但本质上，路径对象仍然是一个字符串：

```py
path / "bexgboost"
```

```py
PureWindowsPath('C:/users/bexgboost')
```

```py
path.parent
```

```py
PureWindowsPath('C:/')
```

```py
path.stem
```

```py
'users'
```

```py
path.rename(r"C:\losers") # Unsupported
```

```py
AttributeError: 'PureWindowsPath' object has no attribute 'rename'
```

## 结论

如果你注意到，我在文章标题中撒了谎。实际上新技巧和函数的数量是大约 30 个，而不是 15 个。

我不想吓到你。

但我希望我已经足够说服你放弃`os.path`，开始使用`pathlib`来进行更简单和更可读的路径操作。

锻造一个新的*路径*，如果你愿意 :)

![](img/0c5f3ba9e8226957b62ee0afc66e34ae.png)

Path. — Midjourney

如果你喜欢这篇文章，并且，老实说，其怪异的写作风格，考虑通过注册成为 Medium 会员来支持我。会员费用为每月 4.99 美元，可以无限制访问我所有的故事以及成千上万篇更有经验的作者写的文章。如果你通过 [这个链接](https://ibexorigin.medium.com/membership) 注册，我将赚取少量佣金，而不会增加你的额外费用。

[](https://ibexorigin.medium.com/membership?source=post_page-----881213ca7c21--------------------------------) [## 通过我的推荐链接加入 Medium — Bex T.

### 获得对我所有⚡高级⚡内容和 Medium 上所有内容的独家访问权限，没有限制。通过购买我一份支持我的工作…

ibexorigin.medium.com](https://ibexorigin.medium.com/membership?source=post_page-----881213ca7c21--------------------------------)
