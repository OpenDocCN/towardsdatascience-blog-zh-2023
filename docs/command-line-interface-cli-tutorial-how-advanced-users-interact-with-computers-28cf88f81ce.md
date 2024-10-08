# 命令行接口（CLI）教程 — 高级用户如何与计算机交互

> 原文：[`towardsdatascience.com/command-line-interface-cli-tutorial-how-advanced-users-interact-with-computers-28cf88f81ce`](https://towardsdatascience.com/command-line-interface-cli-tutorial-how-advanced-users-interact-with-computers-28cf88f81ce)

## 提高与计算机交互生产力的 CLI 介绍

[](https://medium.com/@fmnobar?source=post_page-----28cf88f81ce--------------------------------)![Farzad Mahmoodinobar](https://medium.com/@fmnobar?source=post_page-----28cf88f81ce--------------------------------)[](https://towardsdatascience.com/?source=post_page-----28cf88f81ce--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----28cf88f81ce--------------------------------) [Farzad Mahmoodinobar](https://medium.com/@fmnobar?source=post_page-----28cf88f81ce--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----28cf88f81ce--------------------------------) ·12 分钟阅读·2023 年 3 月 1 日

--

![](img/25e1e0e3eec9d9d55cad9636d0333b2a.png)

程序员兔子，图片来源 [DALL.E 2](https://openai.com/dall-e-2/)

你是否还记得电影中那种场景：黑客突然在他们的笔记本电脑上打开一个黑暗的屏幕，猛力敲击键盘几秒钟，随后发生了一些重要事件？在这些场景中（如果这些事件真的发生的话），黑客通过文本输入直接与计算机交互，这与我们通常通过图形用户界面（GUI）与手机或笔记本电脑交互的方式不同。在这篇文章中，我们将介绍如何仅通过文本与计算机交互。到文章结尾时，你甚至可能会发现这种方法比通常的 GUI 更高效。

命令行接口或 CLI 是一种通过在终端（或控制台）中输入文本命令与计算机交互的方法。这与大多数用户习惯的图形用户界面（GUI）不同。GUI 的一个例子是 iPhone 的 iOS，界面是图形化的，用户可以点击和滑动以与设备互动。高级和技术用户认为 CLI 是与计算机交互的更优方法，因为 CLI 提供了更高效和直接的控制。

我记得在开始尝试使用 CLI 之前，它让我感到多么令人畏惧。但是学习过程证明是非常简单、值得的，更重要的是很有趣。所以我决定创建这个教程来分享我的学习经验——只要你阅读完这篇文章，你也可以认为自己已经“入门”了！

我将首先包含一个包含我在本篇文章中介绍的命令的表格式速查表，这对将来使用很方便，然后将详细介绍这些命令，并附上示例。

开始吧！

*(除非另有说明，否则所有图片均由作者提供。)*

# CLI 速查表

可以参考这个表格（或保存它）以备将来使用，在你阅读了带有示例的教程之后。

![](img/cae4ea7e187c044c9fb88dd3dedc2778.png)

CLI 速查表

从这一点开始，我将详细介绍这些命令，并提供示例。我鼓励你按照每一步操作，并在阅读教程的同时通过实践来学习。

# 1\. CLI 在哪里？

我们从如何启动命令行界面（CLI）开始。

+   **Mac:** 我们需要找到“终端”。所以请在你的 Mac 的 Spotlight 搜索中搜索“终端”。

> **专业提示:** 你可以按住“command”键，同时按下空格键，Spotlight 搜索应该会打开。然后只需开始输入“终端”，当它在 Spotlight 搜索中出现时按下“return”。

+   **Windows:** 在 Windows 中，CLI 被称为“命令提示符”。点击“开始”按钮，在搜索栏中输入“cmd”。然后点击“命令提示符”或“Windows 命令处理器”。

# 2\. 了解命令

一旦我的终端启动，我会看到如下内容（你的内容可能会略有不同，但结构相同）：

```py
farzad 20230211_CLI %
```

在上面的示例中，“farzad”是我的用户名，“20230211_CLI”是我当前的目录。这是我创建这个教程的目录，日期为今天。你可能会看到一个波浪号符号（~），它代表主目录。

这是刚开始的地方，我们可以开始输入命令了。

正如我们在这个示例中看到的，命令行界面向我们显示了我们所在的位置（例如，上面的“20230211_CLI”），但如果我们想看到当前路径的完整路径，可以使用`pwd`或打印工作目录命令，如下所示：

```py
farzad 20230211_CLI % pwd
/Users/farzad/Downloads/M/Archive/20230211_CLI
```

第一行是命令，第二行显示从我的主目录（即最高级别目录）到当前所在位置的完整地址。

# 3\. 列出文件和目录

知道我们所在位置的文件和文件夹是非常有用的。在图形用户界面（GUI）中，我们可以直接查看文件和文件夹，而在命令行界面（CLI）中，有一些命令可以做到这一点。

我们可以使用`ls`或列表命令来查看目录的内容。这是我使用此命令时看到的内容：

```py
farzad 20230211_CLI % ls
CLI.ipynb random-folder
```

第一行是命令，第二行是结果。第二行告诉我们有一个名为“CLI.ipynb”的文件（这是我撰写本篇文章的 Jupyter 笔记本）和一个名为“random-folder”的目录。

但如果我们想要看到每个文件或目录的更多细节怎么办？例如，看到文件的大小等。我们可以通过在现有的列表命令中添加`-l`（即长格式）选项来实现，如下所示：

```py
farzad 20230211_CLI % ls -l
total 8
-rw-r--r-- 1 mafarzad staff 3673 Feb 11 17:40 CLI.ipynb
drwxr-xr-x 2 mafarzad staff 64 Feb 11 17:35 random-folder
```

这包含了更多信息。让我们从左边开始了解这所有的含义。

1.  **权限：** 这是一个由 10 个字符组成的系列，表示该特定文件或目录的权限，如下所示：

+   字符 1 表示文件类型：`—` 表示常规文件；`d` 表示目录；`l` 表示符号链接

+   字符 2 到 4：所有者的权限

+   字符 5 到 7：组的权限

+   字符 8 到 10：其他用户的权限

+   这九个字符可以由以下内容组成：

    — `r`：读取

    — `w`：写入

    — `x`：执行

    — `-`：未授予权限

2\. **链接数：** 硬链接到文件或目录的数量

3\. **所有者：** 文件或目录所有者的用户名

4\. **组：** 与文件或目录关联的组名称

5\. **大小：** 以字节为单位的大小

6\. **日期和时间：** 文件或目录上次更改的日期和时间

7\. **名称：** 文件或目录的名称

# 4\. 导航

现在我们知道我的位置下有一个名为“random-folder”的目录，让我们看看如何进入那个目录。

我们可以使用 `cd` 或更改目录命令来导航，如下所示：

```py
farzad 20230211_CLI % cd random-folder 
farzad random-folder % 
```

在上述命令中，我使用了 `cd` 导航到“random-folder”，然后第二行显示当前所在位置是“random-folder”（而不是之前的“20230211_CLI”）。

但我们如何返回上一级目录呢？换句话说，现在我们在“random-folder”位置，如何返回到之前的地方，即“20230211_CLI”？我们可以使用更改目录命令，后跟一个空格和两个句点。我将在下面的示例中使用 `pwd` 以演示这个过程。

```py
farzad random-folder % pwd
/Users/mafarzad/Downloads/M/Archive/20230211_CLI/random-folder
```

首先，我们仅使用 `pwd` 确认我们在“random-folder”中。我们还看到“20230211”仅在我们当前位置的上一级。然后让我们使用 `cd ..` 命令跳转一个级别（确保 `cd` 和 `..` 之间包含一个空格），如下所示：

```py
farzad random-folder % cd ..
farzad 20230211_CLI % pwd
/Users/mafarzad/Downloads/M/Archive/20230211_CLI
```

这里我们首先使用 `cd ..` 返回到“20230211_CLI”，然后使用 `pwd` 确认位置。

由于我想在下一个示例中使用“random-folder”，让我们再练习一次返回到“random-folder”位置，如下所示：

```py
farzad 20230211_CLI % cd random-folder 
farzad random-folder % pwd
/Users/mafarzad/Downloads/M/Archive/20230211_CLI/random-folder
```

# 5\. 创建和删除文件及目录

`touch` 命令可用于创建新文件。在下面的命令中，我将创建一个名为“new_file.txt”的新文件，然后使用列表命令返回该目录的内容：

```py
farzad random-folder % touch new_file.txt
farzad random-folder % ls -l
total 0
-rw-r--r-- mafarzad staff 0 Feb 11 18:04 new_file.txt
farzad random-folder % 
```

在第一行中，我创建了文本文件，然后使用长格式的列表（即 `ls -l`）确认了文件已创建。有趣的是，`total 0` 表示该目录中没有文件，而文件名实际上是列出的，因为新创建的文件此时只是一个名称，没有实际内容。

我们可以使用 `mkdir` 或 make directory，后跟目录名称，来创建一个新文件夹（或目录），如下所示：

```py
farzad random-folder % mkdir new-folder
farzad random-folder % ls -l
total 0
drwxr-xr-x 2 mafarzad staff 64 Feb 11 18:08 new-folder
-rw-r--r-- mafarzad staff 0 Feb 11 18:04 new_file.txt
```

在第一行中，我创建了一个名为“new-folder”的新目录，然后使用长格式列表，我们看到该目录与我们在上一步中创建的文件并排存在。

接下来，让我们使用 `rm` 或 remove 命令删除文本文件，后跟文件名，如下所示：

```py
farzad random-folder % rm new_file.txt 
farzad random-folder % ls -l
total 0
drwxr-xr-x 2 mafarzad staff 64 Feb 11 18:08 new-folder
```

在命令的第一行中，我删除了文本文件，然后使用长格式列表，我们看到当前目录中仅剩下目录，而文本文件已被删除。

最后，我们可以使用 `rmdir` 或 remove directory，后跟其名称，来删除目录，如下所示：

```py
farzad random-folder % rmdir new-folder 
farzad random-folder % ls -l
total 0
```

命令的第一行删除了目录，通过长格式列表确认它已被删除。

# 6\. 复制文件或目录

## 6.1\. 复制文件

这个概念类似于 Mac 和 Windows 操作系统中的复制和粘贴，但它可以用 `cp` 或 copy 命令在一行内完成，命令格式是源文件名和目标文件名之间用空格分隔。命令格式如下：

```py
cp source-file destination-file
```

让我们通过一个示例来实现它。首先，我们创建一个名为“file-1.txt”的文件，以便我们有东西可以复制，然后使用列出命令确认文件存在。

```py
farzad random-folder % touch file-1.txt
farzad random-folder % ls
file-1.txt
```

接下来，让我们将“file-1.txt”复制到一个名为“file-2.txt”的新文件中，并再次使用列出命令确认我们的更改。

```py
farzad random-folder % cp file-1.txt file-2.txt
farzad random-folder % ls
file-1.txt file-2.txt
```

正如预期的那样，现在有两个文件，包括第一个文本文件（即源文件）和第二个文本文件（即目标文件）。

## 6.2\. 复制目录

这个概念与文件复制非常相似，但我们在 `cp` 命令中添加了递归选项 `-r`（即 `cp -r`）。递归意味着 `cp` 命令会复制目录及其所有内容。此类命令的整体格式为：

```py
cp -r source-directory destination-directory
```

让我们通过一个示例来操作。首先，看看我们当前目录中存在哪些文件。

```py
farzad random-folder % ls
file-1.txt file-2.txt
```

接下来，让我们在目录中上移一级，查看那里有哪些目录和文件。

```py
farzad random-folder % cd ..
farzad 20230211_CLI % ls 
CLI.ipynb random-folder
```

现在，让我们将“random-folder”目录及其所有内容复制到第二个名为“random-folder-2”的目录中，如下所示：

```py
farzad 20230211_CLI % cp -r random-folder random-folder-2
```

接下来，让我们使用列出命令确认新目录已创建。然后我们将使用更改目录命令进入新创建的目录（即“random-folder-2”），并在那里使用列出命令，确保原始目录“random-folder”中存在的所有内容都已复制到目标目录“random-folder-2”中。

```py
farzad 20230211_CLI % ls
CLI.ipynb random-folder random-folder-2
farzad 20230211_CLI % cd random-folder-2
farzad random-folder-2 % ls
file-1.txt file-2.txt
```

正如预期的那样，新创建的“random-folder-2”包含了我们在“random-folder”源目录中的两个文本文件。

# 7\. 删除（删除）文件或目录

这个操作的原理很简单——我们只是希望删除文件或文件夹。我们可以使用 `rm` 或 remove 命令，后跟一个空格和文件名来删除文件。删除目录也是完全一样的，但我们需要在命令中添加递归选项 `-r`。以下是需要遵循的一般格式：

```py
rm name-of-file-to-be-removed
rm -r name-of-directory-to-be-removed
```

让我们看看这个目录中有哪些文件，然后可以删除其中一个以进行练习。

```py
farzad random-folder-2 % ls
file-1.txt file-2.txt
farzad random-folder-2 % rm file-1.txt 
farzad random-folder-2 % ls
file-2.txt
```

第一行使用 `list` 命令列出了当前目录的内容，其中包含两个文本文件。在第三行（或第二行命令），我们使用 `remove` 命令删除了 “file-1.txt”，然后使用 `list` 命令确认它已被删除。

# 8\. 查看文件内容

到目前为止，我们只查看了文件和目录的名称，但如果我们想查看文件的内容呢？我们可以使用 `cat` 或 `concatenate` 命令来查看文本文件的内容。为此，我在名为 “file-1.txt” 的文件中添加了一个链接，让我们看看如何使用 `concatenate` 命令来查看该文件的内容。

```py
farzad random-folder-2 % ls
file-1.txt file-2.txt
farzad random-folder-2 % cat file-1.txt 
If you liked this post, follow me on Medium at: 
https://medium.com/@fmnobar
```

第一行命令使用 `list` 列出当前目录中的文件名。在第二行，我们看到当前位置有两个文件。然后在第三行（或第二行命令），我们使用 `concatenate` 命令查看 “file-1.txt” 文件的内容，结果显示在最后两行，如下所示：

```py
If you liked this post, follow me on Medium at: 
https://medium.com/@fmnobar
```

# 9\. 搜索

有很多时候我们需要搜索一串字符，这可以通过使用 `grep` 命令来完成。这个命令允许我们在文件中搜索一个模式（在这个上下文中，我们称这串字符为 “模式”）。例如，如果我们想在 “file-1.txt” 中搜索 “medium” 一词，可以按照以下方式进行：

```py
farzad random-folder-2 % grep medium file-1.txt
https://medium.com/@fmnobar
```

第一行命令使用 `grep` 或全局正则表达式打印命令搜索 “file-1.txt” 中的 “medium” 一词，第二行是此搜索的结果。注意，“file-1.txt” 包含两行，但 `grep` 命令只返回包含我们寻找的模式的行。

`grep` 命令非常多功能且灵活。例如，让我们看看可以使用的两个选项：

1.  `-c` 选项用于仅显示包含我们寻找的模式的行数。例如，让我们查看 “file-1.txt” 中 “medium” 模式出现了多少次。我们知道结果应该是 1，让我们验证一下。

```py
farzad random-folder-2 % grep -c medium file-1.txt
1
```

2\. `-v` 选项用于显示所有不包含我们寻找的模式的行。我们知道在 “file-1.txt” 中只有一行不包含 “medium” 模式——让我们在 CLI 中验证一下。

```py
farzad random-folder-2 % grep -v medium file-1.txt
If you liked this post, follow me on Medium at: 
```

# 结论

在这篇文章中，我们讨论了命令行界面（CLI）如何不同于图形用户界面（GUI），以及为什么技术用户倾向于更喜欢 CLI 而非 GUI，以获得更高的效率、生产力和灵活性。然后我们通过示例介绍了 CLI 中最常用的命令。在通过这些示例之后，你应该能够舒适地启动终端并开始在日常工作中使用 CLI。类似于其他技能，使用 CLI 的次数越多，你会发现它变得越来越简单和有益。希望这篇文章能给你的 CLI 之旅提供一个良好的开端！

# 感谢阅读！

如果你觉得这篇文章对你有帮助，请[在 Medium 上关注我](https://medium.com/@fmnobar)并订阅以接收我最新的文章！

[## 通过我的推荐链接加入 Medium - Farzad Mahmoodinobar](https://medium.com/@fmnobar/membership?source=post_page-----28cf88f81ce--------------------------------)

### 阅读 Farzad（以及 Medium 上其他作者）的每一个故事。你的会员费直接支持 Farzad 和其他…

[medium.com](https://medium.com/@fmnobar/membership?source=post_page-----28cf88f81ce--------------------------------)
