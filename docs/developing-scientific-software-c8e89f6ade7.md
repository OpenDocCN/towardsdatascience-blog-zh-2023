# 开发科学软件

> 原文：[`towardsdatascience.com/developing-scientific-software-c8e89f6ade7?source=collection_archive---------8-----------------------#2023-07-01`](https://towardsdatascience.com/developing-scientific-software-c8e89f6ade7?source=collection_archive---------8-----------------------#2023-07-01)

## 第一部分：测试驱动开发的原则

[](https://medium.com/@cdacostaf?source=post_page-----c8e89f6ade7--------------------------------)![Carlos Costa, Ph.D.](https://medium.com/@cdacostaf?source=post_page-----c8e89f6ade7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c8e89f6ade7--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c8e89f6ade7--------------------------------) [Carlos Costa, Ph.D.](https://medium.com/@cdacostaf?source=post_page-----c8e89f6ade7--------------------------------)

·

[跟随](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc1d045b63ee9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeveloping-scientific-software-c8e89f6ade7&user=Carlos+Costa%2C+Ph.D.&userId=c1d045b63ee9&source=post_page-c1d045b63ee9----c8e89f6ade7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c8e89f6ade7--------------------------------) · 10 分钟阅读 · 2023 年 7 月 1 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc8e89f6ade7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeveloping-scientific-software-c8e89f6ade7&user=Carlos+Costa%2C+Ph.D.&userId=c1d045b63ee9&source=-----c8e89f6ade7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc8e89f6ade7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeveloping-scientific-software-c8e89f6ade7&source=-----c8e89f6ade7---------------------bookmark_footer-----------)![](img/72729e33f225858ae5308356bbeb08f7.png)

[照片由 Noah Windler](https://unsplash.com/@noahwindler?utm_source=medium&utm_medium=referral)提供，拍摄于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我们生活在一个计算世界快速扩展可能性的时代。人工智能在解决旧问题和新问题方面不断取得进展，常常以完全意想不到的方式进行。庞大的数据集现在几乎在任何领域都变得普遍，而不仅仅是科学家们在昂贵设施中才能获得的东西。

然而，在过去几十年中，处理数据的软件开发面临的许多挑战依然存在——或者在处理这些新的、大量的数据时问题更加严重。

科学计算领域，传统上专注于开发快速而准确的方法来解决科学问题，近年来已超越其原始的狭窄范围变得相关。在本文中，我将揭示在开发高质量科学软件时出现的一些挑战，以及一些克服这些挑战的策略。我们的最终目标是制定一个逐步指南，以确保准确和高效的开发过程。在后续文章中，我将跟随这个逐步指南解决一个 Python 示例问题。阅读后查看！

# TDD 和科学计算：**不是**天作之合？

[测试驱动开发 (TDD)](https://en.wikipedia.org/wiki/Test-driven_development)重新定义了软件工程，使开发人员能够编写更耐用、无漏洞的代码。如果你曾经使用过 TDD，你可能对它在编写高质量软件方面的力量非常熟悉。如果没有，希望通过阅读本文你会理解它的重要性。无论你对 TDD 的经验如何，任何熟悉科学计算的人都知道，自动化测试软件的实施往往很棘手。

我推荐大家至少读一次的 [TDD 开发周期](https://en.wikipedia.org/wiki/Test-driven_development) 提供了一些明智的指示，说明如何以一种每一段代码都通过测试验证的方式开发软件。定期测试可以确保错误常常在被引入之前就被发现。

但 TDD 的一些原则可能与科学软件开发过程完全相悖。例如，在 TDD 中，测试是在代码*之前*编写的；代码是为了适应测试而编写的。

但设想你正在实现一种全新的数据处理方法。你如何在甚至没有代码的情况下编写测试？TDD 依赖于**预期行为**：如果在实施新方法之前没有办法量化行为，那么首先编写测试在逻辑上是不可能的！我会认为这种情况很少见，但即使发生了，TDD 仍然可以帮助我们。怎么做呢？

# 验证与确认

[Rilee 和 Clune](https://ieeexplore.ieee.org/document/7017328) 观察到（强调部分为我所加）：

> *有效的数值软件测试需要一套全面的预言器 […] 以及对不可避免的数值误差的稳健估计 […] 初看这些问题常常显得极具挑战性甚至难以克服。然而，我们认为这种普遍看法是不正确的，并由* ***(1) 模型验证与软件确认的混淆*** *以及 (2) 科学界倾向于开发相对粗糙的大型程序，聚合了许多算法步骤* 驱动。

**Oracle** 是已知的输入/输出对，这些对可能涉及或不涉及复杂的计算。Oracle 用于传统的 TDD，但它们通常非常简单。它们在科学软件中扮演着更重要的角色，而不仅仅是作为单元测试的一部分！

当我们谈论使用 oracles 检查某些预期行为时，我们谈论的是软件的 **验证**。对于软件来说，它不关心验证什么，只要输入 X 导致输出 Y。另一方面，**确认** 是确保代码的输出 Y 准确匹配科学家期望的过程。这个过程 *必须必然利用* 科学家的领域知识，以实验、模拟、观察、文献调查、数学模型等形式呈现。

这个重要的区别并非科学计算领域所独有。任何 TDD 实践者无论是隐性还是显性地开发了包含验证和确认两个方面的测试。

假设你正在编写代码以将一组人分配到一组标记的椅子上。一个验证测试可以检查 N 个人和 M 把椅子的列表是否输出了 N 个 2 元组。或者检查如果任何列表为空，输出也必须是空列表。与此同时，一个确认测试可以检查如果输入列表包含重复项，函数是否抛出错误。或者检查在任何输出中，没有两个人被分配到同一把椅子上。这些测试需要对我们的问题有领域知识。

虽然 TDD 涉及验证和确认两个方面，但重要的是不要混淆这两者，并在软件开发的适当阶段使用它们。如果你正在编写科学软件——即任何复杂的数值代码，尤其是性能关键的代码——请继续阅读，以了解如何恰当地利用 TDD 达成这些目的。

# 科学软件中的测试警告

标准软件和科学软件之间的一个重要区别是，在标准软件中，相等性通常是不具争议的。当测试是否有两个人被分配到同一把椅子时，检查标签（可以建模为整数）是否相同是直接的。在科学软件中，浮点数的普遍使用使问题复杂化。相等性不能一般通过 `==` 检查，通常需要选择数值精度。实际上，精度的定义可能会根据应用而有所不同（例如，见 [相对与绝对容差](http://web.mit.edu/10.001/Web/Tips/Converge.htm)）。这里是一些推荐的数值准确性测试实践：

+   从测试容差开始，精度要尽可能接近所使用的最不精确的浮点类型。你的测试可能会失败。如果失败，请逐位松弛精度，直到测试通过。如果你无法获得良好的精度（例如，你需要`10^-2`的容差来使使用 float64 操作的测试通过），可能存在 bug。

+   数值误差通常[随着操作次数的增加而增长](https://floating-point-gui.de/errors/propagation/)。在可能的情况下，*通过领域特定知识*验证精度（例如，泰勒方法具有可以在测试中利用的显式余项，但这种情况很少见）。

+   尽可能偏好绝对容差，避免在比较接近零的值时使用相对容差（“准确性”）。

+   在不同机器上运行测试数千次时，精度单元测试失败并不罕见。如果这种情况持续发生，则要么精度要求过于严格，要么引入了错误。在我的经验中，后一种情况更为常见。

![](img/096a69dff1d4b0e2260f948b18bda23a.png)

浮点数 :P（照片由 [Johannes W](https://unsplash.com/@johanneswre?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)）

## 测试新方法

在开发科学软件时，不能仅仅依赖数值准确性。通常，新方法可以提高准确性或完全改变解决方案，从而从科学家的角度提供“更好的”解决方案。在前一种情况下，科学家可能会使用具有降低容差的先前神谕来确保正确性。在后一种情况下，科学家可能需要创建一个全新的神谕。创建一个**精心挑选的神谕示例套件**至关重要，这些示例可能会或可能不会被检查数值精度，但科学家可以检查这些示例。

+   精心挑选一组具有代表性的示例，以便可以自动或手动检查。

+   示例应该具有代表性。这可能涉及运行计算密集型任务。因此，重要的是要与单元测试套件解耦。

+   尽可能定期运行这些示例。

## 随机测试

科学软件可能需要处理非确定性行为。关于如何处理这一点有许多不同的哲学。我个人的方法是通过种子值尽可能控制随机性。这已经成为机器学习实验中的标准，我相信这也是进行通用科学计算的“正确方式”。

我还相信，[猴子测试](https://en.wikipedia.org/wiki/Monkey_testing)（即模糊测试）——在每次运行时测试随机值的做法——在开发科学软件中扮演着极其宝贵的角色。适当使用猴子测试可以发现隐蔽的错误并增强你的单元测试库。如果使用不当，它可能会创建一个完全不可预测的测试套件。好的猴子测试具有以下特点：

+   测试*必须可重现*。记录所有重新运行测试所需的种子。

+   随机输入必须覆盖*所有*可能的输入，并且*仅*覆盖这些可能的输入。

+   如果可以预测边界情况，则单独处理这些情况。

+   测试应该能够捕捉错误和其他不良行为，除了测试准确性外。如果测试不能标记不良行为，则毫无意义。

+   应该将不良行为作为单独的测试进行研究和隔离，这些测试检查会生成这些错误的整个情况类别（例如，如果输入 -1 失败，并且经过调查发现所有负数都失败，则创建一个针对所有负数的测试）。

# 分析

除了验证和确认外，开发高性能科学软件的开发人员必须注意性能回归。因此，分析是开发过程的一个重要部分，确保你能从代码中获得最佳性能。

但分析可能很棘手。以下是我在分析科学软件时使用的一些指导原则。

+   分析单元。类似于测试单元，你应该分析性能关键的代码单元。NVIDIA 的 CUDA 最佳实践模型是 [评估、并行化、优化、部署 (APOD)](https://docs.nvidia.com/cuda/cuda-c-best-practices-guide/index.html#assess-parallelize-optimize-deploy)。分析单元让你能够评估是否要将代码移植到 GPU 上。

+   首先分析重要的部分。尽量小心，但不要分析不会反复运行的代码片段，或者优化这些片段不会带来显著的收益。

+   多样化分析。分析 CPU 时间、内存和应用程序的任何其他有用指标。

+   确保用于分析的环境是可重复的。包括库版本、CPU 负载等。

+   尝试在单元测试中进行分析。你不需要让回归测试失败，但至少应该标记它们。

# 将一切整合起来：逐步模型

在这一部分，我们将简要描述我为科学软件应用的开发方法学的主要阶段。这些步骤得到了在学术界、工业界和开源项目中编写科学软件的经验的启发，遵循了上述最佳实践。虽然我不能说我总是应用了这些方法，但我可以诚实地说，我总是后悔没有这样做！

## 实施周期

1.  **收集需求**。你将如何使用你的方法？考虑它必须提供什么功能、灵活性、输入和输出、是否独立或作为某个更大代码库的一部分。考虑它现在必须做什么以及你将来可能希望它做什么。在这个阶段很容易过早优化，所以记住：“[保持简单，傻瓜](https://en.wikipedia.org/wiki/KISS_principle)” 和 “[你不会需要它](https://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it)”。

1.  **勾画设计**。创建一个模板，无论是代码还是图示，以建立满足上述要求的设计。

1.  **实现初始测试**。你在第 3 步，迫不及待想开始编码。深呼吸！你会开始编码，但不是你的方法/功能。在这个步骤你编写超简单的测试。真的很小。从简单的验证测试开始，然后转到基本的验证测试。对于验证测试，我建议在开始时尽可能利用分析预设。如果无法做到，请跳过它们。

1.  **实现你的 alpha 版本**。你已经有了测试（验证），可以开始实际实现代码来满足这些测试，而不必担心（很）错误。这个初始实现不必是最快的，但*需要是正确的*（验证）！我的建议是：从一个简单的实现开始，利用标准库。依赖标准库可以大大降低不正确实现的风险，因为它们利用了自己的测试套件。

1.  **建立预设库**。我不能过分强调这一点！在这一点上，你需要建立*可靠的*预设，以便你在未来的实现和/或方法变更中始终可以依赖它们。这部分通常在传统的测试驱动开发（TDD）中缺失，但在科学软件中至关重要。它确保你的结果不仅在数值上是正确的，而且可以防止新的或可能不同的实现*科学上不准确*。在构建验证预设时来回折腾实现和探索脚本是正常的，但避免同时编写测试。

1.  **重新审视测试**。利用你辛勤保存的预设，编写更多的验证单元测试。同样，避免在实现和测试之间来回折腾。

1.  **实现性能分析**。在你的单元测试内部和外部设置性能分析。一旦你完成了第一次迭代，你会回来处理这个问题。

## 优化周期

1.  **优化**。你现在要使这个函数在你的应用中尽可能快速。利用你的测试和性能分析工具，你可以发挥你的科学计算知识来加速它。

1.  **重新实现**。在这里你可以考虑新的实现方式，例如使用硬件加速库如 GPU、分布式计算等。我建议使用 NVIDIA 的 APOD（[评估、并行化、优化、部署](https://docs.nvidia.com/cuda/cuda-c-best-practices-guide/index.html#assess-parallelize-optimize-deploy)）作为一种良好的优化方法论。你可以返回实现周期，但现在你总是拥有一堆预设和测试。如果你预期功能会改变，请参见下面。

## 新方法周期

1.  **实现新方法**。按照实现周期进行操作，直到第 6 步，包括第 6 步，就像你没有任何预设一样。

1.  **验证与之前策划的神谕的对比**。在神谕构建步骤之后，你可以利用之前实施中的神谕示例，以确保新的神谕在某种程度上“更好”。这一步骤在开发对各种数据都具有鲁棒性的算法和方法中至关重要。在工业界，这一过程被频繁使用，以确保新算法在各种相关情况下表现良好。

# 下一步

许多这些原则可能只有在查看具体示例时才真正有意义。科学计算涉及多种不同类型的软件，服务于许多目的，因此一种方法很少适用于所有情况。

我鼓励你查看[本系列的下一部分](https://medium.com/@cdacostaf/developing-scientific-software-d023a96188a3)，了解如何在实践中实施这些步骤。
