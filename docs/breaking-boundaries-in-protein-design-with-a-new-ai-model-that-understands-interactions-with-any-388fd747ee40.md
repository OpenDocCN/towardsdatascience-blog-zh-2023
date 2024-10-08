# 用一种新的 AI 模型打破蛋白质设计的界限，该模型理解与任何类型分子的相互作用

> 原文：[`towardsdatascience.com/breaking-boundaries-in-protein-design-with-a-new-ai-model-that-understands-interactions-with-any-388fd747ee40`](https://towardsdatascience.com/breaking-boundaries-in-protein-design-with-a-new-ai-model-that-understands-interactions-with-any-388fd747ee40)

## 这一新模型有助于 **扩展 ML 模型在工程蛋白质中所需功能的适用性，通过调整其与任何类型分子的特定相互作用，从而有效影响生物技术和临床应用。**

[](https://lucianosphere.medium.com/?source=post_page-----388fd747ee40--------------------------------)![LucianoSphere (Luciano Abriata, PhD)](https://lucianosphere.medium.com/?source=post_page-----388fd747ee40--------------------------------)[](https://towardsdatascience.com/?source=post_page-----388fd747ee40--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----388fd747ee40--------------------------------) [LucianoSphere (Luciano Abriata, PhD)](https://lucianosphere.medium.com/?source=post_page-----388fd747ee40--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----388fd747ee40--------------------------------) ·阅读时间 5 分钟·2023 年 6 月 21 日

--

![](img/e3fcdb27db7b7b4c76aa9f48fa1aacbb.png)

作者通过编辑 Dall-E-2 生成的概念艺术图“蛋白质工程”（最初使用 [此处](https://medium.com/advances-in-biological-science/how-computer-modeling-simulations-and-artificial-intelligence-impact-protein-engineering-in-4d8473bd59ff)）。

**在 Deepmind 的 AlphaFold 在结构生物学领域引发革命之后，紧密相关的蛋白质设计领域最近通过深度学习的力量进入了一个新的进步时代。然而，现有的蛋白质设计机器学习 (ML) 模型在将非蛋白质实体纳入设计过程中存在局限，只能处理蛋白质组件。在我们的新预印本中，我们介绍了一种新的深度学习模型，“CARBonAra”，该模型考虑了蛋白质周围的任何分子环境，因此能够设计能够结合任何类型分子的蛋白质：药物类似的配体、辅因子、底物、核酸，甚至其他蛋白质。通过利用我们之前的 ML 模型中的几何变换器架构，CARBonAra 从骨架支架中预测蛋白质序列，同时考虑到任何性质的分子施加的约束。这种开创性的方法有助于通过调整与任何类型细胞成分的特定相互作用来扩展 ML 模型在工程蛋白质中所需功能的多样性。**

![](img/ec37d143fa618953a8877814295d4744.png)

方案概述了这个新深度学习模型可以做什么：计算蛋白质设计中氨基酸的概率，从一个目标蛋白质骨架开始，该骨架周围有其他分子（在此处由顶部的绿色分子示例）。图像由作者制作。

# 引言

作为数据科学家，我们不断努力突破可能性的界限。蛋白质设计，即创造具有期望功能和属性的新蛋白质，是一个这样的行动领域；特别是一个在生物学、医学、生命技术和材料科学等多个学科中具有深远影响的领域。尽管基于物理的方法在找到折叠成给定蛋白质结构的氨基酸序列方面取得了进展，但深度学习技术已经成为游戏规则的改变者，显著提高了设计成功率和多样性。

我最近在这里讨论了四种现代机器学习模型用于蛋白质设计和工程：

[](/the-era-of-machine-learning-for-protein-design-summarized-in-four-key-methods-d6f1dac5de96?source=post_page-----388fd747ee40--------------------------------) ## 蛋白质设计中的机器学习时代，总结为四种关键方法

### 由于这些基于人工智能的方法和工具，蛋白质生物技术进入了前所未有的激动人心的时代

[towardsdatascience.com

虽然这些模型在许多蛋白质设计任务中取得了成功，但它们在设计过程中考虑非蛋白质实体的能力有限——它们根本无法处理这些实体，这一限制影响了它们的多样性并缩小了它们的应用范围。

为了克服这一挑战，我们在最新的预印本中介绍了一种名为 CARBonAra 的新模型，该模型通过接受作为输入的目标蛋白质支架和任何类型的相互作用分子来彻底改变蛋白质序列设计。这里是预印本：

[](https://www.biorxiv.org/content/10.1101/2023.06.19.545381v1?source=post_page-----388fd747ee40--------------------------------) [## 面向上下文的几何深度学习用于蛋白质序列设计

### 蛋白质设计和工程在利用深度学习的进展方面正以空前的速度发展。当前……

[www.biorxiv.org](https://www.biorxiv.org/content/10.1101/2023.06.19.545381v1?source=post_page-----388fd747ee40--------------------------------)

CARBonAra 基于我们的蛋白质结构变换器（PeSTo），一种几何变换器架构，该架构处理原子点云，忽略原子类型以元素名称直接表示分子。我之前详细描述了 PeSTo：

[详细了解新预印本](https://towardsdatascience.com/new-preprint-describes-a-novel-parameter-free-geometric-transformer-of-atomic-coordinates-to-c6545150855e?source=post_page-----388fd747ee40--------------------------------) [## 新预印本描述了一种新型的无参数几何变换器，用于…

### 并且运行速度如此之快，以至于它甚至可以扫描大量蛋白质结构以搜索易于交互的氨基酸…

[详细了解新预印本](https://towardsdatascience.com/new-preprint-describes-a-novel-parameter-free-geometric-transformer-of-atomic-coordinates-to-c6545150855e?source=post_page-----388fd747ee40--------------------------------)

CARBonAra 的核心基于 PeSTo 模型，使其能够将任何种类的非蛋白质分子，包括核酸、脂质、离子、小配体、辅因子或其他蛋白质，纳入新蛋白质的设计过程中。因此，给定一个带有一个或多个配体的输入蛋白质结构，CARBonAra 预测氨基酸的残基信心，通过这些信心的最大值可以重建蛋白质序列。为此，CARBonAra 以骨架作为输入，并生成潜在序列的空间，这些序列可以通过特定的功能或结构要求进一步约束——例如，固定某些氨基酸，若它们对某一功能至关重要。CARBonAra 通过考虑蛋白质的分子背景，为蛋白质设计提供了前所未有的灵活性和深度，这意味着它可以为绑定离子、底物、核酸、脂质、其他蛋白质等特定区域进行设计。

在我们的评估中，CARBonAra 的表现与诸如 ProteinMPNN 和 ESM-IF1 等最先进的方法相当，同时展示了类似的计算效率——都非常快速。该模型在设计蛋白质单体和蛋白质复合物方面实现了与 ProteinMPNN 和 ESM-IF1 相似的序列恢复率，但除此之外，它还能处理包含非蛋白质分子的蛋白质设计，这是其他方法无法处理的。

CARBonAra 的一个显著特点是其能够通过整合各种约束来定制序列以满足特定目标。例如，它可以优化序列相似性、最小化相似度或实现低序列相似性。此外，通过利用 CARBonAra 与分子动力学模拟的结构轨迹，我们观察到可以提高序列恢复率，特别是在以前的方法显示成功率较低的情况下。

要了解更多关于该方法，特别是 ML 架构的细节，请查看我们在 bioRxiv 上的预印本：

[相关文献](https://www.biorxiv.org/content/10.1101/2023.06.19.545381v1?source=post_page-----388fd747ee40--------------------------------) [## 上下文感知的几何深度学习用于蛋白质序列设计

### 蛋白质设计和工程正在以空前的速度发展，利用深度学习的进步。目前…

[www.biorxiv.org](https://www.biorxiv.org/content/10.1101/2023.06.19.545381v1?source=post_page-----388fd747ee40--------------------------------)

# 一些与结构生物学中的人工智能相关的文章

[](https://medium.com/advances-in-biological-science/over-one-year-of-alphafold-2-free-for-everyone-to-use-and-of-the-revolution-it-triggered-in-biology-f12cac8c88c6?source=post_page-----388fd747ee40--------------------------------) [## 超过一年 AlphaFold 2 免费使用及其在生物学中引发的革命

### 自信地建模蛋白质结构，预测它们与其他生物分子的相互作用，甚至蛋白质…

[medium.com](https://medium.com/advances-in-biological-science/over-one-year-of-alphafold-2-free-for-everyone-to-use-and-of-the-revolution-it-triggered-in-biology-f12cac8c88c6?source=post_page-----388fd747ee40--------------------------------) [](https://javascript.plainenglish.io/a-web-app-to-design-stable-proteins-via-the-consensus-method-created-with-javascript-esmfold-and-d319d2441ae7?source=post_page-----388fd747ee40--------------------------------) [## 通过共识方法设计稳定蛋白质的网络应用程序，使用 JavaScript、ESMFold 创建…

### 结合现代技术和工具进行高效工作，创建一个实现最简单但如今最有效的应用程序…

[javascript.plainenglish.io](https://javascript.plainenglish.io/a-web-app-to-design-stable-proteins-via-the-consensus-method-created-with-javascript-esmfold-and-d319d2441ae7?source=post_page-----388fd747ee40--------------------------------) [](/ml-everything-balancing-quantity-and-quality-in-machine-learning-methods-for-science-baa0477f5ac9?source=post_page-----388fd747ee40--------------------------------) ## “ML-Everything”？平衡科学中机器学习方法的数量和质量

### 需要适当的验证和良好的数据集，客观且平衡，并且预测在现实中有用…

[towardsdatascience.com [](/how-huge-protein-language-models-could-disrupt-structural-biology-6b98193f880b?source=post_page-----388fd747ee40--------------------------------) ## 巨大的蛋白质语言模型如何颠覆结构生物学

### 结构预测的准确性与 AlphaFold 相似，但速度提高了高达 60 倍——同时开发了新的人工智能方法…

[towardsdatascience.com

[***www.lucianoabriata.com***](https://www.lucianoabriata.com/) *我写作和拍摄的内容涵盖了我广泛兴趣领域中的一切：自然、科学、技术、编程等*

[***在这里给我小费***](https://lucianoabriata.altervista.org/office/donations.html)或者[***成为 Medium 会员***](https://lucianosphere.medium.com/membership)*以获取所有它的故事（我将获得少量收入，你无需付费）*。[***订阅以获取我的新故事***](https://lucianosphere.medium.com/subscribe)***通过电子邮件****。***[***在我的服务页面上咨询有关小型工作***](https://lucianoabriata.altervista.org/services/index.html)*。您可以* [***在这里联系我***](https://lucianoabriata.altervista.org/office/contact.html)***。***
