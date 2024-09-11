# 监控无结构数据以用于LLM和NLP

> 原文：[https://towardsdatascience.com/monitoring-unstructured-data-for-llm-and-nlp-efff42704e5b?source=collection_archive---------10-----------------------#2023-06-27](https://towardsdatascience.com/monitoring-unstructured-data-for-llm-and-nlp-efff42704e5b?source=collection_archive---------10-----------------------#2023-06-27)

## 使用文本描述符的代码教程

[](https://medium.com/@elena.samuylova?source=post_page-----efff42704e5b--------------------------------)[![Elena Samuylova](../Images/bc3024500f8b90a97f13d82ecaa1c9e7.png)](https://medium.com/@elena.samuylova?source=post_page-----efff42704e5b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----efff42704e5b--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----efff42704e5b--------------------------------) [Elena Samuylova](https://medium.com/@elena.samuylova?source=post_page-----efff42704e5b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9621354b583a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmonitoring-unstructured-data-for-llm-and-nlp-efff42704e5b&user=Elena+Samuylova&userId=9621354b583a&source=post_page-9621354b583a----efff42704e5b---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----efff42704e5b--------------------------------) · 14分钟阅读 · 2023年6月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fefff42704e5b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmonitoring-unstructured-data-for-llm-and-nlp-efff42704e5b&user=Elena+Samuylova&userId=9621354b583a&source=-----efff42704e5b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fefff42704e5b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmonitoring-unstructured-data-for-llm-and-nlp-efff42704e5b&source=-----efff42704e5b---------------------bookmark_footer-----------)![](../Images/9449b853160c4015be247a0c901aee69.png)

图片由作者提供。

一旦你部署了基于NLP或LLM的解决方案，你需要一种方法来监控它。但是你如何监控无结构数据以理解大量文本呢？

这里有几种方法，从 [检测原始文本数据中的漂移](https://www.evidentlyai.com/blog/tutorial-detecting-drift-in-text-data) 和 [嵌入漂移](https://www.evidentlyai.com/blog/embedding-drift-detection) 到使用正则表达式进行规则检查。

在本教程中，我们将深入探讨一种特定的方法——跟踪可解释的文本描述符，这有助于为每段文本分配特定属性。

**首先，我们将讨论一些理论：**

+   什么是文本描述符，以及何时使用它们。

+   文本描述符示例。

+   如何选择自定义描述符。

**接下来，开始编码吧！** 你将使用电子商务评论数据并完成以下步骤：

+   获取文本数据的概览。

+   使用标准描述符评估文本数据漂移。

+   使用外部预训练模型添加自定义文本描述符。

+   实施管道测试以监控数据变化。

我们将使用[Evidently开源Python库](https://github.com/evidentlyai/evidently)来生成文本描述符并评估数据变化。

> ***代码示例：*** *如果你更喜欢直接查看代码，这里是* [*示例笔记本*](https://github.com/evidentlyai/community-examples/blob/main/tutorials/How_to_add_a_custom_text_descriptor.ipynb)*。*

# 什么是文本描述符？

文本描述符是描述文本数据集中对象的任何特征或属性。例如，文本长度或其中的符号数量。

你可能已经拥有有用的元数据来伴随你的文本，作为描述符。例如，电子商务用户评论可能会附带用户分配的评级或主题标签。

否则，你可以生成自己的描述符！你可以通过向文本数据添加“虚拟特征”来做到这一点。每个描述符都有助于使用一些有意义的标准描述或分类你的文本。

![](../Images/4d3ad2d6d7d7d91249e42b8d12b13dec.png)

作者提供的图片。

通过创建这些描述符，你基本上提出了自己的简单“嵌入”，并将每个文本映射到几个可解释的维度。这有助于理解那些原本无结构的数据。

然后你可以使用这些文本描述符：

+   **监控生产 NLP 模型。** 你可以跟踪数据的属性，并检测它们何时发生变化。例如，描述符有助于检测文本长度的突增或情感的漂移。

+   **在更新期间测试模型。** 当你迭代模型时，可以比较评估数据集的属性和模型响应。例如，你可以检查LLM生成的答案的长度是否保持一致，并且它们是否始终包含你期望看到的单词。

+   **调试数据漂移或模型衰退。** 如果你检测到[嵌入漂移](https://www.evidentlyai.com/blog/embedding-drift-detection)或直接观察到模型质量下降，你可以使用文本描述符来探索其来源。

# 文本描述符示例

以下是我们认为良好的默认文本描述符：

## 文本长度

![](../Images/184e6533f48040200fe1378cad612a11.png)

*作者提供的图片。使用* [*Evidently Python 库*](https://github.com/evidentlyai/evidently)*的文本概览度量可视化。*

一个很好的起点是简单的文本统计。例如，你可以查看**文本长度**，按单词、符号或句子来度量。你可以评估平均长度、最小值和最大值，并查看分布情况。

你可以根据你的使用案例设定期望。例如，产品评论通常在5到100个单词之间。如果评论更短或更长，这可能表示上下文发生了变化。如果固定长度评论的数量激增，这可能表示垃圾邮件攻击。如果你知道负面评论通常较长，你可以跟踪超过某一长度的评论比例。

还有一些快速的合理性检查：如果你运行聊天机器人，你可能期望有非零响应或者有一些有意义输出的最小长度。

## 词汇表外的单词

![](../Images/a65c56974f1b1f3ae2d41514a07315b3.png)

作者提供的图片。*示例参考数据集中OOV的平均比例为5.378%。*

评估超出定义词汇范围的单词比例是一个很好的“粗略”数据质量衡量标准。你的用户是否开始用新的语言撰写评论？用户是否用Python而非英语与聊天机器人对话？用户是否用“ggg”代替实际单词填写响应？

这是检测各种变化的一个实际措施。一旦发现变化，你可以进行更深入的调试。

你可以根据长期积累的“良好”生产数据示例来设定对OOV单词比例的期望。例如，如果你查看以往产品评论的语料库，你可能会期望OOV在10%以下，并监控其值是否超过这个阈值。

## 非字母字符

相关，但有个小变化：这个描述符将统计所有非字母或数字的特殊符号，包括逗号、括号、井号等。

有时你会期望出现相当多的特殊符号：你的文本可能包含代码或以JSON格式结构化。有时，你只期望在人类可读的文本中出现标点符号。

检测非字母字符的变化可以揭示数据质量问题，比如HTML代码泄漏到评论文本中、垃圾邮件攻击、意外的使用场景等。

## 情感

![](../Images/0b6feb7078990f2fd42ebf4054b2cb2b.png)

作者提供的图片。*示例电子商务评论数据集中“基准”情感分布。*

文本情感是另一个指标。它在各种场景中都很有用：从聊天机器人对话到用户评论和营销文案编写。你通常可以设定你处理的文本的情感预期。

即使情感“无关”，这可能意味着期望的主要是中立的语调。负面或正面的语调的潜在出现是值得跟踪和调查的。这可能表明意外的使用场景：用户是否将你的虚拟抵押贷款顾问用作投诉渠道？

你也可能期望某种平衡：例如，总会有一部分对话或评论带有负面语调，但你会期望它不会超过某个阈值，或者评论情感的整体分布保持稳定。

## 触发词

![](../Images/b8cb44f8a4202672b43ca925422086b8.png)

作者提供的图片。*评论一致提到连衣裙：未检测到分布漂移。*

你还可以检查文本是否包含特定列表中的单词，并将其视为二进制特征。

这是一种强大的方式来编码对你文本的多个期望。你需要一些努力来手动策划列表，但你可以通过这种方式设计许多有用的检查。例如，你可以创建触发词列表，如：

+   提及产品或品牌。

+   提及竞争对手。

+   提及地点、城市、地方等。

+   提及代表特定主题的单词。

你可以策划（并不断扩展）类似的列表，这些列表是特定于你的用例的。

例如，如果一个顾问聊天机器人帮助选择公司提供的产品之一，你可能会期望大多数响应中包含该列表中某个产品的名称。

## RegExp 匹配

包含特定单词的例子是你可以制定的正则表达式模式之一。你可以想出其他模式：你是否期望你的文本以“hello”开头并以“thank you”结尾？包含电子邮件？包含已知的命名元素？

如果你期望模型输入或输出符合特定格式，你可以使用正则表达式匹配作为另一种描述符。

## 自定义描述符

你可以进一步扩展这个想法。例如：

+   **评估其他文本属性**：毒性、主观性、语气的正式程度、可读性评分等。你通常可以找到开放的预训练模型来完成这项任务。

+   **计数特定组件：** 电子邮件、URL、表情符号、日期和词性。你可以使用外部模型或甚至简单的正则表达式。

+   **深入统计数据：** 如果这些数据对你的用例有意义，你可以追踪非常详细的文本统计数据，例如，追踪单词的平均长度，它们是大写还是小写，唯一单词的比例等。

+   **监控个人可识别信息**：例如，当你不期望它出现在聊天机器人对话中时。

+   **使用命名实体识别：** 提取特定实体并将其视为标签。**‍**

+   **使用主题建模** 来建立主题监控系统。这是最繁琐的方法，但做得好时非常强大。当你期望文本保持在主要话题上并且拥有之前的示例语料库来训练模型时，它非常有用。你可以使用无监督主题聚类并创建一个模型来将新文本分配到已知的簇中。然后，你可以将分配的类别作为描述符来监控新数据中主题分布的变化。

![](../Images/56ae75356635ea5aebe10c8b079438e6.png)

作者提供的图片。*多个描述符的总结漂移报告示例。*

设计描述符时要记住的几点：

+   **最好保持专注**，尽量找出少量适合的质量指标，这些指标与用例匹配，而不是监控所有可能的维度。将描述符视为模型特征。你希望找到一些强大的特征，而不是生成大量弱的或无用的特征。许多特征往往是相关的：语言和 OOV 单词的比例、句子和符号的长度等。选择你最喜欢的！

+   **使用探索性数据分析** 评估现有数据中的文本属性（例如，以前对话的日志），以测试你的假设，然后再将它们添加到模型监控中。

+   **从模型失败中学习**。每当你遇到一个你期望再次出现的生产模型质量问题（例如，外语文本），考虑如何开发一个测试用例或描述符，以便在未来检测到它。

+   **注意计算成本。** 使用外部模型按每个可能的维度评分文本是很诱人的，但这会带来额外的成本。考虑一下在处理较大的数据集时：每个外部分类器都是一个额外的模型。通常可以通过更少或更简单的检查来解决问题。

# 分步教程

为了说明这个想法，让我们通过以下场景来演示：你正在构建一个分类器模型，以对用户在电子商务网站上留下的评论进行评分并按主题标记。一旦模型投入生产，你希望检测数据和模型环境的变化，但你没有真实标签。你需要运行一个单独的标记过程来获取它们。

没有标签的情况下你如何跟踪变化？

让我们以一个示例数据集为例，按照以下步骤进行操作：

> ***代码示例：*** *请前往* [*示例笔记本*](https://github.com/evidentlyai/community-examples/blob/main/tutorials/How_to_add_a_custom_text_descriptor.ipynb) *以查看所有步骤。*

# 💻 1\. 安装 Evidently

首先，安装 Evidently。使用 Python 包管理器在你的环境中安装它。如果你在 Colab 中工作，运行 *!pip install*。在 Jupyter Notebook 中，你还需要安装 nbextension。查看 [安装说明](https://docs.evidentlyai.com/user-guide/install-evidently/?utm_source=website&utm_medium=referral&utm_campaign=blog_text&utm_content=unstructured-data-monitoring) 以获取你的环境信息。

你还需要导入一些其他库，如 *pandas* 和特定的 Evidently 组件。按照笔记本中的说明进行操作。

# 🔡 2\. 准备数据

一旦你设置好了所有内容，让我们来看看数据吧！你将使用一个来自电子商务评论的开放数据集。

这就是数据集的样子：

![](../Images/61dc47d6fb8c0c43b9c82d3180b95766.png)

图片由作者提供。

我们将重点关注“Review_Text”列进行演示。在生产中，我们想要监控评论文本的变化。

你需要使用列映射指定包含文本的列：

```py
column_mapping = ColumnMapping( 
  numerical_features=['Age', 'Positive_Feedback_Count'], 
  categorical_features=['Division_Name', 'Department_Name', 'Class_Name'], 
  text_features=['Review_Text', 'Title']
)
```

你还应该将数据分成两部分：参考数据和当前数据。假设“参考”数据是某个代表性过去时期的数据（例如，上个月），而“当前”是当前生产数据（例如，本月）。这两个数据集是你将使用描述符进行比较的数据集。

> ***注意：*** *建立合适的历史基线很重要。选择一个反映你对未来数据预期的时期。*

我们为每个样本选择了5000个例子。为了增加趣味性，我们通过选择负面评论来人为地引入了一些偏差。

```py
reviews_ref = reviews[reviews.Rating > 3].sample(n=5000, replace=True, ignore_index=True, random_state=42)
reviews_cur = reviews[reviews.Rating < 3].sample(n=5000, replace=True, ignore_index=True, random_state=42)
```

# 📊 3\. 探索性数据分析

为了更好地理解数据，你可以使用 Evidently 生成可视化报告。这里有一个预设的**文本概览预设**，帮助快速比较两个文本数据集。它结合了各种描述性检查，并评估整体数据漂移（在这种情况下，使用[基于模型的漂移检测方法](https://www.evidentlyai.com/blog/evidently-data-quality-monitoring-and-drift-detection-for-text-data)）。

此报告还包括一些标准描述符，并允许你使用触发词列表添加描述符。我们将在报告中查看以下描述符：

+   文本长度

+   OOV词汇的比例

+   非字母符号的比例

+   评论的情感

+   包含“dress”或“gown”这两个词的评论

+   包含“blouse”或“shirt”这两个词的评论

> *查看 Evidently* [*有关描述符的文档以获取详细信息*](https://docs.evidentlyai.com/user-guide/customization/text-descriptors-parameters/?utm_source=website&utm_medium=referral&utm_campaign=blog_text&utm_content=unstructured-data-monitoring)*。*

这是运行此报告所需的代码。你可以为每个描述符分配自定义名称。

```py
text_overview_report = Report(metrics=[ 
  TextOverviewPreset(column_name="Review_Text", descriptors={ 
    "Review texts - OOV %" : OOV(), 
    "Review texts - Non Letter %" : NonLetterCharacterPercentage(), 
    "Review texts - Symbol Length" : TextLength(), 
    "Review texts - Sentence Count" : SentenceCount(), 
    "Review texts - Word Count" : WordCount(), 
    "Review texts - Sentiment" : Sentiment(), 
    "Reviews about Dress" : TriggerWordsPresence(words_list=['dress', 'gown']), 
    "Reviews about Blouses" : TriggerWordsPresence(words_list=['blouse', 'shirt']), 
  }) 
]) 
text_overview_report.run(reference_data=reviews_ref, current_data=reviews_cur, column_mapping=column_mapping) 
text_overview_report
```

运行这样的报告有助于探索模式，并形成对特定属性的期望，例如文本长度分布。

“情感”描述符的分布迅速揭示了我们在拆分数据时所做的操作。我们将排名高于3的评论放入“参考”数据集中，而将更多负面评论放入“当前”数据集中。结果一目了然：

![](../Images/f6f0128ec9976fe67cdfb3d79e3cd4b2.png)

作者提供的图片。

默认报告非常全面，可以一次查看多个文本属性。可以探索描述符与数据集中其他列之间的相关性！

你可以在探索阶段使用它，但这可能不是你需要一直使用的工具。

幸运的是，它很容易自定义。

> ***Evidently预设和指标***。Evidently有报告预设，可以快速生成报告。不过，还有很多个别指标可供选择！你可以将它们组合起来创建自定义报告。浏览一下* [*预设*](https://docs.evidentlyai.com/presets/all-presets/?utm_source=website&utm_medium=referral&utm_campaign=blog_text&utm_content=unstructured-data-monitoring) *和指标以了解有哪些。*

# 📈 4\. 监控描述符漂移

假设根据探索性分析和你对业务问题的理解，你决定仅跟踪少量属性：

你希望注意到何时有统计变化：这些属性的分布与参考期不同。为了检测它，你可以使用Evidently中实现的漂移检测方法。例如，对于“情感”这样的数值特征，它将默认使用Wasserstein距离监控漂移。你也可以选择其他方法。

这是你如何创建一个简单的漂移报告以跟踪三个描述符变化的方法。

```py
descriptors_report = Report(metrics=[ 
  ColumnDriftMetric(WordCount().for_column("Review_Text")), 
  ColumnDriftMetric(Sentiment().for_column("Review_Text")), 
  ColumnDriftMetric(TriggerWordsPresence(words_list=['dress', 'gown']).for_column("Review_Text")), 
]) 
descriptors_report.run(reference_data=reviews_ref, current_data=reviews_cur, column_mapping=column_mapping) 
descriptors_report
```

一旦你运行报告，你将得到所有选定描述符的综合可视化。这里是一个示例：

![](../Images/167750d2843c66fe380500152b7584da.png)

作者提供的图片。

深绿色线条是参考数据集中情感的平均值。绿色区域覆盖了平均值的一个标准差。你可以注意到当前的分布（红色）明显更为负面。

> ***注意：*** *在这种情况下，监控输出漂移也是有意义的：通过跟踪预测类别的变化。你可以使用分类的* [*数据漂移检测方法*](https://docs.evidentlyai.com/reference/data-drift-algorithm/?utm_source=website&utm_medium=referral&utm_campaign=blog_text&utm_content=unstructured-data-monitoring)*，如JS散度。我们在教程中没有涵盖这一点，因为我们仅关注输入而不生成预测。在实际应用中，预测漂移通常是反应的第一个信号。*

# 😍 5\. 添加一个“情感”描述符

假设你决定跟踪一个更有意义的属性：评论中表达的情感。整体情感是一个方面，但它也有助于区分“伤心”和“愤怒”的评论，例如。

让我们添加这个自定义描述符吧！你可以找到一个合适的外部开源模型来评分你的数据集。然后，你将把这个属性作为额外的列进行处理。

我们将使用来自Huggingface的[Distilbert模型](https://huggingface.co/bhadresh-savani/distilbert-base-uncased-emotion)，它通过五种情感对文本进行分类。

你可以考虑为你的用例使用任何其他模型，如命名实体识别、语言检测、毒性检测等。

你必须安装transformers才能运行模型。有关更多细节，请查看[安装说明](https://huggingface.co/docs/transformers/installation)。然后，将其应用于评论数据集：

```py
from transformers import pipeline 
classifier = pipeline("text-classification", model='bhadresh-savani/distilbert-base-uncased-emotion', top_k=1) 
prediction = classifier("I love using evidently! It's easy to use", ) 
print(prediction)
```

> ***注意：*** *这一步将使用外部模型对数据集进行评分。根据你的环境，这可能需要一些时间才能执行完毕。要在不等待的情况下了解原理，请参考示例笔记本中的“简单示例”部分。*

在将新列“emotion”添加到数据集后，你必须在列映射中反映这一点。你应该指定这是数据集中的一个新分类变量。

```py
column_mapping = ColumnMapping( 
  numerical_features=['Age', 'Positive_Feedback_Count'], 
  categorical_features=['Division_Name', 'Department_Name', 'Class_Name', 'emotion'], 
  text_features=['Review_Text', 'Title'] )
```

现在，你可以将“emotion”分布漂移监控添加到报告中。

```py
descriptors_report = Report(metrics=[ 
  ColumnDriftMetric(WordCount().for_column("Review_Text")), 
  ColumnDriftMetric(Sentiment().for_column("Review_Text")), 
  ColumnDriftMetric(TriggerWordsPresence(words_list=['dress', 'gown']).for_column("Review_Text")), 
  ColumnDriftMetric('emotion'), ]) 
descriptors_report.run(reference_data=reviews_ref, current_data=reviews_cur, column_mapping=column_mapping) 
descriptors_report
```

这是你得到的结果！

![](../Images/8ca2d594a2f48d88dec8b624c5ca703b.png)

图片来源：作者。

你可以看到“sad”评价显著增加，而“joy”评价减少。

追踪随时间变化是否有帮助？你可以继续通过对新数据进行评分来运行此检查。

# 🏗️ 6\. 运行管道测试

为了定期分析数据输入，将评估打包成测试是有意义的。在这种情况下，你会得到一个明确的“通过”或“失败”结果。如果所有测试都通过，你可能不需要查看图表。你只对变化感兴趣！

Evidently 有一个替代接口，称为测试套件，它以这种方式工作。

这里是如何创建一个测试套件以检查相同四个描述符中的统计分布：

```py
descriptors_test_suite = TestSuite(tests=[ 
  TestColumnDrift(column_name = 'emotion'), 
  TestColumnDrift(column_name = WordCount().for_column("Review_Text")), 
  TestColumnDrift(column_name = Sentiment().for_column("Review_Text")), 
  TestColumnDrift(column_name = TriggerWordsPresence(words_list=['dress', 'gown']).for_column("Review_Text")), 
]) 
descriptors_test_suite.run(reference_data=reviews_ref, current_data=reviews_cur, column_mapping=column_mapping) 
descriptors_test_suite
```

> ***注意：*** *我们使用默认设置，但你也可以设置* [*自定义漂移方法和条件*](https://docs.evidentlyai.com/user-guide/customization/options-for-statistical-tests/?utm_source=website&utm_medium=referral&utm_campaign=blog_text&utm_content=unstructured-data-monitoring)*。*

这是结果。输出结构清晰，你可以看到哪些描述符发生了漂移。

![](../Images/77d27b490b8c0aa4b642b8509c854d7e.png)

图片来源：作者。

检测统计分布漂移是监控文本属性变化的方法之一。还有其他方法！有时，根据描述符的最小值、最大值或均值运行基于规则的期望也很方便。

假设你想检查所有评价文本是否长于两个词。如果至少有一条评价短于两个词，你希望测试失败并在响应中查看短文本的数量。

你可以选择一个**TestNumberOfOutRangeValues()** 检查。此次，你应设置一个自定义边界：期望范围的“左”侧是两个词。你还必须设置一个测试条件：**eq=0**。这意味着你期望这个范围外的对象数量为0。如果高于此值，你希望测试返回失败。

```py
descriptors_test_suite = TestSuite(tests=[ 
  TestNumberOfOutRangeValues(column_name = WordCount().for_column("Review_Text"), left=2, eq=0), 
]) 
descriptors_test_suite.run(reference_data=reviews_ref, current_data=reviews_cur, column_mapping=column_mapping) 
descriptors_test_suite
```

这是结果。你还可以看到显示定义期望的测试细节。

![](../Images/c7522aa55016dfa55e61e8499366d7d2.png)

图片来源：作者。

你可以遵循这一原则设计其他检查。

# 支持 Evidently

> *喜欢这个教程吗？给* [*Evidently 在 GitHub 上点个星*](https://github.com/evidentlyai/evidently) *以回馈支持！这有助于我们继续为社区创建免费的开源工具和内容。* [*⭐️ 在 GitHub 上点星 ⟶*](https://github.com/evidentlyai/evidently)

# 总结

文本描述符将文本数据映射到可解释的维度，你可以将其表示为数值或分类属性。它们帮助描述、评估和监控非结构化数据。

在本教程中，你学会了如何使用描述符来监控文本数据。

你可以使用这种方法来监控 NLP 和 LLM 驱动模型的生产行为。你可以将你的描述符与其他方法结合使用，如 [监控嵌入漂移](https://www.evidentlyai.com/blog/embedding-drift-detection)。

你认为还有其他普遍有用的描述符吗？告诉我们！加入我们的 [Discord 社区](https://discord.com/invite/xZjKRaNp8b) 分享你的想法。

*最初发布于* [*https://www.evidentlyai.com*](https://www.evidentlyai.com/blog/unstructured-data-monitoring) *于2023年6月27日。感谢* [*Olga Filippova*](https://www.evidentlyai.com/author/olga-fillipova) *共同撰写本文。*
