# 蛋白质设计的机器学习时代，概括为四种关键方法

> 原文：[`towardsdatascience.com/the-era-of-machine-learning-for-protein-design-summarized-in-four-key-methods-d6f1dac5de96`](https://towardsdatascience.com/the-era-of-machine-learning-for-protein-design-summarized-in-four-key-methods-d6f1dac5de96)

## 感谢这些基于人工智能的方法和工具，蛋白质生物技术迎来了如此激动人心的时代

[](https://lucianosphere.medium.com/?source=post_page-----d6f1dac5de96--------------------------------)![LucianoSphere (Luciano Abriata, PhD)](https://lucianosphere.medium.com/?source=post_page-----d6f1dac5de96--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d6f1dac5de96--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d6f1dac5de96--------------------------------) [LucianoSphere (Luciano Abriata, PhD)](https://lucianosphere.medium.com/?source=post_page-----d6f1dac5de96--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d6f1dac5de96--------------------------------) ·阅读时间 9 分钟·2023 年 5 月 5 日

--

![](img/71a78023a81209822864b580563b95a3.png)

图由作者使用 Dall-E-2 和自定义编辑创建。

蛋白质设计和工程是分子生物学中的重要目标，具有广泛的应用，包括医学、生物技术和材料科学等领域。科学家们已经探索了几十种设计新型蛋白质和工程现有蛋白质以微调其性质的方法。尽管基于物理的方法在找到折叠成特定蛋白质结构的氨基酸序列方面取得了一些成功，但深度学习方法的最新发展显示出了更高的成功率和多样性。在这篇文章中，我将概述四种值得注意的机器学习（ML）工具，用于蛋白质设计和工程，并探讨它们在推动该领域发展中的重要性。

除了这些工具在化学和生物科学领域立即产生的影响之外，它们引入的方法甚至这些项目本身也为数据科学家、机器学习从业者和人工智能研究人员提供了令人兴奋的机会，让他们可以与化学和生物科学家合作，提出新想法和方法，最终将计算机科学应用于有益的领域。实际上，以下我要讨论的工具展示了应用不同种类的深度学习算法来解决生物技术中的复杂挑战的强大能力。通过利用这些工具，数据科学、机器学习和人工智能领域的专业人士也可以为医学、生物技术和材料科学的进步做出贡献，亲眼见证自己领域的直接影响，即使在其领域之外！

简而言之，我将按发布顺序介绍称为 ProteinMPNN、ESM2-InverseFold、RoseTTaFold Diffusion 和 MASIF-Seed 的工具。重要的是，这些模型都在 Deepmind 的 AlphaFold 模型对结构生物学领域产生巨大影响之后开始出名：

[](https://medium.com/advances-in-biological-science/over-one-year-of-alphafold-2-free-for-everyone-to-use-and-of-the-revolution-it-triggered-in-biology-f12cac8c88c6?source=post_page-----d6f1dac5de96--------------------------------) [## 一年多的 AlphaFold 2 免费使用及其在生物学中引发的革命

### 蛋白质结构的自信建模、预测其与其他生物分子的相互作用，甚至蛋白质…

medium.com](https://medium.com/advances-in-biological-science/over-one-year-of-alphafold-2-free-for-everyone-to-use-and-of-the-revolution-it-triggered-in-biology-f12cac8c88c6?source=post_page-----d6f1dac5de96--------------------------------)

# ProteinMPNN

ProteinMPNN，由贝克实验室开发，是第一个获得实验测试设计蛋白质的机器学习工具。

这个模型基于编码器-解码器神经网络，是第一个显示生成经过实验验证能按预期折叠的蛋白质序列的工具。两篇论文，“基于深度学习的蛋白质序列设计使用 ProteinMPNN”和“幻想对称蛋白质组装”，发表于*Science*期刊的 2022 年底，展示了该方法（前者）和该工具在各种蛋白质设计问题中的适用性（后者）。

我专门写了一篇关于 ProteinMPNN 的博客文章，尽管这篇文章已经算“旧”（尽管发布不到一年，展示了领域发展的速度！）。所以我不会在这里多讲，你可以查看我之前的文章：

[](/new-deep-learned-tool-designs-novel-proteins-with-high-accuracy-41ae2a7d23d8?source=post_page-----d6f1dac5de96--------------------------------) ## 新的深度学习工具高精度设计新型蛋白质

### 贝克实验室的这款新软件设计的蛋白质在湿实验室中实际有效。你可以用它来…

towardsdatascience.com

# ESM-InverseFold

由 Meta 开发的 ESM2-InverseFold 基于 ESMFold 蛋白质语言模型，但其设计目的是从结构生成蛋白质序列，而不是从序列预测结构。

发现 ESMFold 能生成在已知自然序列之外高度多样的蛋白质序列。预印本《语言模型超越自然蛋白质的泛化》描述了其核心功能，并展示了几个成功的设计例子。

要了解更多关于 ESMFold 的信息，请查看我之前的文章：

[](/how-huge-protein-language-models-could-disrupt-structural-biology-6b98193f880b?source=post_page-----d6f1dac5de96--------------------------------) ## 巨大的蛋白质语言模型如何扰乱结构生物学

### 结构预测的准确性与 AlphaFold 相似，但速度快达 60 倍，并且开发了新的 AI 方法。

[towardsdatascience.com

这是基于此的蛋白质设计工具“ESM-InverseFold”的预印本：

[](https://www.biorxiv.org/content/10.1101/2022.12.21.521521v1?source=post_page-----d6f1dac5de96--------------------------------) [## 语言模型超越自然蛋白质的一般化

### 从进化过程中序列中学习蛋白质设计模式可能对生成性蛋白质有潜力。

[www.biorxiv.org](https://www.biorxiv.org/content/10.1101/2022.12.21.521521v1?source=post_page-----d6f1dac5de96--------------------------------)

ESM-InverseFold 是一个蛋白质设计工具，利用机器学习生成前所未见的新蛋白质。该工具基于语言模型，这些模型通过掩码语言建模在进化过程中对数百万种不同的自然蛋白质进行了训练。这些模型生成的基序将序列与结构设计关联，并能在新的序列和结构背景中应用它们。ESM-InverseFold 提供了两种生成性蛋白质设计任务：固定骨架设计和自由生成。固定骨架设计通过从语言模型指定的条件分布中使用马尔科夫链蒙特卡洛法和模拟退火来生成蛋白质序列。自由生成完全移除结构上的约束，通过从语言模型指定的序列和结构的联合分布中进行采样来生成新蛋白质。ESM-InverseFold 显示出高实验成功率，在 67%的评估蛋白质中通过尺寸排除色谱法产生了可溶解且单体化的物种。正如作者所示，该工具使用的语言模型能够访问超越自然蛋白质的设计空间，基于蛋白质设计的深层模式生成新颖的解决方案，包括自然蛋白质中的结构基序。

# RoseTTAFold Diffusion

基于扩散模型的 RoseTTAFold Diffusion 是贝克实验室最新的工具，也已在 bioRxiv 上预印。

[](https://www.biorxiv.org/content/10.1101/2022.12.09.519842v2?source=post_page-----d6f1dac5de96--------------------------------) [## 广泛适用且准确的蛋白质设计，通过整合结构预测网络和…

### 最近在使用深度学习方法设计新蛋白质方面取得了相当大的进展[1][1]-[9][2]。尽管…

[www.biorxiv.org](https://www.biorxiv.org/content/10.1101/2022.12.09.519842v2?source=post_page-----d6f1dac5de96--------------------------------)

来自 Baker 实验室博客，目前 Rosetta 套件中表现最好的蛋白质设计方法是：

[## 蛋白质设计的扩散模型](https://www.bakerlab.org/2022/11/30/diffusion-model-for-protein-design/?source=post_page-----d6f1dac5de96--------------------------------)

### 由 Baker 实验室科学家 Joseph Watson、David Juergens、Nate Bennett、Brian Trippe 和 Jason Yim 领导的团队创建了…

[www.bakerlab.org](https://www.bakerlab.org/2022/11/30/diffusion-model-for-protein-design/?source=post_page-----d6f1dac5de96--------------------------------)

RoseTTaFold Diffusion 是一种基于去噪扩散概率模型的生成模型，利用深度学习从简单的分子规格生成多样、复杂且功能性的蛋白质。它通过在蛋白质结构去噪任务上微调 RoseTTaFold 结构预测网络，获得蛋白质主链的生成模型。RoseTTaFold Diffusion 通过模拟在训练过程中从蛋白质数据银行中采样的结构的噪声过程生成蛋白质结构。该方法通过将先前步骤中的噪声坐标转化为预测结构来生成新的蛋白质结构，这些预测结构以模型的输入为条件，这些输入可以包括部分序列、折叠信息或固定功能基序坐标。该方法使用两种不同策略进行训练：1）类似于“经典”扩散模型的方式，在每个时间步的预测独立于前一个时间步的预测；2）具有自我条件化，模型可以在时间步之间条件化先前的预测。RoseTTaFold Diffusion 可以在没有额外输入的情况下生成蛋白质结构，也可以通过对各种输入进行条件化来生成蛋白质结构，并且能够生成与任何已知蛋白质结构具有较少总体结构相似性的多样蛋白质结构。该方法在蛋白质结构生成方面优于其他深度学习方法，并且在广泛的设计挑战中表现出最先进的性能，包括蛋白质单体设计、蛋白质结合体设计、对称寡聚体设计、酶活性位点支架设计以及治疗性和金属结合蛋白质设计的对称基序支架设计。

# MaSIF-seed

MaSIF-seed，由[Michael Bronstein](https://medium.com/u/7b1129ddd572?source=post_page-----d6f1dac5de96--------------------------------)实验室和我的机构（[EPFL 扩展学院](https://medium.com/u/b91d50f7c43c?source=post_page-----d6f1dac5de96--------------------------------)）的 Correia 实验室共同合作，并于本月在*Nature*上发表，专注于通过学习的蛋白质表面指纹设计蛋白质相互作用：

[## De novo design of protein interactions with learned surface fingerprints - Nature](https://www.nature.com/articles/s41586-023-05993-x?source=post_page-----d6f1dac5de96--------------------------------)

### 蛋白质之间的物理相互作用对于大多数生命过程至关重要。然而，...

[## De novo design of protein interactions with learned surface fingerprints - Nature](https://www.nature.com/articles/s41586-023-05993-x?source=post_page-----d6f1dac5de96--------------------------------)

该工具在设计蛋白质单体和聚合物方面表现出色，包括目标结合蛋白质和自然界中未见的结构。它基于自身团队的前期工作 Masif，一种从表面特征预测相互作用的机器学习工具。

与其他方法相比，Masif-seed 采用以表面为中心的方法，专注于蛋白质的表面特性及表面斑点之间的相互作用。它的神经网络输出向量指纹描述符，这些描述符在相互作用的蛋白质对的斑点之间是互补的，而在非相互作用的对之间则不同。匹配的表面斑点与目标位点对齐，并用第二个神经网络进行评分，该网络输出界面后对齐分数，以进一步提高表面描述符的区分性能。与其他工具相比，MaSIF-seed 在基于丰富的表面特征区分真实结合物和诱饵方面表现出色。此外，它比其他方法假设上更快且更准确。

介绍该方法的论文描述了多个使用该工具设计全新蛋白质结合物以接触具有挑战性和与疾病相关的蛋白质靶点的例子。使用 MaSIF-seed 的完整蛋白质设计流程包括几个步骤，从识别具有高结合倾向的蛋白质靶点位点开始，然后从源自片段的表面指纹数据库中搜索子集，以寻找可能靶向所选位点的结合种子，然后使用专门的 Rosetta 协议将其移植到与种子的结合模式兼容的蛋白质支架上。最后，优化结合物界面，并在实际应用中，通过突变库实验筛选设计，以微调最终序列。

# 设计蛋白质序列以折叠并按科学家的需求工作

在所有这四种工具中，模型的输入是一个骨架结构，可能有某些氨基酸身份被限制，模型会在此基础上制作预期按设计折叠的蛋白质序列。虽然这些模型可以生成相互作用的蛋白质序列，但它们无法在设计过程中原生考虑非蛋白质分子。这一限制阻碍了它们在涉及与非蛋白质分子结合的设计中的应用，除非用户根据所需功能手动修正某些残基。尽管这种策略有些低效，因为需要了解感兴趣的系统，但它已在 2023 年初由贝克实验室在酶的设计中取得了成功：

[## 从头设计萤光素酶使用深度学习 - Nature](https://www.nature.com/articles/s41586-023-05696-3?source=post_page-----d6f1dac5de96--------------------------------)

### 从头设计酶的目标是引入预测能催化反应的活性位点和底物结合口袋…

[www.nature.com](https://www.nature.com/articles/s41586-023-05696-3?source=post_page-----d6f1dac5de96--------------------------------)

就像那个例子一样，这些工具的发展为设计新型蛋白质和工程改造现有蛋白质开辟了激动人心的可能性。这些工具在药物开发、材料科学和生物技术领域特别有用，可以将蛋白质的性质精细调节以满足特定需求。生成经过实验验证的按预期折叠的蛋白质序列的能力，对新治疗方法和疗法的开发具有巨大意义，特别是对于复杂疾病。例如，请参见[这种特殊类型的类似疫苗的制剂](https://www.science.org/doi/10.1126/science.aay5051)，它由计算机设计的蛋白质表位混合而成——目前使用的是更传统的物理工具。

此外，这些工具有可能显著减少蛋白质设计和工程所需的时间和资源，使这一研究领域更加可及。它们也更容易部署和运行，这进一步帮助了其使用的民主化。实际上，看看你可以多么轻松地将常规 ESMFold 适配为分析现实蛋白质设计，例如来自 HuggingFace 上运行的 ProteinMPNN，仅在你的网页浏览器中： 

[## 通过共识方法设计稳定蛋白质的网页应用，使用 JavaScript、ESMFold 创建…](https://javascript.plainenglish.io/a-web-app-to-design-stable-proteins-via-the-consensus-method-created-with-javascript-esmfold-and-d319d2441ae7?source=post_page-----d6f1dac5de96--------------------------------)

### 融合现代技术和工具进行高效工作，创建一个实现最简单但如今最…

javascript.plainenglish.io](https://javascript.plainenglish.io/a-web-app-to-design-stable-proteins-via-the-consensus-method-created-with-javascript-esmfold-and-d319d2441ae7?source=post_page-----d6f1dac5de96--------------------------------)

总结来说，我们可以毫不犹豫地声明，在经历了 AlphaFold 的蛋白质结构预测炒作之后，我们现在正处于蛋白质设计的炒作浪潮中，每月都有新方法出现，而我在这里介绍了我认为目前最相关的四种方法——主要因为它们都经过了实验验证。

这些新的蛋白质设计模型展示了令人印象深刻的结果，并且无疑将在不久的将来成为蛋白质生物技术实验室和公司的重要组成部分。尽管仍存在限制，这些工具的潜在应用巨大，预计在未来几年将对医学、生物技术和材料科学产生重大影响。

# 相关文章

了解计算机建模、模拟和人工智能如何影响蛋白质工程，请查看此内容：

[](https://medium.com/advances-in-biological-science/how-computer-modeling-simulations-and-artificial-intelligence-impact-protein-engineering-in-4d8473bd59ff?source=post_page-----d6f1dac5de96--------------------------------) [## 计算机建模、模拟和人工智能如何影响蛋白质工程的…

### 概述了不同复杂度、成功率和应用的计算方法，并指向关键…

medium.com](https://medium.com/advances-in-biological-science/how-computer-modeling-simulations-and-artificial-intelligence-impact-protein-engineering-in-4d8473bd59ff?source=post_page-----d6f1dac5de96--------------------------------)

在这篇文章中，我探讨了为什么蛋白质设计/工程问题如此困难，即便是针对单一残基：

[## 论文总结：为什么预测稳定性变化如此困难，当一种…

### Louis 和 Abriata。分子生物技术 2021 [开放访问这里]

lucianosphere.medium.com](https://lucianosphere.medium.com/why-is-it-so-difficult-to-predict-the-changes-in-stability-that-result-when-a-protein-is-mutated-2df96b2037c5?source=post_page-----d6f1dac5de96--------------------------------)

你可能还会对我关于在科学中平衡机器学习质量和数量的文章感兴趣，其中我特别讨论了与蛋白质设计的 ML 模型相关的内容：

[](/ml-everything-balancing-quantity-and-quality-in-machine-learning-methods-for-science-baa0477f5ac9?source=post_page-----d6f1dac5de96--------------------------------) ## “ML-一切”？在科学中平衡机器学习方法的数量和质量

### 对于适当的验证和良好的数据集的需求，客观和均衡，以及预测在现实中有用的…

[towardsdatascience.com

[***www.lucianoabriata.com***](https://www.lucianoabriata.com/) *我撰写并拍摄关于我广泛兴趣领域的一切内容：自然、科学、技术、编程等。* [***成为 Medium 会员***](https://lucianosphere.medium.com/membership) *以访问所有故事（平台的会员链接，您无需付费即可获得我少量收入）并* [***订阅以通过电子邮件获取我的新故事***](https://lucianosphere.medium.com/subscribe) ***。要* ***咨询关于小项目的事宜，*** *请查看我的* [***服务页面***](https://lucianoabriata.altervista.org/services/index.html)*。您可以* [***在这里联系我***](https://lucianoabriata.altervista.org/office/contact.html)***。***
