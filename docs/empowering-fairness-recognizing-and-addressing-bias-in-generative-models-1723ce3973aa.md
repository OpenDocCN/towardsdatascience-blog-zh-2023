# 赋能公平：识别与解决生成模型中的偏见

> 原文：[`towardsdatascience.com/empowering-fairness-recognizing-and-addressing-bias-in-generative-models-1723ce3973aa`](https://towardsdatascience.com/empowering-fairness-recognizing-and-addressing-bias-in-generative-models-1723ce3973aa)

## 随着人工智能融入我们的日常生活，一个有偏见的模型可能对用户产生严重后果

[](https://medium.com/@kevin.berlemont?source=post_page-----1723ce3973aa--------------------------------)![Kevin Berlemont, PhD](https://medium.com/@kevin.berlemont?source=post_page-----1723ce3973aa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1723ce3973aa--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1723ce3973aa--------------------------------) [Kevin Berlemont, PhD](https://medium.com/@kevin.berlemont?source=post_page-----1723ce3973aa--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----1723ce3973aa--------------------------------) ·阅读时间 6 分钟·2023 年 7 月 6 日

--

![](img/49c317d74bb76b8a63d74a410f64cb29.png)

图片由[Dainis Graveris](https://unsplash.com/@dainisgraveris?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

2021 年，普林斯顿大学信息技术政策中心发布了一份报告，发现机器学习算法可能会从训练数据中学习到类似于人类的偏见。其中一个引人注目的例子是关于亚马逊**[1]**的 AI 招聘工具的研究。该工具在之前一年提交给亚马逊的简历上进行了训练，并对不同的候选人进行了排名。由于过去十年科技职位中性别比例严重失衡，该算法学会了将语言与女性相关联，比如女性运动队，并降低了这些简历的排名。这个例子突显了不仅需要公平和准确的模型，还需要公平的数据集，以消除训练中的偏见。在 ChatGPT 等生成模型快速发展的背景下，以及人工智能日益融入我们的日常生活中，一个有偏见的模型可能会带来严重后果，侵蚀用户信任和全球接受度。因此，从商业角度来看，解决这些偏见是必要的，数据科学家（广义上的定义）必须意识到这些偏见，以减轻其影响，并确保其与原则一致。

# 生成模型中的偏见示例

第一个想到的生成模型广泛使用的任务类型是翻译任务。用户输入一种语言 A 的文本，期望得到语言 B 的翻译。例如，不同语言不一定使用相同类型的性别代词，例如英文中的*“The senator”*可以是女性或男性，而法语中则是*“La senatrice”*或*“Le senateur”*。即使在句子中指定了性别（见下例），生成模型在翻译过程中强化性别刻板印象角色也并不罕见。

![](img/1f139699f5d40454bf4c3d0c442551be.png)

使用 ChatGPT 的翻译任务示例

类似于翻译任务，字幕生成任务要求模型基于某些输入生成新的文本，即将图像翻译成文本。一项最近的研究**[2]**分析了生成型变换器模型在 Common Objects in Context 数据集上进行字幕生成任务（见下图）的表现。

![](img/7e7a67c133b7be3514c7710863c91b9e.png)

图源：“理解与评估种族偏见” [`arxiv.org/pdf/2106.08503.pdf`](https://arxiv.org/pdf/2106.08503.pdf)

尽管这些描述符在所有图像中并不适用，但生成模型为字幕分配了各种种族和文化描述。这些描述符仅由更新的生成模型学习，并显示出这些模型的偏见增加。值得注意的是，变换器模型对于这个数据集也表现出性别偏见，例如，通过房屋/房间的背景将某人识别为女性，从而加剧了男女不平衡的比例。

**偏差为何会发生？**

生成模型的构思阶段为模型中的偏差发展留出了大量空间。偏差可能来自数据本身、标签和注释、内部表示甚至模型（请参见 [`huggingface.co/blog/ethics-soc-4`](https://huggingface.co/blog/ethics-soc-4) 以获取针对 Text-to-Image 模型的详细列表）。

生成模型训练所需的数据来自众多来源，通常是在线的。为了确保训练数据的完整性，人工智能公司通常使用知名新闻网站等类似资源来构建其数据库。在这个数据集上训练的模型将由于考虑的群体人口特征限制（通常是白人、中年、上层中产阶级）而延续偏见的联想。

标签偏差 ([`www.ncbi.nlm.nih.gov/pmc/articles/PMC3994857/`](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3994857/)) 可能是更明显的，因为它会导致标签数据中引入偏差，这通常是无意的。生成模型被训练以重现/近似其训练数据集，因此标签中的偏差将对模型的输出表示产生严重影响。幸运的是，使用多个版本的标签并交叉检查它们可以缓解这些偏差。

最后两种偏见类型，即内部表示和模型偏见，都来自建模过程中的特定步骤。第一种在预处理阶段引入，无论是手动还是算法性。这一阶段容易引入偏见和文化细微差别的丧失，特别是当原始数据集缺乏多样性时。模型偏见则简单地来源于基于歧视特征的目标函数以及为提高模型准确性而放大的偏见。

# 生成模型中的偏见检测

正如本文所强调的，生成模型中的偏见以各种形式和不同条件下的方式存在。检测偏见的方法必须像它们试图检测的偏见一样多样化。

语言模型中偏见的主要衡量方法之一是词嵌入关联测试。该分数测量的是在嵌入空间（内部表示）中，两个词集之间的相似性。高分数表示强关联。更具体地说，它计算的是目标词集与两个输入词集之间相似性的差异，例如将*[home, family]*作为目标，将*[he, man]/[she, woman]*作为输入。如果分数为 0，则表示模型完全平衡。该指标被用来证明 RoBERTa 是最偏见的生成模型之一 ([`arxiv.org/pdf/2106.13219.pdf`](https://arxiv.org/pdf/2106.13219.pdf))。

测量生成模型（反事实评估）偏见的一个创新方法，尤其是性别偏见，是通过交换词汇的性别并观察预测准确性的变化。如果修改后的准确性与原始准确性不同，则表明模型中存在偏见，因为一个公正的生成模型应该在输入的性别独立的情况下具有相同的准确性。这一措施的主要缺陷是它只捕捉到性别偏见，因此需要其他测量方法来全面评估偏见来源。在类似的思路下，可以使用双语评估替代（经典翻译测量）来比较性别交换输入和原始输入产生的输出之间的相似性。

目前的生成模型基于使用一种叫做*注意力*的特征的变换器模型，以根据输出预测结果。研究已经直接从模型中调查了性别与角色之间的关系 ([`arxiv.org/abs/2110.15733`](https://arxiv.org/abs/2110.15733))。这允许比较模型中的不同部分，以检测哪个模块对偏见的贡献更大。如果通过这种措施表明生成模型在维基百科数据集上引入了性别偏见，这一措施的一个缺陷是注意力值不代表概念之间的直接效应和相似性，需要深入分析才能得出结论。

**如何克服生成模型中的偏见？**

研究人员开发了各种技术以提供更少偏见的生成系统。这些技术大多包括在建模中的额外步骤，如设置一个控制变量以基于先前的信息确定性别，或添加另一个模型以提供上下文信息。然而，所有这些步骤不一定解决固有偏见数据集的问题。此外，大多数生成模型基于英文训练数据，极大地限制了这些模型的文化和社会多样性。

完全克服生成模型中的偏见需要建立一个正式的框架和基准，以测试和评估跨多种语言的模型。这将允许检测到在多样化的人工智能模型中以细微方式存在的偏见。

# 参考文献

+   *亚马逊废弃了一个显示出对女性有偏见的秘密 AI 招聘工具，* Jeffrey Dastin*,* [`www.reuters.com/article/us-amazon-com-jobs-automation-insight-idUSKCN1MK08G`](https://www.reuters.com/article/us-amazon-com-jobs-automation-insight-idUSKCN1MK08G)

+   *理解和评估图像描述中的种族偏见，* Dora Zhao, Angelina Wang, Olga Russakovsky, [`arxiv.org/pdf/2106.08503.pdf`](https://arxiv.org/pdf/2106.08503.pdf)

关注我在 [Medium](https://medium.com/@kevin.berlemont) 上，获取更多关于数据科学的内容！

如果你喜欢阅读这样的故事并想支持我作为作者，可以考虑 [注册成为 Medium 会员](https://medium.com/@kevin.berlemont/membership)。每月 $5，你将无限制访问 Medium 上的所有故事。如果你 [使用我的链接注册](https://medium.com/@kevin.berlemont/membership)，我将获得少量佣金。

[](https://medium.com/@kevin.berlemont/membership?source=post_page-----1723ce3973aa--------------------------------) [## 使用我的推荐链接加入 Medium - Kevin Berlemont, PhD

### 作为 Medium 的会员，你的部分会费将用于支持你阅读的作者，并且你可以完全访问每一个故事……

medium.com](https://medium.com/@kevin.berlemont/membership?source=post_page-----1723ce3973aa--------------------------------)
