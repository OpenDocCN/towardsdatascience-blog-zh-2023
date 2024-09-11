# 模型使用：推理发生在训练之后，而不是训练期间

> 原文：[https://towardsdatascience.com/model-employment-the-inference-comes-after-training-not-during-6129efdf8e90?source=collection_archive---------16-----------------------#2023-04-06](https://towardsdatascience.com/model-employment-the-inference-comes-after-training-not-during-6129efdf8e90?source=collection_archive---------16-----------------------#2023-04-06)

## 训练和使用模型是两个独立的阶段

[](https://medium.com/@valefonsecadiaz?source=post_page-----6129efdf8e90--------------------------------)[![Valeria Fonseca Diaz](../Images/880222be555e8fa7df660f9dd1233818.png)](https://medium.com/@valefonsecadiaz?source=post_page-----6129efdf8e90--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6129efdf8e90--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6129efdf8e90--------------------------------) [Valeria Fonseca Diaz](https://medium.com/@valefonsecadiaz?source=post_page-----6129efdf8e90--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6e363caf1c79&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmodel-employment-the-inference-comes-after-training-not-during-6129efdf8e90&user=Valeria+Fonseca+Diaz&userId=6e363caf1c79&source=post_page-6e363caf1c79----6129efdf8e90---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6129efdf8e90--------------------------------) ·5分钟阅读·2023年4月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6129efdf8e90&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmodel-employment-the-inference-comes-after-training-not-during-6129efdf8e90&user=Valeria+Fonseca+Diaz&userId=6e363caf1c79&source=-----6129efdf8e90---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6129efdf8e90&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmodel-employment-the-inference-comes-after-training-not-during-6129efdf8e90&source=-----6129efdf8e90---------------------bookmark_footer-----------)![](../Images/266a5d9950575df712e50ee2a143a083.png)

（图像由作者提供）构建模型与使用模型

在完成模型训练阶段后，整个建模流程的新阶段会被激活。机器学习社区中最常见的一个阶段是模型部署。对于那些不熟悉这一概念的人来说，这基本上指的是将模型放置在某个地方。如果你曾[阅读这些帖子](/is-interpreting-ml-models-a-dead-end-f5b9dd78ba77)关于一般的机器学习话题，你可能会记得将模型比作汽车的类比。当一辆车被制造和组装完成后，“部署”可能例如是将其送到汽车经销商店。这个整个流程中的另一个阶段是模型应用。

“模型应用”？是的。这仅指与我们模型的**实用性**相关的一切。这包括进行预测、确定重要参数、解释特征等。虽然这是一种定义模型实际用途的方式，但所有这些的技术术语是*推论*。模型应用可能不一定紧随其后。它始终取决于模型的实际用途。然而，它必须且应当总是发生在**模型训练之后**，而不是训练过程中。尽管模型部署按定义要求将模型放置在某处，但它几乎不会与训练步骤混淆。这不幸的是，模型应用有时会影响正确的推论。

**机器学习社区的方法**

几十年前，当机器学习社区逐渐成型时，这些概念被很好地消化，并且很容易被那些投入模型训练和预测实践的人应用到实际中。直到今天，我们不仅在全球范围内拥有无限类型的模型为我们提供不同的服务，而且这些模型的复杂程度已经增加到我们非常接近构建那种像我们的朋友ChatGPT一样进化我们物种的机器。这种复杂程度不允许机器学习流程混合步骤，也不允许负责人混淆不同阶段。多亏了这些井然有序的阶段划分，我们能够做出更准确的预测，并信任我们模型得出的推论。

**经典统计学社区的方法**

如果你曾修过任何经典统计学课程，你就会处理许多不同类型的模型，以测试一些假设并找出某个响应变量相关特征的所谓显著差异。这是[费舍尔经典统计范式](https://nautil.us/how-eugenics-shaped-statistics-238014/)的遗产。在这种实践中，模型训练阶段完全与模型应用阶段混淆在一起。是的，我们确实会根据输入特征的显著性水平（应用）来选择这些特征（训练），而且都是使用相同的数据。

![](../Images/ba7a7561e89a1192896879d481c6bad1.png)

(Image by author) 复杂性和结构的范围

**整个范围的中间部分**

随着复杂性水平的简化，现实已经向我们展示了模型训练与使用之间的分离变得越来越模糊。许多数据科学家在训练和使用模型时面对不同的复杂性水平。有些模型可能非常容易训练但难以使用，反之亦然。如果我们审计低复杂度的机器学习项目，可能会发现没有单一对象保存模型，没有明确的部署环境，并且没有特别用于解释或显著性测试的独特阶段。我们可能会发现许多模型被遗忘在文件夹中，作为多行代码散落着报告感兴趣的数字。那么，这会有多大问题呢？

![](../Images/ce0d5164d23a79448e334e5b3534ff53.png)

(Image by author) 一种写作工具，完成品与散落的零件

**我们的模型需要成为完整的工具**

是的，就像一支简单的铅笔或一个复杂的设备。统计模型，从简单的线性回归到深度学习模型，都是工具。我们构建这些工具是有目的的。这个目的可能因领域或应用而异，但**最终**，它们是为了那个目的而被使用的。当使用阶段没有与训练分开时，我们最终得到的模型就像是尚未组装的散落零件。当我们基于一个未组装的工具报告模型的使用或推断时，误导性推断的风险相当高。这就像用拆开的铅笔写字或开拆开的车一样。这有什么意义？为什么要冒这个风险？

**顺畅过渡到适当的模型使用**

成功的模型使用通常取决于至少某种形式的 [模型持久性](https://scikit-learn.org/stable/model_persistence.html)。即使你的模型仅在本地使用，也总会有至少一个用户：研究人员/数据科学家或任何将使用模型报告结果的人。因为已经有至少一个用户，所以必须至少 [保存模型](https://machinelearningmastery.com/finalize-machine-learning-models-in-r/) 为某种可用格式。一旦按下保存按钮或运行保存功能，瞧！你的模型现在作为一个完整的工具存在。

在你准备好**使用**模型之前的任何时刻都不应进行。当你准备好时，这里有一些有益的/必要的实践：

+   ***将模型训练和模型使用的脚本分开***。显著性测试报告应与模型估计分开存在于不同的脚本或软件文件中。

+   ***模型使用的方法不应用于模型训练。*** 就像测试集中的预测性能不用于调整模型参数一样，显著性测试也不应在同一训练数据上用于选择输入特征。

+   ***将模型训练结果与模型使用结果分别报告。*** 因为它们是不同的阶段，所以在发布或报告结果时，这些阶段也应该分别报告。

**为所有级别提供更好的模型使用**

我向ChatGPT询问了模型训练后的模型使用情况，它表示通常来说这是正确的。那么，这难道不是一成不变的事实吗？其实，机器人提出了一个很好的观点。在在线学习中，模型的使用会产生反馈来及时更新模型，因此任务会持续互动。“没错！”但请注意，通过反馈连接并不等同于被混淆。在在线学习中，特定的模型训练和使用的相互关系本身就是一个完整的建模方案。即使在这里，更新阶段、预测阶段和反馈流程都有明确的定义和方法。因此，概念上的分离仍然是有效的。

看待我们的模型及其相关实践时，模型训练与模型使用的区别归结为组织和结构，而非建模技能。一旦我们能够分离建模流程的各个阶段，我们的模型就应该能够持续存在并提供可靠的推断。这难道不是我们的终极目标吗？

*让我们保持良好的结构！*
