# p 值：以简单语言理解统计显著性

> 原文：[`towardsdatascience.com/p-values-understanding-statistical-significance-in-plain-language-41a00ff68f23`](https://towardsdatascience.com/p-values-understanding-statistical-significance-in-plain-language-41a00ff68f23)

## 选择通向显著结果的路径

[](https://medium.com/@MahamsMultiverse?source=post_page-----41a00ff68f23--------------------------------)![马哈姆·哈鲁恩](https://medium.com/@MahamsMultiverse?source=post_page-----41a00ff68f23--------------------------------)[](https://towardsdatascience.com/?source=post_page-----41a00ff68f23--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----41a00ff68f23--------------------------------) [马哈姆·哈鲁恩](https://medium.com/@MahamsMultiverse?source=post_page-----41a00ff68f23--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----41a00ff68f23--------------------------------) ·8 分钟阅读·2023 年 8 月 21 日

--

![](img/7e4971f4271b52b0b806314cb1c098d5.png)

图片由 Jens Lelie 提供于 Unsplash ([`unsplash.com/@madebyjens`](https://unsplash.com/@madebyjens))

你好！

今天，我们将进行一次有趣的统计学探索，处理一个既熟悉又经常被误解的概念——难以捉摸但始终存在的 p 值。如果你以前对它感到困惑，不用担心；我会以一种引人入胜且清晰的方式来解读它。

# p 值的显著性

在我们深入探讨之前，让我们从一个相关的场景开始：

想象一下，作为一名刚刚毕业的数据科学家，你在寻找你的第一份工作。你已经做了充分的准备，花费了无数小时解决像 LeetCode 这样的编码挑战，并掌握了复杂的机器学习算法概念。你对你的第一次工作面试感到准备充分且自信。面试官热情友好，气氛轻松，问题也在你的知识范围之内，然后他们问你：“p 值到底是什么？”

尽管你以前遇到过这个术语，但你当时的回应可能是：“它表示我们的假设的重要性。”然而，当面试官进一步挖掘时，你意识到你可能进入了比预期更深的水域。如果这个场景听起来很熟悉，请放心——你并不孤单。在这篇博客文章中，我们将真正尝试解构 p 值是什么以及它不是什么。我们会一步一步地进行，以便下次你遇到这个概念时，能够正确理解它。

从本质上讲，“p 值”一词代表“概率值”。然而，相信我，它的意义远非简单。这个概念可能有点不直观且难以理解，主要由于常见的误解甚至行业中的滥用。

# 用一个例子来设定场景

想象一个虚构的制药公司 MM 制药公司推出“药物 Alpha”作为治疗头痛的药物。问题是：药物 Alpha 是否真的能缓解头痛？为了检验其有效性，MM 制药公司进行了一项研究，涉及两组——一组接受药物 Alpha，另一组接受 [安慰剂](https://www.webmd.com/pain-management/what-is-the-placebo-effect)。

MM 制药公司的科学家们自然持怀疑态度，认为药物 Alpha 对缓解头痛的影响与安慰剂类似，即药物 Alpha 没有实质性的影响。因此，在分析所进行研究的结果时，他们预计会支持这一假设的结果。然而，令他们惊讶的是，结果显著偏离了如果药物 Alpha 类似于安慰剂的预期结果。这一异常引起了他们的注意，促使他们进行进一步的调查。这种情况就是一个非常低 p 值的实例。

现在，让我们介绍一些关键术语。 [**原假设**](https://statisticsbyjim.com/hypothesis-testing/null-hypothesis/) 作为我们的初始假设，也称为现状假设——即药物 Alpha 缺乏缓解头痛的能力，与安慰剂的效果相似。这个假设类似于 MM 制药公司的科学家们所持的怀疑态度。它代表了我们的基线观点，表明药物 Alpha 没有明显的效果。这是我们围绕其进行研究的假设。相反，**备择假设**认为药物 Alpha 确实能够缓解头痛——这是我们认为不太可能的结果。这个备择假设是我们正在严格测试的。

**输入 p 值！** p 值量化了我们测试结果与原假设假设的一致性。高 p 值表明结果与原假设一致，意味着我们的结果并不令人惊讶，初始假设是有价值的。

然而，低 p 值引入了一个意外的元素，正如 MM 制药公司的科学家们所观察到的。测试结果偏离了原假设下的预期结果。这促使我们重新评估我们的起始假设，考虑到我们的初始假设可能是错误的。在这种**不太可能**的情况下，p 值提供了药物 Alpha 可能真正缓解头痛的机会。

本质上，p 值为我们提供了一个工具来评估我们的观察结果是否与初始假设一致。高 p 值与零假设一致，而低 p 值则提示我们需要重新考虑假设并进行进一步调查。因此，p 值是一个帮助我们确定证据是否足够强以质疑先入为主的观念的指标。然而，需要注意的是，p 值本身不是证据、证明或客观的度量，而是一个指导性指标。

简单来说：

> p 值告诉我们如果零假设为真，我们获得观察结果的概率。

# p 值的统计解释

从数学角度看，p 值表示在零假设有效的假设下，观察到的数据与我们所收集的数据极端程度相同的可能性。一个显著低的 p 值（通常小于 0.05）意味着在零假设下，我们观察到的数据是不太可能的。这使我们质疑零假设，并考虑可能存在显著效应的可能性。

我们可以从数学上定义 p 值为：

**P 值 = P(结果|零假设)**

需要注意的是：关于什么构成足够小的 p 值（即 0.05 或 0.01）以被认为是不太可能事件的显著性是主观的。一般而言，事件发生的越少，p 值越小。

# 查看一些 Python 代码以区分两个结果

在这个演示中，我们尝试通过涉及两个组的实验来模拟头痛治疗药物的效果：一个组服用药物，另一个组服用安慰剂。我们使用独立样本 t 检验来比较这两个组的均值。[scipy.stats](https://docs.scipy.org/doc/scipy/reference/stats.html)模块中的 ttest_ind 函数计算 t 统计量和 p 值。

p 值表示我们观察到的头痛治疗效果差异的可能性，假设药物和安慰剂产生相同的结果。当 p 值低于预定义的显著性阈值（通常为 0.05，称为 alpha）时，我们倾向于质疑零假设的真实性，并推断替代假设即药物确实能缓解头痛。

```py
import numpy as np
from scipy import stats
import matplotlib.pyplot as plt
import seaborn as sns
import random
sns.set(style="whitegrid")

def pvalue_significance_estimator(placebo_group, drug_group):
    t_stat, p_value = stats.ttest_ind(placebo_group, drug_group)

    alpha = 0.05

    fig, (ax1, ax2, ax3) = plt.subplots(1, 3, figsize=(18, 5))

    # Plot individual distributions
    sns.histplot(placebo_group, color='blue', label='Placebo Group', ax=ax1)
    ax1.set_xlabel('Headache Relief (0-1)')
    ax1.set_ylabel('Number of People')
    ax1.set_title('Placebo Group Distribution')
    ax1.legend()

    sns.histplot(drug_group, color='orange', label='Drug Group', ax=ax3)
    ax3.set_xlabel('Headache Relief (0-1)')
    ax3.set_ylabel('Number of People')
    ax3.set_title('Drug Group Distribution')
    ax3.legend()

    # Plot comparison using a box plot
    data = [placebo_group, drug_group]
    labels = ['Placebo Group', 'Drug Group']
    ax2.boxplot(data, labels=labels)
    ax2.set_ylabel('Headache Relief (0-1)')
    ax2.set_title('Comparison between Groups')

    # Add p-value annotation
    p_value_text = f'p-value: {p_value:.4f}\n'
    if p_value < alpha:
        p_value_text += 'Significant difference'
    else:
        p_value_text += 'No significant difference'

    ax2.text(0.5, 0.85, p_value_text, transform=ax2.transAxes,
             ha='center', va='center', fontsize=12,
             bbox=dict(facecolor='white', edgecolor='gray', boxstyle='round,pad=0.5'))

    plt.tight_layout()
    plt.show()
```

下面，我们查看两种情景：一种是 p 值较大，表明在零假设下观察结果与预期结果之间没有显著差异；另一种是 p 值表明两组之间存在显著差异。

## 情景 1：显著的 p 值，观察结果与预期结果之间没有显著差异

由于我们的目标不是进行实际的临床试验，我们通过为安慰剂组和药物组设计使用均匀分布的情景来模拟试验，然后将其引入我们的 pvalue_significance_estimator 函数中。

```py
placebo_group = [round(random.uniform(0, 1), 1) for _ in range(100)] # Placebo group data
drug_group = [round(random.uniform(0, 1), 1) for _ in range(100)]  # Drug group data

pvalue_significance_estimator(placebo_group, drug_group)
```

![](img/b1c8b9100d081f28eb319ad153d55caa.png)

图片由作者提供

当我们可视化结果时，我们观察到在中央子图中，两组的均值相对接近，导致了一个较大的 p 值（约 0.24）。检查数据后，似乎两组具有相似的头痛缓解属性。

## 情景 2：极端/非常小的 p 值，观察结果与预期结果之间的显著差异

为了模拟第二种情景，我们对药物组的数值引入了 1 的偏倚，注意到这种操作在真实的临床试验中**非常**值得怀疑，可能产生深远的影响。我们再次将数据输入到我们的 pvalue_significance_estimator 函数中。

```py
np.random.seed(123)

placebo_group = [round(random.uniform(0, 1), 1) for _ in range(100)] # Placebo group data
drug_group = [round(random.uniform(0, 1), 1) for _ in range(100)]  # Drug group data

bias_percentage = 40  # Percentage of values to bias towards 1
bias_factor = 0.75

drug_group = [
    round(value * (1 - bias_factor) + bias_factor, 1)
    if random.uniform(0, 100) <= bias_percentage
    else value
    for value in drug_group
]

pvalue_significance_estimator(placebo_group, drug_group)
```

![](img/b29d1478429b04dc8c4b0125e3b08cc6.png)

图片来源：作者

在这里，我们可以看到两个组均值的显著差异，以及一个非常小的 p 值。这表明药物确实有影响。尽管这种简化分析仅比较了两个组的均值，且数据直接，但即使在这种情况下，指向药物有效性的备择假设仍需进一步研究。

重要的是要认识到这个例子是简化的，以传达一个概念。在实际设置中，你会处理更大的数据集、更复杂的统计测试和额外的复杂性。

# 理解 p 值的作用及其限制

在我们得出结论之前，我们应该讨论 p 值的一些细微限制。p 值确实是一个强大的假设检验工具。它允许我们评估观察数据与初始假设的一致性，并帮助我们在假设问题上做出明智的决策。然而，p 值不是判决或真理的最终衡量标准。它只是一个指标，引导研究者进一步探讨给定的假设。

另一个需要注意的是，虽然低 p 值可能表明我们的观察结果与原假设下的预期有显著差异，但它并没有提供效应的大小。换句话说，它不能告知我们发现的实际或量化意义，甚至是现实世界的影响。此外，高 p 值并不是原假设的证明，也不能**明确**否定备择假设。因此，使用 p 值得出结论时需要谨慎。

此外，通常对预定义的 alpha / 显著性水平的依赖（通常设定为 0.05）会带来更多的模糊性，例如 0.051 与 0.049 是否显著不同？确定的 alpha 是否真的适用于所考虑的假设？

还有 Type I 和 Type II 错误的概念。低 p 值并不能消除 Type I 错误的可能性，即错误地拒绝真实的原假设。同样，高 p 值也不能保证避免 Type II 错误，即未能拒绝虚假的原假设。

总之，更深入地理解 p 值是迈向更好统计思维和更优结果的一步。认识到它作为指引而非确定答案的角色，能帮助我们在数据分析和假设检验的复杂性中导航。同时，承认其局限性可以帮助我们谨慎对待结果。

# 总结

本质上，[p 值](http://galton.uchicago.edu/~thisted/Distribute/pvalue.pdf)作为研究人员的一个指引。它引导他们进一步探索，标示出数据何时偏离初步假设。因此，下次遇到 p 值时，希望你能明确它的作用。

欢迎在下面的评论中分享你对 p 值的看法和问题。我会认真倾听。
