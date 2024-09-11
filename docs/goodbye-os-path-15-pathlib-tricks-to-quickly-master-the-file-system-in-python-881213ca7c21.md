# 告别 os.path：15 个 Pathlib 技巧快速掌握 Python 文件系统

> 原文：[`towardsdatascience.com/goodbye-os-path-15-pathlib-tricks-to-quickly-master-the-file-system-in-python-881213ca7c21?source=collection_archive---------0-----------------------#2023-04-07`](https://towardsdatascience.com/goodbye-os-path-15-pathlib-tricks-to-quickly-master-the-file-system-in-python-881213ca7c21?source=collection_archive---------0-----------------------#2023-04-07)

## 从`os.path`中没有头痛和难读的代码

[](https://ibexorigin.medium.com/?source=post_page-----881213ca7c21--------------------------------)![Bex T.](https://ibexorigin.medium.com/?source=post_page-----881213ca7c21--------------------------------)[](https://towardsdatascience.com/?source=post_page-----881213ca7c21--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----881213ca7c21--------------------------------) [Bex T.](https://ibexorigin.medium.com/?source=post_page-----881213ca7c21--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39db050c2ac2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgoodbye-os-path-15-pathlib-tricks-to-quickly-master-the-file-system-in-python-881213ca7c21&user=Bex+T.&userId=39db050c2ac2&source=post_page-39db050c2ac2----881213ca7c21---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----881213ca7c21--------------------------------) ·7 分钟阅读·2023 年 4 月 7 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F881213ca7c21&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgoodbye-os-path-15-pathlib-tricks-to-quickly-master-the-file-system-in-python-881213ca7c21&user=Bex+T.&userId=39db050c2ac2&source=-----881213ca7c21---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F881213ca7c21&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgoodbye-os-path-15-pathlib-tricks-to-quickly-master-the-file-system-in-python-881213ca7c21&source=-----881213ca7c21---------------------bookmark_footer-----------)![](img/d29d81ae086f20c107547e340856efe7.png)

机器人伙伴。 — 通过 Midjourney

Pathlib 可能是我最喜欢的库（显然排在 Sklearn 之后）。考虑到有超过 13 万个库，这已经是很高的评价了。Pathlib 帮助我将像这样用 `os.path` 编写的代码：

```py
import os

dir_path = "/home/user/documents"

# Find all text files inside a directory
files = [os.path.join(dir_path, f) for f in os.listdir(dir_path) \
        if os.path.isfile(os.path.join(dir_path, f)) and f.endswith(".txt")]
```

变成这样：

```py
from pathlib import Path

# Find all text files inside a directory
files = list(dir_path.glob("*.txt"))
```

Pathlib 于 Python 3.4 中发布，作为`os.path`的替代品。它也标志着 Python 语言的重要里程碑：他们终于将所有事物都变成了对象（甚至是[空值](https://docs.python.org/3/c-api/none.html)）。

`os.path` 最大的缺点是将系统路径视为字符串，这导致了难以阅读、混乱的代码和陡峭的学习曲线。

通过将路径表示为完善的**对象**，Pathlib 解决了所有这些问题，并为路径处理引入了优雅、一致性和一丝清新的空气。
