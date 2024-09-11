# 期望校准误差（ECE）：逐步可视化解释

> 原文：[https://towardsdatascience.com/expected-calibration-error-ece-a-step-by-step-visual-explanation-with-python-code-c3e9aa12937d?source=collection_archive---------0-----------------------#2023-07-12](https://towardsdatascience.com/expected-calibration-error-ece-a-step-by-step-visual-explanation-with-python-code-c3e9aa12937d?source=collection_archive---------0-----------------------#2023-07-12)

## 通过一个简单的示例和 Python 代码

[](https://medium.com/@majapavlo?source=post_page-----c3e9aa12937d--------------------------------)[![Maja Pavlovic](../Images/a3b8a94c236519bc86c5c6319db5bc66.png)](https://medium.com/@majapavlo?source=post_page-----c3e9aa12937d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c3e9aa12937d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c3e9aa12937d--------------------------------) [Maja Pavlovic](https://medium.com/@majapavlo?source=post_page-----c3e9aa12937d--------------------------------)

·

[查看](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9b1766e00cb4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexpected-calibration-error-ece-a-step-by-step-visual-explanation-with-python-code-c3e9aa12937d&user=Maja+Pavlovic&userId=9b1766e00cb4&source=post_page-9b1766e00cb4----c3e9aa12937d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c3e9aa12937d--------------------------------) ·8分钟阅读·2023年7月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc3e9aa12937d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexpected-calibration-error-ece-a-step-by-step-visual-explanation-with-python-code-c3e9aa12937d&user=Maja+Pavlovic&userId=9b1766e00cb4&source=-----c3e9aa12937d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc3e9aa12937d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexpected-calibration-error-ece-a-step-by-step-visual-explanation-with-python-code-c3e9aa12937d&source=-----c3e9aa12937d---------------------bookmark_footer-----------)![](../Images/94b66e02fd61acdc004df7270bcd8aee.png)

作者提供的图片

在分类任务中，机器学习模型输出的是**估计概率或*也称为置信度*** *(见上图)*。这些值告诉我们模型在其标签预测中的确定性。然而，对于大多数模型，这些置信度与它们预测的事件的真实频率并不一致。它们需要***校准***！

**模型校准**旨在将模型的预测与真实概率对齐，从而确保模型的预测是***可靠且准确***的（*有关模型校准重要性的更多细节，请参见* [*博客文章*](https://www.unofficialgoogledatascience.com/2021/04/why-model-calibration-matters-and-how.html) *）*。

好的，既然模型校准很重要，那么我们如何衡量它呢？有几个选项，但本文的**目的和重点**是解释并仅介绍一种***简单的*** *但相对充分的* ***测量方法***来评估模型校准：**期望校准误差（ECE）**。它计算估计“概率”的加权平均误差，从而得出一个单一值，我们可以用来比较不同的模型。

我们将按照论文中描述的 ECE 公式进行演示：[*现代神经网络的校准*](https://arxiv.org/pdf/1706.04599.pdf)*。为了简单起见，我们将看一个包含 9 个数据点和二进制目标的小示例。然后，我们还将在 **Python** 中编写这个简单示例的**代码**，最后展示一个多类分类的示例代码。

## 定义

ECE（期望校准误差）衡量了模型估计的“概率”与真实（观察到的）概率的匹配程度，通过对准确性*(acc)*和置信度*(conf)*之间的绝对差异进行加权平均来实现：

![](../Images/e9326052edfb47643fca00c269856e9c.png)

该测量方法涉及将数据分成 M 个等间隔的箱子*。* ***B*** 用于表示“箱子”，***m*** 用于表示箱子的编号。我们稍后将详细介绍这个公式中的各个部分，如 ***B***、***|Bₘ|***、***acc(Bₘ)*** 和 ***conf(Bₘ)***。首先，让我们看看我们的示例，这将帮助逐步理解公式。

# 示例

我们有 9 个样本，具有估计的概率，也称为‘置信度’ ***(pᵢ)***，用于预测 0 或 1。如果标签 0 的概率 ***pᵢ*** 高于 0.5，则预测标签将为 0。如果低于 0.5，则标签 1 的概率会更高，因此预测标签将为 1（见下表）。最后一列显示了样本 ***i*** 的真实标签。

![](../Images/57158db74a0635c0bee6f63df4cba747.png)

**表 1** | 图片由作者提供

从上表中我们可以看到我们有 9 个样本，***n=9***。为了确定公式中的其余部分，我们首先需要将样本分成多个箱（bins）。

仅使用确定预测标签的概率来计算 ECE。因此，我们将仅根据标签的最大概率对样本进行分箱（参见表 2）。为了简化示例，我们将数据分成 5 个 **等间隔** 的箱子 ***M=5***（见右侧的分箱图 1）***。*** 让我们为每个箱子分配一个颜色：

![](../Images/095e3d1be665f7a853279e007eb940af.png)

现在，如果我们查看每个样本的最大估计概率，我们可以将其分组到 5 个分箱之一。样本 ***i=1*** 的估计概率为 ***0.78***，这高于 0.6 但低于 0.8，这意味着我们将其分到 ***B₄*** 中，见下图。现在看看样本 ***i=3***，其估计为 ***0.92***。这落在 0.8 和 1 之间，因此属于分箱 ***B₅***。我们对每个样本 ***i*** 重复这一过程，最终得到表格 2 中的分类（见下方）。

![](../Images/9fc061194b4ab16a908a231631fd12e8.png)![](../Images/0c1a9fc914b68cbf714ee1e88e245078.png)

**表格 2 和分箱图 1** | 图片由作者提供

***B₁*** 和 ***B₂*** 不包含任何样本 *（由于二进制示例的性质，最大概率在二进制情况下总是 ≥ 0.5）*。***B₃*** 包含 ***2*** 个样本。***4*** 个样本最终落入分箱 ***B₄***，而 ***3*** 个样本落入 ***B₅***。这已经为我们开始填写上面的 ECE 公式提供了一些信息。具体来说，我们可以计算样本落入分箱 ***m*** 的经验概率：***|Bₘ|*/*n***（见下方红色高亮）。

![](../Images/1521c47a0b4a2265f60c95eb6be767ca.png)

我们知道 ***n*** 等于 ***9***，并且从上述分箱过程中我们也知道每个分箱的大小：***|Bₘ|*** *（集合 S 的大小记作 |S| — 对于值，请参见上面的数字）*。如果我们为每个分箱拆分出颜色编码的公式，则得到如下结果：

![](../Images/931b7119c208e1b1787ceb13adbe51e0.png)

对于 ***B₁*** 和 ***B₂***，我们有 0 个样本（|B***₁***|=|B***₂***|=0），因此这些分箱的值为 0。

从上述分箱中，我们现在也可以确定 ***conf(Bₘ)***，它表示分箱 ***m*** 中的平均估计概率，论文中定义如下：

![](../Images/e6bffe6deb5ef79a4f6419917160b058.png)

计算 ***conf(Bₘ)*** 时，我们将表格 2 中每个分箱 ***m*** 的最大估计概率 ***p̂ᵢ*** 相加，然后除以分箱的大小 **|*Bₘ*|**，见下方右侧：

![](../Images/b6d86d8d6b8df198ac6defa8dca0ee31.png)![](../Images/874c95a862f950f2af34b6d224f1f186.png)

**表格 3 和计算** | 图片由作者提供

然后我们可以用这些值更新 ECE 计算：

![](../Images/6c3929e93e5fd4ad359142e2b8791bb1.png)

现在我们只剩下填写 ***acc(Bₘ)***，它表示每个分箱 ***m*** 的 ***准确率***，论文中定义如下：

![](../Images/070098521fbe859e7aae1c1ce2dad61a.png)

**1** 是一个 [*指示函数*](https://www.statlect.com/fundamentals-of-probability/indicator-functions#:~:text=The%20indicator%20function%20of%20an,the%20event%20does%20not%20happen.)，表示当预测标签 ***ŷᵢ*** 等于真实标签 ***yᵢ*** 时，它的值为 1，否则为 0。 这意味着你需要计算每个区间 ***m*** 中正确预测的样本数量，并将其除以区间的大小 **|*Bₘ*|**。 要做到这一点，我们需要首先确定样本是否被正确预测。 我们使用以下颜色：

![](../Images/605c5ae33ca661f6add2eb7415ead4b3.png)

并将其应用于最后 2 列，然后我们可以以相同的方式给右侧图中的样本上色：

![](../Images/382ee862054f5898b1d9a63c0d697ab9.png)![](../Images/1bab28334c4902bf85ea575f103db5f0.png)

**表 4 & 分箱图 2** | 作者提供的图片

查看上图的 **右侧**，我们可以看到在区间 ***B₃*** 中有 2 个样本和 *1 个正确* 预测，这意味着 ***B₃*** 的准确度为 1/2。 对于 ***B₄*** 重复这一过程，得到准确度为 3/4，因为在区间 ***B₄*** 中有 3 个正确预测和 4 个样本。 最后，查看 ***B₅***，我们有 3 个样本和 2 个正确预测，所以最终的准确度为 2/3。 这给出了每个区间的准确度值：

![](../Images/b2748e91f40c057b46dd5d0f0ddc68f5.png)

**我们现在拥有计算 ECE 所需的所有元素：**

![](../Images/601824f944f93d9e28bb4e8be5c6ec3d.png)

在我们这个包含 9 个样本的小例子中，我们得到的 ECE 为 0.10445。 一个完全校准的模型将具有 0 的 ECE。 ECE 越大，模型越不校准。

ECE 是一个有用的初步度量，广泛用于评估模型校准。 然而，ECE 也有一些缺点，使用时应注意 *(参见：* [*深度学习中的校准测量*](https://openaccess.thecvf.com/content_CVPRW_2019/papers/Uncertainty%20and%20Robustness%20in%20Deep%20Visual%20Learning/Nixon_Measuring_Calibration_in_Deep_Learning_CVPRW_2019_paper.pdf)*).* 

# Python 代码

## Numpy

首先我们将设置上述相同的示例：

```py
import numpy as np

# Binary Classification
samples = np.array([[0.78, 0.22],
                    [0.36, 0.64],
                    [0.08, 0.92],
                    [0.58, 0.42],
                    [0.49, 0.51],
                    [0.85, 0.15],
                    [0.30, 0.70],
                    [0.63, 0.37],
                    [0.17, 0.83]])

true_labels = np.array([0,1,0,0,0,0,1,1,1])
```

我们接着定义 ECE 函数如下：

```py
def expected_calibration_error(samples, true_labels, M=5):
    # uniform binning approach with M number of bins
    bin_boundaries = np.linspace(0, 1, M + 1)
    bin_lowers = bin_boundaries[:-1]
    bin_uppers = bin_boundaries[1:]

    # get max probability per sample i
    confidences = np.max(samples, axis=1)
    # get predictions from confidences (positional in this case)
    predicted_label = np.argmax(samples, axis=1)

    # get a boolean list of correct/false predictions
    accuracies = predicted_label==true_labels

    ece = np.zeros(1)
    for bin_lower, bin_upper in zip(bin_lowers, bin_uppers):
        # determine if sample is in bin m (between bin lower & upper)
        in_bin = np.logical_and(confidences > bin_lower.item(), confidences <= bin_upper.item())
        # can calculate the empirical probability of a sample falling into bin m: (|Bm|/n)
        prob_in_bin = in_bin.mean()

        if prob_in_bin.item() > 0:
            # get the accuracy of bin m: acc(Bm)
            accuracy_in_bin = accuracies[in_bin].mean()
            # get the average confidence of bin m: conf(Bm)
            avg_confidence_in_bin = confidences[in_bin].mean()
            # calculate |acc(Bm) - conf(Bm)| * (|Bm|/n) for bin m and add to the total ECE
            ece += np.abs(avg_confidence_in_bin - accuracy_in_bin) * prob_in_bin
    return ece
```

在 **二分类示例** 上调用函数返回与我们上述计算的相同值 ***0.10444***（四舍五入）。

```py
expected_calibration_error(samples, true_labels)
```

除了二分类示例，我们现在还可以快速浏览一个多分类的案例。 我们使用 [*James D. McCaffrey*](https://jamesmccaffrey.wordpress.com/2021/01/22/how-to-calculate-expected-calibration-error-for-multi-class-classification/) 的例子。 这给了我们 5 个目标类别和相关的样本置信度。 我们实际上只需要目标索引来进行计算：[0,1,2,3,4]，可以忽略它们对应的标签。 查看样本 ***i=1***，我们可以看到我们现在有 5 个估计概率，每个类别一个：[0.25,0.2,0.22,0.18,0.15]。

```py
# Multi-class Classification
samples_multi = np.array([[0.25,0.2,0.22,0.18,0.15],
                          [0.16,0.06,0.5,0.07,0.21],
                          [0.06,0.03,0.8,0.07,0.04],
                          [0.02,0.03,0.01,0.04,0.9],
                          [0.4,0.15,0.16,0.14,0.15],
                          [0.15,0.28,0.18,0.17,0.22],
                          [0.07,0.8,0.03,0.06,0.04],
                          [0.1,0.05,0.03,0.75,0.07],
                          [0.25,0.22,0.05,0.3,0.18],
                          [0.12,0.09,0.02,0.17,0.6]])

true_labels_multi = np.array([0,2,3,4,2,0,1,3,3,2])
```

在multi-classexample中调用函数返回***0.192***（*与* [*McCaffrey’s*](https://jamesmccaffrey.wordpress.com/2021/01/22/how-to-calculate-expected-calibration-error-for-multi-class-classification/) *计算的* ***0.002*** *不同，* *由于四舍五入的差异！*）。

尝试一下***Google Colab Notebook***，在numpy或PyTorch中亲自试试看（见下文）。

> 你现在应该知道如何手动计算ECE以及使用numpy :)

[**Google Colab Notebook链接**](https://colab.research.google.com/github/majapavlo/medium/blob/main/ece_medium.ipynb)*，其中包含了numpy和PyTorch中的二分类和多分类示例。**注意：本文中的代码改编自* [*论文*](https://arxiv.org/pdf/1706.04599.pdf) *的ECE torch类，来自他们的* [*GitHub库*](https://github.com/gpleiss/temperature_scaling)*。*
