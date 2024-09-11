# 从黑客到和谐：在推荐中构建产品规则

> 原文：[https://towardsdatascience.com/from-hacks-to-harmony-structuring-product-rules-in-recommendations-838af0873f18?source=collection_archive---------7-----------------------#2023-09-23](https://towardsdatascience.com/from-hacks-to-harmony-structuring-product-rules-in-recommendations-838af0873f18?source=collection_archive---------7-----------------------#2023-09-23)

## 不要让启发式规则削弱你的机器学习，学会将它们结合起来

[](https://roizner.medium.com/?source=post_page-----838af0873f18--------------------------------)[![Michael Roizner](../Images/bcb68ee626ea57234b62e512ed4b383b.png)](https://roizner.medium.com/?source=post_page-----838af0873f18--------------------------------)[](https://towardsdatascience.com/?source=post_page-----838af0873f18--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----838af0873f18--------------------------------) [Michael Roizner](https://roizner.medium.com/?source=post_page-----838af0873f18--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1bee5af37d8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-hacks-to-harmony-structuring-product-rules-in-recommendations-838af0873f18&user=Michael+Roizner&userId=1bee5af37d8&source=post_page-1bee5af37d8----838af0873f18---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----838af0873f18--------------------------------) ·6 min read·2023年9月23日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F838af0873f18&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-hacks-to-harmony-structuring-product-rules-in-recommendations-838af0873f18&user=Michael+Roizner&userId=1bee5af37d8&source=-----838af0873f18---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F838af0873f18&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-hacks-to-harmony-structuring-product-rules-in-recommendations-838af0873f18&source=-----838af0873f18---------------------bookmark_footer-----------)

在当今的数据驱动环境中，推荐系统驱动了从社交媒体信息流到电子商务的所有内容。尽管很容易认为机器学习算法承担了所有的重任，但这只是故事的一半。现实世界中的系统通常依赖于机器学习和启发式规则的混合——通常被称为产品规则、业务规则或简单的黑客——以生成最相关的推荐。

例如：

+   你不能过于频繁地推荐来自同一艺术家的曲目；

+   你应该在信息流中包含订阅内容，但不要使其过于繁重；

+   如果用户已经对某一类别或作者表示不喜欢，相关内容应受到惩罚或甚至被过滤掉；

+   明确的内容不能被推荐——除非情况适当。

![](../Images/11efea3666e246f87dabb9ab4f9c293b.png)

照片由 [Cam Bradford](https://unsplash.com/@cambradford?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

规则有两种类型：硬规则和软规则。硬规则作为过滤器，禁止在特定上下文中推荐某些文档；不符合规定被视为产品缺陷。这些规则本身没有什么错误，但它们的数量应该受到限制。此外，它们应该尽可能早地应用于排名过程，最好是在候选生成阶段，甚至在索引构建过程中。另一方面，软规则更像是指导方针：你可以推荐这些项目，但最好不要推荐过多（或者相反，多推荐也可以）。拥有过多的软规则可能会使系统调试和开发变得非常具有挑战性。

> **规则是技术债务。**

我发现系统中此类规则的数量通常依赖于团队内部的权力动态。产品经理通常发现通过规则表达约束很方便，而工程师通常不喜欢这些权宜之计。在我之前的团队中，我为我们能够将这些规则的数量保持在最低限度而感到自豪。

在我的职业生涯中，我经常遇到一个反复出现的模式。工程团队难以训练系统，以产生良好的推荐（无论是整体还是在特定方面）。产品团队随后转而使用他们最熟悉的方法——添加新规则。这些补丁在需要快速修复时是合理的，但后期很难去除。系统通常会保持这种补丁状态，直到进行重大重构，就像常规的技术债务一样。

故事的寓意——不要吝啬于聘请强大的工程师🙂

在一个理想的系统中，不应该有这样的规则；所有模糊的逻辑都应该由足够先进的模型处理。我梦想着有一天我们能达到这种技术状态（我有一个关于如何实现它的假设）。然而，就目前而言，这并不现实。因此，与其完全禁止这些规则，我将讨论一种允许某种组织并限制混乱的方法。

# 一种结构化的方法：重新排名框架

这个框架允许将机器学习模型与产品规则执行结合起来，帮助构建这些规则，同时避免完全混乱。然而，它是灵活的，不会过于限制，因此不能保证完全的秩序。在某种程度上，它只是描述规则的一种语言。在我看来，它相当方便。

在本讨论中，我们将重点关注排名的最终阶段，此时剩余文档不多——例如，从几十个到几百个——我们希望从中编制出最佳列表。这个阶段的有趣之处在于，我们不仅仅是在尽可能准确地评估每个文档在当前上下文中的表现，还在考虑这些文档之间的相互组合。这时，listwise 排名方法就发挥了作用（与用于学习排名的 listwise 学习不同，后者仅仅是损失函数依赖于查询中的所有文档，而排名函数并不依赖）。这种 listwise 方法的一个典型应用是提升结果的多样性。

下面是该方法的关键原则。

1.  结果是通过迭代生成的，从第一个位置开始，直到最后一个位置。在每次迭代中，我们选择最合适的文档放入接下来的位置。这是大多数重新排序策略的工作原理，如著名的DPP用于多样化。对于非线性输出，可以按重要性对位置进行排序。

1.  在每次迭代中，我们将所有剩余文档按值函数进行排序。这可以从简单的点击概率模型输出到更复杂的情况：各种模型输出（或多个模型）预测的不同事件、相似性组件（如与之前文档的相似性）以及手动加权等。值函数可以在每次迭代中重新计算，因此可以依赖于位置和最终输出中已有的文档。它必须具备计算效率。设计正确的值函数本身是一个丰富的话题；该框架既不限制也不简化这一方面。

1.  乘积规则表示如下：在一个位置的子集***X***中，具有属性***f***的文档数量应当高于或低于某个阈值***C***。通常，***X***是一个起始位置的范围，例如1到10（第一页）。属性***f***最好表示为某个特征的阈值规则，即**[feature(doc) > threshold]**。如有必要，这种格式可以推广到包含非二元属性的情况。

1.  规则具有优先级。如果我们不能满足所有规则，我们将丢弃优先级最低的规则。更准确地说：如果在给定位置可以实现最高优先级的规则，它将被严格执行；否则，将不被执行。如果在这些条件下可以实现下一个最高优先级的规则，它将被执行；否则，我们将跳过它。依此类推。换句话说，我们选择实现规则的字典序最高的掩码。

这里有一些格式规则的示例：

+   整个输出中至少一半的文档应为订阅内容。然而，如果所有订阅文档已经被阅读，这条规则将变得不可行，并会被丢弃。

+   前10个位置中的低质量文档数量不应超过2个。

+   在位置 10 和 20 之间，应该至少有一个来自新类别的文档。

值得注意的是，像“前 10 个位置中必须至少有 5 个文档具有某种属性”这样的规则，可能会导致前 5 个位置被缺乏该属性的文档填充，然后是 5 个具有该属性的文档。为了使分布更均匀，你可以为中间范围添加规则：前 2 个位置中至少有 1 个，前 4 个位置中至少有 2 个，依此类推。

高效实现这个框架是一个很好的挑战，但完全可以做到。这是一个 Python 代码草图，展示了如何实现描述的重新排序框架。请记住，这个代码并没有优化效率，但应该能提供一个很好的起点。

```py
def rerank(documents, count, rules, scorer):
    result = []
    while len(result) < count and len(documents) > 0:
        position = len(result)
        candidates = documents
        for rule in rules:
            filtered = [doc for doc in candidates if rule(position, doc)]
            if len(filtered) > 0:
                candidates = filtered
        next_doc = max(candidates, key=lambda doc: scorer(position, doc))
        result.append(next_doc)
        documents.remove(next_doc)
        scorer.update(position, next_doc)
        for rule in rules:
            rule.update(position, next_doc)
    return result
```

最后，通过记录所有执行和丢弃的规则，可以大大提升系统的可调试性和可控性。

正如我们所见，目前‘无规则’推荐系统仍然更像是一个理想而非现实。但这并不意味着我们陷入了混乱。一个结构良好的规则管理框架可以提供所需的组织，同时不会抑制系统的潜力。
