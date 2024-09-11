# 机器学习中的各种部署类型

> 原文：[`towardsdatascience.com/various-types-of-deployment-in-machine-learning-b503017e6bae?source=collection_archive---------17-----------------------#2023-01-06`](https://towardsdatascience.com/various-types-of-deployment-in-machine-learning-b503017e6bae?source=collection_archive---------17-----------------------#2023-01-06)

## 学习各种部署策略，以成功构建端到端的机器学习管道

[](https://suhas-maddali007.medium.com/?source=post_page-----b503017e6bae--------------------------------)![Suhas Maddali](https://suhas-maddali007.medium.com/?source=post_page-----b503017e6bae--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b503017e6bae--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b503017e6bae--------------------------------) [Suhas Maddali](https://suhas-maddali007.medium.com/?source=post_page-----b503017e6bae--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2a74f90399ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvarious-types-of-deployment-in-machine-learning-b503017e6bae&user=Suhas+Maddali&userId=2a74f90399ae&source=post_page-2a74f90399ae----b503017e6bae---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b503017e6bae--------------------------------) · 6 分钟阅读 · 2023 年 1 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb503017e6bae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvarious-types-of-deployment-in-machine-learning-b503017e6bae&user=Suhas+Maddali&userId=2a74f90399ae&source=-----b503017e6bae---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb503017e6bae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvarious-types-of-deployment-in-machine-learning-b503017e6bae&source=-----b503017e6bae---------------------bookmark_footer-----------)![](img/5aebe133a5f888bb2c8983f4938f860d.png)

图片由 [Jiawei Zhao](https://unsplash.com/@jiaweizhao?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

机器学习有很大的**范围**和**需求**，尤其是在最新的自动驾驶行业中，驾驶员通过 AI 的帮助获得辅助。此外，还有其他行业受益，如制药行业，它们开始使用 AI 来开发有趣的产品，这些产品本质上用于**预测性医疗**。其他行业还包括电子商务，在这些行业中，最相关的产品被推荐给用户，提高了客户购买产品的倾向。

通常，关于机器学习的能力以及它们如何在大量任务中取得最先进结果以实现高精度的讨论很多。然而，最少讨论的话题是如何在实时中进行**部署**，以及在**生产阶段**进行持续监控和评估。这是许多在线机器学习和深度学习课程中被忽视的关键因素。一个机器学习模型只有在我们能够将其作为**应用**提供给最终用户时才算优秀。

查看所有依赖机器学习的不同类型行业会让很多人**倾向于**这个领域，并使公司取得成功。有大量的在线课程突出机器学习的关键领域，如特征工程、数据准备、模型构建、超参数调整等等。然而，这些课程中缺少一个重要的元素：部署。

![](img/d13450975c5a376cc2f89ddba55b47f4.png)

照片由[Mediamodifier](https://unsplash.com/@mediamodifier?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

在这篇文章中，我们将详细了解各种部署策略，这些策略对于希望通过建立**AI 能力**来给团队留下深刻印象的人来说是至关重要的。现在，让我们详细探讨一下机器学习的部署策略。

## 批量推理

现在你已经训练并进行了机器学习模型的超参数调整，是时候将最佳模型投入生产。批量推理是一种部署策略，其中机器学习模型以实时方式部署，只接受**数据批次**。采用这种策略，模型通常能够离线工作或处理周期性任务，例如生成报告或预测。

批量推理可以在我们希望对客户对各种产品的**情感**进行分类的场景中非常有用。换句话说，他们可能会给出评论，如果我们想了解客户对产品的整体情感，批量推理可以是一种很好的机器学习模型部署策略。

## 实时推理

这是一种将机器学习模型在**接收数据**时进行实时运行的部署方式。因此，它们会准备好以某种格式接收数据并提供实时预测，以便可以采取相应的行动。此外，根据项目和团队目标，可能还需要满足实时推理的要求，如低延迟系统或更高的预测准确性。

实时推理的一个经典例子是在进行交易时检测**欺诈**活动的可能性。机器学习模型最初会使用包含欺诈和非欺诈交易的数据进行训练。在选择出最佳模型后，它会进行实时推理，以便客户能够了解是否发生了欺诈活动。

## 本地部署

在产品实时部署之前，机器学习团队通常需要进行**高安全性**措施和数据合规检查。在这种情况下，数据和生产中的机器学习代码的重要性更高。

本地部署涉及在组织设施内的**物理设备或服务器**上部署机器学习模型。因此，它可以提供对数据和模型的高安全性和控制。

本地部署在预测性维护中可能非常有用，其中使用机器学习模型来确定各种制造设备的故障可能性。我们不依赖互联网提供实时预测，而是使用我们自己的一组**服务器**和机器，这些设备能够提供机器学习预测所需的计算能力。每当模型预测制造材料存在各种缺陷时，人们可以通过更换这些产品来采取行动。

## 云部署

这是一种将我们的机器学习服务提供到云中的部署方式。因此，我们利用**集群**设备的计算资源和内存。因此，我们应能根据用户执行的各种机器学习操作的流量来扩展我们的应用程序。

云部署在我们不确定训练和部署模型所需资源数量时可能会很有用。此外，这些服务仅在用户使用我们的预测时才会初始化。

使用云部署的一个流行示例是预测客户在特定服务集上流失的可能性。如果我们建立了一个服务，让订阅者使用，我们将基于一组预测特征预测客户是否会离开服务。由于我们无法完全了解可能注册服务和同时离开服务的客户总数，因此在**云**中部署训练模型是一种好的方法，因为这会根据流量需求简化扩展。

## 移动部署

这是一种在移动设备（如智能手机和平板电脑）上部署机器学习模型的部署方式。这种类型的部署示例包括个人助理、图像识别和语言翻译应用程序。

由于我们在**资源受限**的环境中部署模型，这与在服务器上部署的环境不同，因此必须在最终形成 ML 产品之前考虑硬件因素。机器学习应用可能非常有用，并且可以具有合理的准确性。然而，如果模型在硬件资源较少的情况下无法生成预测，那么它可能不适合用于移动应用程序。

在尝试实时在移动设备上部署这些产品时，必须考虑**低延迟**要求、偏差-方差权衡以及其他因素。

## 边缘部署

这是一种在边缘设备（如**物联网 (IoT)**）上部署机器学习模型的方式。这些设备位于网络的边缘，并依赖于稳定的互联网连接来进行预测。尽管如此，也有一些物联网设备不需要互联网连接，而是拥有能够生成预测的硬件。

在尝试使用这种类型的部署时，必须考虑一些要求。重要的考虑因素包括处理能力、内存容量和连接性。这些因素对用于机器学习的物联网设备的**性能**有很大影响。

另一个重要的考虑因素是优化模型以适应边缘部署，并考虑在云端运行模型的可行性。这可能涉及减少需要在边缘设备上使用的 ML 模型的**复杂性**或大小。因此，部署类型将取决于用于提供机器学习能力的设备类型。

## 结论

总的来说，我们已经探讨了很多机器学习和深度学习模型的部署选项。在许多在线课程中，重点强调了机器学习模型及其内部工作原理。这些课程很好地突出了一些关于这些模型的**细微差别**，这些差别可以在测试之前进行深入解读。然而，也应该对部署方面给予足够重视，因为由于众多考虑因素，实时部署这些模型可能具有挑战性。

阅读完这篇文章后，希望你对机器学习的常见部署选项有了一些了解。谢谢。

以下是你可以联系我或查看我工作的方式。

**GitHub:** [suhasmaddali (Suhas Maddali ) (github.com)](https://github.com/suhasmaddali)

**YouTube:** [`www.youtube.com/channel/UCymdyoyJBC_i7QVfbrIs-4Q`](https://www.youtube.com/channel/UCymdyoyJBC_i7QVfbrIs-4Q)

**LinkedIn:** [(1) Suhas Maddali, Northeastern University, Data Science | LinkedIn](https://www.linkedin.com/in/suhas-maddali/)

**Medium:** [Suhas Maddali — Medium](https://suhas-maddali007.medium.com/)
