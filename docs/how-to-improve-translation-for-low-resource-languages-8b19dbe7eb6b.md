# 如何改善低资源语言的翻译

> 原文：[`towardsdatascience.com/how-to-improve-translation-for-low-resource-languages-8b19dbe7eb6b`](https://towardsdatascience.com/how-to-improve-translation-for-low-resource-languages-8b19dbe7eb6b)

## 非洲语言的评估案例

[](https://medium.com/@konkiewicz.m?source=post_page-----8b19dbe7eb6b--------------------------------)![Magdalena Konkiewicz](https://medium.com/@konkiewicz.m?source=post_page-----8b19dbe7eb6b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8b19dbe7eb6b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8b19dbe7eb6b--------------------------------) [Magdalena Konkiewicz](https://medium.com/@konkiewicz.m?source=post_page-----8b19dbe7eb6b--------------------------------)

·发布在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----8b19dbe7eb6b--------------------------------) ·阅读时间 7 分钟·2023 年 1 月 16 日

--

![](img/8b3f929a137bf81296822e97b27b4229.png)

图片由[Gerd Altmann](https://pixabay.com/users/geralt-9301/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=110775)提供，来源于[Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=110775)

**介绍**

近年来，机器翻译系统质量的提升使得使用这些系统的解决方案和设备对普通消费者变得可用。许多应用程序提供自动翻译文本、语音或图像的功能，允许用户在不懂语言的情况下进行沟通或理解。

然而，并非所有语言都如此。事实上，占全球语言三分之一的非洲语言并未得到同样程度的进步。这是因为近年来为其提供的自然语言处理和机器翻译资源较少。

最新的全球机器翻译会议（WMT2022）举办了一项特别任务，由 Meta、Microsoft 和 Toloka 的研究人员准备，旨在解决这个问题。结果对 24 种不同的非洲语言进行了评估，这些语言由 14 种不同的机器翻译系统生成，使用了自动评分和人工评估。

本文总结了该项目的主要成就。

**挑战**

研究包括了下面图片中的 24 种不同的非洲语言，以及用于评估的英语和法语。这使得可以检查大约 100 种不同的语言方向。

![](img/1c5a1cf13b63051d8829273ce2026c30.png)

参与该任务的 24 种非洲语言——作者提供的照片

**数据集**

非洲语言的公共资源非常有限，参与者被鼓励贡献涉及评估语言的新型单语、双语和多语言数据集。下面是六个新提交的数据集的信息。

+   [**LAVA**](https://drive.google.com/drive/folders/179AkJ0P3fZMFS0rIyEBBDZ-WICs2wpWU) **—** 包含数百万个平行双语句子的语料库，涉及五种非洲语言（afr, kin, lug, nya, saw）。

+   [**MAFAND-MT**](https://github.com/masakhane-io/lafand-mt) — 包含几千个高质量、人工翻译的平行句子的语料库，涉及新闻领域中的 21 种非洲语言，覆盖了挑战中评估的 14 种语言（amh, hau, ibo, kin, lug, luo, nya, sna, swh, tsn, wol, xho, yor, zul）。

+   [**KenTrans**](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi%3A10.7910%2FDVN%2F6N5V1K) — 包含 12400 个斯瓦希里语与其他两种肯尼亚语言（Dholuo 和 Luhya）之间翻译句子的语料库。

+   [**来自 ParaCraw 的单语非洲语言**](https://data.statmt.org/martin/) — 包含来自互联网档案和定向爬取的单语数据的语料库。覆盖了 22 种评估语言（afr, amh, fuv, hau, ibo, kam, lin, lug, luo, nso, nya, orm, sna, ssw, swh, tsn, tso, umb, wol, xho, yor, zul）。

+   [**SA Languages**](https://drive.google.com/drive/folders/1jYwpxEdRxqXlB7BSmE6JxDar61U91xfI) **—** 使用来自南非政府网站的公开数据构建的语料库。覆盖了 4 种评估语言（nso, tsn, xho, zul）。

+   [**WebCrawlAfrican**](https://github.com/pavanpankaj/Web-Crawl-African) **—** 包含近 700000 个句子的平行语料库，涉及不同的非洲语言和英语。覆盖了挑战中涉及的 15 种语言（afr, ling, ssw, amh, lug, tsn, nya, hau, orm, xho, ibo, tso, yor, swh, zul）。

**系统和团队**

参与挑战的翻译系统仅允许使用上述资源或其他两个著名的语料库进行训练。共有八个团队提交了十四个系统。

+   **CAIR ANVITA —** 团队提交了一个以英语为中心的多语言变换器模型，支持 24 种非洲语言。作者应用了几种启发式方法来过滤数据，并展示了使用较大变换器模型比使用较小版本表现更好。

+   **开普敦** — 他们的系统专注于八种南部和东南非洲语言。作者训练了一个多语言 NMT 系统，并探索了使用合成和抽样技术来平衡语料库。

+   **GMU** — 团队专注于对 26 种语言的预训练多语言 DeltaLM 进行微调。微调基于语言及语言家族。

+   **IIAI DenTra** — 他们提交了一个模型，该模型结合了传统的翻译损失和自监督任务，并使用了未标记的单语数据。该模型通过翻译和回译执行去噪任务，覆盖了 24 种语言。

+   **Masakhane —** 他们提交了一个基于 M2M-100 的模型，通过在正样本（干净数据）和来自自动对齐数据的负样本上进行微调。该模型在使用过滤数据后显示出显著改进。

+   **腾讯 Borderline —** 他们提交了一个**大型**的变换器模型（1.02B 参数），通过标记回译和自我翻译增强了零-shot 对。

+   **字节跳动 VolcTrans —** 他们的系统使用了并行和单语数据，并通过启发式方法进一步清理（修正标点符号不匹配、去除过短/过长或噪声句子）。此外，数据还通过来自英语和法语的回译进行增强。

+   **SRPH-DAI —** 他们提交了一个基于 mT5 的模型，增加了额外的适配器，针对每个翻译任务进行了微调，然后通过适配器融合进行合并。该模型使用了一组启发式方法过滤的数据，并没有使用其他数据增强技术。

**系统评估**

系统的评判标准包括 [BLUE](https://en.wikipedia.org/wiki/BLEU) 、C[hrF](https://aclanthology.org/W15-3049/) 和人工评估。前两者是机器翻译中使用的知名自动化指标。另一方面，由于资源限制和注释员的可用性，人工评估在之前的研究中没有被广泛使用。因此，这在该领域的研究中是一个重要的转变。

本研究通过众包和 [Toloka](https://toloka.ai/) 进行人工评估。它涉及精心挑选的注释员在 0 到 100 的范围内评判翻译质量，如下面的屏幕截图所示。

![](img/d867f8410ebe9498454fbb82f4f159f2.png)

带有标注任务的界面，包括源句子和从提交的模型中随机选择的两种翻译（屏幕截图中的语言对为 Afrikaans — English）。— 作者提供的照片

众包可以成为评估机器翻译系统时获取注释员的有效方式，但它需要对参与者进行仔细挑选并实施严格的质量控制机制。需要确保注释员在源语言和目标语言中的流利度，本研究通过地理选择和语言测试来实现。此外，相同的任务会分配给多个注释员，然后汇总结果以获得更高的分数可信度。下面是本挑战中使用的确切流程。

![](img/d98d59fc725d8433006a5afbeb336c29.png)

Toloka Crowd 机器翻译评估流程和质量控制 — 作者提供的照片

关于人工评估的一个有趣发现是，它与其他指标如 BLUE 和 ChrF 高度相关，这证明它是一种有效的评分方法。

**结果**

表现最佳的系统是腾讯 Borderline，它在包括 BLUE、ChrF 和人工评估在内的所有评估指标中都超过了其他系统。紧随其后的是 GMU 系统，排名第二。下面的截图显示了每个系统的详细性能结果。

![](img/ccd2d6feef886bbc7fbde9de5f0612a6.png)

WMT2022 中针对非洲语言的系统评估结果，如[研究论文](https://www.statmt.org/wmt22/pdf/2022.wmt-1.72.pdf)所示 — 作者截图

很明显，用于训练系统的数据对性能产生了重要影响。使用更多数据、数据增强技术以及广泛的启发式方法清理数据的系统在不同语言对中的表现更好。

此外，与去年结果相比，取得了较大的进展。平均系统在 72 种语言对中，BLEU 分数提高了约 7.5 点（从 15.09 提高到 22.60）。值得一提的是，无论是以英语为中心的语言对还是非洲语言对，都出现了改进，其中后者尤其具有挑战性。下面你可以看到按改进幅度排序的前十种语言对。

![](img/8eff3bf47f7fe056b81ec6a72c5a629c.png)

去年最佳结果的前十种语言对的最大改进 — 作者截图

另一个重要的观察是，南非语言似乎更容易翻译（平均约 29 BLEU），而西非语言则更具挑战性（平均约 15 BLEU）。此外，翻译成非洲语言比从非洲语言翻译要困难。

**总结**

在这篇文章中，你了解了 WMT2022 中非洲语言机器翻译的最新进展。

评估了大约 100 种语言对，翻译质量与前几年相比有了显著提升。该领域的一些新进展包括使用广泛的人工评估来打分系统，并且包括数据跟踪，其中参与者提交了新的语言语料库。

如果你想了解更多细节，可以阅读以下[论文](https://www.statmt.org/wmt22/pdf/2022.wmt-1.72.pdf)，该论文专门讨论了这一任务。

*PS: 我在 Medium 上撰写了简单易懂的基本数据科学概念文章，并在*[***aboutdatablog.com***](https://www.aboutdatablog.com/)*上发布。你可以订阅我的*[***电子邮件列表***](https://medium.com/subscribe/@konkiewicz.m)*，每次我写新文章时都会通知你。如果你还不是 Medium 会员，你可以* [***在这里加入***](https://medium.com/@konkiewicz.m/membership)*。

*以下是一些你可能会喜欢的其他文章：*

[](/detecting-and-fixing-data-drift-in-computer-vision-8d0853af1246?source=post_page-----8b19dbe7eb6b--------------------------------) ## 计算机视觉中的数据漂移检测与修复

### 可以运行的代码实用案例研究

towardsdatascience.com [](/top-8-magic-commands-in-jupyter-notebook-c1582e813560?source=post_page-----8b19dbe7eb6b--------------------------------) ## Jupyter Notebook 中的 8 个神奇命令

### 通过学习最有用的命令来提升你的生产力

towardsdatascience.com [](/how-to-do-data-labeling-versioning-and-management-for-ml-ba0345e7988?source=post_page-----8b19dbe7eb6b--------------------------------) ## 如何进行数据标注、版本控制和管理

### 一个丰富食品数据集的案例研究

towardsdatascience.com [](/jupyter-notebook-autocompletion-f291008c66c?source=post_page-----8b19dbe7eb6b--------------------------------) ## Jupyter Notebook 自动补全

### 如果你还没有使用，这是数据科学家应当使用的最佳生产力工具…

towardsdatascience.com
