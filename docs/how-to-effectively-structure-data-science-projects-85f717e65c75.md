# 如何有效地构建数据科学项目

> 原文：[https://towardsdatascience.com/how-to-effectively-structure-data-science-projects-85f717e65c75?source=collection_archive---------5-----------------------#2023-08-21](https://towardsdatascience.com/how-to-effectively-structure-data-science-projects-85f717e65c75?source=collection_archive---------5-----------------------#2023-08-21)

## 使用 PSW 工具的简要说明

[](https://radmilamandzhi.medium.com/?source=post_page-----85f717e65c75--------------------------------)[![Radmila M.](../Images/f3722a0ca0c96b5f6abb8f23a1162488.png)](https://radmilamandzhi.medium.com/?source=post_page-----85f717e65c75--------------------------------)[](https://towardsdatascience.com/?source=post_page-----85f717e65c75--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----85f717e65c75--------------------------------) [Radmila M.](https://radmilamandzhi.medium.com/?source=post_page-----85f717e65c75--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1b144e8ba52a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-effectively-structure-data-science-projects-85f717e65c75&user=Radmila+M.&userId=1b144e8ba52a&source=post_page-1b144e8ba52a----85f717e65c75---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----85f717e65c75--------------------------------) ·8分钟阅读·2023年8月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F85f717e65c75&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-effectively-structure-data-science-projects-85f717e65c75&user=Radmila+M.&userId=1b144e8ba52a&source=-----85f717e65c75---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F85f717e65c75&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-effectively-structure-data-science-projects-85f717e65c75&source=-----85f717e65c75---------------------bookmark_footer-----------)![](../Images/ac62b73df82d4963b2f4a9e2c9352602.png)

图片来源：[Ross Sneddon](https://unsplash.com/es/@rosssneddon?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

我曾经参与过大量的数据科学项目，帮助客户处理各种回归和分类任务：相似模型和推荐系统、自然语言处理问题、预测分析，仅举几例。

客户往往忙于日常工作，因此他们无法安排长时间的会议来详细描述他们希望在项目中看到的内容。因此，拥有一个详细且结构良好的议程是非常重要的。

在与专家的电话会议中，我经常使用**PSW（问题陈述工作表）方法**来充分理解客户的需求。

> **PSW是一个业务任务描述模板，主要用于咨询，但也非常适合几乎任何IT项目。**

在这篇文章中，我将展示如何使用PSW工具来更好地理解数据科学项目的关键点，并通过使客户会议更具一致性和简洁性，从中获得最大收益。

PSW通常包含六个主要部分：

+   **背景**。这个部分填充了关于项目当前状态和导致其启动的挑战的简要信息。

+   **成功标准**。在这里，重要的是要弄清楚如何评估解决项目任务的可能决策，并将所有标准按重要性进行排序。

+   **解决方案空间的范围**。这个部分提供了对分析边界的理解。最好与客户明确哪些领域不应再考虑在内。

+   **解决方案空间的约束**。在这里，我们列出决策空间中可能出现的障碍。这可能是要使用的特定编程语言、某些模型要求，或严格的预算限制。

+   **利益相关者**。这是一个将影响决策和项目成功的人员名单。这些人可以分为决策者、帮助者和阻碍者。

+   **关键洞察来源**。这个部分旨在回答“我们从哪里获取数据来解决项目任务？”的问题。

    将信息来源分成相关组更为理想，例如：

    1) 书籍、相关研究文章；

    2) 最新的行业报告；

    3) 类似的项目等。

接下来，我将逐一考虑每个部分，并举例说明它们可以填充的信息。

# 1\. 背景

![](../Images/ecb9e9f1851b8bae8a7634d6dd8e616e.png)

由[Keith Misner](https://unsplash.com/@keithmisner?utm_source=medium&utm_medium=referral)拍摄，照片来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

这是第一个部分，通常在互致问候之后自然发生。在这里，我经常要求客户提供关于项目的更多背景信息：**它为何出现，它对公司有何重要性等**。一方面，这些细节将为深入探讨项目的细节打下坚实的基础，另一方面——有助于制定项目的主要目标。

**如果你能用一句话定义项目目标，那么你对项目的理解就非常到位。**

这里是一个基于客户提供的信息的背景部分的典型示例：

> 任何移动应用程序都必须考虑用户需求，以便为他们提供最便捷的解决方案。用户进入应用程序的目的是明确的，他们会执行某些操作。但通过在屏幕上添加推荐，可能会缩短这个过程，例如，使向其他用户进行交易的速度更快。这就是基于**机器学习（ML）**的推荐系统可以提供帮助的地方。
> 
> 作为项目的一部分，有必要根据每个用户的转账数量对联系人进行排名。已经有尝试训练模型，所以基线已经存在，但现在的任务是提高其准确性5%或更多，同时应用**机器学习**推荐算法。

如你所见，背景部分有助于将项目任务融入业务的整体背景中（**使应用程序更加用户友好**），并且在必要时，可以根据全球目标（**应用基于ML的推荐系统**）调整这些任务。

# 2\. 成功标准

![](../Images/e0092828aff7a1bea13ef663eccbade9.png)

图片由[Guille Álvarez](https://unsplash.com/@guillealvarez?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在这里，你可以询问客户**项目将根据哪些主要参数进行评估**以及**用于确定“项目成功” 的标准是什么**。这些可以是财务指标（如降低成本）和非财务指标（例如应用程序的活跃用户数量、构建模型的准确性等）。除了具体标准外，了解客户的所有不可量化的愿望也很重要。也许你的客户会通过你提出的措施来彻底改变他们的企业文化（为什么不呢？！）。

继续以移动应用程序和推荐系统为例，以下是这个项目可能的成功标准：

> 1) 选择适当的**机器学习（ML）**模型用于系统的解释。
> 
> 2) 基线模型已经改进了5%或更多。
> 
> 3) 模型的运行速度从启动到结果接收不超过6小时。
> 
> 4) 模型性能在可用数据上进行检查——测试集上的准确性应超过85%。

# 3\. 解决方案空间范围

![](../Images/7a9f9b5619a13c38f19f2e0cb6d5ce85.png)

图片由[Nicolas Lobos](https://unsplash.com/@lobosnico?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在这里，重要的是要了解项目的边界。通常，这一部分PSW包含项目的简要背景——项目主题为何现在重要且相关，市场上已经存在什么解决方案和基准，并且这些解决方案和基准可以如何进一步修改以满足客户的要求。

> 如果谈论推荐系统，需要注意的是创建推荐系统有几种方法。
> 
> 我们可以考虑基于内容（content-based）或知识（knowledge-based）的方法，使用协同过滤（collaborative filtering）或混合方法。混合系统结合了多种系统的优势，使其成为一个一站式的推荐工具。

# 4\. 解决空间中的约束

![](../Images/1df549e2246e237c7276c50b7f559d03.png)

[Joshua Hoehne](https://unsplash.com/@mrthetrain?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上的照片

在这一块中，我们要概述可接受和不可接受的解决方案范围。你可以直接询问客户：“我们的限制是什么？”这个问题可能会有所帮助。在这里，你可能会听到有关方法/技术/编程语言的限制。对于我们分析的项目，存在与使用开源数据集进行机器学习模型训练和结果可复现性相关的限制。后者可以通过提供详细项目描述的`README`文件来实现。

> 1\. 第三方资源使用的限制：在开发推荐系统时，切勿使用开放数据进行模型预训练。
> 
> 2\. 实现方法的可复现性：在另一台PC上重新启动模型时，应该得到类似的结果。

## 注意

PSW中的第3和第4块可能会让人困惑。究竟如何理解解决空间和约束之间的区别呢？让我们来看一个例子。

想象一下你发现了一封旧信，信中提到你祖父多年前在家里的后院藏了一箱黄金。他没有具体说明藏在哪里，所以整个后院就是解决空间。一旦你读了这封信，你希望尽快找到宝藏，并考虑使用电梯来寻找。不幸的是，后院被围栏围住，无法通过电梯到达。在这种情况下，不能使用电梯就是解决空间内的一个明确约束。

![](../Images/0640958bb3fa49b33374add6668fa23d.png)

[Jean-Frederic Fortier](https://unsplash.com/@jffortier?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上的照片

# 5\. 利益相关者

![](../Images/add0b1c20f30b06cfe0c7fca0a1edc3b.png)

[airfocus](https://unsplash.com/@airfocus?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上的照片

PSW的这一块提供了在实施项目时应考虑的人的意见。通常，利益相关者是对项目结果感兴趣的人。他们可能是项目团队成员、项目经理、执行官、项目投资者、客户和最终用户。

> 利益相关者是会在项目生命周期的任何阶段受到影响的人，他们的意见可以直接影响结果。在开发推荐系统的情况下，其集成到应用程序中将对以下两大群体有利：
> 
> 1) 使用该系统的移动应用用户可以节省时间。
> 
> 2) 应用程序开发人员通过使他们的产品更具功能性，从而增加用户忠诚度。

# 6. 主要见解来源

![](../Images/3f99a3d6df557d828acd5a0e0ed99fd5.png)

由[Susan Q Yin](https://unsplash.com/@syinq?utm_source=medium&utm_medium=referral)拍摄的照片，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

通常，这个块包含任何相关信息，以帮助全面理解主题——例如，开源 API 库的链接 [1]、教程 [2]、代码库、研究文章等。

在这里，询问客户之前在该项目上已经做了什么是至关重要的。如果有的话，不要犹豫，请他们分享哪些方面做得好，哪些方面在项目初期实施得不佳。这将为你提供有关可能的进一步行动和项目发展方向的一些线索。

> 对于考虑中的 Data Science 项目及推荐系统，使用任何材料，包括机器学习和预测分析领域的文章，例如，一篇关于该行业最新成就的综合评论可能是一个很好的起点 [3]。
> 
> 关注解决类似排名和推荐问题的最新方法。

# 结论

我希望这篇帖子中的信息能帮助你为任何客户会议做好充分准备，并提出适当的问题。

以下是我对 PSW 方法的主要见解：

1.  在应用 PSW 时，不要忘记记录客户告诉你的所有时刻。我通常将所有信息汇总到一个**后续跟进**文件中，并在 Data Science 项目实施过程中使用。

1.  PSW 工具不仅对客户会议有用。此外，它还可以帮助新成员更快地深入 Data Science 项目组，同时向经验丰富的项目组成员提出有价值的问题。

1.  请记住，虽然 PSW 是一个很棒且易于使用的工具，但它并不是一个万能的“一刀切”解决方案。在某些情况下，它可能不起作用。

> 一般来说，PSW 方法对于数据科学项目效果良好，在这些项目中，任务有明确的愿景，并且有来自客户的一些输入和初步解决任务的尝试。在这种情况下，客户可以与我分享这些信息，以便我们共同利用 PSW 解决他们的挑战。然而，在项目特点有许多未知见解和不明确的前景时，应用 PSW 工具将会困难。例如，如果客户要求对尚未开始的数据科学项目进行一些创意生成，PSW 方法将不适用，这时需要选择其他方法。

感谢阅读，祝您的项目顺利！

# 参考文献列表

1.  提供推荐的推荐系统 REST API: [https://github.com/recommender-system/reco-api?ysclid=lll99344l9788228410](https://github.com/recommender-system/reco-api?ysclid=lll99344l9788228410)

1.  初学者教程：Python 中的推荐系统: [https://www.datacamp.com/tutorial/recommender-systems-python](https://www.datacamp.com/tutorial/recommender-systems-python)

1.  关于多模态推荐系统的综合调查：分类、评估及未来方向: [https://arxiv.org/pdf/2302.04473.pdf](https://arxiv.org/pdf/2302.04473.pdf)
