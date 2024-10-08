# 神经原型树

> 原文：[`towardsdatascience.com/neural-prototype-trees-f7bac36437a9`](https://towardsdatascience.com/neural-prototype-trees-f7bac36437a9)

## 通过模仿人类推理来实现可解释的图像分类。

[](https://medium.com/@upadhyan?source=post_page-----f7bac36437a9--------------------------------)![Nakul Upadhya](https://medium.com/@upadhyan?source=post_page-----f7bac36437a9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f7bac36437a9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f7bac36437a9--------------------------------) [Nakul Upadhya](https://medium.com/@upadhyan?source=post_page-----f7bac36437a9--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f7bac36437a9--------------------------------) ·6 分钟阅读·2023 年 6 月 2 日

--

机器学习和人工智能现在被应用于大量领域，但随着使用的增加，模型面临着更多的风险和伦理测试。

让我们通过最近的新闻来激发思考，关于[一辆特斯拉在自动驾驶模式下撞上树木的事件](https://upnorthlive.com/news/local/woman-recovering-after-tesla-crashes-in-self-driving-mode)。根据当局的说法，司机表示车辆在她启用自动驾驶模式后向右偏移，驶离了道路，并撞上了一棵树。目前，这些说法正在调查中，但想象一下，识别汽车突然做出怪异决策的原因是多么困难。它是否发生了误分类？它看到了什么让它困惑的东西？对于传统的黑箱模型来说，调查模型内部非常困难且昂贵。

那么，替代方案是什么？是否有一种可解释的图像分类方法？是的，通过原型学习[2]和神经原型树[1]！借助这些架构，模型采用了一种非常直观的预测方法：识别看起来熟悉的部分。那只鸟有长长的喙吗？它有红色的喉咙吗？那一定是一只蜂鸟！

在本文中，我旨在提供有关这些模型如何工作的资讯，并讨论使用这些模型的一些优缺点。我将频繁引用的两篇主要论文如下，我强烈建议有兴趣的读者阅读这些论文：

[](https://arxiv.org/abs/1806.10574?source=post_page-----f7bac36437a9--------------------------------) [## 这看起来像那样：用于可解释图像识别的深度学习

### 当我们面临具有挑战性的图像分类任务时，我们通常通过解构图像来解释我们的推理……

[arxiv.org](https://arxiv.org/abs/1806.10574?source=post_page-----f7bac36437a9--------------------------------) [openaccess.thecvf.com](https://openaccess.thecvf.com/content/CVPR2021/html/Nauta_Neural_Prototype_Trees_for_Interpretable_Fine-Grained_Image_Recognition_CVPR_2021_paper.html?ref=https%3A%2F%2Fgithubhelp.com&source=post_page-----f7bac36437a9--------------------------------) [## CVPR 2021 开放获取库

### 神经原型树用于可解释的细粒度图像识别 Meike Nauta、Ron van Bree、Christin Seifert 等

[openaccess.thecvf.com](https://openaccess.thecvf.com/content/CVPR2021/html/Nauta_Neural_Prototype_Trees_for_Interpretable_Fine-Grained_Image_Recognition_CVPR_2021_paper.html?ref=https%3A%2F%2Fgithubhelp.com&source=post_page-----f7bac36437a9--------------------------------)

这是我撰写的关于神经决策树变体的第二篇文章。如果你还没有读过第一篇文章，我强烈建议你浏览一下，因为这里阐述的许多概念都是基于标准神经树构建的。

towardsdatascience.com ## 神经网络作为决策树

### 利用神经网络的强大能力和决策树的可解释结构

[towardsdatascience.com

# 什么是原型？

图像识别中的原型思想首次由 Chen & Li 等（2019）在他们的论文“这看起来像那样：用于可解释图像识别的深度学习” [2] 中提出。这是一种潜在表示，表示与给定类别相关的某些训练图像补丁。正如名字所示，该模型通过解剖输入图像并找到提供证据的原型部分来工作，以表明图像属于某一类别。网络简单地计算欧几里得距离，并将其反转以创建相似度分数。这些分数随后通过一个全连接层生成最终的类别概率：

![](img/fe94c79b2ad077b6e29b0a0ecaac7d02.png)

图 1: ProtoPNet 架构（图源自 Chen & Li 等，2019 [2]）

一旦模型训练完成，用户可以简单地将学到的原型与训练集中的补丁进行匹配，从而创建对任何预测的非常可解释的解释：

![](img/ec5c309129dd9c0966fb055207056740.png)

图 2: 鸟类预测的推理过程（图源自 Chen & Li 等，2019 [2]）

# 神经原型树

使用原型包模型（例如来自原始原型论文 [2] 的 ProtoPNet）的一个问题是，原型匹配是同时进行的，但人类图像识别依赖于一系列步骤。如果某物没有爪子或耳朵但有尾巴，那它可能不是猫，因此网络不应该给它打上猫的标签。这就是神经原型树 [2] 发挥作用的地方。与原型包不同，Nauta 等人 [1] 选择将他们的模型设计为神经决策树。这种决策树提供了这种顺序决策，并提供了全局可解释性，而不仅仅是局部可解释性。

## 软决策树的回顾

神经原型树是软决策树，而不是硬决策树。虽然硬决策树强制执行确定性分支（你要么向左走，要么向右走），但软决策树使用的是概率分支（你有 *p* 的概率向左走和 *1-p* 的概率向右走）。此外，虽然硬决策树输出一个单一的值，但软决策树输出的是所有可能类别的概率分布，其中类别的概率是到达叶子节点所经过概率的乘积，分类决策则是概率最高的类别。

## 原型树中的决策制定

像标准神经树一样，每个叶子节点包含一个关于类别的概率分布。分支决策是通过计算图像补丁到节点中给定原型的距离来做出的。每张图像的得分是图像中补丁与原型之间找到的最小距离，然后转换为概率。简单来说：如果在图像中找到原型，就向右走；如果找不到该原型，就向左走！

![](img/56af0995f2165aec9fd71586e3222bfa.png)

图 3：ProtoTree 中的预测机制（图源自 Nauta 等人 2021 [1]）

这个机制显然允许我们使用与可视化普通原型模型相同的机制来可视化出极其可解释的模型。

![](img/7f55a06009e874700897d9319d72c417.png)

图 4：原型树的全局解释（图源自 Nauta 等人 2021 [1]）

## 学习叶子分布

在普通决策树中，叶子的标签是通过查看最终到达该叶子的样本来学习的，但在软树中，叶子中的分布是全局学习问题的一部分。然而，作者注意到将叶子的学习与原型的学习结合在一起会导致分类结果不佳。为了解决这个问题，他们利用了一种无导数策略来获取叶子概率的更新方案：

![](img/b11b18cfe6c621a6338f729caf3ffc16.png)

图 5：更新方案。c_l^t 是第 t 轮中叶子 l 的叶子概率。y 是真实值，yhat 是预测值。pi 是到该叶子的路径概率。（图自 Nauta 等，2021 [1]）

该更新方案与小批量梯度相结合，以学习原型和卷积参数，从而创建一个高效的学习过程。

## 剪枝

为了提高可解释性，作者还引入了剪枝机制。如果一个叶节点包含有效的均匀分布，它的区分能力不强，因此最好对其进行剪枝，因为较小的树更易于阅读和解释。从数学上讲，作者定义了一个阈值 *t* 并移除所有最高类别概率小于 *t*（*max*(c_l) ≤ t*）的叶子。如果一个子树中的所有叶子都被移除，则可以移除该子树及其相关原型，从而使树变得更加紧凑。通常，*t = 1/K + epsilon* 其中 K 是类别数，*epsilon* 是一个非常小的数值，表示容差。

![](img/371d42f0b2fe698980f1fbb4435aad3c.png)

图 5：剪枝可视化（图自 Nauta 等，2021 [1]）

## 性能

![](img/8fea1e7c77b79fd2edeab2d7dd74a0c8.png)

图 6：平均准确率和标准差。ProtoTree ens. 是 3 棵或 5 棵原型树的集合。 （图自 Nauta 等，2021 [1]）

作者使用 CARS 和 CUBS 数据集对他们的方法进行了基准测试，与其他可解释的图像识别方法（如基于注意力的可解释性方法）进行比较。他们发现，通过使用相对较小的树木集合（9 棵和 11 棵），他们能够接近 SOTA 准确率。

# 结论

可解释的深度学习图像分类器相对于黑箱模型提供了许多优势。它们可以帮助建立信任，改善调试，并解释预测。此外，它们还可以用于探索数据，了解不同特征之间的关系。

总的来说，神经原型树是一种有前途的新方法，用于以可信赖的方式进行图像识别。如果医生能够检查模型所观察到的图像的特征，他更可能相信癌症检测模型。这些原型树甚至可以通过添加注意力等措施进一步提高准确性！

# 资源和参考文献

1.  神经原型树的 Github： [`github.com/M-Nauta/ProtoTree`](https://github.com/M-Nauta/ProtoTree)

1.  如果你对可解释的机器学习和人工智能感兴趣，可以考虑关注我：[`medium.com/@upadhyan`](https://medium.com/@upadhyan)。

## 参考文献

[1] M. Nauta, R.v. Bree, C. Seifert. [神经原型树用于可解释的细粒度图像识别](https://openaccess.thecvf.com/content/CVPR2021/html/Nauta_Neural_Prototype_Trees_for_Interpretable_Fine-Grained_Image_Recognition_CVPR_2021_paper.html?ref=https%3A%2F%2Fgithubhelp.com) (2021). IEEE/CVF 计算机视觉与模式识别会议（CVPR），2021

[2] C. Chen, O. Li, C. Tao. A.J. Barnett, J. Su, C. Rudin. [这看起来像那样：可解释图像识别的深度学习](https://arxiv.org/abs/1806.10574) (2019). 第 33 届神经信息处理系统会议。
