# Bash 处理速度很重要

> 原文：[`towardsdatascience.com/bash-processing-speed-matters-d83e4c5adf32`](https://towardsdatascience.com/bash-processing-speed-matters-d83e4c5adf32)

## 我将 bash 命令行的执行速度提高了 500 倍以上，使其变得非常方便使用

[](https://medium.com/@mattiadigangi?source=post_page-----d83e4c5adf32--------------------------------)![Mattia Di Gangi](https://medium.com/@mattiadigangi?source=post_page-----d83e4c5adf32--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d83e4c5adf32--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d83e4c5adf32--------------------------------) [Mattia Di Gangi](https://medium.com/@mattiadigangi?source=post_page-----d83e4c5adf32--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----d83e4c5adf32--------------------------------) ·阅读时间 5 分钟·2023 年 3 月 30 日

--

![](img/3c5892a7dbb49c0b78ff636aafe30db5.png)

由[Chris Liverani](https://unsplash.com/@chrisliverani?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

当你需要处理文本或表格形式的数据时，长期以来的一个受欢迎选择肯定是[GNU Bash](https://www.gnu.org/software/bash/)，这是具有“内置功能”的 Linux 旗舰 shell。如果你从未使用过它，你错过了很多，绝对应该尝试一下。

Bash 附带的工具遵循 Unix 哲学“只做一件事，并做好它”，并且对许多不同的任务进行了超级优化。[Find](https://linux.die.net/man/1/find)、[grep](https://linux.die.net/man/1/grep)、[sed](https://www.gnu.org/software/sed/manual/sed.html)和[awk](https://www.gnu.org/software/gawk/manual/gawk.html)只是一些能够通过 Bash 的[管道和过滤器架构](https://dev.to/desi109/architectural-styles-by-examples-387b)进行互操作的强大工具，用于文本文件处理。

最近，我需要执行简单的文本处理，bash 非常适合这个任务。我有一个输入文件，每行包含一个绝对文件路径，我需要生成一个输出文件，每行是输入文件中对应路径的基本名称，并有不同的扩展名。实际上，这两个文件作为输入传递给另一个程序，该程序将把输入文件中提到的文件（wav 格式）转换为输出文件中列出的文件（mp4 格式，通过添加一些视频）。

我本可以用 Python 完成这项任务，但对于这个任务来说，bash 看起来更实用。不过，在这个故事的最后，我会展示一个 Python 实现以供比较。然后，我急忙走到键盘前，完成了以下内容：

```py
$ cat input.txt | while read line; do echo $(echo $(basename $line) | sed "
s/.wav/.mp4/") >> output.txt; done
```

代码是正确的，但极其慢。我的文件有 300 万行，这个命令需要 1 小时才能完成。为了了解每秒处理的行数，让我在一个包含 10000 行的文件上运行它并测量其运行时间。

```py
$ time cat input.txt | while read line; do echo $(echo $(basename $line) | sed "
s/.wav/.mp4/") >> output.txt; done

real    0m13.297s
user    0m19.688s
sys     0m1.881s
```

一些明显的低效之处包括使用 `cat` 启动命令，以及 `echo` 的双重使用（带有嵌套的命令调用）。它们都是 I/O 密集型操作，因此非常慢。它们可以被轻松替换，而且由于我们知道输入文件中的所有路径都具有相同的扩展名，我们还可以去掉 `sed`，直接使用 `basename` 进行扩展名的移除。然后，我们用相同的 10000 行的文件运行新命令：

```py
$ time while read line; do name=$(basename $line .txt); echo ${name}.mp4 >> output.txt; done < input.txt

real    0m6.626s
user    0m5.723s
sys     0m1.131s
```

在这里我们有了显著的改进。仅去掉 `cat`，我们得到了不到 13 秒的实际时间（相对改进 ~2%），其余的通过用 `basename` 替换 `sed` 和第二个 `echo` 完成。不幸的是，它仍然相当慢。以大约 1500 行/秒的速度完成 3,000,000 行需要 2000 秒，即大约 30 分钟。幸运的是，我们可以通过替换 `read` 获得显著的提升。[Read](https://linuxize.com/post/bash-read/) 从标准输入读取一行并将其内容分配给一个或多个变量（它可以很容易地与表格数据一起使用），但在我们的情况下并不需要，因为逐行处理是任何 bash 命令都在做的事情。

不幸的是，我们不得不放弃方便的 `basename` 以仅提取文件名，但我们可以用 [cut](https://linux.die.net/man/1/cut) 替代它，`cut` 可以根据分隔符删除文本片段，以及 [rev](https://linux.die.net/man/1/cut) ，它只是反转字符序列——这是用 `cut` 提取最后一个字段的常用技巧，默认情况下这是不可行的。

执行的操作数看起来比之前更多，但我们最终获得了巨大的加速，如我们在示例玩具文件中看到的：

```py
$ time rev input.txt | cut -d/ -f1 | rev | sed "s/.wav/.mp4/" >> output.txt

real    0m0.011s
user    0m0.010s
sys     0m0.013s
```

以 ~910 Klines/秒的新速度，我们可以在 3.3 秒内处理 3,000,000 行，相当于 606 倍的加速。

最重要的是，虽然实际的数字取决于执行命令的硬件，但不同硬件之间的相对改进将保持一致。

# Python 实现

这里我们可以看到一个等效的 Python 实现以供比较：

```py
# convert.py
import os
import sys

def convert(tgt_ext: str):
    for line in sys.stdin:
        base, _ = os.path.splitext(os.path.basename(line))
        print(base + tgt_ext)

if __name__ == '__main__':
    if len(sys.argv) != 2:
        print(f"Usage: {sys.argv[0]} TARGET_EXT")
        sys.exit(1)

    convert(sys.argv[1])
    exit(0)
```

现在我们可以测量它的时间：

```py
$ time python3 convert.py .mp4 < input.txt > output.txt

real    0m0.022s
user    0m0.021s
sys     0m0.000s
```

而且这个实现的时间大约是我们用 bash 得到的最佳时间的两倍。它需要编写更多的代码，但这些代码非常快速，而且可能更容易为许多人进行修改。

# 结论

Bash 在处理文本文件的许多数据处理任务时非常方便。它配备了许多经过高度优化的工具，但仍有些工具在实现相同结果时比其他工具更快。当处理短文件时，这种差异可能不重要，但在这篇文章中，我展示了当文件有成千上万行或数百万行时，这种差异开始变得重要。

了解我们喜欢的程序的性能影响可以节省我们等待工作的时间，并显著提高生产力。此外，我们看到尽管 Python 以慢著称，但在我们的用例中，Python 实现也非常快速。它确实需要更多的编码，但也更灵活。我肯定会选择 Python 来处理那些用 bash 解决起来过于复杂的情况。

感谢你阅读到这里，祝你编程愉快！

# 更多我的内容

[## 不再拖延：自动化开发环境和构建](https://towardsdatascience.com/without-further-ado-automate-dev-environments-and-build-f2f9bcaaae1e?source=post_page-----d83e4c5adf32--------------------------------)

### 通过环境和构建自动化使你的软件易于使用，给你的同事带来快乐。与…

[## 数据处理自动化与 inotifywait](https://towardsdatascience.com/data-processing-automation-with-inotifywait-663aba0c560a?source=post_page-----d83e4c5adf32--------------------------------)

### 如何在拥有一个生产就绪的 MLOps 平台之前进行自动化

[## 动态添加参数到 Argparse | Python Patterns](https://towardsdatascience.com/data-processing-automation-with-inotifywait-663aba0c560a?source=post_page-----d83e4c5adf32--------------------------------) [## 语音增强介绍：第二部分 — 信号表示](https://towardsdatascience.com/introduction-to-speech-enhancement-part-2-signal-representation-ab1deca2fa74?source=post_page-----d83e4c5adf32--------------------------------)

### 如何使用 `argparse.ArgumentParser` 根据用户输入指定不同的参数。

[## 数据处理自动化与 inotifywait](https://towardsdatascience.com/dynamically-add-arguments-to-argparse-python-patterns-a439121abc39?source=post_page-----d83e4c5adf32--------------------------------) [## 语音增强介绍：第二部分 — 信号表示](https://towardsdatascience.com/introduction-to-speech-enhancement-part-2-signal-representation-ab1deca2fa74?source=post_page-----d83e4c5adf32--------------------------------)

### 让我们深入探讨信号表示、傅里叶变换、频谱和谐波。

[## 语音增强介绍：第二部分 — 信号表示](https://towardsdatascience.com/introduction-to-speech-enhancement-part-2-signal-representation-ab1deca2fa74?source=post_page-----d83e4c5adf32--------------------------------)

# Medium 会员

你喜欢我的写作，并考虑订阅 Medium 会员以获得无限访问文章的权限吗？

如果你通过这个链接订阅，你将以不增加额外费用的方式支持我 [`medium.com/@mattiadigangi/membership`](https://medium.com/@mattiadigangi/membership)
