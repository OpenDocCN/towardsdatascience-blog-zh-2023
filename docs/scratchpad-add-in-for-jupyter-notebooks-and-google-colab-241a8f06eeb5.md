# Jupyter Notebooks 和 Google Colab 的 Scratchpad 插件

> 原文：[`towardsdatascience.com/scratchpad-add-in-for-jupyter-notebooks-and-google-colab-241a8f06eeb5`](https://towardsdatascience.com/scratchpad-add-in-for-jupyter-notebooks-and-google-colab-241a8f06eeb5)

## 在 Jupyter notebooks 和 Google Colab 中访问 Scratchpad 功能

[](https://nrlewis929.medium.com/?source=post_page-----241a8f06eeb5--------------------------------)![Nicholas Lewis](https://nrlewis929.medium.com/?source=post_page-----241a8f06eeb5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----241a8f06eeb5--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----241a8f06eeb5--------------------------------) [Nicholas Lewis](https://nrlewis929.medium.com/?source=post_page-----241a8f06eeb5--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----241a8f06eeb5--------------------------------) ·阅读时间 3 分钟·2023 年 1 月 31 日

--

![](img/7a1958120a4ddd93fde115d40e7a5583.png)

图片由 [Justin Morgan](https://unsplash.com/fr/@justin_morgan?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在过去的几年里，我一直使用 Jupyter notebooks 来开发想法和探索数据。在众多方便的 [插件](https://jupyter-contrib-nbextensions.readthedocs.io/en/latest/) 中，有一个我已经彻底融入到我的工作流程中，以至于每次开始使用不同的计算机时，我都会立即添加它。它就是下面展示的 Scratchpad。

![](img/28371595f7cb6161fde29f7a0d765fae.png)

使用 Jupyter Notebook Scratchpad 扩展的截图。截图由作者提供

当你在开发新代码和探索数据时，你可能会发现自己需要快速测试一个代码片段是否有效。或者，也许你想输出一些内容，但在当前工作单元中这样做会干扰到其他内容。使用 Scratchpad，你可以非常方便地按下 `ctrl+B` 来弹出一个新单元。我几乎比其他任何功能都更频繁地使用这个功能。它不仅可以更好地保持额外单元不混入代码中并且不容易遗忘，而且也 *更快* —— 只需几次 `ctrl+B` 的点击即可打开和关闭，而不需要添加和删除新单元格以及上下滚动页面。它几乎就像是为你的 Jupyter Notebook 配备了一个双显示器。

有很多教程展示了如何[下载](https://jupyter-contrib-nbextensions.readthedocs.io/en/latest/install.html)和添加这个扩展（在此过程中也可以探索其他非官方扩展，如果有你喜欢的请告诉我！）。我本来不会写这篇文章，除了一个原因。我最近使用 Google Colab 的频率大大增加，感到很沮丧，因为似乎没有办法获得相同的功能。这几乎让我不再使用 Colab。我尝试通过几次 Google 搜索找到解决方法，但我遇到的“[Scratchpad](https://colab.research.google.com/notebooks/empty.ipynb)”完全不像我所寻找的那样。然后我偶然发现了一个类似功能的东西。现在，在写这篇文章的时候，它的速度不如在 Jupyter 笔记本中获取 scratchpad 快，但至少它存在！它确实保持了我最终代码的整洁，我不会有忘记的单元格扰乱后续流程的情况。

1) 在打开的 Colab 笔记本中，选择一个单元格，然后选择下方显示的弹出窗口图标。

![](img/7affbeb311eb666239127a571bb15a43.png)

作者截屏

2) 这会打开一个侧边窗口，复制你正在编辑的单元中的行。如果你在这里编辑代码，它会传输到工作单元。它不完全像我认为对测试代码有用的未连接的 scratchpad。你必须再次选择弹出窗口图标。

![](img/bc81298b2c74586a8dcec7fb7fdc1b24.png)

作者截屏

3) 这会打开一个你可以自由使用的测试单元，它的功能就像 Jupyter 中的 scratchpad 版本一样。现在你可以尽情编码了！

![](img/c5262485d9764cd7b80aed277f53b560.png)

作者截屏

就这样！我希望这篇文章很快会过时，并且能够有一个键盘快捷键来访问这个功能。如果你像我一样，使用 Scratchpad 的频率高于其他任何功能，这对你在使用 Colab 时是一个可行的选项。随时在[LinkedIn](https://www.linkedin.com/in/nicholas-lewis-0366146b/)上与我联系，告诉我哪些扩展对你的工作流程有帮助。希望这个扩展能在你处理一些[类似我做过的项目](https://nrlewis929.medium.com/)时帮助到你！
