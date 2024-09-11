# 从纸张到像素：评估数字化手写文本的最佳技术

> 原文：[https://towardsdatascience.com/from-paper-to-pixel-evaluating-the-best-techniques-for-digitising-handwritten-texts-9cc39e7a457d?source=collection_archive---------2-----------------------#2023-09-14](https://towardsdatascience.com/from-paper-to-pixel-evaluating-the-best-techniques-for-digitising-handwritten-texts-9cc39e7a457d?source=collection_archive---------2-----------------------#2023-09-14)

## 对OCR、变换器模型和基于提示工程的集成技术的比较分析

[](https://medium.com/@RedjaiSani?source=post_page-----9cc39e7a457d--------------------------------)[![Sohrab Sani](../Images/b4a9309f91479873b59d0e182ddd4128.png)](https://medium.com/@RedjaiSani?source=post_page-----9cc39e7a457d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9cc39e7a457d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9cc39e7a457d--------------------------------) [Sohrab Sani](https://medium.com/@RedjaiSani?source=post_page-----9cc39e7a457d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc7a4f1e52b82&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-paper-to-pixel-evaluating-the-best-techniques-for-digitising-handwritten-texts-9cc39e7a457d&user=Sohrab+Sani&userId=c7a4f1e52b82&source=post_page-c7a4f1e52b82----9cc39e7a457d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9cc39e7a457d--------------------------------) ·16分钟阅读·2023年9月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9cc39e7a457d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-paper-to-pixel-evaluating-the-best-techniques-for-digitising-handwritten-texts-9cc39e7a457d&user=Sohrab+Sani&userId=c7a4f1e52b82&source=-----9cc39e7a457d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9cc39e7a457d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-paper-to-pixel-evaluating-the-best-techniques-for-digitising-handwritten-texts-9cc39e7a457d&source=-----9cc39e7a457d---------------------bookmark_footer-----------)

作者: [Sohrab Sani](https://medium.com/u/c7a4f1e52b82?source=post_page-----9cc39e7a457d--------------------------------) 和 [Diego Capozzi](https://medium.com/u/d2b5153934d4?source=post_page-----9cc39e7a457d--------------------------------)

组织长期以来一直在处理数字化历史手写文档这一繁琐且昂贵的任务。之前，光学字符识别（OCR）技术，如 AWS Textract (TT) [1] 和 Azure 表单识别器 (FR) [2]，在这方面领先。虽然这些选项可能广泛可用，但它们有许多缺点：价格高、需要长时间的数据处理/清洗，并且准确性可能不尽如人意。最近，利用 Transformer 架构的深度学习进展在图像分割和自然语言处理领域，使得 OCR 自由的技术得以发展，例如 Document Understanding Transformer (Donut)[3] 模型。

在这项研究中，我们将使用自定义数据集比较 OCR 和基于 Transformer 的技术，该数据集由一系列手写表单创建。对这一相对简单任务的基准测试旨在引导向更复杂的应用，特别是在更长的手写文档上。为了提高准确性，我们还探索了利用提示工程结合 gpt-3.5-turbo 大型语言模型（LLM）的集成方法，以结合 TT 和微调的 Donut 模型的输出。

这项工作的代码可以在[这个](https://github.com/srsani/hvdu) GitHub 仓库中查看。数据集可以在我们的 Hugging Face 仓库[这里](https://huggingface.co/datasets/ift/handwriting_forms)获得。

**目录：**

· [数据集创建](#d01a)

· [方法](#7cc5)

∘ [Azure 表单识别器 (FR)](#7956)

∘ [AWS Textract (TT)](#3c12)

∘ [Donut](#59bf)

∘ [集成方法：TT、Donut、GPT](#c3ca)

· [模型性能测量](#48df)

∘ [FLA](#d926)

∘ [CBA](#0510)

∘ [覆盖范围](#c673)

∘ [成本](#96fa)

· [结果](#79c4)

· [其他考虑因素](#266a)

∘ [Donut 模型训练](#dc66)

∘ [提示工程的变异性](#682d)

· [结论](#0887)

· [下一步](#13a4)

· [参考文献](#15bb)

· [致谢](#e00f)

# 数据集创建

本研究从 NIST 特殊数据库 19 数据集[4]的 2100 张手写表单图像中创建了一个自定义数据集。图 1 提供了这些表单之一的样本图像。最终的集合包括 2099 张表单。为了整理这个数据集，我们裁剪了每个 NIST 表单的顶部部分，目标是红框中高亮的 DATE、CITY、STATE 和 ZIP CODE（现在称为“ZIP”）键[图 1]。这种方法启动了基准测试过程，进行了一项相对简单的文本提取任务，使我们能够快速选择和手动标记数据集。在撰写时，我们不知道有任何公开的带标记的手写表单图像数据集可以用于 JSON 键字段文本提取。

![](../Images/617a9f0dc196f1b3434e2561b8a7f795.png)

**图 1.** 来自 NIST 特殊数据库 19 数据集的示例表单。红框标识了裁剪过程，该过程仅选择此表单中的 DATE、CITY、STATE 和 ZIP 字段。（作者提供的图像）

我们手动从文档中提取每个键的值，并仔细检查其准确性。总共丢弃了68个表单，因为它们包含至少一个无法辨认的字符。表单中的字符被记录为实际出现的样子，无论拼写错误或格式不一致。

为了对Donut模型进行微调以处理缺失数据，我们添加了67个空表单，以便为这些空字段进行训练。表单中的缺失值在JSON输出中表示为“None”字符串。

图2a显示了我们数据集中的一个样本表单，而图2b则展示了与该表单相关的JSON。

![](../Images/6f923942368211ce6bcb195a41d4d2f7.png)

**图2。** (a) 数据集中的示例图像；(b) JSON格式的提取数据。（图像由作者提供）

表1提供了数据集中每个键的变异性分解。从最具变异性到最少的顺序是ZIP、CITY、DATE和STATE。所有日期均在1989年，这可能降低了整体DATE的变异性。此外，尽管美国只有50个州，但由于不同的首字母缩略词或大小写敏感的拼写，STATE的变异性有所增加。

![](../Images/56c0e7820a90665b0da77890964b931f.png)

**表1。** 数据集的总结统计。（图像由作者提供）

表2总结了我们数据集中各种属性的字符长度。

![](../Images/0ed712f724804866090125286a9cda55.png)

**表2。** 字符长度和分布的总结。（图像由作者提供）

上述数据表明，CITY条目的字符长度最长，而STATE条目的字符长度最短。每个条目的中位数值紧随其各自的均值，表明各类别的字符长度围绕平均值的分布相对均匀。

数据标注后，我们将其分为三个子集：训练集、验证集和测试集，样本量分别为1400、199和500。这里是一个[链接](https://github.com/srsani/hvdu/blob/main/src/notebooks/3_0_make_train_val_test.ipynb)，用于此笔记本。

# 方法

我们将详细阐述每种测试方法，并将其与包含更多细节的相关Python代码关联起来。方法的应用首先单独描述，即FR、TT和Donut，然后是TT+GPT+Donut集成方法。

## Azure Form Recognizer (FR)

图3展示了使用Azure FR从表单图像中提取手写文本的工作流程。

1.  **存储图像：** 可以存储在本地驱动器或其他解决方案中，如S3桶或Azure Blob存储。

1.  **Azure SDK**：用于从存储中加载每个图像并通过Azure SDK将其传输到FR API的Python脚本。

1.  **后处理**：使用现成的方法意味着最终输出通常需要精细调整。以下是21个需要进一步处理的提取键：

    [ ‘DATE’, ‘CITY’, ‘STATE’, ‘’DATE’, ‘ZIP’, ‘NAME’, ‘E ZIP’,’·DATE’, ‘.DATE’, ‘NAMR’, ‘DATE®’, ‘NAMA’, ‘_ZIP’, ‘.ZIP’, ‘print the following shopsataca i’, ‘-DATE’, ‘DATE.’, ‘No.’, ‘NAMN’, ‘STATE\nZIP’]

    一些键有额外的点或下划线，需要去除。由于文本在表单中的位置非常接近，提取的值有许多实例被错误地关联到不正确的键。这些问题随后会得到合理程度的解决。

1.  **保存结果**：将结果以 pickle 格式保存到存储空间中。

![](../Images/0dbf5f93c95ac09e1c116ce0ebb89a46.png)

**图 3.** Azure FR 工作流的可视化。（作者提供的图片）

## *AWS Textract (TT)*

图 4 描述了使用 AWS TT 从表单图像中提取手写文本的工作流：

1.  **存储图像：** 图像存储在 S3 桶中。

1.  **SageMaker Notebook**：Notebook 实例帮助与 TT API 进行交互，执行脚本的后处理清理，并保存结果。

1.  **TT API：** 这是由 AWS 提供的现成的基于 OCR 的文本提取 API。

1.  **后处理：** 使用现成的方法意味着最终输出通常需要进行精细调整。TT 产生了一个包含 68 列的数据集，而 FR 方法只有 21 列。这主要是由于检测到图像中额外的文本，这些文本被认为是字段。在基于规则的后处理中会解决这些问题。

1.  **保存结果**：经过处理的数据会使用 pickle 格式存储在 S3 桶中。

![](../Images/0d6ca9f7abbbe60d3d27c0179d7bf501.png)

**图 4.** TT 工作流的可视化。（作者提供的图片）

## *Donut*

与不能通过自定义字段和/或模型再训练适应特定数据输入的现成基于 OCR 的方法相比，本节深入探讨了使用基于 Transformer 模型架构的 Donut 模型来改进无 OCR 方法。

首先，我们使用我们的数据对 Donut 模型进行了微调，然后将模型应用于我们的测试图像，以 JSON 格式提取手写文本。为了高效地重新训练模型并防止潜在的过拟合，我们使用了 PyTorch Lightning 的 EarlyStopping 模块。批量大小为 2，训练在 14 个周期后结束。以下是 Donut 模型微调过程的更多细节：

+   我们分配了 1,400 张图像用于训练，199 张用于验证，其余 500 张用于测试。

+   我们使用了一个 [naver-clova-ix/donut-base](https://huggingface.co/naver-clova-ix/donut-base) 作为基础模型，它可以在 Hugging Face 上访问。

+   然后使用 24GB 内存的 Quadro P6000 GPU 对模型进行了微调。

+   整个训练时间大约为 3.5 小时。

+   有关更复杂的配置细节，请参考仓库中的 `[train_nist.yaml](https://github.com/srsani/hvdu/blob/main/src/config/train_nist.yaml)`。

该模型也可以从我们的 Hugging Face 空间 [代码库](https://huggingface.co/ift/handwriting_forms) 下载。

## **集成方法：TT、Donut、GPT**

探索了各种集成方法，其中 TT、Donut 和 GPT 的组合表现最佳，具体解释如下。

一旦通过单独应用 TT 和 Donut 获得 JSON 输出，这些输出被用作输入，随后传递给 GPT。目的是利用 GPT 将这些 JSON 输入中的信息与上下文 GPT 信息结合，创建一个新的/更清晰的 JSON 输出，以提高内容的可靠性和准确性 [表 3]。**图 5** 提供了这一集成方法的视觉概述。

![](../Images/73cb815c9d345287c6b2491ef6900439.png)

**图 5.** 结合 TT、Donut 和 GPT 的集成方法的视觉描述。（图片由作者提供）

为此任务创建适当的 GPT 提示是一个迭代过程，并需要引入临时规则。根据[**附加考虑**](#266a)部分的说明，为此任务 — 以及可能的数据集 — 定制 GPT 提示是本研究需要探索的一个方面。

# **模型性能测量**

本研究主要通过使用两种不同的准确度测量来评估模型性能：

+   字段级准确率（FLA）

+   基于字符的准确度（CBA）

还测量了其他数量，如**覆盖率**和**成本**，以提供相关的背景信息。所有指标如下所述。

## **FLA**

这是一个二元测量：如果预测的 JSON 中所有键的字符与参考 JSON 中的字符匹配，则 FLA 为 1；如果有一个字符不匹配，则 FLA 为 0。

请考虑以下示例：

```py
JSON1 = {'DATE': '8/28/89', 'CITY': 'Murray', 'STATE': 'KY', 'ZIP': '42171'}
JSON2 = {'DATE': '8/28/89', 'CITY': 'Murray', 'STATE': 'KY', 'ZIP': '42071'}
```

使用 FLA 比较 JSON1 和 JSON2 由于 ZIP 不匹配得分为 0。然而，将 JSON1 与自身比较则提供 FLA 得分为 1。

## **CBA**

该准确度测量的计算方法如下：

1.  确定每对对应值的 Levenshtein 编辑距离。

1.  通过将所有距离相加并除以每个值的总组合字符串长度来获得标准化得分。

1.  将此得分转换为百分比。

两个字符串之间的 Levenshtein 编辑距离是将一个字符串转换为另一个字符串所需的更改次数。这包括计数替换、插入或删除。例如，将“marry”转换为“Murray”需要两个替换和一个插入，共计三个更改。这些修改可以以各种顺序进行，但至少需要三次操作。对于此计算，我们使用了 NLTK 库中的 edit_distance 函数。

下面是一个代码片段，展示了所描述算法的实现。此函数接受两个 JSON 输入，并返回一个准确度百分比。

```py
def dict_distance (dict1:dict,
                  dict2:dict) -> float:

   distance_list = []
   character_length = []

   for key, value in dict1.items():
       distance_list.append(edit_distance(dict1[key].strip(),
                                          dict2[key].strip()))

       if len(dict1[key]) > len(dict2[key]):
           character_length.append((len(dict1[key])))

       else:              
           character_length.append((len(dict2[key])))

   accuracy = 100 - sum(distance_list)/(sum(character_length))*100

   return accuracy
```

为了更好地理解该函数，让我们看看它在以下示例中的表现：

```py
JSON1 = {'DATE': '8/28/89', 'CITY': 'Murray', 'STATE': 'KY', 'ZIP': '42171'}
JSON2 = {'DATE': 'None', 'CITY': 'None', 'STATE': 'None', 'ZIP': 'None'}
JSON3 = {'DATE': '8/28/89', 'CITY': 'Murray', 'STATE': 'None', 'ZIP': 'None'}
```

1.  `dict_distance(JSON1, JSON1)`: 100% JSON1 和 JSON1 之间没有差异，因此我们获得了 100% 的完美得分。

1.  `dict_distance(JSON1, JSON2)`: 0% JSON2 中的每个字符都需要修改才能匹配 JSON1，从而得出 0% 的得分。

1.  `dict_distance(JSON1, JSON3)`: 59% JSON3 中的 STATE 和 ZIP 键的每个字符都必须更改以匹配 JSON1，从而得到 59% 的准确性得分。

我们现在将重点关注分析图像样本的 CBA 平均值。这些准确性测量非常严格，因为它们衡量了检查字符串中的所有字符及字符大小写是否匹配。由于 FLA 的二元特性，使其在处理部分正确的情况时表现得特别保守。尽管 CBA 比 FLA 更不保守，但仍被认为是有些保守的。此外，CBA 能够识别部分正确的实例，但它也考虑了文本大小写（大写与小写），这可能根据是否关注于恢复文本的适当内容或保留书写内容的准确形式而具有不同的重要性。总体而言，我们决定使用这些严格的测量方法，以便采取更保守的方法，因为我们优先考虑文本提取的正确性而非文本语义。

## **覆盖率**

这个量度定义为字段已全部从输出 JSON 中提取的表单图像的比例。这有助于监测从表单中提取所有字段的整体能力，而不考虑其正确性。如果 Coverage 非常低，则表示某些字段系统性地被遗漏在提取过程中。

## **成本**

这是对将每种方法应用于整个测试数据集所产生的成本的简单估算。我们没有计算微调 Donut 模型的 GPU 成本。

# **结果**

我们评估了所有方法在测试数据集上的表现，该数据集包含 500 个样本。这一过程的结果汇总在表 3 中。

使用 FLA 时，我们观察到传统的基于 OCR 的方法 FR 和 TT 表现相似，准确率相对较低（FLA~37%）。虽然不理想，但这可能是由于 FLA 的严格要求。作为替代，当使用 CBA Total，即考虑所有 JSON 键的平均 CBA 值时，TT 和 FR 的表现远远更可接受，值大于 77%。特别是，TT（CBA Total = 89.34%）比 FR 高出约 15%。这种行为在关注单个表单字段的 CBA 值时也得以保持，尤其是在 DATE 和 CITY 类别 [表 3] 中，并且在测量整个样本 2099 张图像的 FLA 和 CBA Totals 时（TT: FLA = 40.06%；CBA Total = 86.64%；FR: FLA = 35.64%；CBA Total = 78.57%）。虽然应用这两种模型的 Cost 值相同，但 TT 更有利于提取所有表单字段，其 Coverage 值比 FR 高出约 9%。

![](../Images/38cc52e9abbcd8c713f05d6fe1ecc6a2.png)

**表 3.** 性能指标值是根据测试数据集计算的。CBA 总体和 CBA 关键（key= 日期、城市、州、邮政编码）分别是样本平均值和 JSON 键的总值和单独值。（图片由作者提供）

量化这些更传统的基于 OCR 的模型的性能为我们提供了一个基准，我们用这个基准来评估使用纯 Donut 方法与将 Donut 与 TT 和 GPT 结合使用的优缺点。我们从使用 TT 作为基准开始。

采用这种方法的好处体现在微调的 Donut 模型上，该模型在 1400 张图像及其对应的 JSON 上进行了微调。与 TT 结果相比，该模型的全球 FLA 为 54%，CBA 总体为 95.23%，分别提高了 38% 和 6%。最显著的提高体现在 FLA 上，显示出该模型能够准确检索超过一半测试样本中的所有表单字段。

CBA 的提高是显著的，因为用于微调模型的图像数量有限。Donut 模型显示出其优势，表现在 Coverage 和基于关键的 CBA 指标的总体值有所改善，增加幅度在 2% 到 24% 之间。Coverage 达到 100%，表明该模型可以从所有表单字段中提取文本，从而减少了将这种模型投入生产所需的后处理工作。

基于此任务和数据集，这些结果表明，使用经过微调的 Donut 模型产生的结果优于 OCR 模型。最后，还探索了集成方法，以评估是否可以继续进行额外的改进。

由 gpt-3.5-turbo 支持的 TT 和经过微调的 Donut 的集成性能表明，如果选择特定的指标（如 FLA），是有可能实现改进的。与我们经过微调的 Donut 模型相比，该模型的所有指标（不包括 CBA 状态和 Coverage）都有所增加，增加幅度在 ~0.2% 到 ~10% 之间。唯一的性能下降体现在 CBA 状态上，与我们经过微调的 Donut 模型测得的值相比，下降了 ~3%。这可能归因于使用的 GPT 提示，进一步微调可能会改善这一指标。最后，Coverage 值保持在 100% 不变。

与其他单独字段相比，日期提取（见 CBA 日期）效率更高。这可能是由于日期字段的变异性有限，因为所有日期都来源于 1989 年。

如果性能要求非常保守，那么FLA的10%提升是显著的，并可能值得构建和维护更复杂的基础设施的更高成本。这也应考虑到LLM提示词修改引入的变异来源，如[**附加考虑**](#266a)部分所述。然而，如果性能要求不那么严格，那么该集成方法所带来的CBA指标改进可能不值得额外的成本和努力。

总体而言，我们的研究表明，虽然基于OCR的个别方法——即FR和TT——各有其优点，但仅在1400个样本上微调的甜甜圈模型轻松超越了它们的准确性基准。此外，通过gpt-3.5-turbo提示词对TT和微调后的甜甜圈模型进行集成，进一步提高了通过FLA指标测量的准确性。[**附加考虑**](#266a)也必须考虑甜甜圈模型和GPT提示词的微调过程，接下来将进行探讨。

# 附加考虑

## **甜甜圈模型训练**

为了提高甜甜圈模型的准确性，我们尝试了三种训练方法，每种方法都旨在提高推理准确性，同时防止过拟合训练数据。表4展示了我们结果的总结。

![](../Images/212826578131013e96299cb9b2b9cdff.png)

**表4.** 甜甜圈模型微调的总结。（作者提供的图像）

**1. 30-周期训练**：我们使用甜甜圈GitHub仓库提供的配置训练了30个周期的甜甜圈模型。该训练过程持续了约7小时，结果显示FLA为50.0%。不同类别的CBA值有所不同，CITY达到90.55%，ZIP达到98.01%。然而，当我们检查val_metric时注意到模型在第19周期后开始过拟合。

**2. 19-周期训练**：根据初步训练获得的见解，我们将模型微调了19个周期。我们的结果显示FLA显著提高，达到55.8%。整体CBA以及基于关键的CBAs显示了准确性值的改善。尽管这些指标很有前景，但我们检测到了过拟合的迹象，如val_metric所示。

**3. 14-周期训练**：为了进一步完善我们的模型并抑制潜在的过拟合，我们使用了PyTorch Lightning的EarlyStopping模块。这种方法在14个周期后终止了训练。结果显示FLA为54.0%，CBAs与19周期训练相比相当，甚至更好。

比较这三次训练的输出时，尽管19周期训练的FLA略有改善，但14周期训练的CBA指标整体上更优。此外，val_metric强化了我们对19周期训练的担忧，表明略有过拟合的倾向。

总结来说，我们推断出，经过14个周期使用EarlyStopping微调的模型是最强健且最具成本效益的。

## 提示工程的变化

我们研究了两种提示工程方法（ver1和ver2），通过将微调的Donut模型与TT结果进行集成，以提高数据提取效率。在训练模型14个周期后，Prompt ver1取得了更好的结果，其FLA为59.6%，并且所有键的CBA指标均较高[表5]。相比之下，Prompt ver2出现了下降，其FLA降至54.4%。详细查看CBA指标显示，ver2中每个类别的准确度评分略低于ver1，突显了这一变化的显著差异。

![](../Images/863e0a6af87a6a4eeedcc748ff5347b3.png)

**表5**。结果总结：从表单中提取手写文本。（图片由作者提供）

在对数据集进行手动标注过程中，我们利用了TT和FR的结果，并在对表单文本进行标注时开发了Prompt ver1。尽管Prompt ver2在本质上与其前身相同，但经过了轻微修改。我们的主要目标是通过去除Prompt ver1中的空行和冗余空格来改进提示。

总之，我们的实验突显了看似微小调整的细微影响。尽管Prompt ver1展示了更高的准确性，但将其精炼和简化为Prompt ver2的过程，悖论地导致了所有指标上的性能下降。这凸显了提示工程的复杂性以及在最终确定提示之前进行细致测试的必要性。

Prompt ver1可在[这个Notebook](https://github.com/srsani/hvdu/blob/main/src/notebooks/5_1_ensemble_FR_TT_GPT.ipynb)中找到，Prompt ver2的代码可以在[这里](https://github.com/srsani/hvdu/blob/main/src/notebooks/utilities.py)查看。

# 结论

我们创建了一个基准数据集，用于从包含四个字段（DATE、CITY、STATE 和 ZIP）的手写表单图像中提取文本。这些表单被手动标注为JSON格式。我们使用此数据集评估了基于OCR的模型（FR和TT）和一个Donut模型，该模型随后使用我们的数据集进行了微调。最后，我们采用了一个通过提示工程构建的集成模型，该模型利用了LLM（gpt-3.5-turbo）与TT和我们微调的Donut模型输出。

我们发现 TT 的表现优于 FR，并以此作为基准来评估 Donut 模型在单独使用或与 TT 和 GPT 组合使用（即集成方法）时可能产生的改进。正如模型性能指标所显示的那样，这种微调的 Donut 模型显示出明显的准确性提升，这证明了其优于基于 OCR 的模型。集成模型显著提高了 FLA，但成本较高，因此可以在要求更严格的性能要求的情况下考虑使用。尽管使用了相同的基础模型 gpt-3.5-turbo，但我们观察到当提示中进行微小更改时，输出 JSON 表单存在显著差异。这种不可预测性是使用现成 LLM 的一个重大缺陷。我们目前正在开发基于开源 LLM 的更紧凑的清理流程来解决这个问题。

# 下一步

+   表 2 中的价格列显示，OpenAI API 调用是本研究中最昂贵的认知服务。因此，为了减少成本，我们正在通过利用完全微调、提示微调[5] 和 QLORA [6] 等方法来微调 LLM 以进行 seq2seq 任务。

+   由于隐私原因，数据集中图像上的名称框被黑色矩形覆盖。我们正在通过向数据集中添加随机的名字和姓氏来更新这一点，这将使数据提取字段从四个增加到五个。

+   未来，我们计划通过将这项研究扩展到包括整个表单或其他更大文档的文本提取任务，从而增加文本提取任务的复杂性。

+   调查 Donut 模型超参数优化。

# 参考文献

1.  Amazon Textract，[AWS Textract](https://aws.amazon.com/textract/ocr/)

1.  表单识别器，[表单识别器（现为文档智能）](https://learn.microsoft.com/en-us/azure/ai-services/document-intelligence/faq?view=doc-intel-3.1.0)

1.  Kim, Geewook 和 Hong, Teakgyu 和 Yim, Moonbin 和 Nam, JeongYeon 和 Park, Jinyoung 和 Yim, Jinyeong 和 Hwang, Wonseok 和 Yun, Sangdoo 和 Han, Dongyoon 和 Park, Seunghyun，[无 OCR 文档理解转换器](https://arxiv.org/abs/2111.15664) (2022)，欧洲计算机视觉会议 (ECCV)

1.  Grother, P. 和 Hanaoka, K. (2016) NIST 手写表单和字符数据库 (NIST 特殊数据库 19)。DOI: [http://doi.org/10.18434/T4H01C](http://doi.org/10.18434/T4H01C)

1.  Brian Lester, Rami Al-Rfou, Noah Constant，[规模化的参数高效提示微调的力量](https://arxiv.org/abs/2104.08691) (2021)，arXiv:2104.08691

1.  Tim Dettmers, Artidoro Pagnoni, Ari Holtzman, Luke Zettlemoyer，[QLoRA: 高效的量化 LLM 微调](https://arxiv.org/abs/2305.14314) (2023)，[https://arxiv.org/abs/2305.14314](https://arxiv.org/abs/2305.14314)

# 致谢

我们要感谢我们的同事**大卫·罗德里格斯**博士，他对这个项目的持续支持和讨论。我们还要感谢[Kainos](https://www.kainos.com/)的支持。
