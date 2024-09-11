# 对比增强CT基础知识

> 原文：[https://towardsdatascience.com/the-basics-of-contrast-enhanced-ct-eba2ab7f3c78?source=collection_archive---------11-----------------------#2023-03-08](https://towardsdatascience.com/the-basics-of-contrast-enhanced-ct-eba2ab7f3c78?source=collection_archive---------11-----------------------#2023-03-08)

## **静脉对比增强对于CT成像至关重要。它可能如何影响你的CT图像数据集**

[](https://alexweston013.medium.com/?source=post_page-----eba2ab7f3c78--------------------------------)[![亚历山大·韦斯顿博士](../Images/a8c16e593076deea9567ff219fcd9eea.png)](https://alexweston013.medium.com/?source=post_page-----eba2ab7f3c78--------------------------------)[](https://towardsdatascience.com/?source=post_page-----eba2ab7f3c78--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----eba2ab7f3c78--------------------------------) [亚历山大·韦斯顿博士](https://alexweston013.medium.com/?source=post_page-----eba2ab7f3c78--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1f2053b66a67&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-basics-of-contrast-enhanced-ct-eba2ab7f3c78&user=Alexander+Weston%2C+PhD&userId=1f2053b66a67&source=post_page-1f2053b66a67----eba2ab7f3c78---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----eba2ab7f3c78--------------------------------) ·7分钟阅读·2023年3月8日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Feba2ab7f3c78&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-basics-of-contrast-enhanced-ct-eba2ab7f3c78&user=Alexander+Weston%2C+PhD&userId=1f2053b66a67&source=-----eba2ab7f3c78---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Feba2ab7f3c78&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-basics-of-contrast-enhanced-ct-eba2ab7f3c78&source=-----eba2ab7f3c78---------------------bookmark_footer-----------)![](../Images/3719092527f37443f16a95e6e6b537ca.png)

“风格如文森特·梵高的脑部CT”。 [DALL-E 2.](https://openai.com/product/dall-e-2)

对比剂的使用在放射成像中无处不在，但网上关于这个主题的信息却出奇地少。在这里，我将描述IV对比剂的基础知识，它在CT图像中的表现，以及它可能如何影响你的成像数据集。

作为从事医学影像研究的数据科学家，了解对比剂在数据集中的使用情况非常重要，因为对比剂会强烈影响CT检查的外观，包括强度和纹理。根据应用场景，对比剂可能是成像特征的主要来源，或者是一个不希望出现的变异来源。你还会发现对比剂的使用可能很微妙，有时标记也不准确，因此对这一主题有良好的工作知识将帮助你更有效地组织数据集和规划研究。

## **CT衰减基础**

要理解为什么在CT检查中使用对比剂，首先必须了解一点X射线物理学。CT扫描仪是一个3D成像系统，它测量被体内器官和组织吸收的X射线能量。在扫描仪内部，旋转的X射线管将高能光子射穿身体并进入探测器阵列，探测器测量光子能量的衰减率或*衰减*。

![](../Images/05788a5ede08ba5e2f35ca75fb6ecffb.png)

在计算机断层扫描中，Radon变换用于将旋转的1D X射线投影重建为2D轴向图像。GIF来自[WikiMedia](https://commons.wikimedia.org/wiki/File:Algebraic_Reconstruction_Technique_-_animated.gif)。

在体内，密度较高的结构如皮质骨会阻挡更多的光子，增加衰减率。这些高衰减区域在最终扫描中会显得非常明亮。

密度较低的结构，如充满空气的肺部，会通过光子并显得较暗。衰减以*Hounsfield单位（HU）*（以CT发明者戈弗雷·侯恩斯菲尔德命名）报告，并进行标准化，使空气的衰减值为-1000HU，水的衰减值为0HU。脂肪组织的典型衰减值为-100到-50HU，血液的衰减值为+30HU，而软组织的衰减值为+10到+50HU。

## **对比材料**

对比剂的目的是突出那些否则很难与周围环境区分开的组织。如果没有对比剂，体内大部分软组织的X射线吸收非常相似，会显得均匀的灰色。

对比剂通常是静脉注射（注射到静脉中）或口服（吞咽），尽管对比剂也已被改编为[所有可能的给药方法](https://radiopaedia.org/articles/routes-of-administration-and-retrieval?lang=us)。口服对比剂用于突出胃、小肠和大肠的胃肠道成像。静脉（IV）对比剂更为常见，应用于多种指示（也可以与口服对比剂结合使用）。

在这两种情况下，对比剂由惰性重金属如碘或钡组成，这些重金属悬浮在液体溶液中。重金属具有非常高的衰减率，优先阻挡X射线能量，在最终CT图像中显得明亮。

## **静脉对比在CT中的表现**

静脉对比度捕捉了血液在体内的流动。最简单的层面上，血液更多的组织会显得更亮，或*增强*。在下面的腹部CT检查中，左侧CT是在注射对比剂前获得的，右侧CT是在注射后获得的。脂肪组织（脂肪）血流少，在两幅图像中表现相似，而肝脏和脾脏则由于接受高血流，在最右侧的图像中显得更亮（更*增强*）。注意最大区别在于主动脉以及肝脏和脾脏的血管。

![](../Images/a82c48cfdd1c59434a5576f103ee8509.png)

无对比（左）和后对比（右）腹部CT。图片来自 [Radiopaedia](https://radiopaedia.org/cases/neuroendocrine-carcinoma-of-gallbladder-metastatic?lang=us)，注释由作者添加。

下面展示了另一个例子，一个来自经历出血性中风的个体的后对比头部CT扫描。扫描中明亮的对比剂区域指示了出血的位置和严重程度。

![](../Images/4f8047363d7e09000b477e1faced9f15.png)

CT血管造影点征标示了中风患者出血的位置和严重程度。图片来自 [Radiopaedia](https://radiopaedia.org/articles/ct-angiographic-spot-sign-intracerebral-haemorrhage?lang=us)。

## **对比*阶段*捕捉了随时间变化的灌注变化**

循环是一个动态过程。考虑将少量对比剂注入手臂的静脉（为了确保注射的时间保持一致，对比剂通过泵注入，[有时压力高达300psi](https://radiopaedia.org/articles/saline-flush-during-contrast-medium-administration?lang=us)）。这种*注射量*的对比剂需要几秒钟才能到达心脏和肺部。从心脏，它将经过主动脉，分支到主要动脉，再通过毛细血管系统进入体内组织，最后通过静脉循环返回心脏。在多个心动周期中，肾脏会开始从血液中滤除对比剂，并将其排泄到膀胱中。

![](../Images/f44e07600d09c288e0a985415b8b5305.png)

一名女性接受对比CT检查。照片来自 [Wikimedia Commons](https://en.wikipedia.org/wiki/Contrast_CT#/media/File:Contrast_CT.jpg)。

CT对比成像利用循环的这一动态特性，通过在注射后一个或多个特定时间点，或*阶段*捕捉图像。

数百种CT成像协议捕捉了这些对比阶段的各种组合。这些阶段对比CT将精确地定时到对比剂注射的时刻，定时会根据扫描目的和要求测试的放射科医师的偏好有所不同。

例如，对比增强特别有用来成像肾脏，肾脏有一个特征性的灌注模式。为了捕捉这一点，影像学协议被定时以捕捉对比剂通过肾脏的精确时刻。我在研究生阶段参与的一个[出版物](https://www.ajronline.org/doi/full/10.2214/AJR.18.20331)是对使用肾脏对比CT训练的分类器的可解释AI技术的验证。

![](../Images/985f12f364a6dc8ae363f0a900e691ad.png)

肾脏影像学的常见阶段。来自一份[在线发布的案例报告](https://epos.myesr.org/posterimage/esr/ecr2020/155283/mediagallery/843411)。

一般而言，在所有指示下，CT对比阶段被简化为4个类别：

1.  **前对比**或**非对比**，体内没有静脉对比剂（检查可能在注射对比剂之前进行，或可能根本没有使用对比剂）。

1.  **动脉期**，通常在注射后15–30秒，对比剂在心脏、主动脉和动脉中进行灌注。

1.  **静脉期**，通常在注射后约60秒，对比剂在组织中进行灌注并返回循环。

1.  **延迟期**或**肾图像期**，通常在注射后120–180秒，对比剂正在被肾脏过滤并从循环中去除。

一项CT研究可能只有一个对比阶段，可能有所有四个阶段，或者任何组合。

## **你需要知道的关于数据科学项目的事项**

首先，要意识到对比剂的使用会影响体内几乎每种组织的外观，除了脂肪组织和游离液体；即使是你可能不预期的组织，如骨头，[基于对比剂的使用，其强度可以变化20–30HU](https://www.ajronline.org/doi/full/10.2214/AJR.16.16744)。

对比剂系列的组合将根据指示和下单检查的放射科医生的偏好而有所不同。协议也会定期更新。这意味着如果你将来自不同患者群体、不同医院或不同扫描仪的检查组合在一起，你的数据集中可能会出现不一致的对比检查组合。

处理这个问题有两种方法。如果你使用CT进行其预期的应用（例如，你在对头部CT的胶质母细胞瘤病例进行建模），请准确确定要包括哪些CT对比阶段，并将其作为选择标准。例如，你可能要求所有胶质母细胞瘤病例必须有**前对比**和**动脉期对比扫描**；如果检查没有这种确切的组合，则将其排除。额外的系列也会被排除。这通常是在讨论研究设计时与放射科医生进行的对话。

如果你使用 CT 进行一种偶发应用，你通常可以更加宽容。例如，我们的骨密度模型可以将脊柱分割出来，是在所有四个对比相位图像上进行训练的。如果病人有多个对比相位序列，我们只需将所有相位上的骨测量值平均即可。我们进行了全面的验证，证明静脉注射对模型准确性没有影响，只对后续密度测量产生轻微影响（误差不超过10%），我们承认这是我们目前方法的局限性。

最后，这可能是最不幸的事实，对比相位通常没有很好的标注。通常在系列描述中用缩写形式表示，但这些可能很难解释。DICOM 头部有一组标签，从[*(0018, 0010) 对比剂注射剂*](https://dicom.innolitics.com/ciods/ct-image/contrast-bolus)开始，但我发现这些报告不一致。我发现的唯一可靠的方法是通过阅读图像本身来准确标记对比相位，并且有 100% 的信心。

CT 对比相位是一个在深度学习的最新突破中有望受益很多的领域。我的同事兼好朋友 Gian Marco Conte 在《放射学杂志》发表了一篇文章，他训练了一个 GAN 来合成脑胶质母细胞瘤缺失的 MRI 序列，类似的方法也可以用于合成 CT 对比相位。

## **作为一个病人...**

虽然我从未进行过 CT 检查，但我个人做过几次静脉注射 MRI，我可以向你保证，使用对比剂是无痛的。在程序开始之前，在你的手臂上放置了一根静脉注射管，在检查过程中，当注射对比剂时，你会感到一阵冷风吹到你的手臂上。有些人声称他们口中有一种金属味，但我个人没有经历过这种感觉。
