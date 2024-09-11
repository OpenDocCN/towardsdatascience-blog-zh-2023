# 数据科学家如何节省时间

> 原文：[`towardsdatascience.com/how-data-scientists-save-time-607a28468643?source=collection_archive---------9-----------------------#2023-06-01`](https://towardsdatascience.com/how-data-scientists-save-time-607a28468643?source=collection_archive---------9-----------------------#2023-06-01)

[](https://towardsdatascience.medium.com/?source=post_page-----607a28468643--------------------------------)![TDS 编辑](https://towardsdatascience.medium.com/?source=post_page-----607a28468643--------------------------------)[](https://towardsdatascience.com/?source=post_page-----607a28468643--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----607a28468643--------------------------------) [TDS 编辑](https://towardsdatascience.medium.com/?source=post_page-----607a28468643--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7e12c71dfa81&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-data-scientists-save-time-607a28468643&user=TDS+Editors&userId=7e12c71dfa81&source=post_page-7e12c71dfa81----607a28468643---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----607a28468643--------------------------------) · 发送为 通讯 · 3 分钟阅读 · 2023 年 6 月 1 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F607a28468643&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-data-scientists-save-time-607a28468643&user=TDS+Editors&userId=7e12c71dfa81&source=-----607a28468643---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F607a28468643&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-data-scientists-save-time-607a28468643&source=-----607a28468643---------------------bookmark_footer-----------)

据说我们中间有些人拥有所有需要的自由时间；如果你是其中四位幸运的个体之一，你可能正悠闲地在迷雾森林中冥想，而不是阅读这些文字。

我们中的其他人则与时间、效率以及平衡竞争需求的紧张关系有着更复杂的关系。几小时之间可能会影响到一个机器学习项目按时交付与错过截止日期的差别。它们可能决定你是否能和朋友共度一个下午、从零开始制作一个奶酪蛋糕、重温*继承*大结局，或者……无法做任何这些事情。

互联网充满了生产力技巧和节省时间的窍门；我们相信你可以自己找到它们。本周，我们提供的是务实的、动手的方式，以加速数据专业人员每天执行的工作流。从选择合适的工具到简化数据清理过程，通过学习我们的作者的经验，你有望节省宝贵的时间，所以我们希望你好好利用这些时间——或者不利用！这是 *你的* 时间。

+   分析地理空间数据可能是一个缓慢而复杂的过程。作为他 TDS 的首次亮相，[Markus Hubrich](https://medium.com/u/3b63a2f93113?source=post_page-----607a28468643--------------------------------) 突出了 R-trees 在 **显著提高空间搜索速度** 方面的强大功能，并使用（令人愉快的）树映射示例来说明他的观点。

+   许多库和工具承诺改善 Python 著名的缓慢性能。[Theophano Mitsa](https://medium.com/u/7709c007f0ca?source=post_page-----607a28468643--------------------------------) **对新近发布的 Pandas 2.0 进行基准测试，与 Polars 和 Dask 等竞争者进行比较**，这样你可以在设计下一个项目时做出最明智的决定。

+   继续讨论加速以 Python 为中心的工作流，[Isaac Harris-Holt](https://medium.com/u/ae5d6a2a28aa?source=post_page-----607a28468643--------------------------------) 的最新教程展示了如何通过 **将 Rust 嵌入到你的 Python 代码中** 利用 Rust 的灵活性，借助 PyO3 和 maturin 等工具。（保持我们的主题，这也是一篇快速而简洁的文章。）

![](img/407b45c66a9bdc407da5dec8615b67d3.png)

由 [Annie Spratt](https://unsplash.com/@anniespratt?utm_source=medium&utm_medium=referral) 拍摄，图片来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

+   大型语言模型在执行复杂、细微的过程时效果如何？结论可能还未揭晓，但 **它们在文本摘要方面已经显示出潜力**— [Andrea Valenzuela](https://medium.com/u/a6f3f1654c3?source=post_page-----607a28468643--------------------------------) 的最新指南解释了如何使用它们快速且一致地生成高质量的摘要。

+   对于 [Vicky Yu](https://medium.com/u/cd08464b29cc?source=post_page-----607a28468643--------------------------------) —— 我们猜测也包括许多人——数据清洗可能会很快变得乏味。为了顺利完成项目的这个阶段，Vicky 推荐创建用户定义函数（UDFs），这些函数可以**简化你的 SQL 查询，避免在表中的多个列上重复编写相同的逻辑**。

+   CPU 还是 GPU？如果你处理大量数据，你可能已经知道**硬件配置的选择可能对你所需的时间和资源产生巨大的影响**。 [Robert Kwiatkowski](https://medium.com/u/9f55d2ee5cad?source=post_page-----607a28468643--------------------------------) 的有用入门指南涵盖了多个使用案例，并对两种处理器选项的优缺点进行了比较。

如果你还有几分钟空闲时间，我们希望你能花时间阅读我们其他一些近期的优秀文章：

+   机器学习模型是如何产生的，它们是如何随着时间变化的？ [Valeria Fonseca Diaz](https://medium.com/u/6e363caf1c79?source=post_page-----607a28468643--------------------------------) 探讨了模型的生命周期。

+   在他们的新深度分析中，[Marco Tulio Ribeiro](https://medium.com/u/4274f519efce?source=post_page-----607a28468643--------------------------------) 和 [Scott Lundberg](https://medium.com/u/3a739af9ef3a?source=post_page-----607a28468643--------------------------------) 提倡使用与其他软件相同的原则来测试 LLM 构建的应用程序。

+   [Nazlı Alagöz](https://medium.com/u/4ba02da50bf?source=post_page-----607a28468643--------------------------------) 最近分享了一篇关于学术界与行业数据科学实践之间惊人相似性的深刻反思。

+   如果你想要对逻辑回归的直观和易懂的介绍，不要错过 [Igor Šegota](https://medium.com/u/e5f8ebca4ad8?source=post_page-----607a28468643--------------------------------) 的初学者友好解释。

感谢你支持我们的作者！如果你喜欢在 TDS 上阅读的文章，可以考虑[成为 Medium 会员](https://bit.ly/tds-membership) —— 这将解锁我们所有的档案（以及 Medium 上的其他所有文章）。

直到下一个 Variable，

TDS 编辑
