# Bash 处理速度至关重要

> 原文：[`towardsdatascience.com/bash-processing-speed-matters-d83e4c5adf32?source=collection_archive---------5-----------------------#2023-03-30`](https://towardsdatascience.com/bash-processing-speed-matters-d83e4c5adf32?source=collection_archive---------5-----------------------#2023-03-30)

## 我将一个 bash 命令行的执行速度提升了超过 500 倍，使其使用起来轻而易举

[](https://medium.com/@mattiadigangi?source=post_page-----d83e4c5adf32--------------------------------)![Mattia Di Gangi](https://medium.com/@mattiadigangi?source=post_page-----d83e4c5adf32--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d83e4c5adf32--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d83e4c5adf32--------------------------------) [Mattia Di Gangi](https://medium.com/@mattiadigangi?source=post_page-----d83e4c5adf32--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8a5b9f193a3c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbash-processing-speed-matters-d83e4c5adf32&user=Mattia+Di+Gangi&userId=8a5b9f193a3c&source=post_page-8a5b9f193a3c----d83e4c5adf32---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d83e4c5adf32--------------------------------) ·5 分钟阅读·2023 年 3 月 30 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd83e4c5adf32&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbash-processing-speed-matters-d83e4c5adf32&source=-----d83e4c5adf32---------------------bookmark_footer-----------)![](img/3c5892a7dbb49c0b78ff636aafe30db5.png)

图片来源：[Chris Liverani](https://unsplash.com/@chrisliverani?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

当你需要处理文本或表格形式的数据时，长期以来的首选之一无疑是 [GNU Bash](https://www.gnu.org/software/bash/)，这是 Linux 的旗舰 shell，内置了“电池”。如果你从未使用过它，你会错过很多，应该试试看。

Bash 附带的工具遵循 Unix 的哲学——“只做一件事，并且做到最好”，并且针对许多不同的任务进行了超级优化。[find](https://linux.die.net/man/1/find)、[grep](https://linux.die.net/man/1/grep)、[sed](https://www.gnu.org/software/sed/manual/sed.html) 和 [awk](https://www.gnu.org/software/gawk/manual/gawk.html) 只是其中一些强大的工具，这些工具由于 Bash 的 [管道和过滤器架构](https://dev.to/desi109/architectural-styles-by-examples-387b) 而能够进行互操作，专门用于文本文件处理。

最近，我需要进行简单的文本处理，而 bash 是非常合适的。我有一个输入文件，其中每一行包含一个绝对文件路径，我需要生成一个输出文件，其中每一行是输入文件中相应路径的基本名称，并且具有不同的扩展名。实际上，这两个文件作为输入需要提供给另一个程序，该程序会将输入文件中提到的文件（wav 格式）转换为输出文件中列出的文件（mp4 格式，通过添加一些视频）。

我本可以用 Python 完成，但 bash 在这项任务中看起来更实用。然而，在这个故事的最后，我将展示…
