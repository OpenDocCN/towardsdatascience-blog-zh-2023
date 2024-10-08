# 透过镜子，谷歌在眼睛中发现了什么

> 原文：[`towardsdatascience.com/through-the-looking-glass-and-what-google-find-there-in-the-eye-e7a836eb9571`](https://towardsdatascience.com/through-the-looking-glass-and-what-google-find-there-in-the-eye-e7a836eb9571)

## | 计算机视觉 | 医疗 AI | CNN

## 或者，谷歌如何利用深度学习来诊断眼部照片中的疾病

[](https://salvatore-raieli.medium.com/?source=post_page-----e7a836eb9571--------------------------------)![Salvatore Raieli](https://salvatore-raieli.medium.com/?source=post_page-----e7a836eb9571--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e7a836eb9571--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e7a836eb9571--------------------------------) [Salvatore Raieli](https://salvatore-raieli.medium.com/?source=post_page-----e7a836eb9571--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----e7a836eb9571--------------------------------) ·阅读时间 12 分钟·2023 年 3 月 30 日

--

![](img/f4f232c4e1c318f5fce9ec857c2775d1.png)

图片由作者使用 OpenAI DALL-E 生成

谷歌最近发布了一篇科学论文，展示了人工智能模型如何通过一张简单的眼睛照片预测多个系统性生物标志物。

> 这如何运作？这些结果是如何得出的？这为何重要？我们将在本文中讨论这些问题。

# 眼睛中的隐藏宝藏

![](img/35db24ac8670c2348b942cd5ebabcba1.png)

图片由[v2osk](https://unsplash.com/it/@v2osk)提供，来自 Unsplash

疾病诊断通常需要昂贵仪器的检查，然后由受过训练的医疗专业人员进行解读。这并不总是可行。并非所有医院都有相同的仪器，有时还缺乏专家。

例如，[糖尿病视网膜病变（DR）](https://en.wikipedia.org/wiki/Diabetic_retinopathy)的诊断需要使用检查眼睛后部的眼底相机。这需要由资质高的人员进行分析。此检查还可以揭示其他状况，如心血管风险、贫血、慢性肾病及其他系统性参数。

曾经认为，眼底图像可以使用机器学习算法进行分析。然而，[谷歌发布的一篇论文](https://www.nature.com/articles/s41551-018-0195-0.epdf?author_access_token=YWBi0EzCgfAVb_S540xl-tRgN0jAjWel9jnR3ZoTv0OMsbBDq-7d5VZef-dAA8S4kHGY_hXONc93gwXXjuO908b_ruUDVkgB5jW3RnvvRdLFLmvpTsPku5cXZoTEtr09fPvTK40ZbWzpoOGfLab-NA%3D%3D)在 2017 年展示了外部眼睛照片能够诊断糖尿病视网膜病变并检测血糖控制不佳。

![](img/63c3f7204bfb6f3869cbebcd9c593a1a.png)

“糖尿病相关并发症可以通过使用专用相机拍摄眼底照片来诊断，这些照片可视化眼睛的后部区域。相反，使用标准消费级相机进行前眼成像可以揭示影响眼睑、结膜、角膜和晶状体的情况。” 图片来源：[这里](https://europepmc.org/article/med/35352000)

在这项工作中，作者使用了来自加利福尼亚和其他队列的 145,832 名糖尿病患者的照片。作者然后使用了[Inception V3](https://en.wikipedia.org/wiki/Inceptionv3)（它之前已经在[ImageNet](https://en.wikipedia.org/wiki/ImageNet)上进行过训练）进行这项研究，结果显示：

> 我们的结果显示，眼睛的外部图像包含糖尿病相关视网膜病变、血糖控制不良和脂质升高的信号。([来源](https://europepmc.org/article/med/35352000))

简而言之，[Inception V3](https://arxiv.org/pdf/1512.00567.pdf) 当时在 ImageNet 上展示了最先进的性能（准确率 > 78.1%）。此外，Inception 比以前的模型计算效率更高。该模型通过使用并行结构（在同一个块中使用不同类型的[卷积](https://en.wikipedia.org/wiki/Convolution)层）和积极的[正则化](https://en.wikipedia.org/wiki/Regularization_(mathematics))来达到这些结果。在同一篇文章中，作者定义了一些原则，这些原则在接下来的几年里塑造了[卷积神经网络](https://en.wikipedia.org/wiki/Convolutional_neural_network)（CNN）领域：

+   避免表示瓶颈。表示大小应从输入到输出逐渐减少。

+   高维表示在网络内部本地处理起来更容易。这显示出可以更快地训练网络。

+   空间聚合可以更好地减少维度而不会丢失信息

+   平衡网络的宽度和深度

![](img/7a5263c9e49e8ffd7d07d770761d3768.png)

“模型的高层次图示” 图片来源：[这里](https://cloud.google.com/tpu/docs/inception-v3-advanced)

作者使用了经典的监督学习，实际上，他们使用了患者眼睛的图像作为是否存在疾病（糖尿病视网膜病、升高的血糖或升高的脂质）的基础真值。因此，训练出的模型在糖尿病视网膜病诊断方面显示出超过 80%的曲线下面积（AUC），而对于血糖和脂质则显示出突出的（但较低的）结果。

结果令人惊讶，因为通常这些系统参数可以从前眼得出，而这项初步研究表明，从外眼的照片中，通过深度学习也可以得出相同的结论。

此外，通过使用[消融研究](https://en.wikipedia.org/wiki/Ablation_(artificial_intelligence))和[显著性图](https://en.wikipedia.org/wiki/Saliency_map)，作者也能更好地理解模型为何做出这些预测：

> 首先，消融分析表明，对于所有预测，图像的中心（瞳孔/晶状体、虹膜/角膜和结膜/巩膜）比图像的周边（例如眼睑）更为重要。其次，显著性分析同样表明，DLS 最受图像中心附近区域的影响。（[来源](https://europepmc.org/article/med/35352000)）

![](img/e5c51685f8324c3a4046b59fb3327d1a.png)

重要性图。图片来源：[这里](https://europepmc.org/article/med/35352000)

![](img/dcf74de03ebebe8ff23339e2601cae14.png)

消融研究：图像不同区域的重要性。图片来源：[这里](https://europepmc.org/article/med/35352000)

这些结果表明，对于日益增长的糖尿病患者群体，一些参数可以在不需要专业医疗人员的情况下进行测量。此外，外眼的照片也可以使用简单相机拍摄获取。

> 虽然需要进一步工作来确定是否有额外的要求，例如光线、拍摄距离或角度、图像稳定性、镜头质量或传感器保真度，但我们希望通过外眼图像进行疾病检测的技术最终可以广泛提供给患者，无论是在诊所、药店还是在家中。（[来源](https://europepmc.org/article/med/35352000)）

无论如何，目前这些模型并不打算取代广泛筛查，而是用于标识哪些患者需要进一步筛查（这种方法比问卷调查更可靠）。

作者们继续评估模型，并引起了对[潜在偏见和包含问题](https://www.forbes.com/sites/bernardmarr/2022/09/30/the-problem-with-biased-ais-and-how-to-make-ai-better/?sh=1ced823c4770)的关注。确实，生物医学领域人工智能模型最大的问题之一是，如果数据集不能代表总体人群，这可能会导致误导性结果。

> 我们的开发数据集覆盖了美国范围内多个不同地点，包括在 301 个糖尿病视网膜病筛查点拍摄的超过 300,000 张去标识化照片。我们的评估数据集包括来自 18 个美国州的 198 个地点的超过 95,000 张图像，其中包括主要为西班牙裔或拉丁裔患者的数据集、主要为黑人患者的数据集以及包括非糖尿病患者的数据集。我们对不同人口统计学和身体特征（如年龄、性别、种族和民族、白内障存在、瞳孔大小，甚至相机类型）的患者群体进行了广泛的亚组分析，并将这些变量作为协变量进行控制。（[来源](https://ai.googleblog.com/2022/03/detecting-signs-of-disease-from.html)）

# 眼睛，灵魂的镜子

![](img/b22db7de19e379484e3a775415a6e26a.png)

图片由[Caroline Veronez](https://unsplash.com/it/@carolineveronez)在 Unpslash 提供

Google 和其他研究人员也认为这种方法很有前途。因此，他们后来尝试将其扩展到其他标记和其他疾病。

到目前为止，作者已展示他们的模型能够有效诊断眼科疾病（糖尿病视网膜病）。另一方面，疾病种类繁多，诊断复杂（例如，找到合适的测试，昂贵的仪器并不总是存在等等）。因此，问题仍然是模型是否也能捕捉到眼睛图像中的其他疾病迹象。

> 我们可以将这种方法扩展到其他疾病吗？

毕竟，深度学习模型能够识别那些微妙的模式，而这些模式可能对非专家来说难以识别。考虑到这些假设，[Google 研究人员决定测试](https://www.nature.com/articles/s41551-018-0195-0)是否可以在眼底图像中检测心血管风险因素。

心血管疾病是[全球主要死亡原因](https://pubmed.ncbi.nlm.nih.gov/32501203/)，能够早期诊断可能拯救无数生命。此外，[风险分层](https://www.nci.nlm.nih.gov/pmc/articles/PMC8299990/)对于识别和管理高风险患者群体至关重要。通常，通过病史和不同测试（血糖和胆固醇水平的血液样本、年龄、性别、吸烟状态、血压和体重指数）获得的多个变量用于诊断和分层患者。有时所有必要的数据可能并不齐全（[如一项综述研究所示](https://pubmed.ncbi.nlm.nih.gov/25593051/)）。

这项研究的作者展示了不仅可以预测一些患者特征（在某些数据未记录或缺失的情况下非常有用），例如[BMI](https://www.nhlbi.nih.gov/health/educational/lose_wt/BMI/bmicalc.htm)、年龄、性别和吸烟状态，还可以预测与心血管疾病相关的参数，例如[收缩压](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC1124431/)（SBP）和[舒张压](https://www.webmd.com/hypertension-high-blood-pressure/guide/diastolic-and-systolic-blood-pressure-know-your-numbers)（DBP）。

![](img/4442b4bc8dac94439a3a2321ec8d27f9.png)

“左上角的图像是来自英国生物库数据集的彩色视网膜图像样本。其余图像显示的是相同的视网膜图像，但为黑白色。每个预测的软注意力热图以绿色覆盖，指示神经网络模型在进行图像预测时所使用的热图区域。” 图片来源：[这里](https://arxiv.org/ftp/arxiv/papers/1708/1708.09843.pdf)

作者们也使用了相同的 Inception V3 模型。此外，为了处理连续变量，作者们使用了分箱技术，基本上是将变量划分为不同的区间，使用不同的分割点（例如，<120，120–140，140–160 和 ≥160 作为 SBP）。

此外，作者使用了一种称为[soft attention](https://arxiv.org/pdf/1502.03044.pdf)的技术来识别与特定特征相关的区域。简而言之，[soft attention](https://arxiv.org/pdf/1507.01053.pdf)是一种考虑图像不同子区域并使用[梯度下降](https://en.wikipedia.org/wiki/Gradient_descent)和[反向传播](https://en.wikipedia.org/wiki/Backpropagation)的方法（无需在模型中实现注意力机制）。

![](img/fbea3de2132c10d1dc35e7d5c8bd32fa.png)

soft-attention 可以帮助发现模型的预测错误。图像来源：[这里](https://arxiv.org/pdf/1502.03044.pdf)

在另一项工作中，作者测试了[贫血](https://en.wikipedia.org/wiki/Anemia)。一种影响[超过 16 亿](https://www.cambridge.org/core/journals/public-health-nutrition/article/worldwide-prevalence-of-anaemia-who-vitamin-and-mineral-nutrition-information-system-19932005/E201EDE33949AF3D632F6596052FCF8F)人的病症，需要监测[血红蛋白](https://en.wikipedia.org/wiki/Hemoglobin)浓度以进行诊断（这是一种侵入性测试，可能引起疼痛和感染风险）。

![](img/57786f580f3571154ef396c9b0be2162.png)

使用深度学习对贫血分类进行预测。图像来源：[这里](https://arxiv.org/ftp/arxiv/papers/1904/1904.06435.pdf)

在这种情况下，他们使用了[Inception V4](https://arxiv.org/pdf/1602.07261.pdf)。这是一个比之前描述的模型更晚的版本（在这篇后续文章中，作者描述了通过添加[残差连接](https://arxiv.org/pdf/1512.03385.pdf)可以改进 Inception V3 的架构）。Inception V4 展示了如何测试和使用不同类型的 Inception 块（其中有不同的卷积层，既有并行也有顺序）。

![](img/da7f0c17b72ccb25c495b12aa3ef67ac.png)

残差连接。图像来源：[这里](https://arxiv.org/pdf/1602.07261.pdf)

在这项后续工作中，[Google 展示了这种方法不限于分类](https://www.nature.com/articles/s41551-019-0487-z)（患者是否有贫血），还包括是否可以测量[血红蛋白浓度](https://www.ncbi.nlm.nih.gov/books/NBK259/)（回归任务）。作者训练了一个用于分类的模型和一个用于[回归任务](https://www.sciencedirect.com/topics/computer-science/regression-task)的模型（Inception V4，预训练于 ImageNet）。

对于回归问题，作者仅使用了[均方误差](https://en.wikipedia.org/wiki/Mean_squared_error)作为损失函数（而非[交叉熵](https://en.wikipedia.org/wiki/Cross_entropy)）。最终的预测是通过创建一个包含 10 个模型的[集成](https://en.wikipedia.org/wiki/Ensemble_learning)（以相同方式训练）来实现的，并将这些模型的输出进行平均，以得到最终的预测结果。

![](img/b6ebd0b5261865d71c9f9766c4fc3eab.png)

“预测血红蛋白浓度。每个蓝点代表每位患者的测量血红蛋白浓度和预测值”。图片来源：[这里](https://arxiv.org/ftp/arxiv/papers/1904/1904.06435.pdf)

# 广阔景观的一瞥

![](img/a30a4162cdd27117d2c7a2577b96dcb2.png)

图片由[Bailey Zindel](https://unsplash.com/it/@baileyzindel)提供，来源于 Unsplash

到目前为止，研究人员观察到的通常是少量参数。通常，血液检测可以在一次检查中监测更多的参数。**一个眼睛的照片模型能否估计一组系统性生物标志物？**

这是谷歌今年测试并刚刚发布的内容：

[](https://www.thelancet.com/journals/landig/article/PIIS2589-7500%2823%2900022-5/fulltext?source=post_page-----e7a836eb9571--------------------------------) [## 一种用于外眼照片中新型系统性生物标志物的深度学习模型：…

### 系统性疾病引起的眼部后遗症已被充分记录，并作为全球公认的基础…

www.thelancet.com](https://www.thelancet.com/journals/landig/article/PIIS2589-7500%2823%2900022-5/fulltext?source=post_page-----e7a836eb9571--------------------------------)

显然，这不是一项容易的任务，部分原因是当你想进行这样的分析时，存在发现虚假和错误结果的风险（也称为[多重比较问题](https://en.wikipedia.org/wiki/Multiple_comparisons_problem)）。换句话说，同时进行的统计推断越多，发现错误推断的风险就越大。

![](img/92c4ef6295751a4647815166d0fd6d2b.png)

虚假相关的示例。图片来源：[这里](https://en.wikipedia.org/wiki/Multiple_comparisons_problem)

因此，作者首先将数据集分成两部分。他们在“开发数据集”上训练模型并进行分析，选择了九个最有前景的预测任务，并在测试数据集上评估了模型（他们仍然[纠正了多重比较](https://en.wikipedia.org/wiki/Bonferroni_correction)）。

他们首先收集了包含眼睛图像和相应实验室测试结果的数据集。然后，作者训练了一个卷积神经网络，该网络以外眼图像为输入，并预测临床和实验室测量结果。在这种情况下，这是一个多任务分类，每个任务都有一个预测头（以便可以使用交叉熵作为损失）。作者决定为每个任务选择阈值（与临床医生协商后选择）。

在这种情况下，作者使用了 2020 年发布的 Big Transfer（BiT）模型，该模型在训练时能够在各种其他数据集上很好地推广。简而言之，该模型与 ResNet 非常相似，不过在训练过程中使用了一些技巧，例如[组归一化](https://arxiv.org/pdf/1803.08494.pdf)（GN）和[权重标准化](https://arxiv.org/pdf/1405.0312.pdf)（WS）。你可以在这个[GitHub 仓库](https://github.com/google-research/big_transfer)中找到该模型。

![](img/24d9d9ab63cdb4ca1f761a1c10e58d66.png)

预训练模型的转移性能超越了现有技术。图像来源：[这里](https://arxiv.org/pdf/1912.11370.pdf)

超过了基线模型（对患者数据进行的逻辑回归）。虽然这些结果仍然不足以用于诊断应用，但与初步筛查工具一致（[糖尿病预筛查](https://www.bmj.com/content/343/bmj.d7163.long)）。

![](img/9a1481a4c32a3926055f75c372d02968.png)

**基线模型与深度学习模型的 AUC 比较。图像来源：** [**这里**](https://arxiv.org/ftp/arxiv/papers/2207/2207.08998.pdf)

在本研究及之前的研究中，作者使用了桌面相机（患者还使用了头托）获取图像，并在良好的照明条件下生成了高质量的图像。因此，作者尝试通过降低分辨率来查看模型是否有效。

作者注意到，即使将图像缩小到 150x150 像素，模式仍然对图像质量具有鲁棒性。

> 这个像素计数低于 0.1 百万像素，比典型的智能手机相机要小得多。（[来源](https://ai.googleblog.com/2023/03/detecting-novel-systemic-biomarkers-in.html)）

![](img/4a1892b8042158695500778e524bd56b.png)

“*输入图像分辨率的影响。* ***上图：*** *为本实验缩放到不同尺寸的样本图像。* ***下图：*** *DLS 在图像尺寸下的性能比较*”。图像来源：[这里](https://arxiv.org/ftp/arxiv/papers/2207/2207.08998.pdf)

此外，作者研究了图像的哪个部分对模型的预测目的重要。为此，作者在训练和评估期间遮蔽了几个区域（瞳孔或虹膜，将图像转换为黑白）。

> 结果表明，信息通常不仅仅局限于瞳孔或虹膜，并且颜色信息对大多数预测目标至少有一定的重要性（[来源](https://www.thelancet.com/journals/landig/article/PIIS2589-7500(23)00022-5/fulltext#seccestitle160)）

![](img/de4a2ceafc857c6491b9086938795f16.png)

实验遮蔽不同的图像区域或移除颜色**。** 图像来源：[这里](https://arxiv.org/ftp/arxiv/papers/2207/2207.08998.pdf)

尽管这篇文章令人印象深刻，但仍然存在一些局限性。实际上，认为它可以在现实世界中使用仍然为时尚早。首先，照片是在最佳条件下获取的，我们需要验证在其他条件下获取的照片的准确性。

> 此外，这项工作中使用的数据集主要包括糖尿病患者，并没有充分代表一些重要的亚组——在考虑临床使用之前，需要对 DLS 的改进和评估进行更加针对性的数据收集，包括对更广泛人群和亚组的评估。（[来源](https://ai.googleblog.com/2023/03/detecting-novel-systemic-biomarkers-in.html)）

# 分析思考

![](img/c7c5a81296fe18f347780466460582b4.png)

来自 [Saif71.com](https://unsplash.com/it/@saif71) 在 Unsplash 的图片

如本文所示，深度学习模型能够捕捉眼睛中的模式和信息，这些信息有时难以诊断。此外，诊断通常使用昂贵的工具和侵入性测试，并且需要经验丰富的人员。Google 多年来已经表明，可以通过眼睛的图像获取类似的信息。

未来，许多疾病可以通过简单的眼睛外部照片来诊断（或至少进行初步筛查）。这些照片可以通过手机摄像头简单地拍摄。此外，由于可以获得定量结果（如血红蛋白浓度），这些结果可以用于非侵入性的患者监测。

在技术方面，有趣的是像 Inception V3 这样的模型如何在第一次尝试时就取得了结果。这表明迁移学习和卷积网络的能力。此外，作者对模型进行了分类、回归和多任务分类的适配。

然而，还有几个情境是开放的。显然，Google 计划扩展数据集（正如他们在文章中所写的）。另一方面，作者使用了 CNN，也可以测试其他几种模型，如视觉变换器。此外，未来他们也可能会尝试使用语言模型作为输入，这些输入包括患者或医生的笔记（毕竟，未来是多模态的）。

另一方面，尽管这些应用可以用于没有装备医院的患者，但它也引发了伦理问题。如所见，模型还能够预测诸如年龄、性别、生活方式（吸烟/不吸烟）以及其他参数等敏感数据。这项技术也可能用于其他更具争议的应用。

无论如何，这些研究开启了非常有趣的应用领域。这不仅仅是 Google 在研究这样的模型。例如，其他团队已经显示，其他疾病[如肝胆疾病](https://pubmed.ncbi.nlm.nih.gov/33509389/)可以通过眼睛识别，[其他疾病也可能](https://pubmed.ncbi.nlm.nih.gov/10758212/)在不久的将来被识别。

# 如果你觉得这很有趣：

你可以查看我的其他文章，也可以[**订阅**](https://salvatore-raieli.medium.com/subscribe)以便在我发布文章时获得通知，还可以通过[**LinkedIn**](https://www.linkedin.com/in/salvatore-raieli/)**联系**我。

这是我 GitHub 仓库的链接，我计划在这里收集与机器学习、人工智能等相关的代码和许多资源。

[](https://github.com/SalvatoreRa/tutorial?source=post_page-----e7a836eb9571--------------------------------) [## GitHub - SalvatoreRa/tutorial: 关于机器学习、人工智能、数据科学的教程…

### 提供关于机器学习、人工智能、数据科学的教程，包括数学解释和可重用的代码（用 Python…

[github.com](https://github.com/SalvatoreRa/tutorial?source=post_page-----e7a836eb9571--------------------------------)

或许你对我最近的一篇文章感兴趣：

[](/making-language-models-similar-to-human-brain-b6ea8270be08?source=post_page-----e7a836eb9571--------------------------------) ## 使语言模型更像人脑

### 语言模型和人脑在自然语言处理方面仍存在差距，激励人工智能填补这一差距

towardsdatascience.com [](/google-med-palm-the-ai-clinician-a4482143d60e?source=post_page-----e7a836eb9571--------------------------------) ## 谷歌 Med-PaLM：人工智能临床医生

### 谷歌的新模型经过训练可以回答医学问题。怎么做的？

towardsdatascience.com [](/multimodal-chain-of-thoughts-solving-problems-in-a-multimodal-world-961a8ab9d0fa?source=post_page-----e7a836eb9571--------------------------------) ## 多模态思维链：解决多模态世界中的问题

### 世界不仅仅是文字：如何将思维链扩展到图像和文字？

towardsdatascience.com [](https://levelup.gitconnected.com/stable-diffusion-to-fill-gaps-in-medical-image-data-b78a2a7d6c9d?source=post_page-----e7a836eb9571--------------------------------) [## 稳定扩散填补医疗图像数据中的空白

### 一项新研究表明，稳定扩散可能有助于医学图像分析和稀有疾病。怎么做的？

[在医疗图像数据中填补空白的稳定扩散](https://levelup.gitconnected.com/stable-diffusion-to-fill-gaps-in-medical-image-data-b78a2a7d6c9d?source=post_page-----e7a836eb9571--------------------------------)
