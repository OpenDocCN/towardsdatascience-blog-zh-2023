# 无OCR文档数据提取与变换器（2/2）

> 原文：[https://towardsdatascience.com/ocr-free-document-data-extraction-with-transformers-2-2-38ce26f41951?source=collection_archive---------1-----------------------#2023-08-10](https://towardsdatascience.com/ocr-free-document-data-extraction-with-transformers-2-2-38ce26f41951?source=collection_archive---------1-----------------------#2023-08-10)

## Donut与Pix2Struct在自定义数据上的对比

[](https://toon-beerten.medium.com/?source=post_page-----38ce26f41951--------------------------------)[![Toon Beerten](../Images/f169eaa8cefa00f17176955596972d57.png)](https://toon-beerten.medium.com/?source=post_page-----38ce26f41951--------------------------------)[](https://towardsdatascience.com/?source=post_page-----38ce26f41951--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----38ce26f41951--------------------------------) [Toon Beerten](https://toon-beerten.medium.com/?source=post_page-----38ce26f41951--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3aef462e13b5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Focr-free-document-data-extraction-with-transformers-2-2-38ce26f41951&user=Toon+Beerten&userId=3aef462e13b5&source=post_page-3aef462e13b5----38ce26f41951---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----38ce26f41951--------------------------------) ·7分钟阅读·2023年8月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F38ce26f41951&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Focr-free-document-data-extraction-with-transformers-2-2-38ce26f41951&user=Toon+Beerten&userId=3aef462e13b5&source=-----38ce26f41951---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F38ce26f41951&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Focr-free-document-data-extraction-with-transformers-2-2-38ce26f41951&source=-----38ce26f41951---------------------bookmark_footer-----------)![](../Images/45dc7196c8f321f51a04bce1054c5709.png)

图片由作者提供（[使用](https://huggingface.co/spaces/albarji/mixture-of-diffusers)）

这两种变换器模型对文档的理解如何？在第二部分中，我将展示如何训练它们并比较它们在关键索引提取任务中的结果。

# 调整Donut模型

所以让我们从 [第 1 部分](https://medium.com/towards-data-science/ocr-free-document-data-extraction-with-transformers-1-2-b5a826bc2ac3) 开始，在那里我解释了如何准备自定义数据。我将数据集的两个文件夹打包并上传到一个新的 huggingface 数据集 [这里](https://huggingface.co/datasets/to-be/ghega_dataset_preprocessed)。我使用的 Colab 笔记本可以在 [这里](https://github.com/Toon-nooT/notebooks/blob/main/Donut_vs_pix2struct_2_Ghega_donut.ipynb) 找到。它将下载数据集，设置环境，加载 Donut 模型并进行训练。

在微调了 75 分钟后，我在验证指标（即编辑距离）达到 0.116 时停止了：

![](../Images/f3f7633cd3098c341c9cb21ade7ca5bc.png)

作者提供的图像

在字段级别，我得到这些验证集结果：

![](../Images/f960b805faf7ef905d75be3cfe87e98c.png)

作者提供的图像

当我们查看*Doctype*时，我们发现 Donut 总是正确地将文档识别为*专利*或*数据表*。因此，我们可以说分类达到了 100% 的准确率。同样需要注意的是，即使我们有一个类别*数据表*，它也不需要文档上出现这个确切的词来进行分类。对于 Donut 来说，这并不重要，因为它经过微调以这样识别。

其他领域的得分也相当不错，但仅凭这张图表很难了解内部情况。我想看看模型在特定情况下的正确与错误之处。因此，我在我的笔记本中创建了一个例行程序来生成 HTML 格式的报告表。对于我的验证集中的每个文档，我都有这样的行条目：

![](../Images/2135bed45f368f3479c204a794a4ecb9.png)

作者提供的图像

左侧是识别（推断）数据及其真实值。右侧是图像。我还使用了颜色代码以便快速概览：

![](../Images/459bea0d48308f7f7011fe4bc18fadbe.png)

作者提供的图像

理想情况下，一切都应该用绿色突出显示。如果你想查看验证集的完整报告，可以在 [这里](https://neontreebot.be/data/Donut_vs_Pix2Struct/Donut_result_report.html) 查看，或者本地下载这个 [zip 文件](http://Donut_vs_pix2struct_validation_results.zip)。

有了这些信息，我们可以发现常见的 OCR 错误，如*Dczcmbci*（应为*December*）或*GL420*（应为*GL420*，0 和 O 难以区分），这些错误会导致假阳性。

现在让我们关注表现最差的字段：电压。以下是推断数据、真实值和实际相关文档片段的一些样本。

![](../Images/38f1432cc00e4bad8d920228707e2d48.png)

作者提供的图像

问题在于真实值大多是错误的。是否包括单位（Volt 或 V）没有标准。有时会包含无关文本，有时只是一个（错误的！）数字。我现在明白为什么 Donut 会对此感到困难。

![](../Images/2c144a76bc7dd36ffe3e1019b18a256b.png)

作者提供的图像

上面是一些Donut实际上给出最佳答案的样本，而实际情况是不完整或错误的。

![](../Images/f3d2be65fbde5bad2154dd824fdac5eb.png)

作者提供的图像

上面是另一个糟糕训练数据混淆Donut的好例子。地面真实值中的‘I’字母是OCR读取信息前的垂直线的伪影。有时它存在，有时不存在。如果你对数据进行预处理，使其在这方面一致，Donut将会学习并遵循这种结构。

# 微调Pix2Struct

Donut的结果保持稳定，Pix2Struct的呢？我用来训练的Colab笔记本可以在[这里](https://github.com/Toon-nooT/notebooks/blob/main/Donut_vs_pix2struct_3_Ghega_Pix2Struct.ipynb)找到。

经过75分钟的训练，我得到的编辑距离分数为0.197，而Donut的为0.116。这显然收敛速度较慢。

另一个观察结果是，到目前为止，每个返回的值都以一个空格开头。这可能是ImageCaptioningDataset类中的错误，但我没有进一步调查根本原因。不过，我在生成验证结果时会去掉这个空格。

```py
Prediction: <s_DocType> datasheet</s_DocType></s_DocType> TSZU52C2 – TSZUZUZC39<s_DocType>
    Answer: <s_DocType>datasheet</s_DocType><s_Model>Tszuszcz</s_Model><s_Voltage>O9</s_Voltage>
```

在2小时后我停止了微调过程，因为验证指标再次上升：

![](../Images/9324a3a41e08ae77d58da13021dbfe51.png)

作者提供的图像

那么这对验证集的字段级别意味着什么呢？

![](../Images/016b40e227339838bf67744203460909.png)

作者提供的图像

这看起来比Donut的结果差得多！如果你想查看完整的HTML报告，可以在[这里](https://neontreebot.be/data/Donut_vs_Pix2Struct/Pix2Struct_result_report.html)查看，或者在本地下载[这个zip文件](http://Donut_vs_pix2struct_validation_results.zip)。

只有在*数据表*和*专利*之间的分类似乎还不错（但不如Donut）。其他字段则完全不佳。我们能推断发生了什么吗？

对于*专利*文档，我看到很多橙色线条，这意味着Pix2Struct根本没有返回这些字段。

![](../Images/19611b7234cb2f3788e67025fbb526ca.png)

作者提供的图像

即使在*专利*中返回字段，它们也完全是虚构的。而Donut的错误源于从文档的其他区域提取或有轻微的OCR错误，Pix2Struct在这里则是出现了幻觉。

对Pix2Struct的表现感到失望，我尝试了几次新的训练以期获得更好的结果：

![](../Images/d0505d47bb938ff2050c32530298ed09.png)

作者提供的图像

我尝试将accumulate_grad_batches逐渐从8降到1。但这样学习率过高，会导致超调。将其降低到1e-5会使模型无法收敛。其他组合则导致模型崩溃。即使在一些特定的超参数下，验证指标看起来相当不错，但它给出了很多不正确或无法解析的行，例如：

```py
<s_DocType> datasheet</s_DocType><s_Model> CMPZSM</s_Model><s_StorageTemperature> -0.9</s_Voltage><s_StorageTemperature> -051c 150</s_StorageTemperature>
```

这些尝试都没有给我带来实质性的更好结果，所以我就此停止了。

直到我看到huggingface实现中的交叉注意力 [bug](https://github.com/huggingface/transformers/pull/25200)被修复。因此，我决定再试一次。训练了两个半小时，停在0.1416的验证指标上。

![](../Images/75ea338e70597180b073e5d5230e3d58.png)

作者提供的图片

![](../Images/0f56d46e41ed75b07b8cd2db7e053f11.png)

作者提供的图片

这显然比所有之前的结果都要好。查看HTML [报告](https://neontreebot.be/data/Donut_vs_Pix2Struct/Pix2Struct_result_report_20230804.html)，现在似乎幻觉更少。总体来说，它的表现仍不如Donut。

至于原因，我有两个理论。首先，Pix2Struct主要在HTML网页图像上训练（预测掩码图像部分后面的内容），并且在切换到另一个领域，即原始文本时，遇到了困难。其次，使用的数据集非常具有挑战性。它包含了许多OCR错误和不一致（如包含单位、长度、负号）。在我的其他实验中，我真的发现数据集的质量和一致性比数量更重要。在这个数据集中，数据质量真的很差。也许这就是我无法复制论文中声称Pix2Struct超越Donut表现的原因。

# 推断速度

这两种模型在速度方面如何比较？所有训练都在相同的T4架构上进行，因此时间可以直接比较。我们已经看到Pix2Struct收敛所需的时间要长得多。那么推断时间呢？我们可以比较推断验证集所需的时间：

![](../Images/ffe57189a296986fe0d28e53c9caa148.png)

作者提供的图片

Donut每个文档提取的平均时间为1.3秒，而Pix2Struct则超过两倍。

# **要点**

+   对我来说，最终的赢家是Donut。在易用性、性能、训练稳定性和速度方面。

+   Pix2Struct训练具有挑战性，因为它对训练超参数非常敏感。它收敛较慢，并且在这个数据集上没有达到Donut的结果。可能值得重新考虑使用更高质量的数据集来尝试Pix2Struct。

+   由于Ghega数据集包含太多不一致性，我将避免在进一步实验中使用它。

**是否有其他替代模型？**

+   [Dessurt](https://arxiv.org/pdf/2203.16618v3.pdf)，似乎与Donut有相似的架构，应该表现相当。

+   [DocParser](https://arxiv.org/pdf/2304.12484.pdf)，论文称其表现甚至更好。不幸的是，目前没有计划将该模型发布到未来。

+   [mPLUG-DocOwl](https://arxiv.org/abs/2307.02499)将很快发布，这是另一个有前景的无OCR LLM文档理解工具。

你可能还会喜欢：

[](https://toon-beerten.medium.com/hands-on-document-data-extraction-with-transformer-7130df3b6132?source=post_page-----38ce26f41951--------------------------------) [## 实战：使用🍩变压器进行文档数据提取

### 我使用甜甜圈转换器模型来提取发票索引的经验。

[toon-beerten.medium.com](https://toon-beerten.medium.com/hands-on-document-data-extraction-with-transformer-7130df3b6132?source=post_page-----38ce26f41951--------------------------------)

参考文献：

[](https://arxiv.org/abs/2210.03347?source=post_page-----38ce26f41951--------------------------------) [## Pix2Struct: 截图解析作为视觉语言理解的预训练

### 视觉定位语言无处不在——来源从带有图表的教科书到带有图像的网页等。

[arxiv.org](https://arxiv.org/abs/2210.03347?source=post_page-----38ce26f41951--------------------------------) [](https://arxiv.org/abs/2111.15664?source=post_page-----38ce26f41951--------------------------------) [## 无 OCR 文档理解转换器

### 理解文档图像（例如发票）是一项核心但具有挑战性的任务，因为它需要复杂的功能，比如……

[arxiv.org](https://arxiv.org/abs/2111.15664?source=post_page-----38ce26f41951--------------------------------) [](https://github.com/Toon-nooT/notebooks/tree/main?source=post_page-----38ce26f41951--------------------------------) [## GitHub - Toon-nooT/notebooks

### 通过在 GitHub 上创建帐户来为 Toon-nooT/notebooks 的开发做贡献。

[github.com](https://github.com/Toon-nooT/notebooks/tree/main?source=post_page-----38ce26f41951--------------------------------)
