# 优化还是架构：如何破解卡尔曼滤波

> 原文：[https://towardsdatascience.com/optimization-or-architecture-how-to-hack-kalman-filtering-c0a21ac7d756?source=collection_archive---------3-----------------------#2023-12-26](https://towardsdatascience.com/optimization-or-architecture-how-to-hack-kalman-filtering-c0a21ac7d756?source=collection_archive---------3-----------------------#2023-12-26)

## 为什么即使在神经网络不如卡尔曼滤波器时，它们可能看起来更好——以及如何修复这个问题并改进你的卡尔曼滤波器

[](https://idogreenberg90.medium.com/?source=post_page-----c0a21ac7d756--------------------------------)[![Ido Greenberg](../Images/887c83b2f367133a6eab98b2bb4716ad.png)](https://idogreenberg90.medium.com/?source=post_page-----c0a21ac7d756--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c0a21ac7d756--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c0a21ac7d756--------------------------------) [Ido Greenberg](https://idogreenberg90.medium.com/?source=post_page-----c0a21ac7d756--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe3afd2470982&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foptimization-or-architecture-how-to-hack-kalman-filtering-c0a21ac7d756&user=Ido+Greenberg&userId=e3afd2470982&source=post_page-e3afd2470982----c0a21ac7d756---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c0a21ac7d756--------------------------------) ·6分钟阅读·2023年12月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc0a21ac7d756&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foptimization-or-architecture-how-to-hack-kalman-filtering-c0a21ac7d756&user=Ido+Greenberg&userId=e3afd2470982&source=-----c0a21ac7d756---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc0a21ac7d756&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foptimization-or-architecture-how-to-hack-kalman-filtering-c0a21ac7d756&source=-----c0a21ac7d756---------------------bookmark_footer-----------)

本文介绍了我们在 NeurIPS 2023 上发表的 [近期论文](https://arxiv.org/abs/2310.00675)。代码可在 [PyPI](https://pypi.org/project/Optimized-Kalman-Filter/) 上获取。

# 背景

卡尔曼滤波器 (KF) 自1960年以来一直是一种备受推崇的序列预测和控制方法。尽管在过去几十年中出现了许多新方法，但 KF 的简单设计使其至今仍然是一种实用、稳健且具有竞争力的方法。1960年的[原始论文](https://asmedigitalcollection.asme.org/fluidsengineering/article-abstract/82/1/35/397706/A-New-Approach-to-Linear-Filtering-and-Prediction)在过去5年内已有[12K次引用](https://scholar.google.com/scholar?as_ylo=2019&hl=en&as_sdt=2005&sciodt=0%2C5&cites=5225957811069312144&scipsc=)。其广泛的应用包括[导航](https://ardupilot.org/dev/docs/extended-kalman-filter.html)、[医疗治疗](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4029942/)、[市场分析](https://medium.com/@bqnnguyen/kalman-filter-and-its-application-in-marketing-analytics-d568def19679)、[深度学习](https://arxiv.org/abs/1901.07860) 甚至[登月](https://www.lancaster.ac.uk/stor-i-student-sites/jack-trainer/how-nasa-used-the-kalman-filter-in-the-apollo-program/)。

![](../Images/6101f1c1b4507de6519d5e1eff447d57.png)

KF 在阿波罗任务中的示意图（图源：作者）

从技术上讲，KF 从一系列噪声观测（例如雷达/相机）中预测系统的状态 *x*（例如航天器位置）。它估计状态的分布（例如位置估计 + 不确定性）。每个时间步，它根据动态模型 *F* 预测下一个状态，并根据动态噪声 ***Q*** 增加不确定性。每个观测，它根据新的观测 *z* 及其噪声 ***R*** 更新状态及其不确定性。

![](../Images/bcbed22dbdc7e8ed4df2368fd38073b1.png)

KF 的单步操作示意图（图源：作者）

# 卡尔曼滤波器还是神经网络？

KF 的预测模型 *F* 是线性的，这有些限制。因此，我们在 KF 的基础上构建了一个高端神经网络。这使我们比标准 KF 获得了更好的预测准确性！这对我们来说真是好事！

为了最终确定我们的论文实验，我们进行了几项消融测试。在其中一个测试中，我们*完全移除了神经网络*，仅仅*优化了内部 KF 参数* *Q* 和 *R*。当这个优化后的 KF 不仅超越了标准 KF，还超越了我那高端的网络时，你可以想象我脸上的表情！同样的 KF 模型，完全相同的60年线性架构，仅通过更改噪声参数的值就变得优越了。

![](../Images/b656bd7fb939d4f32614e8c64a4c9c73.png)

KF、神经 KF (NKF) 和优化 KF (OKF) 的预测误差（图源：我们的论文）

KF 击败神经网络虽然有趣，但对我们的问题来说却是*轶事性的*。更重要的是*方法论*的见解：在扩展测试之前，我们差点就宣称网络优于 KF——仅仅因为我们没有正确地比较这两者。

**信息 1：为了确保你的神经网络确实比KF更好，就像优化网络一样优化KF。**

> **备注——这是否意味着KF比神经网络更好？** 我们确实没有做出这样的普遍性声明。我们的声明是关于方法论的——如果你想比较这两种模型，它们都应当类似地优化。话虽如此，我们确实*从经验上*展示了在多普勒雷达问题中，尽管存在非线性，KF可能表现更好。事实上，这让我难以接受，以至于我输了关于我的神经KF的赌注，花费了许多周的超参数优化和其他技巧。

# 优化卡尔曼滤波器

在比较两个架构时，类似地优化它们。这听起来有些微不足道，对吧？实际上，这个缺陷并非我们研究中的独特问题：在非线性滤波文献中，线性KF（或其扩展EKF）通常作为比较的基准，但很少被优化。事实上，这背后有一个原因：标准KF参数“被认为”已经能够提供最优预测，那么何必再进一步优化呢？

![](../Images/70b7f1594bce4d25619337cda490b4b3.png)

KF参数*Q*和R的标准封闭形式方程。我们关注的是离线数据同时提供状态{x}和观测{z}的设置，因此协方差可以直接计算。其他确定Q和R的方法通常用于其他设置，例如没有{x}数据的情况。

不幸的是，封闭形式方程的最优性在实践中并不成立，因为它依赖于一组相当强的假设，而这些假设在现实世界中很少成立。事实上，在简单的经典低维多普勒雷达问题中，我们发现了不少于4个假设的违背。在某些情况下，这种违背甚至很难察觉：例如，我们模拟了独立同分布的观测噪声——但使用的是球坐标系。一旦转换到笛卡尔坐标系，噪声就不再是独立同分布的了！

![](../Images/1bfdc17cbb11accee8c01189bf3ec11c.png)

多普勒雷达问题中4个KF假设的违背：非线性动力学；非线性观测；不准确的初始化分布；以及非独立同分布的观测噪声。（图片由作者提供）

**信息 2：不要相信KF的假设，因此避免使用封闭形式的协方差估计。相反，按照你的损失函数优化参数——就像任何其他预测模型一样。**

换句话说，在现实世界中，噪声协方差估计不再是优化预测误差的代理。这种目标之间的差异会导致意外的异常。在一次实验中，我们用一个*oracle* KF替代噪声估计，该*oracle*知道系统中的准确噪声。这个*oracle*仍然不如优化后的KF，因为准确的噪声估计不是期望的目标，而是准确的状态预测。在另一次实验中，当KF接收到更多数据时，其表现会*恶化*，因为它实际上追求的是与均方误差（MSE）不同的目标！

![](../Images/3a39e8212534b7ac9b919976d0c46e15.png)

测试误差与训练数据大小的关系。标准KF不仅劣于优化后的KF，而且随着数据的增加而*恶化*，因为其参数并未设置以优化期望的目标。（图像摘自我们的论文）

## 那么，如何优化KF呢？

在标准噪声估计方法用于KF调整的背后，是将KF参数视为噪声的代表。这种观点在某些情况下是有益的。然而，如上所述，为了优化，我们应该“忘记”KF参数的这一角色，只将它们视为模型参数，其目标是损失最小化。这种替代观点也告诉我们*如何*进行优化：就像任何序列预测模型，例如RNN！给定数据，我们只需进行预测，计算损失，反向传播梯度，更新模型，并重复进行。

与RNN的主要区别在于，参数*Q*和*R*以协方差矩阵的形式出现，因此它们应该保持对称且正定。为此，我们使用Cholesky分解将*Q*写成*LL**的形式，并优化*L*的条目。这确保了*Q*在优化更新后仍然保持正定。这个技巧同时适用于*Q*和*R*。

![](../Images/5ffae49da232000912445ea2a510e593.png)

OKF训练过程的伪代码（摘自我们的论文）

在我们所有的实验中，这种优化过程被发现既快速又稳定，因为参数数量比典型神经网络少几个数量级。虽然训练过程容易自己实现，但你也可以使用我们的PyPI包，如[这个](https://github.com/ido90/Optimized-Kalman-Filter/blob/master/example.ipynb)示例所示 :)

# 总结

如下图所示，我们的主要信息是KF假设不可靠，因此我们应该直接优化KF——无论我们是将其作为主要预测模型，还是作为与新方法比较的参考。

我们的简单训练过程可以在PyPI上找到。更重要的是，由于我们的架构与原始KF保持一致，任何使用KF（或扩展KF）的系统都可以通过重新学习参数轻松升级为OKF——无需在推理时增加任何代码行。

![](../Images/298fa1f9a83fa1a5598cb391a7dcc433.png)

我们的范围和贡献总结。由于 KF 假设经常被违反，因此必须直接优化 KF。 （图片来自我们的论文）
