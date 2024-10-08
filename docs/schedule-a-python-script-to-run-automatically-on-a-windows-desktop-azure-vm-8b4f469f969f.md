# 在 Windows 桌面/Azure 虚拟机上自动调度 Python 脚本运行

> 原文：[`towardsdatascience.com/schedule-a-python-script-to-run-automatically-on-a-windows-desktop-azure-vm-8b4f469f969f`](https://towardsdatascience.com/schedule-a-python-script-to-run-automatically-on-a-windows-desktop-azure-vm-8b4f469f969f)

## 使用任务调度器自动刷新数据

[](https://mg-subha.medium.com/?source=post_page-----8b4f469f969f--------------------------------)![Subha Ganapathi](https://mg-subha.medium.com/?source=post_page-----8b4f469f969f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8b4f469f969f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8b4f469f969f--------------------------------) [Subha Ganapathi](https://mg-subha.medium.com/?source=post_page-----8b4f469f969f--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8b4f469f969f--------------------------------) ·阅读时间 4 分钟·2023 年 2 月 20 日

--

![](img/7c335d2a83cccaedcd33c874a8944c62.png)

图片来源 [Ales Nesetril](https://unsplash.com/de/@alesnesetril?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 由 [Unsplash](https://unsplash.com/s/photos/tech?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供

**概述**

假设你在 Windows 桌面/Azure 虚拟机上的 Anaconda 环境中开发了几个 Python 脚本。假设你希望按计划执行这些脚本。这里，我们讨论一种自动化脚本运行的方法。由于你使用的是 Anaconda 环境，你可能已经在 Anaconda 中安装了所有 Python 包的依赖项。

**过程**

下面是一个示意图，展示了自动化过程。正如你在下方看到的，我们将使用 Windows 任务调度器来实现这一点。

![](img/b9c9d3bf9cce5708ce55404e304880ae.png)

展示整体过程的示意图（作者提供）

让我们逐步进行 -

**步骤 1 — 编写批处理脚本**

批处理脚本执行两项任务 —

1.  它激活 Anaconda 环境

1.  一旦激活环境，它会在该环境中执行 Python 脚本。

在我们开始编写批处理脚本之前，让我们确定我们计划使用的 Anaconda 环境 —

从‘开始’菜单中，打开‘Anaconda Prompt’并输入以下命令 —

```py
conda info --envs
```

此命令将列出你机器上所有可用的 Anaconda 环境。记下你希望使用的 Anaconda 环境的名称。

现在，让我们继续编写批处理脚本。

要激活 Anaconda 环境，可以将以下内容添加到批处理脚本中 —

```py
@CALL "C:\ProgramData\Anaconda3\Scripts\activate.bat" base
```

上述假设你的 Anaconda 安装位于‘C:\ProgramData\Anaconda3’，并且‘base’是你希望使用的 Anaconda 环境。

为了执行 Python 脚本，可以将以下内容添加到批处理脚本中 -

```py
python C:\Users\Myscripts\Automation\Script_Name.py
```

上述内容假设 Python 脚本位于 C:\Users\Myscripts\Automation，并且名为‘Script_Name.py’。

综合起来，批处理脚本如下 -

```py
@CALL "C:\ProgramData\Anaconda3\Scripts\activate.bat" base
python C:\Users\Myscripts\Automation\Script_Name.py
```

你可以使用任何文本编辑器（如记事本或 Notepad++）来创建此文件。将其保存为 .bat 扩展名（例如‘mybatchfile.bat’）。

**第 2 步 — 以自动化方式安排脚本的执行**

从开始菜单启动‘Windows 任务计划程序’。

点击‘创建任务’ -

![](img/4ab88c0f94933fdcb240fd6fb222c622.png)

为任务命名，以指示脚本的功能。如果你希望脚本在用户未登录时也能运行，请确保选择‘无论用户是否登录都运行’。另外，选择‘以最高权限运行’，以确保没有权限/特权问题影响脚本执行。

![](img/9adfa355ee69b507fbd0c452a963a6c9.png)

现在，点击‘触发器’选项卡，然后点击‘新建’。

![](img/9fcaea9e66c786445d4bc2912112a0f8.png)

选择计划的频率和/或任何你希望添加的随机延迟。

![](img/8862cc5c5e9d993bd2a846dec3e29f59.png)

接下来，转到‘操作’选项卡并点击‘新建’。

![](img/f725e88c7b3758dd9943c9b174c97e5a.png)

选择操作‘启动程序’。然后，在‘程序/脚本’文本框中提供你在第 1 步中创建的批处理文件的位置。点击‘确定’。

![](img/8f688f2978dcc1c6b475c76da13b4c86.png)

如果你希望进一步控制自动化运行，可以查看‘条件’和‘设置’选项卡。

# 总结

你可能会想，为什么不能直接使用任务计划程序来运行 Python 脚本。既然我们所有的包依赖都是通过 Anaconda 安装的，为什么不直接使用它，而是重新安装所有包来通过任务计划程序运行脚本呢？这就是为什么我们使用批处理脚本来激活 Anaconda 环境，然后在那里运行 Python 脚本的原因。祝学习愉快！
